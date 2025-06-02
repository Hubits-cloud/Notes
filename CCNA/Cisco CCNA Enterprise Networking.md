
Created by: Tobias Madsen Belling

Created time: May 19, 2025 12:44 PM

Last updated time: May 20, 2025 12:37 PM

# 📘 CCNA Enterprise Networking Module 1: OSPF

## 1.1 Introduction to OSPF

- **OSPF (Open Shortest Path First)**: Link-state routing protocol, alternative to RIP.
- **OSPFv2**: IPv4 networks | **OSPFv3**: IPv6 networks.
- RIP relies on hop count; OSPF uses cost (bandwidth-based), better for scalability and convergence.
- **Link**: Interface or network segment.
- **Link-State**: Info about link (prefix, length, cost).
- **Focus**: Single-area OSPFv2.

## 1.2 Components of OSPF

### OSPF Packets

1. **Hello** – Discover/maintain neighbors.
2. **DBD** – Summary of LSDB.
3. **LSR** – Request detailed LSA.
4. **LSU** – Send full LSA.
5. **LSAck** – Acknowledge LSU.

### Data Structures

- **Adjacency DB** – Neighbor table.
- **LSDB** – Topology table.
- **Forwarding DB** – Routing table.

### Algorithm

- **Dijkstra SPF Algorithm** – Builds SPF tree, selects shortest path.

## 1.3 Link-State Operation Steps

1. **Establish Neighbor Adjacencies** – Send Hello packets.
2. **Exchange LSAs** – Share link state and cost.
3. **Build LSDB** – Topology table from LSAs.
4. **Run SPF** – Create SPF tree.
5. **Select Best Route** – Best path to IP routing table.

## 1.4 OSPF Areas

- **Single-Area OSPF**: All routers in one area (usually Area 0).
- **Multi-Area OSPF**: Divides routing domain into multiple areas.
  - **ABR**: Connects backbone (Area 0) to other areas.
  - Benefits: Smaller routing tables, reduced SPF frequency and LSA overhead.

## 1.5 OSPFv3 (for IPv6)

- Equivalent to OSPFv2 for IPv6.
- Supports both IPv4 & IPv6 with Address Families (not in scope).
- Operates using same logic, separate processes for v2/v3.

## 1.6 OSPF Neighbor States

1. **Down** – No Hello received.
2. **Init** – Received Hello.
3. **Two-Way** – Bidirectional Hello seen.
4. **ExStart** – Decide who sends DBD first (higher Router ID).
5. **Exchange** – Send DBDs.
6. **Loading** – Request missing LSAs (LSRs/LSUs).
7. **Full** – LSDB synchronized.

## 1.7 DR/BDR

- **Why?** Reduce adjacencies & LSA flooding in multiaccess networks.
- **Adjacency Formula**: `n(n-1)/2`
- **DR/BDR Roles**:
  - **DR**: Central LSA hub.
  - **BDR**: Backup to DR.
  - **DROTHER**: Other routers.

---

# 2.0 Overview

- **Focus**: Single-Area OSPFv2 implementation in point-to-point & multiaccess networks.
- **Benefits**: Fast convergence, route control, manual DR override.

## 2.1 OSPF Basics

### 2.1.1 Router ID

- 32-bit ID used to identify OSPF routers.
- Chosen by:
  1. Manually with `router-id`
  2. Highest loopback IP
  3. Highest active physical IP

### 2.1.2 Configure Router ID

```bash
interface loopback0
 ip address 1.1.1.1 255.255.255.255
router ospf 10
 router-id 1.1.1.1
```

- Use `clear ip ospf process` to apply new ID.

## 2.2 OSPF Interface Configuration

### 2.2.1 Wildcard Masks

- Inverse of subnet mask (e.g., `/24 = 0.0.0.255`)
- Match = 0, Ignore = 1

### 2.2.2 `network` vs `ip ospf`

- `network 10.1.1.0 0.0.0.255 area 0` (global mode)
- `ip ospf 10 area 0` (interface mode)

### 2.2.3 Passive Interfaces

- Stops OSPF Hello msgs on interfaces not facing other routers.
```bash
router ospf 10
 passive-interface Loopback0
```

## 2.3 OSPF Network Types

### 2.3.1 Point-to-Point

- Avoid DR/BDR election with:
```bash
interface g0/0
 ip ospf network point-to-point
```

### 2.3.2 Multiaccess Networks

- Ethernet = broadcast multiaccess.
- DR/BDR elected to minimize LSA flooding.
- Election rules:
  1. Highest priority (0–255)
  2. Highest router ID (if tie)
- Configure priority:
```bash
interface g0/0
 ip ospf priority 255
```

## 2.4 Cost and Metrics

### 2.4.1 Cost Formula

```text
Cost = Reference Bandwidth / Interface Bandwidth
Default = 100,000,000 bps
```

### 2.4.2 Modify Cost

- Change reference bandwidth:
```bash
router ospf 10
 auto-cost reference-bandwidth 10000
```

- Manually set cost:
```bash
interface g0/0
 ip ospf cost 30
```

## 2.5 Default Route Propagation

- ASBR (e.g., R2) needs:
```bash
ip route 0.0.0.0 0.0.0.0 loopback1
router ospf 10
 default-information originate
```

