# Opgave 2 — Site-to-Site IPSec VPN (WatchGuard) — Config Notes

Three-tier campus (Core / Distribution / Access) with HSRP load-balancing, OSPF (authenticated), full LAN-security hardening, DHCP relayed to MLS4, and a Branch-Office IPSec VPN to the DataCenter terminated on WatchGuard firewalls.

> **Assumptions / gaps in the brief (decide before you build):**
> - **Password length conflict:** the brief says *R1 & R2 → all passwords min length 10*, but the topology lists `cisco` / `class` (5 chars). On R1/R2 I use 10+ char passwords (`cisco12345` / `class12345`). Everywhere else keeps `cisco` / `class`.
> - **SVI + HSRP virtual IPs aren't given** — I assigned them below. Adjust to your lab.
> - **DHCP server is MLS4, clients are behind MLS1/MLS2** → you need `ip helper-address` relays on the VLAN SVIs. Not drawn, but required.
> - **R1↔R2 link (172.16.0.8/30)** uses an unlabeled 3rd interface — I call it `g0/0/2`; match your hardware.
> - WatchGuard is configured in **Fireware Web UI**, not IOS.

---

## Addressing table

### L3 transit links (/30 unless noted)

| Link | Subnet | A side | B side |
|---|---|---|---|
| WG1 ↔ R1 | 172.16.0.0/30 | WG1 g1/1 `.1` | R1 g0/0/1 `.2` |
| WG1 ↔ R2 | 172.16.0.4/30 | WG1 g1/3 `.5` | R2 g0/0/1 `.6` |
| R1 ↔ R2 | 172.16.0.8/30 | R1 g0/0/2 `.9` | R2 g0/0/2 `.10` |
| R1 ↔ MLS1 | 172.16.1.0/30 | R1 g0/0/0 `.1` | MLS1 `.2` |
| R2 ↔ MLS2 | 172.16.1.4/30 | R2 g0/0/0 `.5` | MLS2 `.6` |
| MLS1 ↔ MLS2 | 172.16.1.8/30 | MLS1 `.9` | MLS2 `.10` |
| MLS2 ↔ MLS4 | 10.0.1.0/29 | MLS2 `.5` | MLS4 `.4` |
| WG1 ↔ ISP | 10.50.0.0/16 | WG1 g1/2 (external) | DG `10.50.1.1` |
| WG2 ↔ ISP | 10.50.0.0/16 | WG2 external `.3` | DG `10.50.1.1` |
| WG2 ↔ DataCenter | 191.17.1.0/30 | WG2 `.1` | DataCenter `.2` |

### VLANs / SVIs (suggested SVI IPs — HSRP)

| VLAN | Subnet | MLS1 SVI | MLS2 SVI | Virtual IP (GW) | Role |
|---|---|---|---|---|---|
| 20 (Mgmt) | 172.16.2.0/29 | .2 | .3 | **.1** | Management |
| 30 | 192.168.3.0/27 | .2 | .3 | **.1** | Klient 1 |
| 33 | 192.168.3.32/27 | .34 | .35 | **.33** | Klient 2 |
| 999 | — | — | — | — | Blackhole (unused ports) |

MLS4 = DHCP/DNS server `10.0.1.4`.

---

## 1. Basic config (all devices)

```cisco
enable
configure terminal
hostname <NAME>
no ip domain-lookup
service password-encryption          ! encrypt all plaintext passwords
enable secret class                  ! use class12345 on R1/R2 (see note)
banner motd #Authorized access only#

line con 0
 password cisco                      ! cisco12345 on R1/R2
 login
 logging synchronous
 exec-timeout 5 0
line vty 0 4
 password cisco
 login
```

**R1 & R2 only — enforce min length 10:**
```cisco
! set the strong passwords FIRST, then raise the floor
enable secret class12345
security passwords min-length 10
line con 0
 password cisco12345
```
> Order matters: if you run `security passwords min-length 10` *before* setting a password, `enable secret class` (5 chars) is rejected.

---

## 2. VLANs

On **SW1, SW2, MLS1, MLS2**:
```cisco
vlan 20
 name MGMT
vlan 30
 name KLIENT1
vlan 33
 name KLIENT2
vlan 999
 name BLACKHOLE
```

**MLS1 / MLS2 — SVIs + inter-VLAN routing:**
```cisco
ip routing
interface vlan 20
 ip address 172.16.2.2 255.255.255.248     ! MLS2 = .3
interface vlan 30
 ip address 192.168.3.2  255.255.255.224    ! MLS2 = .3
 ip helper-address 10.0.1.4                 ! DHCP relay to MLS4
interface vlan 33
 ip address 192.168.3.34 255.255.255.224    ! MLS2 = .35
 ip helper-address 10.0.1.4
```

