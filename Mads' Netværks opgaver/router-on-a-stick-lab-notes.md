# Router-on-a-Stick + VLAN Lab — Config Notes

## Topology

```
Router  ──trunk──  S1  ══ Po1 (LACP, 2 links) ══  S2
                   │                              │
                 hosts                          hosts
        (DHCP server lives in VLAN 20)
```

- **Router** = inter-VLAN routing (one subinterface per VLAN) + DHCP relay.
- **S1** = holds the router uplink; root bridge.
- **S2** = access switch; bundled to S1 via EtherChannel.

## Addressing / VLAN Plan

| VLAN | Subnet | Gateway (on router) | Role |
|------|--------|---------------------|------|
| 10 | 10.0.10.0/24 | 10.0.10.1 | users |
| 20 | 10.0.20.0/24 | 10.0.20.1 | users + DHCP server (10.0.20.10) |
| 30 | 10.0.30.0/24 | 10.0.30.1 | users |
| 40 | 10.0.40.0/24 | 10.0.40.1 | users |
| 90 | 10.0.90.0/24 | *(native — no IP)* | trunk hygiene |
| 99 | 10.0.99.0/24 | *(none — blackhole)* | dead-end for unused ports |

Convention: gateway = `.1` of each subnet. VLAN 1 stays unused (no hosts, not native, not management).

---

## Base Hardening (all devices)

```cisco
enable secret <STRONG-PASS>
service password-encryption
username admin secret <STRONG-PASS>
no ip domain-lookup
ip domain-name lab.local
crypto key generate rsa modulus 2048
ip ssh version 2
banner motd #Authorized access only. Activity is logged.#
line console 0
 logging synchronous
 exec-timeout 10 0
 login local
line vty 0 4
 transport input ssh
 login local
 exec-timeout 10 0
```

---

## Router (edge / router-on-a-stick)

```cisco
! Physical trunk — no IP, addressing lives on subinterfaces
interface GigabitEthernet0/0
 no ip address
 no shutdown

! User VLAN gateways
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 10.0.10.1 255.255.255.0
 ip helper-address 10.0.20.10        ! DHCP relay → server in VLAN 20
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 10.0.20.1 255.255.255.0  ! no helper — DHCP server is local here
interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 10.0.30.1 255.255.255.0
 ip helper-address 10.0.20.10
interface GigabitEthernet0/0.40
 encapsulation dot1Q 40
 ip address 10.0.40.1 255.255.255.0
 ip helper-address 10.0.20.10

! Native VLAN — declared so router/switch agree; no IP (carries no hosts)
interface GigabitEthernet0/0.90
 encapsulation dot1Q 90 native

! VLAN 99 (blackhole): intentionally NOT present on the router.
```

---

## Switch S1 (router uplink + root bridge)

```cisco
hostname S1
no ip domain-lookup
vtp mode transparent

! --- Spanning tree ---
spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30,40,90 priority 4096   ! S1 = root
spanning-tree portfast bpduguard default          ! BPDU guard on all edge ports

! --- VLAN database ---
vlan 10
 name USERS-10
vlan 20
 name USERS-20
vlan 30
 name USERS-30
vlan 40
 name USERS-40
vlan 90
 name NATIVE
vlan 99
 name BLACKHOLE

! --- Trunk to ROUTER ---
interface GigabitEthernet0/1
 description TRUNK-TO-ROUTER
 switchport trunk encapsulation dot1q       ! 3560-only; omit on 2960
 switchport mode trunk
 switchport trunk native vlan 90
 switchport trunk allowed vlan 10,20,30,40,90   ! 99 excluded

! --- EtherChannel to S2 (LACP) ---
interface range GigabitEthernet0/2 - 3
 description LINK-TO-S2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 90
 switchport trunk allowed vlan 10,20,30,40,90
 channel-group 1 mode active                ! LACP active
interface Port-channel1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 90
 switchport trunk allowed vlan 10,20,30,40,90
 spanning-tree guard root                   ! S2 can never become root

! --- Example host port ---
interface GigabitEthernet0/4
 description PC-VLAN10
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast

! --- Unused ports: blackhole + disabled ---
interface range GigabitEthernet0/5 - 24
 switchport mode access
 switchport access vlan 99
 shutdown
```

---

## Switch S2 (access; bundled to S1)