## 2.6 OSPF Verification

Commands:
```bash
show ip interface brief
show ip route
show ip ospf neighbor
show ip protocols
show ip ospf
show ip ospf interface
```

## 2.7 Summary

- Router ID selection order.
- `network` vs `ip ospf` config methods.
- OSPF network types and their impact on DR/BDR election.
- Metric tuning with cost and reference bandwidth.
- Use of passive interfaces.
- Default route propagation with `default-information originate`.

---

# **Module 3: Network Security Concepts - Summary**

### 🔐 **3.0 Introduction**
- Focus: Vulnerabilities, threats, exploits & mitigations.
- Ethical hacking content; must follow legal/ethical guidelines.

---

## ⚠️ **3.1 Threat Landscape**
- Cybercriminals: Advanced tools, stealthy methods.
- Attack vectors: Internal (insider threats), External (DoS, malware).
- Data Loss: Brand damage, legal issues → use DLP controls.

---

## 👨‍💻 **3.2 Threat Actors**
- **White Hat**: Ethical hackers.
- **Black Hat**: Malicious hackers.
- **Gray Hat**: Mix of both.
- **Hacktivists**: Ideological motives (e.g., Anonymous).
- **State-sponsored**: Use zero-day, advanced threats.

---

## 🧰 **3.3 Attack Tools**
- Tools evolving: Less skill required.
- Both white/black hats use similar tools (e.g., Nmap, Metasploit).

---

## 💣 **3.4 Malware**
- **Virus**: Needs user action.
- **Worm**: Self-replicates.
- **Trojan**: Disguised malware.
- Other types: Rootkit, Ransomware, Spyware, Adware.

---

## 🕵️‍♂️ **3.5 Network Attacks**
- **Reconnaissance**: Scanning, footprinting.
- **Access attacks**: Password cracking, spoofing, MITM.
- **Social engineering**: Phishing, baiting, tailgating, etc.
- **DoS/DDoS**: Overload services → use botnets (zombies).

---

## 🌐 **3.6 IP-based Attacks**
- **IP Spoofing**: Fake source IP.
- **ICMP Abuse**: Network scanning, firewall probing.
- **Reflection/Amplification**: Smurf, DNS/NTP amplification.
- **MAC Spoofing**: CAM table attacks.
- **App Spoofing**: Rogue DHCP servers for MITM.

---

## 📦 **3.7 Transport Layer Attacks**
- **TCP SYN Flood**: Half-open connections.
- **TCP Reset**: Abrupt termination.
- **Session Hijacking**: Sequence number prediction.
- **UDP Flood**: ICMP replies cause bandwidth exhaustion.

---

## 📛 **3.8 LAN Protocol Vulnerabilities**
- **ARP Poisoning**: Redirect traffic (MITM).
- **DNS Attacks**: Tunneling, stealth, domain shadowing.
- **DHCP Spoofing**: Rogue IP config causes MITM or DoS.

---

## 🔐 **3.9 Security Fundamentals**
- **CIA Triad**: Confidentiality, Integrity, Availability.
- **Defense in Depth**: Firewalls, IPS, VPNs, AAA, ESA/WSA.
- **Firewalls**: Enforce access control, inspect packets.
- **IPS**: Detect & block threats based on signatures.

---

## 🧾 **3.10 Cryptography & Secure Comms**
- **Hashing (Integrity)**:
  - MD5 (128-bit) → deprecated.
  - SHA-1 → deprecated.
  - SHA-2 / SHA-3 → recommended.
- **HMAC (Origin Authentication)**: Adds key to hashing.
- **Symmetric Encryption**: AES, 3DES → Fast, shared key.
- **Asymmetric Encryption**: RSA, PKI → Slower, public/private key.
- **Diffie-Hellman**: Shared key exchange, no prior contact.
---

# **Module 4: ACL Concepts - Summary**

## 🚧 **4.0 Introduction**
- ACLs filter traffic like a security guard at a gate.
- Uses Access Control Entries (ACEs) to permit or deny traffic.

---

## 📦 **4.1 ACL Basics**
- ACL = IOS commands that inspect packet headers to allow or deny traffic.
- Default: No ACLs. Must apply to interface to be active.
- Packet filtering compares packet data to ACEs in order.
- **Types**:
  - Standard: Filters by source IP only (Layer 3).
  - Extended: Filters by source/destination IP, protocol, ports (Layer 3/4).
- ACLs apply to **inbound** (before routing) or **outbound** (after routing) traffic.

---

## 🎯 **4.2 Wildcard Masks**
- Used to match bits in IP addresses.
  - `0 = match`, `1 = ignore`
- Example uses:
  - Host: `0.0.0.0`
  - Subnet: `0.0.0.255`
  - Range: `0.0.15.255`
- Shortcut: `Wildcard = 255.255.255.255 - Subnet Mask`
- **Keywords**:
  - `host` = exact match (0.0.0.0)
  - `any` = match all (255.255.255.255)

---

## 🛠️ **4.3 ACL Application & Best Practices**
- Max 4 ACLs per interface (IPv4 in/out + IPv6 in/out).
- Plan and test ACLs before applying.
- Best practices:
  - Follow policy.
  - Use remarks for documentation.
  - Test in lab before deployment.

