# Networking Note

Created by: Tobias
Created time: April 1, 2025 1:54 PM
Category: Netværk, Notes
Last updated time: April 7, 2025 7:46 AM

# 🌐 **Networking Master Note for IT Support EUD**

---

## 🟢 Basic Network Concepts

### 🔸 What is a Network?

A network is a collection of interconnected devices that communicate and share resources such as files, printers, or internet connections.

**Types of Networks:**
- **LAN (Local Area Network)** – Small area
- **WAN (Wide Area Network)** – Large area
- **MAN (Metropolitan Area Network)** – City-wide
- **PAN (Personal Area Network)** – Close proximity devices (Bluetooth)

**Network Topologies:**`Bus`, `Star`, `Ring`, `Mesh`, `Hybrid`

**Common Protocols:**`TCP/IP`, `UDP`, `HTTP/HTTPS`, `FTP/SFTP`, `SSH`, `DNS`

---

## 📊 OSI Model

| Layer | Name | Description |
| --- | --- | --- |
| 7 | Application | User Interface (HTTP, FTP) |
| 6 | Presentation | Data Translation, Encryption |
| 5 | Session | Connection Management |
| 4 | Transport | Data Transfer (TCP/UDP) |
| 3 | Network | Logical Addressing & Routing (IP) |
| 2 | Data Link | MAC Addressing, Error Detection |
| 1 | Physical | Cables, Hubs, Connectors |

---

## 🟢 IP Addressing & Subnetting

### 🔸 IP Versions

- **IPv4:** 32-bit numeric address
- **IPv6:** 128-bit alphanumeric address

### 🔸 Subnetting

**Example:**

`192.168.1.0/24`
- Subnet Mask: `255.255.255.0`
- Host Range: `192.168.1.1 – 192.168.1.254`

**Subnet Formula:**

`Hosts = 2^(32 - subnet bits) - 2`

---

## 🟢 DHCP & DNS

- **DHCP:** Automatically assigns IP addresses to devices.
- **DNS:** Translates domain names (google.com) to IP addresses.

---

## 🟢 Routing & Switching

**Router:** Connects multiple networks

**Switch:** Connects devices in one LAN

**VLAN:** Separates network traffic virtually

**Routing Types:** Static & Dynamic (OSPF, RIP, BGP)

---

## 📶 Wireless Networking

- **Frequencies:** 2.4GHz, 5GHz
- **WiFi Standards:** 802.11 a/b/g/n/ac/ax
- **Security:** WPA2, WPA3

---

## 🔥 Firewalls & NAT

**Firewall:** Controls network traffic

**NAT:** Translates private IPs to public IPs

---

## 🛡️ VPN

**Types:** Remote Access, Site-to-Site, SSL, IPsec

**Benefits:** Encryption, Privacy, Secure Access

---

## 🚨 Network Security Basics

**Common Threats:** DDoS, Phishing, Malware, Packet Sniffing

**Security Measures:** 2FA, VLANs, Patching, IDS/IPS

---

## 🛠️ Monitoring & Troubleshooting

**Useful Tools:**

`ping`, `tracert`, `ipconfig`, `netstat`, `nslookup`, `nmap`

**Troubleshooting Steps:**
1. Identify Issue
2. Establish Theory
3. Test Theory
4. Action Plan
5. Verify
6. Document

---

## 🔌 Network Cables & Standards

**Cable Types:** UTP, STP, Fiber, Coaxial

**Connectors:** RJ45, LC, SC

**Categories:** Cat5e, Cat6, Cat7, Cat8

---

## 🔗 Ports & Protocols

| Port | Protocol |
| --- | --- |
| 20, 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 3389 | RDP |

---

## 🚀 Advanced Network Concepts

- **QoS (Quality of Service)**
- **Load Balancing**
- **Redundancy**
- **Proxy Servers**

---

## ☁️ Cloud Networking

**Models:** IaaS, PaaS, SaaS

**Virtual Networks:** VLAN, SDN

---

## ✅ Exercises

1. **Subnetting:** Calculate subnet mask & hosts for 192.168.10.0/26
2. **IP Assignment:** Assign valid IPs in 192.168.5.0/29
3. **OSI Layers:** Match FTP, IP, TCP, MAC
4. **DNS:** Run `nslookup google.com`
5. **Firewall Rule:** Block HTTP traffic
6. **Troubleshoot:** List five steps to fix “No internet” issue

---

## 🎯 Diagrams

**Basic Network Topology:**

```
[PC1]---\
        \\
         [Switch]---[Router]---(Internet)
        //
[PC2]---//
```

**OSI Model:**

```
+----------------------+
| 7. Application      |
+----------------------+
| 6. Presentation     |
+----------------------+
| 5. Session          |
+----------------------+
| 4. Transport        |
+----------------------+
| 3. Network          |
+----------------------+
| 2. Data Link        |
+----------------------+
| 1. Physical         |
+----------------------+
```

**Subnetting Table:**
| CIDR | Subnet Mask | # of Hosts |
|:—-:|:—————–:|:———:|
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /26 | 255.255.255.192 | 62 |

---

**Keep practicing! Networking knowledge = IT POWER 🚀**