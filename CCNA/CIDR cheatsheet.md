# CIDR cheatsheet

Created by: Tobias
Created time: March 26, 2025 9:47 AM
Category: Cybersecurity, Netværk, Notes
Last updated time: May 2, 2025 8:40 AM

# 📏 CIDR Subnet Cheat Sheet – CCNA

---

## 🧠 What is CIDR?

**CIDR (Classless Inter-Domain Routing)** uses **slash notation** (e.g., `/24`) to define IP subnets more flexibly than classful addressing.

- CIDR = IP address + subnet prefix
    - Example: `192.168.1.0/24`

---

## 📋 Common IPv4 CIDR Subnet Table

| CIDR | Subnet Mask | Hosts per Subnet | # of Subnets (Class C) | Wildcard Mask |
| --- | --- | --- | --- | --- |
| /30 | 255.255.255.252 | 2 | 64 | 0.0.0.3 |
| /29 | 255.255.255.248 | 6 | 32 | 0.0.0.7 |
| /28 | 255.255.255.240 | 14 | 16 | 0.0.0.15 |
| /27 | 255.255.255.224 | 30 | 8 | 0.0.0.31 |
| /26 | 255.255.255.192 | 62 | 4 | 0.0.0.63 |
| /25 | 255.255.255.128 | 126 | 2 | 0.0.0.127 |
| /24 | 255.255.255.0 | 254 | 1 | 0.0.0.255 |
| /23 | 255.255.254.0 | 510 | ½ | 0.0.1.255 |
| /22 | 255.255.252.0 | 1022 | ¼ | 0.0.3.255 |
| /21 | 255.255.248.0 | 2046 | — | 0.0.7.255 |
| /20 | 255.255.240.0 | 4094 | — | 0.0.15.255 |

> ⚠️ Subnet math: # of hosts = 2ⁿ - 2, where n = # of host bits
> 

---

## 🔢 CIDR to Subnet Mask Breakdown

| CIDR | Subnet Mask | Binary Mask |
| --- | --- | --- |
| /8 | 255.0.0.0 | 11111111.00000000.00000000.00000000 |
| /16 | 255.255.0.0 | 11111111.11111111.00000000.00000000 |
| /24 | 255.255.255.0 | 11111111.11111111.11111111.00000000 |
| /30 | 255.255.255.252 | 11111111.11111111.11111111.11111100 |

---

## 🎯 Tips for the CCNA Exam

- **/30** = 2 hosts → great for point-to-point links.
- **/29–/27** = small networks or WAN segments.
- **/24** = common LAN size.
- **/16** = large internal enterprise networks.
- Wildcard mask = **inverted subnet mask** (used in ACLs and OSPF).

---

## 📚 Helpful Quick Math

| Subnet Bits | Subnets (per Class C) | Host Bits | Hosts |
| --- | --- | --- | --- |
| 2 | 4 | 6 | 62 |
| 3 | 8 | 5 | 30 |
| 4 | 16 | 4 | 14 |
| 5 | 32 | 3 | 6 |