---

## 🔢 **4.4 ACL Types & Placement**
- **Standard ACLs**:
  - Use numbers 1–99, 1300–1999.
  - Match source IP only.
  - Place close to **destination**.
- **Extended ACLs**:
  - Use numbers 100–199, 2000–2699.
  - Match source/dest IP, protocol, port.
  - Place close to **source**.
- **Named ACLs**:
  - More descriptive and flexible.
  - Use `ip access-list [standard|extended] NAME`.

---

## 🧪 **4.5 Module Summary**
- ACLs control traffic using ACEs.
- Two types: Standard (source IP), Extended (detailed filtering).
- Use wildcard masks for flexibility.
- Apply ACLs based on traffic direction and efficiency.

---

# **Module 5: ACLs for IPv4 Configuration**


## 📘 5.1 ACL Creation & Syntax

- **Plan ACLs** before configuration (especially with multiple ACEs).
- Use **text editor**, add **remarks**, test in lab.
- **Numbered Standard ACL Syntax**:
  ```bash
  access-list <number> {permit|deny} <source> [wildcard]
  no access-list <number>  # Remove ACL
  ```
- **Named Standard ACL Syntax**:
  ```bash
  ip access-list standard <name>
  {permit|deny} <source> [wildcard]
  ```
- **Apply ACL**:
  ```bash
  interface <int>
  ip access-group <name|number> in|out
  ```

---

## 🔧 5.2 Modifying ACLs

- **Method 1: Text Editor**
  - Copy from `show run`, edit, `no access-list`, re-paste.

- **Method 2: Sequence Numbers**
  - Delete specific ACE: `no <seq>`
  - Add new ACE with specific sequence:
    ```bash
    ip access-list standard <name>
    15 deny 192.168.10.5
    ```

- **Check Matches**:
  ```bash
  show access-lists
  clear access-list counters <name>
  ```

---

## 🔐 5.3 Securing VTY Lines

- **Steps**:
  1. Create ACL for allowed hosts.
  2. Apply to VTY lines using `access-class`.

- **Example**:
  ```bash
  ip access-list standard ADMIN-HOST
    permit 192.168.10.10
    deny any
  line vty 0 4
    login local
    transport input ssh
    access-class ADMIN-HOST in
  ```

---

## 📦 5.4 Extended ACLs

- **Use When**: Need control by protocol, port, source/destination.
- **Syntax**:
  ```bash
  access-list <number> permit tcp <src> <src_wildcard> <dst> <dst_wildcard> eq <port>
  ```
- **Named Extended ACL**:
  ```bash
  ip access-list extended <name>
    permit tcp any any eq www
    permit tcp any any eq 443
  ```

- **Apply**:
  ```bash
  interface <int>
  ip access-group <ACL_name|number> in|out
  ```

- **TCP `established` Keyword**:
  - Allows return traffic only:
    ```bash
    access-list 120 permit tcp any <private_net> established
    ```

---

## 📄 5.5 Verification

- **Commands**:
  ```bash
  show ip interface <int> | include access list
  show access-lists
  show running-config | section access-list
  ```

- **Use remarks & sequence numbers for better organization**.

---

## ✅ Summary
- Create and apply Standard & Extended ACLs.
- Use named ACLs for clarity.
- Modify ACLs via text or sequence numbers.
- Secure VTY access with standard ACLs.
- Verify with `show` commands.

---

# **Module 6: NAT for IPv4**

---

## 🌐 6.1 NAT Overview
- NAT (Network Address Translation) translates private IPs to public IPs.
- Solves IPv4 address exhaustion & hides internal networks.
- **Types of NAT**:
  - **Static NAT**: One-to-one mapping.
  - **Dynamic NAT**: Uses a pool of public IPs.
  - **PAT (NAT Overload)**: Many-to-one using ports.

---

## 🧰 6.2 NAT Operation
- **Inside Local**: Private IP inside network.
- **Inside Global**: Public IP used on the internet.
- **Outside Local/Global**: External host IP (usually the same).
- Router maintains NAT table for translations.

---

## ⚙️ 6.3 Configure Static NAT
```bash
ip nat inside source static <inside-local> <inside-global>
interface G0/0
 ip nat inside
interface S0/1/0
 ip nat outside
```
- Static NAT is used for servers needing constant access.

---

## 🔄 6.4 Configure Dynamic NAT
```bash
ip nat pool POOL_NAME <start-ip> <end-ip> netmask <mask>
access-list 1 permit <source-ip> <wildcard>
ip nat inside source list 1 pool POOL_NAME
```
- Requires access list + NAT pool.

---

## 🔢 6.5 Configure PAT
```bash
ip nat inside source list 1 interface <outside-int> overload
```
- Uses interface IP and port numbers for translations.
- Most commonly used NAT type in networks.

---

## 🧪 6.6 Verify NAT
```bash
show ip nat translations
show ip nat statistics
```
- Use extended pings for testing:
```bash
ping
<target-ip>
source <inside-local-ip>
```

---

## 🛠️ 6.7 Troubleshoot NAT
- Check interface NAT roles: `ip nat inside` / `ip nat outside`
- Verify ACL and NAT pool match intended traffic.
- Check NAT table and stats for expected translations.

---

