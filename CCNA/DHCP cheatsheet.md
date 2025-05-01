# DHCP cheatsheet

Created by: Tobias
Created time: May 1, 2025 8:30 PM
Last updated time: May 1, 2025 8:32 PM

# 📘 DHCPv4 / DHCPv6 & DORA Cheatsheet

---

## 🔄 DORA Process (DHCPv4)

The **DORA** acronym describes the 4-step DHCPv4 process:

| Step | Name | Description | Message Type | Broadcast/Unicast |
| --- | --- | --- | --- | --- |
| 1 | **Discover** | Client broadcasts to locate a DHCP server. | DHCPDISCOVER | Broadcast |
| 2 | **Offer** | Server replies with offered IP configuration. | DHCPOFFER | Unicast |
| 3 | **Request** | Client requests the offered IP. | DHCPREQUEST | Broadcast |
| 4 | **Acknowledgment** | Server confirms lease. | DHCPACK | Unicast |

> 📡 All DORA messages use UDP:
> 
> - Client → Server: **Port 67**
> - Server → Client: **Port 68**

---

## 📦 DHCPv4 Key Configuration Elements

| Parameter | Description |
| --- | --- |
| IP address | Unique host address on the network |
| Subnet mask | Defines the subnet range |
| Default gateway | Router IP to reach other networks |
| DNS servers | IPs for domain name resolution |
| Lease time | How long the address is valid |

---

## ⚙️ DHCPv4 Server IOS Example

```bash
bash
CopyEdit
R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10
R1(config)# ip dhcp pool LAN-POOL
R1(dhcp-config)# network 192.168.1.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.1.1
R1(dhcp-config)# dns-server 8.8.8.8 1.1.1.1

```

---

## 🌐 DHCPv6 Overview

There are **two DHCPv6 modes**:

| Mode | Description | Use Case |
| --- | --- | --- |
| **Stateful DHCPv6** | Like DHCPv4: server assigns full IP config (including IP). | Full device management |
| **Stateless DHCPv6** | Client gets IP via SLAAC, but uses DHCP for DNS info. | Lightweight management |

---

### 🧠 DHCPv6 Key Terms

| Term | Meaning |
| --- | --- |
| **RA** | Router Advertisement (sent by routers) |
| **SLAAC** | Stateless Address Autoconfiguration (uses RA to create IPv6 address) |
| **DHCPv6 Server** | Central server that provides IPv6 config info |
| **Link-local Address** | Used by client to communicate with server or router on the same link |

---

## 💬 DHCPv6 Message Types

| Step | Stateful DHCPv6 | Stateless DHCPv6 | Transport |
| --- | --- | --- | --- |
| 1 | Solicit | Solicit | UDP 546 → 547 |
| 2 | Advertise | Advertise |  |
| 3 | Request | Information-Request |  |
| 4 | Reply | Reply |  |

> ✅ Clients and servers use UDP ports 546 (client) and 547 (server).
> 

---

## 🛠️ DHCPv6 IOS Example (Stateful)

```bash
bash
CopyEdit
R1(config)# ipv6 unicast-routing
R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 enable
R1(config-if)# ipv6 dhcp server STATEFUL-POOL

R1(config)# ipv6 dhcp pool STATEFUL-POOL
R1(config-dhcpv6)# address prefix 2001:db8:acad:1::/64
R1(config-dhcpv6)# dns-server 2001:4860:4860::8888
R1(config-dhcpv6)# domain-name ccnalab.local

```

---

## ✅ DHCPv4 vs DHCPv6 Quick Comparison

| Feature | DHCPv4 | DHCPv6 |
| --- | --- | --- |
| Transport Ports | UDP 67/68 | UDP 546/547 |
| IP Assignment | Server-assigned | Stateful (server) or Stateless (SLAAC + RA) |
| Broadcast | Used | Not used (multicast instead) |
| Autoconfiguration | ❌ | ✅ via SLAAC |
| DORA Process | Yes | Similar but uses Solicit/Advertise |