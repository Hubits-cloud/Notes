# Wireless cheatsheet

Created by: Tobias
Created time: May 1, 2025 5:27 PM
Last updated time: May 1, 2025 5:31 PM

# 📶 CCNA Wireless Devices Cheat Sheet

---

## 🔌 Types of Wireless Network Devices

| Device | Description | Use Case |
| --- | --- | --- |
| **Wireless NIC (WNIC)** | Enables end devices (laptops, smartphones) to connect to a WLAN. | All wireless-capable devices must have one. |
| **Wireless Router** | Combines router, switch, firewall, DHCP, and AP in one. | Home and SOHO networks. |
| **Access Point (AP)** | Provides wireless access to a wired LAN. | Enterprise networks and mesh setups. |
| **Range Extender / Repeater** | Extends wireless coverage by repeating the signal. | Homes with dead zones (not scalable). |
| **Wireless LAN Controller (WLC)** | Centralized AP management. Uses CAPWAP to control lightweight APs. | Enterprise environments with many APs. |
| **Antenna** | Controls signal range and direction. | Omnidirectional (360°), directional (focused). |
| **RADIUS Server** | Authenticates users in enterprise wireless networks. | Used with WPA2/WPA3 Enterprise. |

---

## 📡 Access Point Types

| AP Type | Description |
| --- | --- |
| **Autonomous AP** | Standalone device, configured individually. |
| **Lightweight AP (LWAP)** | Centrally managed by a WLC using CAPWAP protocol. |
| **FlexConnect AP** | Lightweight AP that can locally switch traffic during WAN outages. |

---

## 🛡️ Security Modes

| Mode | Description |
| --- | --- |
| **WEP** | Outdated and insecure. Avoid. |
| **WPA** | Uses TKIP. More secure than WEP, but outdated. |
| **WPA2** | Uses AES/CCMP. Strong and widely used. |
| **WPA3** | Newest. Uses SAE. Offers enhanced encryption and better IoT onboarding. |

---

## 📶 Wireless Standards Overview

| Standard | Band | Max Speed | Features |
| --- | --- | --- | --- |
| 802.11b | 2.4 GHz | 11 Mbps | DSSS |
| 802.11a | 5 GHz | 54 Mbps | OFDM |
| 802.11g | 2.4 GHz | 54 Mbps | Backward compatible with b |
| 802.11n | 2.4 & 5 GHz | 600 Mbps | MIMO |
| 802.11ac | 5 GHz | >1 Gbps | MU-MIMO |
| 802.11ax (Wi-Fi 6) | 2.4 & 5 GHz | ~10 Gbps | OFDMA, MU-MIMO, IoT ready |

---

## 🌐 Wireless Topologies

| Mode | Description |
| --- | --- |
| **Ad Hoc (IBSS)** | Devices connect directly without AP. |
| **Infrastructure** | Clients connect through APs to a wired LAN. |
| **Tethering / Hotspot** | Smartphone shares internet via Wi-Fi. |
| **Mesh** | Multiple APs communicate to expand coverage. |

---

## 📶 Channels and Bands

| Band | Channels | Notes |
| --- | --- | --- |
| **2.4 GHz** | 1–11 (US) | Use channels 1, 6, 11 to avoid overlap. |
| **5 GHz** | 24+ non-overlapping | Less interference, shorter range. |
| **6 GHz** | Wi-Fi 6E only | Very new, ultra-fast, low interference. |

---

## ⚙️ Management Tools & Protocols

| Tool / Protocol | Purpose |
| --- | --- |
| **CAPWAP** | Connects LWAPs to WLCs. Uses UDP 5246 (control), 5247 (data). |
| **SNMP** | Used by WLCs for monitoring and alerting. |
| **CDP** | Cisco Discovery Protocol – detects neighbors (Layer 2). |
| **DHCP** | Assigns IP addresses to wireless clients. |

---

✅ **Quick Tips for CCNA Exam**:

- Always secure with **WPA2 or WPA3**.
- Know **CAPWAP**, **SSID**, **MAC filtering**, and **AP types**.
- Use **channels 1, 6, and 11** for 2.4 GHz.
- Know the difference between **Autonomous** and **Lightweight** APs.
- **RADIUS + 802.1X = Enterprise Authentication**.