## ✅ Summary
- NAT provides public access to private IPs.
- Static = fixed 1:1, Dynamic = pool, PAT = many:1.
- Requires correct ACLs, NAT rules, and interface roles.
- Always verify using `show` commands and ping tests.

---

# **Module 7: WAN Concepts**

---

## 🌐 7.1 WAN Overview

- **LAN vs WAN**: LANs cover small areas; WANs span large geographic regions.
- **Private WANs**: Dedicated links with consistent bandwidth & security.
- **Public WANs**: Shared Internet connections via ISPs; variable bandwidth.

---

## 🕸️ 7.1.3 WAN Topologies

- **Point-to-Point**: Direct link; transparent but expensive.
- **Hub-and-Spoke**: Central hub links to spokes; single point of failure.
- **Dual-Homed**: Two hubs; adds redundancy, complexity, and cost.
- **Fully Meshed**: All nodes connected; high fault tolerance.
- **Partially Meshed**: Some nodes fully connected; cost-effective hybrid.

---

## 🧩 7.1.4 WAN Carrier Connections

- **Single-Carrier**: One provider, low cost, low redundancy.
- **Dual-Carrier**: Two providers, increased availability & redundancy.

---

## 🏢 7.1.5 Network Evolution

- **Small Network**: Basic LAN, DSL.
- **Campus Network**: CAN with multiple switches, servers, firewall.
- **Branch Network**: MAN or WAN for city-wide or remote offices.
- **Distributed Network**: Global offices, VPNs, telework, collaboration tools.

---

## 📐 7.2 WAN Operations

- **Standards**: TIA/EIA, ISO, IEEE.
- **OSI Layers**:
  - **Layer 1**: SDH, SONET, DWDM.
  - **Layer 2**: Ethernet WAN, MPLS, PPP, HDLC, Frame Relay, ATM.

---

## 🔌 7.2 WAN Devices & Connections

- **DTE/DCE**: End-to-end communication from client to provider.
- **Serial** vs **Parallel**: Serial = long-distance; Parallel = short-distance (e.g., data centers).
- **Circuit-Switched**: PSTN, ISDN – dedicated path, inefficient for data.
- **Packet-Switched**: MPLS, Metro Ethernet – efficient, shared path.

---

## 📡 7.3 Traditional WAN Connectivity

- **Leased Lines**: T1/E1, T3/E3 – reliable but expensive.
- **Circuit-Switched**: PSTN (dialup), ISDN – legacy tech.
- **Packet-Switched**: Frame Relay, ATM – legacy, replaced by MPLS/Ethernet.

---

## 🚀 7.4 Modern WAN Connectivity

- **Ethernet WAN**: Metro E, VPLS – fast, scalable, integrates with LANs.
- **MPLS**: High-performance WAN routing, supports QoS, redundancy.

---

## 🌍 7.5 Internet-Based Connectivity

- **Wired**:
  - **DSL**: Asymmetric/Symmetric, connects via DSLAM, supports PPPoE.
  - **Cable**: Shared bandwidth, uses HFC and DOCSIS.
  - **Fiber (FTTx)**: Highest bandwidth, includes FTTH, FTTB, FTTN.

- **Wireless**:
  - **Wi-Fi**: Local or municipal.
  - **Cellular (3G/4G/5G, LTE)**: Widely used for mobile WAN.
  - **Satellite**: Rural/remote; high latency.
  - **WiMAX**: Long-range wireless; mostly replaced by LTE.

---

## 🔐 7.5.8 VPNs

- **Types**:
  - **Site-to-Site**: Routers manage VPN.
  - **Remote Access**: User-initiated, client software.
- **Benefits**: Cost-effective, secure, scalable, broadband-compatible.

---

## 🌐 7.5.9 ISP Connectivity Models

- **Single-homed**: Simple, low-cost.
- **Dual-homed**: Redundancy to one ISP.
- **Multihomed**: Two ISPs, better uptime.
- **Dual-multihomed**: Max redundancy, highest cost.

---

## 📊 7.5.10 Broadband Comparison

| Tech        | Pros                              | Cons                             |
|-------------|-----------------------------------|----------------------------------|
| Cable       | Widely available                  | Shared bandwidth                 |
| DSL         | Not shared                        | Distance-sensitive               |
| Fiber       | High-speed, best option           | Expensive, limited availability  |
| Cellular    | Mobile, flexible                  | Coverage & bandwidth limits      |
| Wi-Fi       | Inexpensive, local                | Limited range, interference      |
| Satellite   | Available remotely                | High latency, weather-sensitive  |

---

## ✅ Summary

- WANs enable large-scale connectivity using a mix of technologies.
- Legacy WANs: Leased lines, Frame Relay, ATM, PSTN, ISDN.
- Modern WANs: Ethernet WAN, MPLS, FTTx, VPNs.
- Internet-based WANs: DSL, Cable, Cellular, Satellite.
- Choose WAN based on performance, redundancy, cost, and scalability.

---

# **Module 8: VPN and IPsec Concepts**

---

## 🔐 8.1 VPN Fundamentals

- **VPN**: Encrypts traffic over public networks for secure private communication.
- **Types**:
  - **Site-to-Site**: Fixed tunnel between VPN gateways.
  - **Remote-Access**: On-demand client connection via IPsec or SSL.
