# DHCPv6

Created by: Tobias
Created time: April 24, 2025 10:03 AM
Category: Netværk, Notes
Last updated time: May 2, 2025 8:11 AM

# 📘 DHCPv6 Configuration Cheat Sheet (CCNA)

---

## 🧭 Enabling IPv6 Unicast Routing

```bash
bash
CopyEdit
enable
configure terminal
ipv6 unicast-routing

```

> Enables forwarding of IPv6 unicast datagrams.
> 

---

## 🧱 Configuring RA Guard Policy

```bash
bash
CopyEdit
enable
configure terminal
ipv6 nd raguard policy raguard-router
 trustedport
 device-role router
 exit

```

> Defines and configures an RA Guard policy.
> 

### 🔗 Apply RA Guard to Interface

```bash
bash
CopyEdit
interface tengigabitethernet 1/0/1
 ipv6 nd raguard attach-policy raguard-router
 exit

```

---

## 🧠 Configuring IPv6 Snooping

```bash
bash
CopyEdit
enable
configure terminal
vlan configuration 1
 ipv6 snooping
 ipv6 nd suppress
 exit

```

> Enables IPv6 snooping and suppresses neighbor discovery multicast.
> 

---

## 🔄 ND Suppress Policy Configuration

```bash
bash
CopyEdit
enable
configure terminal
ipv6 nd suppress policy policy1

```

> Minimizes ND multicast traffic by intercepting or converting it to unicast.
> 

---

## ⚙️ ND Suppress on VLAN / Interface

```bash
bash
CopyEdit
enable
configure terminal
vlan config901
 ipv6 nd suppress
 end

interface gi1/0/1
 ipv6 nd suppress
 end

```

---

## 🌐 Configure IPv6 on a Switch Interface

```bash
bash
CopyEdit
enable
configure terminal
interface vlan 1
 ipv6 address fe80::1 link-local
 ipv6 address 2001:DB8:0:1:FFFF:1234::5/64
 ipv6 address 2001:DB8:0:0:E000::F/64
 ipv6 enable
 end

```

---

## 📦 Configure IPv6 DHCP Pool (Stateful)

```bash
bash
CopyEdit
enable
configure terminal
ipv6 dhcp pool Vlan21
 address prefix 2001:DB8:0:1:FFFF:1234::/64 lifetime 300 10
 dns-server 2001:100:0:1::1
 domain-name example.com
 end

```

---

## 🌀 Stateless Autoconfiguration Without DHCP

```bash
bash
CopyEdit
interface vlan 1
 ipv6 address fe80::1 link-local
 ipv6 address 2001:DB8:0:1:FFFF:1234::5/64
 ipv6 enable
 no ipv6 nd managed-config-flag
 no ipv6 nd other-config-flag
 end

```

---

## 🌀 Stateless Autoconfiguration With DHCP

```bash
bash
CopyEdit
interface vlan 1
 ipv6 address fe80::1 link-local
 ipv6 address 2001:DB8:0:1:FFFF:1234::5/64
 ipv6 enable
 no ipv6 nd managed-config-flag
 ipv6 nd other-config-flag
 end

```

---

## 🖥️ Stateful DHCPv6 (Local Server)

```bash
bash
CopyEdit
ipv6 unicast-routing
ipv6 dhcp pool IPv6_DHCPPOOL
 address prefix 2001:DB8:0:1:FFFF:1234::/64
 dns-server 2001:100:0:1::1
 domain-name example.com
 exit

interface vlan 1
 description IPv6-DHCP-Stateful
 ipv6 address 2001:DB8:0:20::1/64
 ip address 192.168.20.1 255.255.255.0
 ipv6 nd prefix 2001:db8::/64 no-advertise
 ipv6 nd managed-config-flag
 ipv6 nd other-config-flag
 ipv6 dhcp server IPv6_DHCPPOOL

```

---

## 🌐 Stateful DHCPv6 (External Server)

