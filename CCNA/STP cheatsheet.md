# STP cheatsheet

Created by: Tobias
Created time: May 1, 2025 8:48 PM
Last updated time: May 1, 2025 8:50 PM

# 🌲 Spanning Tree Protocol (STP) Cheat Sheet – CCNA

---

## 🧠 What is STP?

**Spanning Tree Protocol (STP)** prevents **Layer 2 loops** in Ethernet networks by **blocking redundant paths** and ensuring a **loop-free topology**.

- Defined by **IEEE 802.1D**
- Operates at **Layer 2**
- Uses **Bridge Protocol Data Units (BPDUs)** for communication

---

## 🧭 STP Roles and Concepts

| Component | Description |
| --- | --- |
| **Root Bridge** | Central switch in the STP topology. All paths are calculated from here. |
| **Root Port (RP)** | Best path from a non-root switch to the root bridge. One per switch. |
| **Designated Port (DP)** | Best forwarding port on a network segment. One per segment. |
| **Non-Designated Port** | Placed into **blocking** state to prevent loops. |
| **Blocked Port** | Does not forward traffic. Used to eliminate loops. |
| **BPDU** | Control message used by STP to share topology information. |

---

## 👑 Root Bridge Election

- Switch with the **lowest Bridge ID (BID)** becomes the root.
- **Bridge ID = Priority + MAC address**
- Default priority = **32,768**
- You can influence the election:

```bash
bash
CopyEdit
Switch(config)# spanning-tree vlan 1 priority 4096

```

> 🏆 Lower priority wins. If tied, lowest MAC address wins.
> 

---

## 📶 STP Port States

| State | Function |
| --- | --- |
| **Blocking** | Receives BPDUs only. No data forwarding. |
| **Listening** | Processes BPDUs, prepares for transition. |
| **Learning** | Learns MAC addresses, doesn’t forward frames. |
| **Forwarding** | Forwards data and learns MACs. |
| **Disabled** | Administratively shut down or not participating. |

⏱ Default transition time: **30–50 seconds** to reach forwarding state.

---

## ⚙️ STP Timers (Default)

| Timer | Value | Purpose |
| --- | --- | --- |
| Hello Time | 2 sec | BPDU send interval by root bridge |
| Forward Delay | 15 sec | Time spent in listening & learning |
| Max Age | 20 sec | Time to keep BPDU info before discarding |

---

## 🚀 STP Variants

| Protocol | Standard | Features |
| --- | --- | --- |
| **STP** | 802.1D | Original; slower convergence (~50s) |
| **RSTP** | 802.1w | Rapid STP; faster convergence (~6s) |
| **PVST+** | Cisco | Per-VLAN STP; uses one instance per VLAN |
| **Rapid PVST+** | Cisco | Combines RSTP with per-VLAN logic |
| **MST** | 802.1s | Multiple VLANs mapped to same STP instance |

> 🏎️ For CCNA, focus on RSTP and PVST+
> 

---

## 🛡️ STP Security Features

| Feature | Purpose | Command |
| --- | --- | --- |
| **BPDU Guard** | Disables port if BPDU is received on a PortFast port. | `spanning-tree bpduguard enable` |
| **PortFast** | Immediately puts access ports into forwarding state. | `spanning-tree portfast` |
| **Root Guard** | Prevents other switches from becoming root. | `spanning-tree guard root` |
| **Loop Guard** | Prevents unidirectional link failure from creating loops. | `spanning-tree loopguard default` |

---

## ✅ STP Key Commands

| Task | Command |
| --- | --- |
| Show STP status | `show spanning-tree` |
| Show STP for specific VLAN | `show spanning-tree vlan 1` |
| Configure priority | `spanning-tree vlan 1 priority 4096` |
| Enable PortFast | `spanning-tree portfast` |
| Enable BPDU Guard | `spanning-tree bpduguard enable` |
| Enable Root Guard | `spanning-tree guard root` |
| Set bridge priority for RSTP | Same as for STP; RSTP auto-enabled on most IOS |

# 🧩 IEEE 802.1 Standards Cheat Sheet – CCNA

---

## 📘 What is IEEE 802.1?

The **IEEE 802.1** working group defines standards related to **network management**, **LAN switching**, **security**, and **bridging** — mostly used with **Layer 2 Ethernet** technologies.

---

## 📚 Common IEEE 802.1 Standards (CCNA-Relevant)

| IEEE Standard | Name / Purpose | Description |
| --- | --- | --- |
| **802.1D** | Spanning Tree Protocol (STP) | Prevents Layer 2 loops by creating a loop-free topology. |
| **802.1w** | Rapid Spanning Tree Protocol (RSTP) | Faster convergence than 802.1D. Backward-compatible. |
| **802.1s** | Multiple Spanning Tree Protocol (MSTP) | Groups VLANs into one STP instance. Scales better than PVST+. |
| **802.1Q** | VLAN Tagging | Adds VLAN info to Ethernet frames (VLAN trunking). |
| **802.1p** | Layer 2 QoS (Priority Tagging) | Prioritizes traffic using 3 bits in 802.1Q tag (CoS bits). |
| **802.1X** | Port-Based Network Access Control | Authenticates users/devices using RADIUS (used with WPA2 Enterprise). |
| **802.1AB** | LLDP (Link Layer Discovery Protocol) | Vendor-neutral alternative to CDP. Used for device discovery. |
| **802.1AE** | MACsec (MAC Security) | Provides data encryption and integrity at Layer 2. |
| **802.1AS** | Timing and Synchronization | Used in time-sensitive networks (e.g., AVB). |

---

## 🧠 Quick Notes by Function

### 🔐 **Security**

- **802.1X** – Authenticates devices before network access (used in Wi-Fi enterprise networks).
- **802.1AE** – Encrypts Ethernet frames at Layer 2 (MACsec).

### 🌐 **VLANs & Trunking**

- **802.1Q** – Adds a 4-byte tag to frames for VLAN identification.
- **802.1p** – QoS within the 802.1Q tag.

### 🔄 **Spanning Tree**

- **802.1D** – Standard STP.
- **802.1w** – Rapid STP.
- **802.1s** – MSTP for VLAN groups.

### 🧭 **Network Discovery & Management**

- **802.1AB** – LLDP for Layer 2 device info exchange.

---

## 📌 Bonus: VLAN ID Ranges (from 802.1Q)

| VLAN Type | Range | Purpose |
| --- | --- | --- |
| **Normal** | 1–1005 | Used in most enterprise networks. |
| **Extended** | 1006–4094 | Requires VTP version 3 or transparent mode. |
| **Reserved** | 0 and 4095 | Reserved; not usable by users. |