- **Tech**: Cisco ASA, AnyConnect, GRE (non-encrypted tunneling).
- **Benefits**: Cost-saving, secure, scalable, remote access.

---

## 🌍 8.1.4 VPN Types

- **Enterprise VPNs**: Managed internally; uses IPsec/SSL.
- **Service Provider VPNs**: Uses MPLS; Layer 2 (VPLS) or Layer 3.

---

## 🧳 8.2 VPN Implementations

- **Remote-Access**:
  - *Clientless*: Browser-based SSL VPN.
  - *Client-based*: AnyConnect; uses IPsec or SSL.

- **SSL VPNs**:
  - Uses TLS & certificates.
  - Easier to deploy than IPsec; IPsec offers stronger security.

- **Site-to-Site IPsec**:
  - VPN gateways encrypt & decrypt traffic between networks.
  - Secure, scalable via IPsec.

- **GRE over IPsec**:
  - GRE encapsulates multicast/routing protocols.
  - Then encrypted via IPsec for secure tunneling.

---

## 🧪 8.2.5 DMVPN

- **DMVPN**: Cisco solution for scalable, dynamic mesh VPNs.
- **Hub-and-Spoke** model using mGRE + IPsec.
- Spokes can dynamically create tunnels to each other.

---

## 🎛️ 8.2.6 IPsec VTI

- Virtual interfaces simplify setup.
- Support both **unicast** and **multicast** encrypted traffic.
- GRE not required for routing support.

---

## 🚚 8.2.7 MPLS VPNs

- **Layer 3 MPLS**: SP handles routing.
- **Layer 2 MPLS (VPLS)**: Customer handles routing.
- Secure customer traffic segregation using MPLS labels.

---

## 🔒 8.3 IPsec Technologies

- **Confidentiality**: AES, 3DES, DES (avoid DES).
- **Integrity**: HMAC with SHA (use SHA-256+), MD5 (legacy).
- **Authentication**:
  - **PSK**: Manual keys (easy, not scalable).
  - **RSA**: Certificate-based; secure, scalable.

- **Diffie-Hellman (DH)**: Secure key exchange.
  - Use DH groups 14+ for AES.
  - ECC groups (19–24) recommended.

---

## 🧱 8.3.3 Protocols

- **AH**: Auth & integrity, no encryption.
- **ESP**: Auth + encryption (preferred).

---

## ✅ Summary

- VPNs encrypt traffic over untrusted networks.
- Site-to-site & remote-access models use IPsec/SSL.
- DMVPN, GRE/IPsec, and IPsec VTIs enhance scalability.
- IPsec provides confidentiality, integrity, auth, and key exchange.
- Use AES/SHA-256+, RSA, DH group 14+ for secure VPNs.

---

# **Module 9: Quality of Service (QoS) Concepts**

---

## 🚦 9.1 Network Congestion & QoS Basics

- **QoS**: Ensures high-priority traffic (voice/video) is delivered reliably.
- **Congestion**: Occurs when traffic exceeds bandwidth; leads to delay, jitter, and packet loss.
- **Delay Types**: Fixed (transmission time) & variable (queuing, processing).
- **Jitter**: Variation in delay between packets; affects real-time traffic.
- **Packet Loss**: Caused by buffer overflow during congestion.

---

## 🎯 9.2 Traffic Characteristics

- **Voice**:
  - Predictable, delay/jitter-sensitive.
  - Needs ≤150ms latency, ≤30ms jitter, ≤1% loss.
  - Uses RTP (ports 16384–32767), needs ≥30 Kbps.

- **Video**:
  - Bursty, high bandwidth, delay/loss-sensitive.
  - Needs ≤400ms latency, ≤50ms jitter, ≤1% loss.
  - Uses RSTP (port 554), needs ≥384 Kbps.

- **Data**:
  - Varies in behavior.
  - Less sensitive to delay/loss (uses TCP).
  - Admins consider QoE: interactivity & criticality.

---

## 📚 9.3 Queuing Mechanisms

- **FIFO**: First-in, first-out; no priority.
- **WFQ**: Fair queuing by flow; auto traffic classification.
- **CBWFQ**: User-defined classes with guaranteed bandwidth.
- **LLQ**: Adds priority queue to CBWFQ; ideal for voice.

---

## 🧩 9.4 QoS Models

- **Best Effort**:
  - No guarantees, all traffic equal.
  - Simple but no prioritization.

- **Integrated Services (IntServ)**:
  - Guarantees per-flow QoS using RSVP.
  - High overhead; not scalable.

- **Differentiated Services (DiffServ)**:
  - Class-based traffic treatment.
  - Scalable and widely used.

---

## 🛠️ 9.5 QoS Implementation Tools

- **Classification & Marking**:
  - Use ACLs, NBAR, class maps.
  - Mark packets at Layer 2 (CoS - 802.1p) or Layer 3 (DSCP).
  - DSCP categories: BE (0), EF (46), AFxy (class/drop precedence).

- **Trust Boundaries**:
  - Mark traffic close to source (e.g., IP phones).
  - Define what devices to trust for marking.

- **Congestion Avoidance**:
  - **WRED**: Drops lower-priority traffic early to avoid buffer overflow.