---

## 3. EtherChannel (LACP trunks)

SW1 dual-homes to both MLS (ch1→MLS1, ch2→MLS2); SW2 does ch4→MLS1, ch5→MLS2. Channel-group numbers are locally significant; using the diagram labels for clarity.

**SW1 → MLS1 (ch1)** — repeat pattern for ch2/ch4/ch5 on both ends:
```cisco
interface range g0/1 - 2                ! member ports
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 999       ! VLAN-hop mitigation (unused native)
 switchport trunk allowed vlan 20,30,33
 switchport nonegotiate                 ! kill DTP
 channel-group 1 mode active            ! LACP
interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 20,30,33
```
Verify: `show etherchannel summary` (look for `SU` / `P`).

---

## 4. HSRP (load-balance + failover)

MLS1 active for user VLANs 30/33; MLS2 active for mgmt VLAN 20 → traffic split. `preempt` gives automatic failback.

**MLS1:**
```cisco
interface vlan 30
 standby 30 ip 192.168.3.1
 standby 30 priority 150
 standby 30 preempt
interface vlan 33
 standby 33 ip 192.168.3.33
 standby 33 priority 150
 standby 33 preempt
interface vlan 20
 standby 20 ip 172.16.2.1
 standby 20 priority 100               ! standby here
```

**MLS2** (mirror — active on 20, standby on 30/33):
```cisco
interface vlan 20
 standby 20 ip 172.16.2.1
 standby 20 priority 150
 standby 20 preempt
interface vlan 30
 standby 30 ip 192.168.3.1
 standby 30 priority 100
interface vlan 33
 standby 33 ip 192.168.3.33
 standby 33 priority 100
```
Optional resilient failover — track the uplink and decrement priority:
```cisco
 standby 30 track g0/0/1 20
```
Verify: `show standby brief`.

---

## 5. OSPF (area 0) + MD5 authentication

Runs on **MLS1, MLS2, R1, R2**. R1/R2 originate the default route toward WG1 (VPN/Internet exit).

**Per-interface auth** (apply to every OSPF-facing transit interface between the four devices):
```cisco
interface <transit-int>
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 OspfKey123
```

**R1** (R2 mirrors with RID 6.6.6.6 / its own subnets):
```cisco
ip route 0.0.0.0 0.0.0.0 172.16.0.1        ! default to WG1
router ospf 1
 router-id 7.7.7.7
 area 0 authentication message-digest
 network 172.16.0.0 0.0.0.3 area 0
 network 172.16.0.8 0.0.0.3 area 0
 network 172.16.1.0 0.0.0.3 area 0
 default-information originate
```

**MLS1** (MLS2 = RID 4.4.4.4, advertises 10.0.1.0/29 + VLAN 20):
```cisco
router ospf 1
 router-id 5.5.5.5
 area 0 authentication message-digest
 network 172.16.1.0 0.0.0.3 area 0
 network 172.16.1.8 0.0.0.3 area 0
 network 192.168.3.0 0.0.0.31 area 0
 network 192.168.3.32 0.0.0.31 area 0
 network 172.16.2.0 0.0.0.7 area 0
 passive-interface vlan 30
 passive-interface vlan 33
 passive-interface vlan 20
```
Verify: `show ip ospf neighbor`, `show ip route ospf`.

---

## 6. LAN Security (SW1 & SW2)

### 6a. Port security — MAC flooding / CAM spoofing (end-user ports)
```cisco
interface range f0/1 - 5                ! end-user ports (e.g. SW1 f0/3, SW2 f0/1)
 switchport mode access
 switchport access vlan 30              ! or 33 per switch
 switchport port-security
 switchport port-security maximum 5     ! max 5 MACs
 switchport port-security mac-address sticky   ! learned dynamically
 switchport port-security violation shutdown    ! (or restrict)
```

### 6b. PortFast + BPDU Guard — fast forwarding + protection
```cisco
interface range f0/1 - 5
 spanning-tree portfast
 spanning-tree bpduguard enable
```

### 6c. Unused ports → Blackhole VLAN, access, shut
```cisco
interface range f0/6 - 24
 switchport mode access
 switchport access vlan 999
 shutdown
```

### 6d. VLAN attack (VLAN hopping) mitigation
Already covered on trunks: `switchport nonegotiate` (no DTP), `switchport trunk native vlan 999`, unused ports parked + shut, and **never use VLAN 1**.