```bash
bash
CopyEdit
ipv6 unicast-routing

interface vlan 1
 description IPv6-DHCP-Stateful
 ipv6 address 2001:DB8:0:20::1/64
 ip address 192.168.20.1 255.255.255.0
 ipv6 nd prefix 2001:db8::/64 no-advertise
 ipv6 nd managed-config-flag
 ipv6 nd other-config-flag
 ipv6 dhcp relay destination 2001:DB8:0:20::2

```

---

## 🔍 Verifying DHCPv6 Pool

```bash
bash
CopyEdit
show ipv6 dhcp pool

```

**Sample Output**:

```
plaintext
CopyEdit
DHCPv6 pool: vlan21
Address allocation prefix: 2001:DB8:0:1:FFFF:1234::/64
DNS server: 2001:100:0:1::1
Domain name: example.com
Active clients: 6

```

# 📘 IPv6 ND `config-flag` Commands Cheat Sheet

**ND = Neighbor Discovery**

These flags are configured on interfaces to **tell clients how to obtain their IPv6 addressing and configuration info**.

---

## 🧠 Overview of the Flags

| Command | Flag | Description |
| --- | --- | --- |
| `ipv6 nd managed-config-flag` | **M** (Managed) | Tells clients to use **stateful DHCPv6** to get their full IP config. |
| `ipv6 nd other-config-flag` | **O** (Other) | Tells clients to use **stateless DHCPv6** for **additional info only** (DNS, domain name). |

---

## 🚦 When to Use Each Flag

| Scenario | M Flag | O Flag | Use Case |
| --- | --- | --- | --- |
| **Stateless Autoconfiguration (SLAAC only)** | ❌ | ❌ | Clients generate IP from RA prefix. No DHCP used. |
| **SLAAC + Stateless DHCPv6** | ❌ | ✅ | IP from RA, but DNS/domain from DHCPv6. |
| **Stateful DHCPv6 (Full config)** | ✅ | ✅ or ❌ | IP and other info from DHCPv6 only. RA tells client not to use SLAAC. |

> 🔧 Note: If managed-config-flag is enabled, clients must use DHCPv6 for addressing and ignore SLAAC.
> 

---

## 🛠️ Interface Config Examples

### ✅ Stateless SLAAC Only (RA only)

```bash
bash
CopyEdit
interface g0/0
 ipv6 nd prefix 2001:db8:acad::/64
 no ipv6 nd managed-config-flag
 no ipv6 nd other-config-flag

```

### ✅ SLAAC + Stateless DHCPv6 (RA + DNS from DHCP)

```bash
bash
CopyEdit
interface g0/0
 ipv6 nd prefix 2001:db8:acad::/64
 no ipv6 nd managed-config-flag
 ipv6 nd other-config-flag

```

### ✅ Stateful DHCPv6 Only (RA provides no address)

```bash
bash
CopyEdit
interface g0/0
 ipv6 nd prefix 2001:db8:acad::/64 no-autoconfig
 ipv6 nd managed-config-flag

```

### ✅ Full Stateful DHCPv6 with DNS

```bash
bash
CopyEdit
interface g0/0
 ipv6 nd prefix 2001:db8:acad::/64 no-autoconfig
 ipv6 nd managed-config-flag
 ipv6 nd other-config-flag

```

---

## 🔍 Verification Command

```bash
bash
CopyEdit
show ipv6 interface g0/0

```

Look for:

```
plaintext
CopyEdit
Managed address configuration: TRUE/FALSE
Other information via DHCP: TRUE/FALSE

```

---

## 📋 Summary Table

| Mode | M Flag | O Flag | IP Address Source | DNS Source |
| --- | --- | --- | --- | --- |
| SLAAC only | ❌ | ❌ | RA prefix | Manual |
| SLAAC + Stateless DHCPv6 | ❌ | ✅ | RA prefix | DHCPv6 |
| Stateful DHCPv6 | ✅ | ❌ or ✅ | DHCPv6 | DHCPv6 |