- **Shaping vs Policing**:
  - **Shaping**: Buffers excess outbound traffic.
  - **Policing**: Drops or re-marks inbound traffic over limit.

---

## ✅ Summary

- QoS ensures performance for real-time traffic via classification, prioritization, and shaping.
- Voice/video require tight delay/jitter/loss control.
- Queuing & marking policies dictate treatment under congestion.
- DiffServ is the most scalable QoS model in use today.

---

# **Module 10: Network Management**

---

## 🧠 10.1 Device Management

- **Cisco IOS Features**:
  - Remote access via SSH/Telnet.
  - Local access via console/aux port.
  - CLI-based configuration, monitoring, and troubleshooting.

- **In-band Management**: Through network (e.g., SSH).
- **Out-of-band Management**: Direct console/aux port (requires physical access).

---

## 🛠️ 10.2 Configuration Files

- **Startup Config**: NVRAM; used on boot.
- **Running Config**: RAM; active settings.
- Save with `copy running-config startup-config`.

---

## 🔄 10.3 Backup & Restore

- **Copy to/from server**:
  ```bash
  copy running-config tftp:
  copy tftp: startup-config
  ```
- **Secure Backup**: Use SCP for encryption.

---

## 📈 10.4 Monitoring & Logging

- **Syslog**:
  - Logs events to local buffer or external server.
  - Levels 0 (emergencies) to 7 (debug).
  - Use `logging trap <level>` to filter.

- **NTP**:
  - Sync device clocks.
  - Client config:
    ```bash
    ntp server <ip>
    ```

- **CDP/LLDP**:
  - Layer 2 discovery of neighboring devices.
  - CDP: Cisco-only. LLDP: Vendor-neutral.

---

## 📊 10.5 SNMP (Simple Network Management Protocol)

- **Components**:
  - **Manager**: Queries/receives info.
  - **Agent**: Runs on devices.
  - **MIB**: Info database on agent.

- **Versions**:
  - v1/v2c: Community string-based.
  - v3: Secure (auth + encryption).

- **Traps**: Alerts from agent to manager.

---

## 🌐 10.6 NetFlow

- **Monitors IP traffic** for patterns, usage, and anomalies.
- Used for billing, analysis, and security monitoring.
- Exported to collector via UDP.

---

## 🧹 10.7 Licensing

- **Smart Licensing**: Cloud-based license management.
- Replaces older PAK-based models.
- Required for most modern Cisco IOS features.

---

## ✅ Summary

- Manage devices via CLI, monitor via Syslog, SNMP, NetFlow.
- Back up configs via TFTP/SCP.
- Use NTP for time sync, CDP/LLDP for topology.
- SNMPv3 and Smart Licensing improve security and efficiency.

---

# **Module 11: Network Design**

---

## 🧱 11.1 Scalable Network Design

- **Why Network Design?** Like spaceship design—must adapt to various uses and sizes.
- Good design supports growth, reliability, and modern business needs.

---

## 🧠 11.1.2 The Need to Scale

- Modern networks must be accessible anytime, anywhere, on any device.
- Needs:
  - Critical application support
  - Converged traffic handling
  - Diverse business needs
  - Centralized control
- Scale includes LAN to campus designs.

---

## 🌐 11.1.3 Borderless Switched Networks

- **Cisco Borderless Network**: Secure, scalable access from any device.
- Merges wired/wireless, policy control, and performance into one architecture.

---

## 🏗️ 11.1.4 Hierarchical Design Principles

- **Hierarchical**: Simplifies design and troubleshooting.
- **Modular**: Easy expansion and service integration.
- **Resilient**: Keeps network online.
- **Flexible**: Efficient resource use via load sharing.

---

## 🧩 11.1.5 Layered Functions

- **Access Layer**: Edge connectivity for users/devices.
- **Distribution Layer**: Aggregates access, enforces policies, routing.
- **Core Layer**: High-speed backbone; links all layers.

---

## 📐 11.1.6 Tiered Models

- **Three-Tier**: Access → Distribution → Core.
- **Two-Tier (Collapsed Core)**: Access → Distribution/Core combined; for smaller sites.

---

## 🔄 11.2.1 Design for Scalability

- Use modular/expandable hardware.
- Create hierarchical IP plans.
- Use Layer 3 to limit broadcasts.
- Implement:
  - Redundant links
  - EtherChannel for link aggregation
  - Scalable routing (e.g., OSPF)
  - Wireless access

---

## 🔁 11.2.2 Redundancy

- Prevents service disruption via:
  - Duplicate equipment
  - Redundant paths (STP or Layer 3)

---

## 💥 11.2.3 Reduce Failure Domains

- **Failure Domain**: Area affected by failure.
- Use Layer 3 in distribution to isolate issues.
- **Switch Block**: Distributes users to minimize impact.

---

## 📶 11.2.4-11.2.5 Expandability

- Use EtherChannel to prevent bottlenecks.
- Expand access via wireless with planning for coverage and interference.

---

## 🧠 11.2.6 Tuning Routing

- Use OSPF for fast convergence.
- Synchronizes link-state databases; recalculates best paths dynamically.

---

## 🧰 11.3 Switch Hardware

- **Types**:
  - Campus LAN (2960–6800 series)
  - Cloud-managed (Meraki)
  - Data center (Nexus)
  - SP switches
  - Virtual switches