### 6e. DHCP spoofing + starvation (DHCP snooping)
```cisco
ip dhcp snooping
ip dhcp snooping vlan 30,33
no ip dhcp snooping information option        ! avoid option-82 drops across the L3 relay
interface <uplink-portchannel toward MLS/DHCP path>
 ip dhcp snooping trust                       ! trust only the server-facing uplinks
interface range f0/1 - 5
 ip dhcp snooping limit rate 10               ! starvation rate-limit
```
> Spoofing → untrusted access ports can't offer DHCP. Starvation → rate-limit + port-security max 5 caps spoofed-MAC floods.

Optional hardening — Dynamic ARP Inspection (uses the snooping binding table):
```cisco
ip arp inspection vlan 30,33
interface <uplink>
 ip arp inspection trust
```

---

## 7. SSH (SW1, SW2, MLS1, MLS2, R1, R2)

```cisco
hostname <NAME>
ip domain-name opgave2.local
username admin privilege 15 secret cisco       ! cisco12345 on R1/R2
crypto key generate rsa modulus 1024
ip ssh version 2
line vty 0 4
 transport input ssh
 login local
```
Test from a client: `ssh -l admin 172.16.2.x`.

---

## 8. DHCP on MLS4

```cisco
ip dhcp excluded-address 192.168.3.1 192.168.3.5      ! SVIs + VIP
ip dhcp excluded-address 192.168.3.33 192.168.3.37
!
ip dhcp pool VLAN30
 network 192.168.3.0 255.255.255.224
 default-router 192.168.3.1                            ! HSRP VIP
 dns-server 10.0.1.4
!
ip dhcp pool VLAN33
 network 192.168.3.32 255.255.255.224
 default-router 192.168.3.33
 dns-server 10.0.1.4
```
> Because MLS4 is remote from the clients, DHCP only works with the `ip helper-address 10.0.1.4` relays on the VLAN 30/33 SVIs (section 2).

---

## 9. Site-to-Site IPSec VPN — WatchGuard (Fireware Web UI)

Configured on **WG1 (campus)** and **WG2 (DataCenter)**. Both sides must use identical Phase 1/2 parameters.

**Encryption domain (traffic to protect):**
- Local (WG1): `192.168.3.0/27` + `192.168.3.32/27` (clients VLAN 30 & 33)
- Remote (WG2): `191.17.1.0/30` (DataCenter, `191.17.1.2`)

**Phase 1 (IKE / BOVPN Gateway):**
| Setting | Value |
|---|---|
| Peer (from WG1) | WG2 external IP `10.50.1.3` |
| Credential | Pre-shared key (identical both sides) |
| Encryption | AES-256 |
| Hash | SHA-256 |
| DH Group | 14 |
| SA lifetime | 8 h |

**Phase 2 (IPSec / Tunnel Route):**
| Setting | Value |
|---|---|
| Protocol | ESP |
| Encryption | AES-256 |
| Auth | SHA-256 |
| PFS | Enabled (DH 14) |
| Lifetime | 1 h / 128000 KB |

**Steps (WG1, mirror on WG2):**
1. **Network → Interfaces:** external = 10.50.x, trusted = 172.16.0.0/30 side (toward R1), a second trusted toward R2.
2. **VPN → Branch Office VPN → Gateways:** create gateway to WG2 peer, set Phase 1 + PSK.
3. **VPN → BOVPN → Tunnels:** add tunnel, define the local/remote encryption-domain pairs above, set Phase 2.
4. **Firewall policy:** allow the BOVPN traffic (auto-created "BOVPN-Allow" policy) both directions.
5. **Static routes on WG1** back to the campus LAN so it can reach the clients:
   - `192.168.3.0/25 → 172.16.0.2` (via R1) and `→ 172.16.0.6` (via R2), or run OSPF on the WG.

> On R1/R2 the default route `0.0.0.0/0 → WG1` sends client→DataCenter traffic into the tunnel automatically.

---

## 10. Verification

```cisco
! From Klient 1 (VLAN 30) and Klient 2 (VLAN 33):
ping 191.17.1.2                     ! reach DataCenter over the tunnel

! Infrastructure checks
show ip ospf neighbor               ! MLS1/MLS2/R1/R2 all FULL
show standby brief                  ! MLS1 Active 30/33, MLS2 Active 20
show etherchannel summary           ! ch1/2/4/5 = P (bundled)
show port-security                  ! max 5, sticky, secure-up
show ip dhcp snooping binding       ! learned client bindings
show ip dhcp binding                ! (on MLS4) leases issued
```
**WatchGuard:** *System Status → VPN Statistics* — tunnel should show **Up**, with Phase 1 + Phase 2 SAs established and packet counters incrementing during the pings.

**Pass criteria:** full bidirectional connectivity between VLAN 30 / VLAN 33 clients and the DataCenter (`191.17.1.2`), traversing the IPSec tunnel.