```cisco
hostname S2
no ip domain-lookup
vtp mode transparent

spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30,40,90 priority 8192   ! S2 = secondary root
spanning-tree portfast bpduguard default

vlan 10
 name USERS-10
vlan 20
 name USERS-20
vlan 30
 name USERS-30
vlan 40
 name USERS-40
vlan 90
 name NATIVE
vlan 99
 name BLACKHOLE

! --- EtherChannel to S1 ---
interface range GigabitEthernet0/1 - 2
 description LINK-TO-S1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 90
 switchport trunk allowed vlan 10,20,30,40,90
 channel-group 1 mode active
interface Port-channel1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 90
 switchport trunk allowed vlan 10,20,30,40,90

! --- Example host port ---
interface GigabitEthernet0/3
 description PC-VLAN20
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast

! --- Unused ports: blackhole + disabled ---
interface range GigabitEthernet0/4 - 24
 switchport mode access
 switchport access vlan 99
 shutdown
```

---

## Linux DHCP Server (ISC DHCP) — 10.0.20.10

> ISC DHCP is EOL upstream; **Kea** is the supported successor (same concepts, JSON config).

**Switch port for the server** — access in VLAN 20:

```cisco
interface GigabitEthernet0/X
 description DHCP-SERVER
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
```

**Server NIC:** static `10.0.20.10/24`, gateway `10.0.20.1`. Single NIC, single subnet — the router's relay brings the other subnets to it.

**Install:**
```bash
sudo apt update && sudo apt install isc-dhcp-server
```

**`/etc/dhcp/dhcpd.conf`:**
```conf
default-lease-time 3600;
max-lease-time 86400;
option domain-name "lab.local";
option domain-name-servers 10.0.20.10;   # point at real DNS
authoritative;

subnet 10.0.20.0 netmask 255.255.255.0 {   # server's own subnet — required to start
  range 10.0.20.50 10.0.20.200;
  option routers 10.0.20.1;
}
subnet 10.0.10.0 netmask 255.255.255.0 {    # relayed
  range 10.0.10.50 10.0.10.200;
  option routers 10.0.10.1;
}
subnet 10.0.30.0 netmask 255.255.255.0 {    # relayed
  range 10.0.30.50 10.0.30.200;
  option routers 10.0.30.1;
}
subnet 10.0.40.0 netmask 255.255.255.0 {    # relayed
  range 10.0.40.50 10.0.40.200;
  option routers 10.0.40.1;
}
```

**Listening interface — `/etc/default/isc-dhcp-server`:**
```conf
INTERFACESv4="ens18"
```

**Start + firewall:**
```bash
sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf      # syntax check first
sudo systemctl enable --now isc-dhcp-server
sudo ufw allow 67/udp                        # if a firewall is active
```

---

## Key Concepts & Gotchas

- **Native VLAN must match on every trunk** (90 here: router↔S1 and both ends of S1↔S2). Mismatch → CDP warning + traffic leak.
- **VLAN 99 (blackhole):** exists in each switch's DB only, pruned from every trunk, never on the router. Unused ports parked here **and** shut down. A new device in a blackhole port gets nothing — secure by default.
- **EtherChannel:** both member ports per side must have identical trunk settings or they won't bundle. STP sees the bundle as **one logical link**, so nothing is blocked.
- **RSTP isn't blocking anything in this topology** (no redundant L2 path). It buys a deterministic root, sub-second convergence on change, and instant-forwarding edge ports.
- **BPDU guard = edge/PortFast ports only, never trunks.** The global `portfast bpduguard default` enforces that automatically. Tripped port → err-disabled (manual recovery by default).
- **DHCP relay:** `ip helper-address` on **client** VLANs only (10/30/40), not on the server's VLAN 20. Router stamps `giaddr` (= its `.1` in that subnet); server uses it to pick the scope.
- **Every relayed VLAN needs a matching `subnet` block** on the server, with `option routers` = the router's `.1`. Missing block = silently ignored requests.

---

## Verification Cheat-Sheet

```text
# VLANs / trunks
show vlan brief
show interfaces trunk

# EtherChannel  (want member ports flagged "P", protocol LACP)
show etherchannel summary

# Spanning tree  (S1 should say "This bridge is the root")
show spanning-tree vlan 10
show spanning-tree summary

# BPDU guard / err-disabled ports
show interfaces status err-disabled

# Router subinterfaces + relay
show ip interface brief
show ip interface GigabitEthernet0/0.10   | include Helper

# DHCP server (Linux)
journalctl -u isc-dhcp-server -f
cat /var/lib/dhcp/dhcpd.leases
```

---

## Adding a New Host

Unused ports are blackholed + shut, so adding a host is always deliberate:

```cisco
interface GigabitEthernet0/6
 no shutdown
 description PC-VLAN30
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
```