- **Form Factors**:
  - Fixed: set ports/features
  - Modular: changeable slots
  - Stackable: act as one unit
  - Thickness measured in rack units (U)

---

## 🔌 11.3.3–11.3.6 Key Specs

- **Port Density**: High-density modular saves space/power.
- **Forwarding Rate**: Impacts performance; critical for distribution/core.
- **PoE**: Powers APs/phones via Ethernet.
- **Multilayer Switching**: Combines Layer 2 & Layer 3; efficient packet forwarding.

---

## 💼 11.3.7 Business Considerations

- Evaluate cost, density, power, speed, reliability, and scalability when choosing switches.

---

## 🌍 11.4 Router Design

- Routes IP packets based on destination prefix.
- Provides redundancy, gateway function, security (ACLs), and segment isolation.

---

## 📦 11.4.2 Router Types

- **Branch Routers**: ISR 4000
- **Edge Routers**: ASR 9000
- **Service Provider Routers**: NCS 6000
- **Industrial Routers**: Cisco 1100 Series

---

## 🔧 11.4.3 Router Form Factors

- **Fixed**: Predefined ports.
- **Modular**: Customizable interfaces.
- Examples:
  - Cisco 900, ASR 1000/9000, NCS 5500, 800 Industrial Series.

---

## ✅ Summary

- Hierarchical design enables scalability, flexibility, and reliability.
- Use tiered architecture: access, distribution, core.
- Select switches/routers based on scale, form, speed, and function.
- Build redundancy and modularity into every level.

---

# **Module 12: Network Troubleshooting**

---

## 🛠️ 12.1 Troubleshooting Principles

- **Purpose**: Find and fix network problems quickly to reduce downtime.
- **Structured Approach**:
  - Identify the problem
  - Establish a theory
  - Test the theory
  - Establish a plan
  - Implement the solution
  - Verify
  - Document

---

## 🧪 12.2 Troubleshooting Models

- **OSI Model**: Layer-by-layer troubleshooting (bottom-up/top-down).
- **Cisco 3-Layer Model**:
  - Access
  - Distribution
  - Core

---

## 🔍 12.3 Common Issues by Layer

- **Layer 1 (Physical)**:
  - Cable faults, bad ports, power issues.
  - Use LEDs, `show interfaces`, cable testers.

- **Layer 2 (Data Link)**:
  - Duplex mismatch, VLAN issues, STP problems.
  - Use `show vlan`, `show interfaces`, `show spanning-tree`.

- **Layer 3 (Network)**:
  - IP misconfigurations, ACL errors, routing issues.
  - Use `ping`, `traceroute`, `show ip route`, `show access-lists`.

- **Layer 4–7 (Transport–App)**:
  - Port blocks, NAT issues, firewall/policy errors.
  - Use `telnet <ip> <port>`, inspect service configs.

---

## 🔄 12.4 End-to-End Connectivity Checks

- **Ping**: Reachability and latency.
- **Traceroute**: Path tracking and delay points.
- **Telnet/SSH**: Test remote access and port availability.

---

## 🔧 12.5 Interface Troubleshooting

- Check:
  - Status: `show ip interface brief`
  - Errors: `show interfaces`
  - Duplex/speed mismatch
  - Layer 1/2 indications (cabling, speed, VLAN)

---

## 📶 12.6 VLAN & IP Troubleshooting

- **VLAN**:
  - Verify membership: `show vlan`
  - Port mode: `show interfaces switchport`

- **IP**:
  - Check IP/subnet, DHCP, SVI, routing table.

---

## 🔀 12.7 ACL & NAT Troubleshooting

- **ACLs**:
  - Check direction, order, permit/deny logic.
  - Verify counters: `show access-lists`

- **NAT**:
  - Interfaces: `ip nat inside|outside`
  - Translations: `show ip nat translations`

---

## 📡 12.8 Wireless Troubleshooting

- Common issues:
  - Interference, wrong security key, weak signal.
- Check:
  - SSID, AP status, IP config, RF environment.

---

## ✅ Summary

- Troubleshoot systematically using OSI & Cisco models.
- Use show/debug commands to isolate faults.
- Address physical, VLAN, IP, ACL, NAT, and wireless issues.
- Document fixes for future reference.

---

# **Module 13: Network Virtualization**

---

## ☁️ 13.1 Cloud Computing

- **Cloud Benefits**:
  - On-demand access, reduced costs, scalability.
  - Shifts capex to opex ("pay-as-you-go").

- **Cloud Service Models (NIST)**:
  - **SaaS**: Apps via internet (e.g., Office 365).
  - **PaaS**: Dev tools/environment for programmers.
  - **IaaS**: Virtualized infrastructure for IT managers.

- **Cloud Deployment Models**:
  - **Public**: General access (e.g., Google Drive).
  - **Private**: Org-specific, more secure.
  - **Hybrid**: Combines private/public.
  - **Community**: For specific user groups (e.g., healthcare).

---

## 🧱 13.2 Virtualization Concepts

- **Virtualization**: Separates OS from hardware. Enables dynamic server provisioning.
- **Dedicated Servers**: High cost, low utilization, single point of failure.
- **Server Virtualization**: Consolidates hardware, increases efficiency using **hypervisors**.

- **Hypervisor Types**:
  - **Type 1 (Bare-metal)**: Runs directly on hardware (efficient, scalable).
  - **Type 2 (Hosted)**: Runs atop existing OS (good for testing/dev).

- **Benefits**:
  - Lower cost, energy, and space usage.
  - Easy prototyping and disaster recovery.
  - Faster provisioning and better uptime.
  - Legacy support.

- **Abstraction Layers**: Hardware → Firmware → Hypervisor → OS → Services.

---

## 🌐 13.3 Network Virtualization

- **Challenges**:
  - VLANs, East-West traffic, dynamic workloads.
  - Complex manual configuration in traditional networks.

- **Solutions**:
  - **Virtual Interfaces**, **VRFs**, and dynamic provisioning.
  - **Control Plane**: Routing logic (CPU).
  - **Data Plane**: Packet forwarding (ASIC/NPUs).

- **CEF (Cisco Express Forwarding)**: Uses FIB/Adjacency tables to offload control plane.

---

## 🧠 13.4 Software-Defined Networking (SDN)

- **Concept**: Separates control plane from data plane.
- **Controller**: Centralizes routing logic and flow control.

- **Planes**:
  - **Control**: Makes decisions (routing, STP).
  - **Data**: Forwards traffic.
  - **Management**: SSH, SNMP, HTTP(s) access.

- **Key Tech**:
  - **OpenFlow**: Protocol between controller and switches.
  - **OpenStack**: Orchestrates scalable cloud environments.

---

## 🧰 13.5 SDN Controllers & Cisco ACI

- **Flow Management**:
  - **Flow Table**: Match and act on packets.
  - **Group Table**: Applies actions to multiple flows.
  - **Meter Table**: Rate limits and QoS controls.

- **Cisco ACI**:
  - Hardware-based SDN.
  - Uses **APIC** controller with **Nexus 9000** switches.
  - Employs **Spine-Leaf** topology.

- **ACI Core Components**:
  - **ANP**: Defines apps, services, and policies.
  - **APIC**: Central controller.
  - **Spine-Leaf Fabric**: All devices are 1-hop apart.

---

## 🕹️ 13.6 SDN Types

- **Device-based**: Programmable devices (e.g., Cisco OnePK).
- **Controller-based**: Central control logic (e.g., OpenDaylight).
- **Policy-based**: GUI-driven workflows; no coding (e.g., Cisco APIC-EM).

- **APIC-EM Features**:
  - Auto-discovery
  - Topology views
  - Path trace with ACL analysis
  - Easy policy deployment

---

## ✅ Summary

- Virtualization underpins cloud computing, reducing cost and improving flexibility.
- SDN separates network logic from forwarding, simplifying management.
- Cisco ACI offers advanced policy-driven network control via APIC and spine-leaf design.
- SDN types vary from low-level programmability to policy-driven automation.

---

# **Module 14: Network Automation**

---

## 🤖 14.1 Automation and Programmability

- **Why Automate?**
  - Manual CLI config is time-consuming, error-prone, and not scalable.
  - Networks must adapt dynamically to modern demands.

- **Benefits**:
  - Scalability, agility, consistency.
  - Reduced human error and configuration time.

---

## 🧠 14.1.3 Network Management Evolution

- **Past**: CLI + SNMP + NetFlow.
- **Present/Future**: APIs, controllers, machine learning.

- **Controller-Led Architecture**:
  - Centralized device config, telemetry, and app control.
  - Examples: Cisco DNA Center, SD-WAN controllers.

---

## ⚙️ 14.1.4 Controllers and APIs

- **Controller**: Central automation hub; configures devices and collects data.
- **Northbound APIs**: Interface between apps and controller.
- **Southbound APIs**: Interface between controller and devices (e.g., NETCONF, RESTCONF).

---

## 💬 14.1.5 APIs Overview

- **API**: Software interface for structured interactions.
- **REST**:
  - Stateless, uses HTTP verbs (GET, POST, PUT, DELETE).
  - Simple, scalable, language-agnostic.

---

## 🧾 14.1.6 Data Formats

- **JSON**: JavaScript syntax; most common for REST APIs.
- **XML**: Markup language; more verbose, used in NETCONF.

---

## 📦 14.2 Configuration Tools

- **Ansible**: Agentless, YAML-based, uses SSH.
- **Chef/Puppet**: Agent-based, Ruby DSL.
- **Python**: Used with libraries like Netmiko/NAPALM for custom scripts.

---

## 📘 14.2.2 JSON Example

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com"
}
```

---

## 🧮 14.2.3 Python Basics

- Dynamically typed scripting language, widely used in automation.

```python
name = "John"
print("Hello", name)
```

- Used for REST calls, data parsing, automation scripts.

---

## 🛠️ 14.2.4 Cisco DevNet

- **Cisco DevNet**: Portal for APIs, code samples, learning labs, sandboxes.
- Encourages innovation and rapid development.

---

## ✅ Summary

- Automation shifts from CLI to APIs and scripts.
- Controllers (e.g., DNA Center) centralize config and monitoring.
- APIs (REST, NETCONF) simplify integration.
- Tools like Ansible and Python enhance network automation capabilities.
