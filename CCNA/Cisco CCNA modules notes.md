# Cisco CCNA 1.1-8.4 Notes

Created by: Tobias
Created time: April 17, 2025 4:12 PM
Category: Netværk, Notes
Last updated time: April 29, 2025 8:24 PM

# Cisco CCNA 1.1 Notes

## 1.1.1 Switch Boot Sequence

After a Cisco switch is powered on, it goes through the following five-step boot sequence:

1. **POST**: Loads a power-on self-test program stored in ROM to check CPU, DRAM, and flash filesystem.
2. **Boot Loader Software**: Loads a small program stored in ROM after POST completes.
3. **CPU Initialization**: Boot loader performs low-level CPU initialization.
4. **Flash Filesystem Initialization**: Boot loader initializes the flash file system.
5. **IOS Load**: Boot loader locates and loads a default IOS image into memory and gives control to IOS.

---

## 1.1.2 The `boot system` Command

- Switch uses the BOOT environment variable to boot automatically.
- If not set, it tries to load the first executable file found.
- On Catalyst 2960 switches, the image is usually in a folder matching the image name.

Example:

```bash
S1(config)# boot system flash:/c2960-lanbasek9-mz.150-2.SE/c2960-lanbasek9-mz.150-2.SE.bin
```

**Command breakdown**:

| Command Part | Definition |
| --- | --- |
| `boot system` | Main command |
| `flash:` | Storage device |
| `c2960-lanbasek9-mz.150-2.SE/` | Path to file system |
| `c2960-lanbasek9-mz.150-2.SE.bin` | IOS file name |

Use `show boot` to view the current boot setting.

---

## 1.1.3 Switch LED Indicators

- Cisco Catalyst switches have various LEDs for monitoring.

### Mode Button

- Used to toggle port status, duplex, speed, and PoE status.

### LED Indicators:

| LED | Function | Behavior |
| --- | --- | --- |
| **SYST** | System status | Green = normal, Amber = malfunction |
| **RPS** | Redundant Power System | Green = ready, Amber = fault |
| **STAT** | Port status | Green = link present, Amber = blocked |
| **DUPLX** | Duplex mode | Off = half-duplex, Green = full-duplex |
| **SPEED** | Port speed | Off = 10Mbps, Green = 100Mbps, Blinking Green = 1000Mbps |
| **PoE** | Power over Ethernet | Shows PoE status |

---

## 1.1.4 Recovering from a System Crash

If the switch cannot boot IOS, use boot loader:

1. Connect console cable.
2. Power cycle the switch.
3. Hold the **Mode** button within 15 seconds.
4. Release when System LED changes from flashing green → amber → solid green.
5. Access `switch:` prompt.

Useful Commands:

```bash
switch: set          # View environment variablesswitch: flash_init   # Initialize flashswitch: dir flash:   # List flash contentsswitch: BOOT=flash:filename.bin  # Set new boot pathswitch: boot         # Boot with the new IOS
```

Boot loader allows IOS recovery, flash initialization, and password recovery.

---

## 1.1.5 Switch Management Access

- A **Switch Virtual Interface (SVI)** must be configured with an IP address.
- Remote management across networks requires a **default gateway**.
- SVI is a virtual interface (not a physical port).

---

## 1.1.6 Switch SVI Configuration Example

- **Best Practice**: Use a VLAN other than VLAN 1 for management, e.g., VLAN 99.

### Step 1: Configure Management Interface

```bash
interface vlan 99
ip address 172.17.99.11 255.255.255.0
ipv6 address 2001:db8:acad:99::1/64
```

> Note: VLAN 99 must exist and have active ports for SVI to come up.
> 

For IPv6:

```bash
sdm prefer dual-ipv4-and-ipv6 default
reload
```

### Step 2: Configure Default Gateway

```bash
ip default-gateway 172.17.99.1
```

(IPv6 usually handles this via Router Advertisements.)

### Step 3: Verify Configuration

```bash
show ip interface brief
show ipv6 interface brief
```

- Confirms IPv4 and IPv6 assignment to VLAN 99.

> Note: Assigning IP to an SVI does not enable Layer 3 routing.
> 

# Cisco CCNA 1.2 Notes

## 1.2.1 Duplex Communication

- **Full-Duplex**:
    - Simultaneous transmit and receive (bidirectional).
    - Requires **microsegmentation** (one device per switch port).
    - No collisions occur.
    - Required for Gigabit and 10 Gb Ethernet.
- **Half-Duplex**:
    - One-way communication at a time.
    - Prone to collisions.
    - Common in older devices like hubs.

> Full-duplex disables NIC collision detection and ensures 100% link efficiency.
> 

---

## 1.2.2 Configuring Switch Ports at the Physical Layer

- **Manual Speed and Duplex Setting**:
    
    ```bash
    interface <interface-id>
      duplex full
      speed 100
    ```
    
- **Default**:
    - Cisco Catalyst 2960/3560: Speed and duplex set to `auto`.
    - 10/100 Mbps ports: Can use half- or full-duplex.
    - 1000 Mbps ports: Only full-duplex.

> Tip: Always manually configure speed/duplex for servers, routers, or important devices.
> 
- **Important**: Mismatched duplex/speed leads to connection issues.

---

## 1.2.3 Auto-MDIX

- **Auto-MDIX**: Automatically detects cable type (straight-through or crossover).
- **Enabled by default** on Cisco 2960/3560 switches.
- **Command to enable manually**:
    
    ```bash
    interface <interface-id>
      mdix auto
    ```
    
- **Check Auto-MDIX Status**:
    
    ```bash
    show controllers ethernet-controller <interface> phy | include MDIX
    ```
    

> Requires speed auto and duplex auto to work properly.
> 

---

## 1.2.4 Useful Switch Verification Commands

| Command | Purpose |
| --- | --- |
| `show running-config` | View current configuration |
| `show interfaces` | Interface status and statistics |
| `show controllers ethernet-controller phy` | Check Auto-MDIX |

---

## 1.2.5 Verifying Switch Port Configuration

- **Verify VLANs, IP addresses, and gateway**:
    
    ```bash
    show running-config
    ```
    
- **Verify interface status**:
    
    ```
    show interfaces
    ```
    

Example output:

```bash
FastEthernet0/18 is up, line protocol is up
Full-duplex, 100Mb/s
```

---

## 1.2.6 Common Network Access Layer Issues

- **Interface Status**:
    - `up / line protocol down`: Encapsulation mismatch or remote port error.
    - `down / line protocol down`: Physical issue (cable disconnected or bad).
    - `administratively down`: Port has been manually disabled (shutdown command).

> Use show interfaces to troubleshoot.
> 

---

## 1.2.7 Interface Input and Output Errors

- **Input Errors**:
    - **Runts**: Frames smaller than 64 bytes (caused by collisions or bad NICs).
    - **Giants**: Frames too large.
    - **CRC Errors**: Caused by bad cabling or electrical interference.
- **Output Errors**:
    - **Collisions**: Expected only on half-duplex links.
    - **Late Collisions**: Caused by duplex mismatch or long cables.

---

## 1.2.8 Troubleshooting Network Access Layer Issues

**If interface is down**:

- Check cabling and connectors.
- Verify cable type (if Auto-MDIX is not available).
- Match speed settings manually if needed.

**If interface is up but issues persist**:

- Check for excessive **runts**, **giants**, or **CRC errors** (noise issues).
- Check for **collisions** or **late collisions**.
- Manually configure **full-duplex** on both ends if a mismatch is suspected.

> Ongoing maintenance is crucial to prevent cabling damage, misconfigurations, and new device issues.
> 

# Cisco CCNA 1.3 Notes

## 1.3.1 Telnet Operation

- **Telnet** uses TCP port **23**.
- Sends login credentials and session data **in plaintext** — highly insecure.
- Attackers can capture usernames and passwords using tools like **Wireshark**.
- Example: An attacker can easily see captured credentials like `admin` / `ccna` from a Telnet session.

> ⚠️ Telnet should not be used for remote device management.
> 

---

## 1.3.2 SSH Operation

- **SSH (Secure Shell)** uses TCP port **22**.
- Provides **encrypted** communication for authentication and data.
- Should be used instead of Telnet for **secure** remote access.
- Even if an attacker captures traffic, **credentials and data remain encrypted**.

> 🔒 Always configure and use SSH for switch and router management.
> 

---

## 1.3.3 Verifying SSH Support on a Switch

- Use the `show version` command to check IOS capabilities.
- Look for **"k9"** in the IOS filename — it indicates support for cryptographic (encrypted) features.
- Without a cryptographic IOS, SSH cannot be enabled.

---

## 1.3.4 Configuring SSH on a Switch

**Prerequisites**:

- A **unique hostname** must be set.
- **Network connectivity** must be configured.

### Step-by-Step:

**Step 1: Verify SSH Support**

```bash
show ip ssh
```

- If the command is unrecognized, the IOS does not support SSH.

---

**Step 2: Configure Domain Name**

```bash
ip domain-name cisco.com
```

---

**Step 3: Generate RSA Key Pair**

```bash
crypto key generate rsa
ip ssh version 2
```

- Recommended key size: **1024 bits** or higher.
- **Note**: To delete RSA keys, use `crypto key zeroize rsa`.

---

**Step 4: Create User for Local Authentication**

```bash
username admin secret ccna
```

- Creates a local user with password for SSH login.

---

**Step 5: Configure VTY Lines**

```bash
line vty 0 15
transport input ssh
login local
```

- Restricts remote access to SSH only.
- Requires local username and password.

---

**Step 6: Enforce SSH Version 2**

```bash
ip ssh version 2
```

- Ensures only **SSHv2** is used (more secure than SSHv1).

---

## 1.3.5 Verifying SSH Operation

- **Use an SSH client** like **PuTTY** from a PC.
- Example setup:
    - Switch (S1) IP: `172.17.99.11`
    - PC IP: `172.17.99.21`
    - PC uses SSH to connect to `172.17.99.11`
    - Login with `admin` / `ccna`
- When successful:
    - CLI access to the switch is granted over an encrypted SSH session.
- **Verify SSH Status on Switch**:

```bash
show ip ssh
```

- Confirms SSH version and configuration.

# Cisco CCNA 1.4 Notes

## 1.4.1 Basic Router Configuration

- **Routers** allow devices to send and receive data **outside** their local network.
- Cisco routers and switches:
    - Use a **similar IOS**, command structure, and configuration steps.

### Basic Initial Router Setup:

- **Name the Router**:
    
    ```bash
    hostname R1
    ```
    
- **Configure Passwords**:
    
    ```bash
    enable secret <password>
    line console 0
    password <password>
    login
    line vty 0 4
    password <password>
    login
    ```
    
- **Set a Banner Message** (Legal Warning):
    
    ```bash
    banner motd # Unauthorized Access is Prohibited #
    ```
    
- **Save Configuration**:
    
    ```bash
    copy running-config startup-config
    ```
    

> 🛡️ Setting a banner helps enforce legal requirements and security best practices.
> 

---

## 1.4.3 Dual Stack Topology

- **Routers vs Switches**:
    - Switches: Focused on LAN (multiple FastEthernet/GigabitEthernet ports).
    - Routers: Interconnect LANs and WANs and have various interfaces.
- **Dual Stack**:
    - Means supporting both **IPv4** and **IPv6** configurations on the router interfaces.

---

## 1.4.4 Configuring Router Interfaces

- Routers support many interface types: Ethernet, Serial, DSL, Cable, etc.

### Interface Setup Requirements:

- **Assign an IP Address**:
    - IPv4:
        
        ```bash
        interface gigabitethernet0/0
        ip address 192.168.1.1 255.255.255.0
        ```
        
    - IPv6:
        
        ```bash
        interface gigabitethernet0/0
        ipv6 address 2001:db8:acad::1/64
        ```
        
- **Activate the Interface**:
    
    ```bash
    no shutdown
    ```
    
- **Add a Description** (Recommended Best Practice):
    
    ```bash
    scription Link to LAN Switch
    ```
    

> 🔥 An interface must be both configured and activated to pass traffic.
> 

---

## 1.4.6 IPv4 Loopback Interfaces

- A **loopback interface** is a **logical** (virtual) interface, not tied to physical hardware.
- It is always **up** as long as the router itself is operational.
- Common uses:
    - Testing and managing router functionality.
    - Emulating internal networks for routing practice.
    - Used in lab setups to simulate internet connections.

### Creating a Loopback Interface:

```bash
interface loopback0
ip address 10.1.1.1 255.255.255.0
```

- You can create **multiple loopbacks**.
- Each loopback must have a **unique IPv4 address**.

> 🚀 Loopbacks are reliable, always-up interfaces that are invaluable for testing and simulations.
> 

# Cisco CCNA 1.5 Notes

## 1.5.1 Interface Verification Commands

Before considering a router configuration complete, you must **verify the interfaces and connectivity**.

**Useful `show` commands**:

- `show ip interface brief` / `show ipv6 interface brief`
    
    → Quick summary: IP addresses, status ("up" or "down"), and protocols.
    
- `show running-config interface <interface-id>`
    
    → Displays the configuration for a specific interface.
    
- `show ip route` / `show ipv6 route`
    
    → Displays the routing table (directly connected and local routes).
    

---

## 1.5.2 Verify Interface Status

- Use `show ip interface brief` and `show ipv6 interface brief`:
    - **Status**: Should be `up`
    - **Protocol**: Should be `up`
- If the interface or protocol is down:
    - Possible misconfiguration or cabling issues.

---

## 1.5.3 Verify IPv6 Link-Local and Multicast Addresses

- Every IPv6 interface automatically has:
    - A **Link-Local Address** (`FE80::/10` prefix).
    - An optional **Global Unicast Address**.
- View detailed interface IPv6 info:
    
    ```bash
    show ipv6 interface gigabitethernet 0/0/0
    ```
    
- Also shows multicast addresses (starting with `FF02::`).

---

## 1.5.4 Verify Interface Configuration

- Check the full interface config:
    
    ```bash
    show running-config interface <interface-id>
    ```
    

Other helpful commands:

- `show interfaces` → Interface packet flow and error stats.
- `show ip interface` / `show ipv6 interface` → Layer 3 settings and info.

---

## 1.5.5 Verify Routes

- Use `show ip route` and `show ipv6 route` to verify routing tables.
    
    Route types:
    
    - **C** = Connected network (interface active with IP assigned).
    - **L** = Local route (specific IP of the interface).
- For IPv4:
    - Local route `/32` mask.
- For IPv6:
    - Local route `/128` mask.
- **Ping**:
    - Same command structure for IPv4 and IPv6.
    
    ```bash
    ping <IPv4-address>
    ping <IPv6-address>
    ```
    

---

## 1.5.6 Filtering `show` Command Output

**Managing large outputs**:

- By default, output pauses after 24 lines (`-More--`).
- Adjust output length:
    
    ```bash
    terminal length 0
    ```
    
    (No pauses)
    

**Filter command output** using a pipe (`|`) after the `show` command:

| Filter | Purpose |
| --- | --- |
| `section <keyword>` | Displays full section that starts with the keyword |
| `include <keyword>` | Displays lines containing the keyword |
| `exclude <keyword>` | Displays all lines **except** those containing the keyword |
| `begin <keyword>` | Displays output starting from the first match onward |

Example:

```bash
show running-config | include GigabitEthernet
```

---

## 1.5.8 Command History Feature

- Recall previous commands using:
    - **Ctrl + P** or **Up Arrow**: Older commands
    - **Ctrl + N** or **Down Arrow**: Newer commands
- View command history:
    
    ```bash
    show history
    ```
    
- Increase the number of stored commands during the session:
    
    ```bash
    terminal history size <number>
    ```
    

Example:

```bash
terminal history size 50
```

> 🚀 Command history helps in faster configuration and troubleshooting without typing commands again.
> 

# Cisco CCNA 2.1 Notes

## 2.1.1 Switching in Networking

- **Switching**: The core function in LANs, WANs, and telephony.
- **Ingress**: Port where a frame enters the switch.
- **Egress**: Port where a frame exits the switch.

A **LAN switch**:

- Uses a **MAC address table** to forward traffic.
- Always forwards a frame based on the destination MAC address and ingress port.
- **Never forwards** a frame back out the ingress port.

---

## 2.1.2 The Switch MAC Address Table

- Switches use **destination MAC addresses** to forward frames.
- The **MAC address table**:
    - Maps MAC addresses to switch ports.
    - Is stored in **Content Addressable Memory (CAM)** for high-speed lookup.

**Learning process**:

- Switches learn the MAC address of devices on each port by reading the **source MAC** of incoming frames.

---

## 2.1.3 The Switch Learn and Forward Method

**Every incoming frame goes through two steps**:

1. **Learn** — Source MAC Address:
    - If source MAC **not in** table → **Add** it with the port number.
    - If source MAC **already exists**, refresh the timer or update if port has changed.
2. **Forward** — Destination MAC Address:
    - If destination MAC **found** → Forward frame out the matched port.
    - If destination MAC **not found** → Flood frame out all ports except ingress (Unknown Unicast).
    - Broadcast and multicast frames are **flooded** similarly.

---

## 2.1.4 MAC Address Tables with Connected Switches (Video Summary)

**Scenario**:

- PC-A sends frame to PC-B.
- Switch S1:
    - Learns PC-A’s MAC address.
    - Floods frame (since PC-B's MAC is unknown).
- Switch S2:
    - Learns PC-A’s MAC address from incoming frame.
    - Floods frame.
- PC-B receives frame (MAC match).

**Reverse path** (PC-B → PC-A):

- Switch S1:
    - Learns PC-B’s MAC address.
    - Forwards directly to PC-A (MAC is now known).
- Efficient communication established once MAC tables are populated.

---

## 2.1.5 Switching Forwarding Methods

Switches use **ASICs** (Application-Specific Integrated Circuits) for fast Layer 2 forwarding.

**Two primary methods**:

- **Store-and-Forward Switching**:
    - Entire frame received first.
    - Frame is checked with **FCS** (error detection).
    - Only **error-free frames** are forwarded.
- **Cut-Through Switching**:
    - Starts forwarding **immediately** after reading destination MAC.
    - No FCS error check performed.
    - **Faster** but **may forward corrupted frames**.

---

## 2.1.6 Store-and-Forward Switching

**Characteristics**:

- **Error Checking**:
    - Full frame received.
    - **FCS (Frame Check Sequence)** is validated.
    - Corrupted frames are **dropped**.
- **Automatic Buffering**:
    - Needed when there's a **speed mismatch** between ingress and egress ports.
    - Example: 100 Mbps → 1 Gbps transition.

> Store-and-forward switching is Cisco’s primary LAN switching method.
> 

---

## 2.1.7 Cut-Through Switching

**Characteristics**:

- Makes forwarding decision **after destination MAC is read** — no full frame waiting.
- **No FCS validation** (can forward bad frames).
- Useful for **low-latency** environments (High-Performance Computing).

**Fragment-Free Switching** (a variation):

- Waits for first **64 bytes** to check for collision fragments.
- Slightly more reliable than cut-through but still faster than store-and-forward.

> ⚠️ High error rates in the network can severely impact performance with cut-through switching.
> 

# Cisco CCNA 2.2 Notes

## 2.2.1 Collision Domains

- **Collision Domain**: A network segment where devices share the same bandwidth and can cause **collisions** if they transmit simultaneously.
- **Legacy Hubs**:
    - All devices connected to a hub are in the **same collision domain**.
- **Switches**:
    - Each switch port in **full-duplex mode** becomes its **own collision domain** (no collisions).
    - If a port operates in **half-duplex**, it remains part of a collision domain.
- **Duplex Autonegotiation**:
    - Switch ports by default try to use **full-duplex** if the connected device supports it.
    - If connected to half-duplex devices (e.g., legacy hubs), the port will match that mode.

> ⚡ Full-duplex eliminates collisions entirely!
> 

---

## 2.2.2 Broadcast Domains

- **Broadcast Domain**: All devices that receive a Layer 2 broadcast frame.
- **How broadcasts work**:
    - Broadcast MAC address: **FF:FF:FF:FF:FF:FF** (all 1s in binary).
    - Switches forward broadcasts out **all ports except the ingress port**.
    - All devices connected to switches receive the broadcast.
- **Broadcast Domain Expansion**:
    - Connecting switches together **extends** the broadcast domain.
    - Only a **router** or **Layer 3 device** can **segment** broadcast domains.
- **Impact**:
    - Broadcasts are useful (e.g., device discovery), but excessive broadcasts lead to **congestion** and reduce network performance.

> 🚥 Heavy broadcast traffic slows down networks and causes congestion.
> 

---

## 2.2.3 Alleviating Network Congestion

Switches help minimize congestion through several key features:

### Key Characteristics:

- **Fast Port Speeds**:
    - Access switches: 100 Mbps / 1 Gbps.
    - Distribution/core switches: 10 Gbps / 40 Gbps / 100 Gbps.
- **Fast Internal Switching**:
    - High-speed internal buses or shared memory for faster frame handling.
- **Large Frame Buffers**:
    - Temporary storage for frames during congestion, reducing dropped frames.
    - Especially important when a faster ingress port connects to a slower egress port.
- **High Port Density**:
    - Fewer switches required for many devices.
    - Example: Two 48-port switches are better (and cheaper) than four 24-port switches.
    - Helps **localize traffic** and further reduces congestion.

> 🚀 Full-duplex links, faster ports, and better switch design all boost LAN performance dramatically!
> 

# Cisco CCNA 3.1 Notes

## 3.1.1 VLAN Definitions

- **VLAN (Virtual LAN)**:
    - Logical segmentation of a network into smaller groups.
    - Devices in the same VLAN communicate **as if** they are directly connected, even if physically apart.
- **VLAN Characteristics**:
    - Based on **logical connections**, not physical.
    - **Unicast, broadcast, multicast** packets are only forwarded within the same VLAN.
    - **Routing** is required for communication between different VLANs.
- **Without VLANs**:
    - All devices share the same broadcast domain.
    - Broadcasts (e.g., ARP requests) flood all devices, causing congestion.
- **With VLANs**:
    - VLANs **create smaller, logical broadcast domains** across multiple switches.
    - Each VLAN is its **own independent Layer 2 network**.

> 🎯 VLANs simplify network management, improve performance, and strengthen security.
> 

---

## 3.1.2 Benefits of a VLAN Design

Each VLAN corresponds to an **IP network**, meaning VLAN design must align with **hierarchical network addressing**.

### Benefits of VLANs:

- Improved **security** by isolating sensitive data.
- Reduced **congestion** by limiting broadcasts to VLAN members.
- Better **network management** and flexibility.
- **Logical grouping** of users by department or function.
- Simplified **moves, adds, and changes** (no need to rewire).

---

## 3.1.3 Types of VLANs

### 1. Default VLAN

- VLAN **1** is the default on Cisco switches.
- Characteristics:
    - All ports initially belong to VLAN 1.
    - **Native VLAN** and **Management VLAN** default to VLAN 1.
    - **Cannot** rename or delete VLAN 1.

> ⚠️ Security Tip: It is best practice to avoid using VLAN 1 for user or management traffic.
> 

---

### 2. Data VLAN

- VLANs configured to carry **user-generated traffic**.
- **User VLANs** separate network traffic based on groups.
- Should **not** carry management or voice traffic.

---

### 3. Native VLAN

- Used for **untagged** traffic crossing a **trunk link**.
- Trunk ports use **802.1Q** tagging for VLANs.
- By default, the native VLAN is VLAN **1**.

> 🔒 Best Practice: Set a dedicated, unused VLAN as the native VLAN to enhance security.
> 

---

### 4. Management VLAN

- Dedicated VLAN for **network management traffic**:
    - SSH
    - Telnet
    - HTTPS
    - HTTP
    - SNMP
- Default management VLAN is VLAN **1**, but it should be changed for security.

---

### 5. Voice VLAN

- Designed to carry **Voice over IP (VoIP)** traffic.
- Requirements for VoIP:
    - Assured **bandwidth** for call quality.
    - **Priority** over data traffic.
    - Ability to route around **congested** areas.
    - **Delay** less than 150 ms end-to-end.
- Example:
    - **VLAN 150**: Voice traffic.
    - **VLAN 20**: Student data traffic (for devices like PC5 connected via IP phones).

> 📞 Proper Voice VLAN design is critical for maintaining call quality in IP telephony.
> 

# Cisco CCNA 3.2 Notes

## 3.2.1 Defining VLAN Trunks

- **VLAN Trunks** allow VLAN traffic to pass **between switches**.
- A **trunk** is a point-to-point link that carries **multiple VLANs** over the same connection.
- Cisco supports **IEEE 802.1Q** trunking on Fast, Gigabit, and 10-Gigabit Ethernet.

> 🛤️ Trunks are the backbone that connects VLANs across a network!
> 

**Key points**:

- A trunk does **not** belong to any one VLAN.
- All VLANs are allowed on trunks by default.
- Trunks can connect switches, routers, servers, or other 802.1Q-capable devices.

---

## 3.2.2 Network without VLANs

- Entire network is a **single broadcast domain**.
- Broadcasts flood the entire network, causing **congestion**.
- Example: One PC sends a broadcast → all devices on all switches receive it.

---

## 3.2.3 Network with VLANs

- VLANs are assigned to **individual switch ports**.
- Devices attached to a port are assigned an IP address in that VLAN’s subnet.
- Broadcasts are **limited to the VLAN**:
    - PC in VLAN 10 → Broadcast only reaches other VLAN 10 devices.

**Important**:

- Trunk ports between switches must be configured to **carry multiple VLANs**.

> 🚦 VLANs control traffic flow, limit broadcasts, and improve network efficiency.
> 

---

## 3.2.4 VLAN Identification with a Tag

- **Standard Ethernet frames** don’t carry VLAN information.
- **Tagging** is needed on trunks to indicate VLAN membership.
- **802.1Q tagging**:
    - Adds a **4-byte tag** inside the Ethernet frame.

**802.1Q Tag Fields**:

| Field | Purpose |
| --- | --- |
| Type | Tag Protocol ID (TPID), set to 0x8100 |
| User Priority | 3-bit field for Quality of Service (QoS) |
| CFI (Canonical Format Identifier) | Token Ring compatibility |
| VLAN ID (VID) | 12 bits to support **up to 4096 VLANs** |

> 🏷️ Frames are re-tagged and their checksum (FCS) is recalculated before transmission.
> 

---

## 3.2.5 Native VLANs and 802.1Q Tagging

- **Native VLAN**:
    - The VLAN assigned for **untagged traffic** on a trunk link.
    - Default is **VLAN 1**.
- **Behavior**:
    - Untagged frames received on a trunk are placed into the native VLAN.
    - Tagged frames received with native VLAN ID are **dropped** (security precaution).

**Best Practices**:

- Change the native VLAN to an **unused VLAN**.
- Avoid devices tagging traffic on the native VLAN (could cause issues).
- Ensure trunked devices agree on which VLAN is the native VLAN.

> 🔒 Misconfigurations in native VLANs can cause security vulnerabilities!
> 

---

## 3.2.6 Voice VLAN Tagging

- **Voice VLAN** is needed for **VoIP** traffic:
    - Enables **QoS policies** for reliable voice communication.
    - Prioritizes voice traffic over data traffic.
- Cisco IP Phones:
    - Connect directly to switch access ports.
    - Support **two VLANs**:
        - **Voice VLAN**: For phone traffic.
        - **Data VLAN**: For PC traffic.

**IP Phone Structure**:

| Port | Connects To |
| --- | --- |
| Port 1 | Switch |
| Port 2 | IP phone internal |
| Port 3 | PC or other device |
- Switch sends **CDP packets** instructing the phone on VLAN configurations.

**Traffic Handling**:

- Voice VLAN traffic → Tagged with Layer 2 CoS priority.
- Data VLAN traffic → May be untagged or tagged separately.

---

## 3.2.7 Voice VLAN Verification Example

- Use the command:
    
    ```bash
    show interface fa0/18 switchport
    ```
    
- Sample output shows:
    - **Access VLAN** (e.g., VLAN 20) for PC data traffic.
    - **Voice VLAN** (e.g., VLAN 150) for IP phone voice traffic.

> 🎯 Always verify switchport configuration when setting up Voice VLANs to ensure quality voice service.
> 

# Cisco CCNA 3.3 Notes

## 3.3.1 VLAN Ranges on Catalyst Switches

- Cisco switches support a **large number of VLANs**:
    - **Normal Range VLANs**: IDs **1–1005**
    - **Extended Range VLANs**: IDs **1006–4094**

### Normal Range VLANs:

- Used in **small to medium networks**.
- Stored in **vlan.dat** file in **flash memory**.
- IDs **1, 1002–1005**:
    - Automatically created.
    - Cannot be deleted (reserved for legacy technologies).

### Extended Range VLANs:

- Used by **service providers** and **large enterprises**.
- Stored in **running-config**.
- **VTP transparent mode** is required to create them.

> ℹ️ VLAN ID limit: 4096 because of 12-bit field in 802.1Q headers.
> 

---

## 3.3.2 VLAN Creation Commands

- **Create a VLAN**:
    
    ```bash
    vlan <vlan-id>
    name <vlan-name>
    ```
    
- **Notes**:
    - Best practice: Always name VLANs.
    - VLAN info is saved automatically to **vlan.dat**, but always **save your config** (`copy running-config startup-config`) for other settings.

---

## 3.3.3 VLAN Creation Example

Example:

- Create VLAN 20 called "Students":
    
    ```bash
    vlan 20
    name Students
    ```
    
- You can create **multiple VLANs** at once:
    
    ```bash
    vlan 100,102,105-107
    ```
    

> 🛠️ This creates VLANs 100, 102, 105, 106, and 107.
> 

---

## 3.3.4 VLAN Port Assignment Commands

- **Assign an interface to a VLAN**:
    
    ```bash
    
    interface <interface-id>
    switchport mode access
    switchport access vlan <vlan-id>
    ```
    
- **Tip**:
    
    Use `switchport mode access` to prevent negotiation to trunk mode (security best practice).
    
- **Configure multiple interfaces**:
    
    ```bash
    interface range f0/1 - 4
    ```
    

---

## 3.3.5 VLAN Port Assignment Example

Example:

- Assign port F0/6 to VLAN 20:
    
    ```bash
    interface fa0/6
    switchport mode access
    switchport access vlan 20
    ```
    
- Devices connected to F0/6 will now belong to VLAN 20.

> 🧠 Remember: VLAN is set on the switchport, not on the end device.
> 

---

## 3.3.6 Data and Voice VLANs

- An **access port** can:
    - Belong to **one data VLAN**.
    - Also carry **one voice VLAN** (for IP Phones).

Example:

- PC connected to Cisco IP phone, which connects to switch.
- Voice VLAN carries **VoIP** traffic, Data VLAN carries **PC** traffic.

---

## 3.3.7 Data and Voice VLAN Example

- **Assign a voice VLAN**:
    
    ```bash
    interface fa0/18
    switchport mode access
    switchport access vlan 20
    switchport voice vlan 150
    ```
    
- **Optional**: Enable QoS trust for voice:
    
    ```bash
    mls qos trust cos
    ```
    

> 🎙️ Voice VLAN improves voice quality by prioritizing VoIP packets.
> 

---

## 3.3.8 Verify VLAN Information

Useful **show** commands:

- Show VLANs:
    
    ```bash
    show vlan brief
    ```
    
- Show detailed VLAN info:
    
    ```bash
    show vlan id <vlan-id>
    show vlan name <vlan-name>
    show vlan summary
    ```
    
- Show switchport info for a specific port:
    
    ```bash
    show interfaces <interface-id> switchport
    ```
    
- Show VLAN interface status:
    
    ```bash
    show interfaces vlan <vlan-id>
    ```
    

> 🔎 Always verify VLAN assignments after configuration!
> 

---

## 3.3.9 Change VLAN Port Membership

- **Change a port's VLAN**:
    
    ```bash
    interface <interface-id>
    switchport access vlan <new-vlan-id>
    ```
    
- **Return a port to default VLAN 1**:
    
    ```bash
    no switchport access vlan
    ```
    

Example:

- Move Fa0/18 back to VLAN 1 if needed.

---

## 3.3.10 Delete VLANs

- **Delete a VLAN**:
    
    ```bash
    no vlan <vlan-id>
    ```
    

**Caution**:

- Before deleting a VLAN, reassign its member ports to a different VLAN!
- **Delete the vlan.dat file** (full VLAN reset):
    
    ```bash
    delete flash:vlan.dat
    reload
    ```
    

**Factory Reset (VLAN + Config)**:

1. Erase startup config:
    
    ```bash
    erase startup-config
    ```
    
2. Delete VLAN database:
    
    ```bash
    delete flash:vlan.dat
    ```
    
3. Reload the switch:
    
    ```bash
    reload
    ```
    

> ⚡ Deleting VLANs removes all port assignments — devices will lose connectivity until reconfigured!
> 

# Cisco CCNA 3.4 Notes

## 3.4.1 Trunk Configuration Commands

- A **VLAN trunk** is a Layer 2 link that carries traffic for **multiple VLANs**.
- Unless restricted, a trunk carries **all VLANs** by default.

### Trunk Configuration Commands:

```bash
interface <interface-id>
switchport mode trunk
switchport trunk native vlan <vlan-id>
switchport trunk allowed vlan <vlan-list>
```

> 🛠️ These commands configure the port as a trunk, set the native VLAN, and restrict allowed VLANs.
> 

---

## 3.4.2 Trunk Configuration Example

**Scenario**:

- VLANs 10, 20, and 30 = Faculty, Student, and Guest.
- VLAN 99 = Native VLAN.

### Example Configuration on S1:

```bash
interface fa0/1
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 10,20,30,99
```

**Notes**:

- Cisco Catalyst 2960 switches **automatically use 802.1Q** encapsulation.
- Always **match native VLAN** on both ends of the trunk!
- If trunking settings differ, **errors will occur**.

> ⚡ Consistency is key across trunk links!
> 

---

## 3.4.3 Verify Trunk Configuration

- Use the command:
    
    ```bash
    show interfaces <interface-id> switchport
    ```
    

### Key Items to Verify:

- **Administrative Mode**: Should be `trunk`.
- **Native VLAN**: Should be your configured VLAN (e.g., VLAN 99).
- **Allowed VLANs**: List should match your configuration (e.g., 10, 20, 30, 99).

Another useful command:

```bash
show interface trunk
```

- Summarizes all trunk interfaces and their allowed VLANs.

> 🧐 Always verify after configuring trunks to avoid hidden problems!
> 

---

## 3.4.4 Reset the Trunk to the Default State

### Reset Commands:

```bash
interface <interface-id>
no switchport trunk allowed vlan
no switchport trunk native vlan
```

- This resets the trunk to:
    - **Allow all VLANs**.
    - **Native VLAN set back to VLAN 1**.

### Remove Trunking Completely:

```bash
interface <interface-id>
switchport mode access
```

- This returns the port to **static access mode**.

> 🔄 Resetting trunks is important for cleanup and reconfiguration when network changes occur.
> 

# Cisco CCNA 3.5 Notes

## 3.5.1 Introduction to DTP (Dynamic Trunking Protocol)

- **DTP**: Cisco proprietary protocol for **negotiating trunk links** automatically.
- Enabled by default on **Catalyst 2960** and **3650** switches.
- Operates on a **point-to-point** basis between network devices.

### Key Points:

- **Default mode**: `dynamic auto`
- If connecting to **non-Cisco devices**, disable DTP to avoid misconfigurations:
    
    ```bash
    switchport mode trunk
    switchport nonegotiate
    ```
    
- **Neighbor must support DTP** for automatic trunk negotiation.

**Caution**:

Some non-Cisco devices might forward DTP frames incorrectly — always disable DTP on ports connected to non-Cisco devices.

---

## 3.5.2 Negotiated Interface Modes

Use the `switchport mode` command to control trunk negotiation.

### Interface Mode Options:

| Mode | Description |
| --- | --- |
| `access` | Forces port to be a **permanent access port** |
| `trunk` | Forces port to be a **permanent trunk port** |
| `dynamic auto` | Will **become trunk** if neighbor requests |
| `dynamic desirable` | **Actively negotiates** to become trunk |

### Disable DTP:

```bash
switchport nonegotiate
```

- Stops sending DTP frames.
- Only works if mode is **access** or **trunk**.

> ⚡ Best Practice: Statistically configure trunk links (mode trunk + nonegotiate) to eliminate negotiation ambiguity.
> 

---

## 3.5.3 Results of a DTP Configuration

| Local Mode | Remote Mode | Result |
| --- | --- | --- |
| Access | Access | Access link |
| Trunk | Trunk | Trunk link |
| Dynamic Auto | Dynamic Auto | Access link (no trunk formed) |
| Dynamic Desirable | Dynamic Auto | Trunk link |
| Dynamic Desirable | Dynamic Desirable | Trunk link |

> 🛠️ Best Practice: Statistically configure trunk links instead of relying on dynamic negotiation.
> 

---

## 3.5.4 Verify DTP Mode

- To check DTP status:
    
    ```bash
    show dtp interface
    ```
    

**What to verify**:

- Interface mode (access/trunk/dynamic).
- Whether DTP is actively sending frames.

**Best Practices**:

- When trunking is needed: Set `switchport mode trunk` and `switchport nonegotiate`.
- When trunking is not intended: Disable DTP altogether.

# Cisco CCNA 4.1 Notes

## 4.1.1 What is Inter-VLAN Routing?

- **VLANs** segment Layer 2 networks.
- Devices in different VLANs **cannot communicate** directly without **Layer 3 routing**.

### Inter-VLAN Routing Options:

| Method | Description |
| --- | --- |
| **Legacy Inter-VLAN Routing** | Physical router interfaces per VLAN (outdated) |
| **Router-on-a-Stick** | One physical router interface with subinterfaces |
| **Layer 3 Switch (SVIs)** | Most scalable, uses virtual interfaces |

---

## 4.1.2 Legacy Inter-VLAN Routing

- Each **VLAN** connects to a **separate router interface**.
- **Example Setup**:
    - R1 G0/0/0 connects to VLAN 10.
    - R1 G0/0/1 connects to VLAN 20.

**Flow**:

1. PC1 sends traffic to its gateway (R1 G0/0/0).
2. R1 routes the packet to G0/0/1.
3. Switch forwards the frame to PC2 in VLAN 20.

### Limitations:

- **Scalability Issue**: Requires one physical interface **per VLAN**.
- **Not practical** for more than a few VLANs.

> ❌ Legacy method is no longer used; explained here for historical understanding.
> 

---

## 4.1.3 Router-on-a-Stick Inter-VLAN Routing

- **One physical router interface** handles multiple VLANs using **subinterfaces**.

### How it works:

- Router interface = **Trunk port** (802.1Q tagging).
- Each **subinterface**:
    - Is associated with a **specific VLAN**.
    - Has its own **IP address** (gateway for that VLAN).

**Traffic Flow**:

1. VLAN-tagged frame enters the router.
2. Routed to the correct subinterface based on VLAN ID.
3. Routed traffic exits via appropriate VLAN.

**Example Subinterface Config**:

```bash
interface g0/0.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0

interface g0/0.20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0
```

### Limitations:

- **Scales poorly** beyond ~50 VLANs.

> 🛠️ Router-on-a-stick is simple and effective for small to medium networks.
> 

---

## 4.1.4 Inter-VLAN Routing on a Layer 3 Switch

- Modern networks use **Layer 3 switches** for inter-VLAN routing.

### Using Switched Virtual Interfaces (SVIs):

- **SVI**: Virtual interface assigned to a VLAN.
- Functions **like a router interface**.

### Advantages of Layer 3 Switch Inter-VLAN Routing:

- **Faster**: Hardware switching (ASICs) instead of software routing.
- **No external links** to a router needed.
- **Use of EtherChannels** for **higher bandwidth** trunk links.
- **Lower latency**.
- **Preferred solution** in campus LANs.

### Disadvantages:

- **Higher cost** compared to standard Layer 2 switches.

> 🚀 Layer 3 switches are the standard for modern enterprise inter-VLAN routing!
> 

# Cisco CCNA 4.2 Notes

## 4.2.1 Router-on-a-Stick Scenario

- **Router-on-a-Stick**: One physical router interface divided into **subinterfaces** to route between VLANs.
- **Topology**:
    - R1 G0/0/1 connected to S1 F0/5.
    - S1 F0/1 connected to S2 F0/1.
    - Trunks carry VLAN traffic between switches.

> 📍 The router appears "on a stick" at the network border.
> 

---

## 4.2.2 S1 VLAN and Trunking Configuration

### Steps:

1. **Create and Name VLANs**:
    
    ```bash
    vlan 10
    name Faculty
    vlan 20
    name Students
    vlan 99
    name Management
    ```
    
2. **Create the Management Interface**:
    
    ```bash
    interface vlan 99
    ip address <IP-address> <subnet-mask>
    no shutdown
    ```
    
3. **Configure Access Ports**:
    
    ```bash
    interface fa0/6
    switchport mode access
    switchport access vlan 10
    ```
    
4. **Configure Trunk Ports**:
    
    ```bash
    interface fa0/1
    switchport mode trunk
    
    interface fa0/5
    switchport mode trunk
    ```
    

> 🛠️ Make sure to assign correct VLANs and trunking roles for ports!
> 

---

## 4.2.3 S2 VLAN and Trunking Configuration

- S2 configuration mirrors S1 setup:
    - Create same VLANs.
    - Configure access and trunk ports similarly.

---

## 4.2.4 R1 Subinterface Configuration

- Router's G0/0/1 interface is divided into subinterfaces — one per VLAN.

### Subinterface Configuration:

```bash
interface g0/0/1.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0

interface g0/0/1.20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0

interface g0/0/1.99
encapsulation dot1q 99 native
ip address 192.168.99.1 255.255.255.0

interface g0/0/1
no shutdown
```

**Important**:

- `encapsulation dot1q <vlan-id>` specifies VLAN tagging.
- The `native` keyword is used for native VLAN (VLAN 99).
- **Bring up** the physical interface with `no shutdown`.

> ⚡ Without no shutdown, all subinterfaces remain down!
> 

---

## 4.2.5 Verify Connectivity Between PC1 and PC2

- Use `ipconfig` on PCs to verify correct IP and gateway settings.
- Use `ping`:
    - Ping PC2 from PC1.
    - Ping the switches from PCs.

> ✅ Successful pings confirm that inter-VLAN routing is operational.
> 

---

## 4.2.6 Router-on-a-Stick Inter-VLAN Routing Verification

### Useful Show Commands:

- **Check Routing Table**:
    
    ```bash
    show ip route
    ```
    
    - Look for `C` (Connected) entries for each VLAN.
- **Check Subinterface Status**:
    
    ```bash
    show ip interface brief
    ```
    
    - Confirm IPs are assigned and interfaces are `up`.
- **Verify Subinterface Details**:
    
    ```bash
    show interfaces g0/0/1.10
    show interfaces g0/0/1.20
    show interfaces g0/0/1.99
    ```
    
- **Check Switch Trunk Links**:
    
    ```bash
    show interfaces trunk
    ```
    
    - Confirms trunk ports and allowed VLANs.

> 🔍 Always verify both router and switch configurations when troubleshooting VLAN routing!
> 

# Cisco CCNA 4.3 Notes

## 4.3.1 Layer 3 Switch Inter-VLAN Routing

- **Router-on-a-Stick** is simple but doesn't scale for large networks.
- **Enterprise networks** use **Layer 3 switches** to perform **hardware-based routing** for higher performance.

### Layer 3 Switch Capabilities:

- Route between VLANs using **Switched Virtual Interfaces (SVIs)**.
- Convert Layer 2 switchports into **routed Layer 3 ports**.

> 🚀 SVIs allow Layer 3 switches to route traffic internally without needing a separate router.
> 

---

## 4.3.2 Layer 3 Switch Scenario

**Topology**:

- Layer 3 switch (D1) connects:
    - PC1 (VLAN 10)
    - PC2 (VLAN 20)

**Assigned VLAN IPs**:

| VLAN | IP Address |
| --- | --- |
| 10 | 192.168.10.1 |
| 20 | 192.168.20.1 |

---

## 4.3.3 Layer 3 Switch Configuration

### Step 1: Create VLANs

```bash
vlan 10
name Faculty
vlan 20
name Students
```

---

### Step 2: Create SVI Interfaces

```bash
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown

interface vlan 20
ip address 192.168.20.1 255.255.255.0
no shutdown
```

> 🛠️ These act like "virtual router ports" for VLANs.
> 

---

### Step 3: Configure Access Ports

```bash
interface fa0/6
switchport mode access
switchport access vlan 10

interface fa0/7
switchport mode access
switchport access vlan 20
```

---

### Step 4: Enable IP Routing

```bash
ip routing
```

- Enables the switch to route between VLANs.

---

## 4.3.4 Layer 3 Switch Inter-VLAN Routing Verification

- Use `ipconfig` on PCs to verify IP settings.
- Use `ping`:
    - PC1 should successfully ping PC2.
    - Confirms **inter-VLAN routing** is operational.

---

## 4.3.5 Routing on a Layer 3 Switch (Beyond the Local VLANs)

- If VLANs must communicate with **external networks**:
    - Configure **routed ports**.
    - Use **static or dynamic routing protocols** (e.g., OSPF).

### Convert Switchport to Routed Port:

```bash
interface g0/0/1
no switchport
ip address 10.10.10.1 255.255.255.0
no shutdown
```

> 🌐 Routed ports act like traditional router interfaces.
> 

---

## 4.3.6 Routing Scenario on a Layer 3 Switch

**New Setup**:

- D1 connects to R1 (a router running OSPF).
- R1 advertises:
    - 10.10.10.0/24
    - 10.20.20.0/24
- D1 needs to advertise VLAN 10 and VLAN 20 subnets via OSPF.

---

## 4.3.7 Routing Configuration on a Layer 3 Switch

### Step 1: Configure the Routed Port

```bash
interface g0/0/1
no switchport
ip address 10.10.10.1 255.255.255.0
no shutdown
```

---

### Step 2: Enable IP Routing

```bash
ip routing
```

---

### Step 3: Configure OSPF

```bash
router ospf 1
network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
network 10.10.10.0 0.0.0.255 area 0
```

- Advertises VLAN networks and routed port subnet.

---

### Step 4: Verify Routing

- Check routing table:
    
    ```bash
    show ip route
    ```
    
- Look for OSPF routes (`O`) and directly connected (`C`) networks.

---

### Step 5: Verify Connectivity

- PC1 and PC2 should now **ping** devices on external networks (e.g., servers attached to R1).

# Cisco CCNA 4.4 Notes

## 4.4.1 Common Inter-VLAN Issues

- Always **verify and troubleshoot** after configuring inter-VLAN routing.
- **First step**: Check physical connections (cabling and port assignments).
- **Other common causes**:
    - Missing VLANs
    - Trunking issues
    - Access port misconfiguration
    - Router subinterface misconfigurations

---

## 4.4.2 Troubleshoot Inter-VLAN Routing Scenario

**Scenario**:

- R1 router uses subinterfaces to provide inter-VLAN routing.
- PC1 and PC2 are in different VLANs but initially cannot communicate.

---

## 4.4.3 Missing VLANs

- **Symptoms**:
    - Devices connected to a deleted VLAN become **inactive**.
    - Ports stay assigned to a missing VLAN until manually reassigned.

### Troubleshooting Steps:

- Check VLANs:
    
    ```bash
    show vlan brief
    ```
    
- Check interface VLAN membership:
    
    ```bash
    show interface <interface-id> switchport
    ```
    
- **If VLAN is missing**:
    - Recreate VLAN:
        
        ```bash
        vlan <vlan-id>
        name <vlan-name>
        ```
        
- Ensure you **exit VLAN configuration mode** properly after creating it.

> 🛠️ Deleting VLANs doesn't reset port assignments — ports remain inactive until VLAN is restored or reassigned.
> 

---

## 4.4.4 Switch Trunk Port Issues

- **Symptoms**:
    - No trunk between switches/router.
    - No inter-VLAN traffic flow.

### Troubleshooting Steps:

- Check trunk ports:
    
    ```bash
    show interfaces trunk
    ```
    
- Check interface configuration:
    
    ```bash
    show running-config interface <interface-id>
    ```
    
- Common mistakes:
    - Trunk port is **shutdown**.
    - Port mode is misconfigured.
- **Fix trunking**:
    
    ```bash
    interface <interface-id>
    no shutdown
    switchport mode trunk
    ```
    

> 🔄 Regularly verify trunk ports to avoid broken inter-switch communication!
> 

---

## 4.4.5 Switch Access Port Issues

- **Symptoms**:
    - Host can't ping its gateway.
    - Host cannot reach devices on other VLANs.

### Troubleshooting Steps:

- Verify access port configuration:
    
    ```bash
    show interfaces <interface-id> switchport
    ```
    
- Look for:
    - Port mode = static access
    - VLAN assignment is correct
- **Fix access port assignment**:
    
    ```bash
    interface <interface-id>
    switchport mode access
    switchport access vlan <vlan-id>
    ```
    

> 🖥️ If a device is in the wrong VLAN, it cannot communicate properly with its gateway or other VLANs.
> 

---

## 4.4.6 Router Configuration Issues

- **Symptoms**:
    - Correct trunking on switch.
    - Correct VLANs exist.
    - Still no inter-VLAN communication.

### Troubleshooting Steps:

- Check subinterfaces:
    
    ```bash
    show ip interface brief
    ```
    
- Check VLAN tagging and IP address assignment:
    
    ```bash
    show interfaces | include Gig|802.1Q
    ```
    
- Look for:
    - Wrong VLAN ID in subinterface configuration.
    - IP address mismatches.
- **Fix subinterface VLAN assignment**:
    
    ```bash
    interface g0/0/1.<vlan-id>
    encapsulation dot1q <correct-vlan-id>
    ```
    

> 🛠️ Even a single wrong VLAN ID can completely break inter-VLAN routing.
> 

# Cisco CCNA 5.1 Notes

## 5.1.1 Redundancy in Layer 2 Switched Networks

- **Redundancy**: Adding physical paths to ensure **network availability** even if a link fails.
- **Problem**:
    - Redundant paths can cause **Layer 2 loops**.
    - Ethernet requires a **loop-free topology**.

**Result of loops**:

- Frames can circulate endlessly until a link breaks.

---

## 5.1.2 Spanning Tree Protocol (STP)

- **STP**: A **loop prevention protocol** for Layer 2 networks.
- **Standard**: IEEE 802.1D.
- Allows redundancy **while preventing loops** by logically blocking redundant paths.

> 🌐 STP ensures Ethernet networks remain operational despite redundancy.
> 

---

## 5.1.4 Issues with Redundant Switch Links

Without STP:

- **Layer 2 loops** cause:
    - MAC address table instability
    - Link saturation (overloading links)
    - High CPU utilization
    - Network outages

### Layer 2 vs Layer 3:

- **Layer 3**: TTL (IPv4) and Hop Limit (IPv6) prevent endless loops.
- **Layer 2**: No built-in loop prevention → STP is necessary.

---

## 5.1.5 Layer 2 Loops

Effects of Layer 2 Loops:

- **Broadcast frames** (e.g., ARP requests) loop endlessly.
- **MAC address instability**: Switches keep updating incorrect MAC-table entries.
- **High CPU load**: Switch becomes unable to forward frames.
- **Unknown unicast frames** can also duplicate endlessly.

> ⚡ Without STP, even one broadcast frame can bring down the entire LAN!
> 

---

## 5.1.6 Broadcast Storm

- **Broadcast Storm**: A flood of broadcast frames overwhelms the network.
- Caused by:
    - Hardware issues (e.g., faulty NICs).
    - Layer 2 loops.

Effects:

- Devices become unreachable.
- Switches' MAC tables become unstable.
- Leads to complete network failure.

**Important**:

- Multicasts (like ICMPv6 Neighbor Discovery) are often treated like broadcasts at Layer 2.
- Broadcast storms grow rapidly without control.

> 🛡️ Cisco switches enable STP by default to prevent broadcast storms.
> 

---

## 5.1.7 The Spanning Tree Algorithm (STA)

- **Invented by Radia Perlman** in 1985.
- **Goal**: Create a **loop-free logical topology** from a physically redundant network.

### STA Key Processes:

| Step | Description |
| --- | --- |
| **Select Root Bridge** | All switches agree on a single switch (the root) |
| **Determine Least-Cost Paths** | Each switch finds its best path to the root |
| **Block Redundant Paths** | Prevents loops by disabling redundant links |

---

## STA Scenario Explained

**Topology**:

- Multiple redundant paths between switches.

**Steps**:

1. **Select Root Bridge**:
    - Example: S1 is elected root.
2. **Block Redundant Links**:
    - Redundant links are placed into a **blocking state**.
3. **Establish Loop-Free Paths**:
    - Only one active path between any two switches.

---

## Handling Link Failures

- If a link **fails**:
    - STP **recalculates**.
    - A previously blocked port becomes **active**.
    - Maintains a single active path to the root bridge.

**Example**:

- Link between S2 and S4 fails.
- Previously blocked link between S4 and S5 becomes active.

> 🔄 STP dynamically adapts to maintain a resilient, loop-free network.
> 

# Cisco CCNA 5.2 Notes

## 5.2.1 Steps to a Loop-Free Topology

STP uses a **four-step** process to build a loop-free topology:

1. Elect the **Root Bridge**.
2. Elect the **Root Ports**.
3. Elect the **Designated Ports**.
4. Elect **Alternate (Blocked) Ports**.

Switches exchange **BPDUs (Bridge Protocol Data Units)** to share information.

Each BPDU contains a **Bridge ID (BID)**:

- **Priority** (default 32768)
- **Extended System ID** (VLAN ID)
- **MAC Address**

**Lowest BID wins** during elections.

---

## 5.2.2 Step 1: Elect the Root Bridge

- All switches initially assume they are the root.
- Switches exchange BPDUs.
- **Lowest BID** (priority + MAC address) becomes the **Root Bridge**.

> 📍 The Root Bridge is the logical center of the STP topology.
> 

---

## 5.2.3 Impact of Default BIDs

- If priorities are the same (default 32768 + VLAN ID = 32769 for VLAN 1):
    - **Lowest MAC address** decides the Root Bridge.

**Tip**:

Manually configure root bridges by lowering their priority:

```bash
spanning-tree vlan <vlan-id> priority <lower-value>
```

---

## 5.2.4 Determine the Root Path Cost

- **Root Path Cost**: Sum of the costs of all links to reach the Root Bridge.

| Link Speed | Port Cost (Short) |
| --- | --- |
| 10 Mbps | 100 |
| 100 Mbps | 19 |
| 1 Gbps | 4 |
| 10 Gbps | 2 |
- Path cost is added at each hop.

**Note**:

Administrators can manually adjust **port cost** to influence path selection.

---

## 5.2.5 Step 2: Elect the Root Ports

- Every **non-root switch** chooses **one Root Port**.
- Root Port = port with **lowest path cost** to the Root Bridge.
- If two paths have the same cost, tie-breakers apply (see below).

---

## 5.2.6 Step 3: Elect Designated Ports

- For each network segment:
    - The switch closest to the Root Bridge (lowest path cost) has its port as **Designated Port**.
- **Rules**:
    - All ports on the Root Bridge are Designated Ports.
    - If one end of a link is a Root Port, the other end is Designated.
    - If no Root Port exists, compare BIDs to decide.

**Tip**:

All **ports connecting to end devices** are also Designated Ports.

---

## 5.2.7 Step 4: Elect Alternate (Blocked) Ports

- Any port that is **not a Root Port** or **Designated Port** becomes **Alternate/Blocked**.
- Alternate ports are in **blocking state** to prevent loops.

> 🔒 Blocking ports is crucial for loop prevention in STP.
> 

---

## 5.2.8 Root Port Tie-Breakers

When multiple equal-cost paths exist:

| Order | Tie-Breaker |
| --- | --- |
| 1 | Lowest **Sender BID** |
| 2 | Lowest **Sender Port Priority** |
| 3 | Lowest **Sender Port ID** |

**Example**:

- If Sender BID is the same, check Port Priority.
- If Port Priority is the same, check Port ID (port number).

---

## 5.2.9 STP Timers and Port States

**Timers**:

| Timer | Default | Purpose |
| --- | --- | --- |
| Hello Timer | 2 sec | Interval between BPDUs |
| Forward Delay | 15 sec | Listening/Learning phase duration |
| Max Age | 20 sec | Max time a BPDU is stored |

**Port States**:

| State | Purpose |
| --- | --- |
| Blocking | No user data sent or received |
| Listening | Prepares to participate in STP |
| Learning | Builds MAC address table but no forwarding |
| Forwarding | Fully operational state |
| Disabled | Administratively down |

> 🛡️ IEEE recommends a max of 7 switches diameter when using default STP timers.
> 

---

## 5.2.10 Operational Details of Each Port State

| Port State | STP Activity | Forwarding Frames? | Learning MACs? |
| --- | --- | --- | --- |
| Disabled | No STP activity | No | No |
| Blocking | Receives BPDUs only | No | No |
| Listening | Sends and receives BPDUs | No | No |
| Learning | Sends/Receives BPDUs, builds MAC table | No | Yes |
| Forwarding | Sends/Receives all frames | Yes | Yes |

---

## 5.2.11 Per-VLAN Spanning Tree (PVST)

- In PVST:
    - **Each VLAN** runs its own **separate STP instance**.
    - Different VLANs can have **different Root Bridges**.

**Benefits**:

- Optimized traffic paths.
- Load balancing across redundant links.

**Example**:

- VLAN 10 can have Switch A as Root.
- VLAN 20 can have Switch B as Root.

> 🌐 PVST is Cisco’s default STP behavior on VLAN-enabled networks.
> 

# Cisco CCNA 5.3 Notes

## 5.3.1 Different Versions of STP

- "STP" is often used generically but covers multiple standards:
    - **Original STP** (IEEE 802.1D)
    - **Rapid Spanning Tree Protocol (RSTP)** (IEEE 802.1w)
    - **Multiple Spanning Tree Protocol (MSTP)** (IEEE 802.1s)
- **Modern Standard**:
    - IEEE 802.1D-2004 requires RSTP.
- **Cisco Defaults**:
    - **PVST+** (Per-VLAN Spanning Tree Plus) by default on IOS 15.0+.
    - Rapid Spanning Tree must be **explicitly configured**.

> 🛠️ Professionals must choose the correct STP version based on network needs.
> 

---

## 5.3.2 RSTP Concepts

- **RSTP (IEEE 802.1w)** improves **convergence speed** dramatically (hundreds of milliseconds).
- Maintains **backward compatibility** with original 802.1D STP.
- Uses the same **spanning tree algorithm** for port roles and topology decisions.
- **Rapid PVST+**: Cisco’s per-VLAN implementation of RSTP.

> 🚀 RSTP provides near-instant recovery for network failures compared to original STP.
> 

---

## 5.3.3 RSTP Port States and Roles

| Category | STP | RSTP |
| --- | --- | --- |
| **Port States** | Blocking, Listening, Learning, Forwarding, Disabled | Discarding, Learning, Forwarding |
| **Port Roles** | Root Port, Designated Port, Non-Designated Port | Root Port, Designated Port, Alternate Port, Backup Port |
- **Discarding** (RSTP) = Merges Blocking + Listening states.
- **Alternate Port**: Backup path to Root Bridge.
- **Backup Port**: Backup on shared media (like hubs).

> 🔒 Alternate and backup roles improve rapid failover.
> 

---

## 5.3.4 PortFast and BPDU Guard

**Problem**:

- When a device connects, STP can delay traffic for up to **30 seconds** (Listening + Learning states).
- This delay can disrupt DHCP and other immediate services.

**Solution**:

- **PortFast**:
    - Moves ports **immediately** to Forwarding.
    - Used only for **access ports** (end devices).

**Command Example**:

```bash
interface <interface-id>
spanning-tree portfast
```

- **BPDU Guard**:
    - Protects against switches accidentally connecting on PortFast ports.
    - Shuts down the port if a **BPDU is received**.

**Command Example**:

```bash
interface <interface-id>
spanning-tree bpduguard enable
```

> ⚡ Use PortFast + BPDU Guard together for safe and fast host connectivity!
> 

---

## 5.3.5 Alternatives to STP

As networks grew larger and more complex, new technologies evolved:

| Technology | Description |
| --- | --- |
| **MLAG** (Multi System Link Aggregation) | Combines links across multiple switches |
| **SPB** (Shortest Path Bridging) | Builds optimal, loop-free Layer 2 paths |
| **TRILL** (Transparent Interconnect of Lots of Links) | Layer 2 routing using IS-IS protocol |

### Modern Design Trends:

- Layer 3 routing used **everywhere except the access layer**.
- Redundant Layer 3 paths without blocked ports.
- **Faster convergence** and **better efficiency** compared to STP alone.

> 🔄 While STP remains essential at the access layer, Layer 3 designs dominate in large-scale modern networks.
> 

# Cisco CCNA 6.1 Notes

## 6.1.1 Link Aggregation

- **Link aggregation** solves the need for more bandwidth and redundancy without STP blocking links.
- **EtherChannel**:
    - Combines multiple physical links into **one logical link**.
    - Provides **fault-tolerance**, **load sharing**, **increased bandwidth**, and **redundancy**.

> 🔗 EtherChannel allows multiple cables to act like a single high-speed connection.
> 

---

## 6.1.2 EtherChannel Overview

- Developed by Cisco.
- Groups Fast Ethernet or Gigabit Ethernet ports into a **port channel**.
- The physical links are bundled into a **single virtual interface**.

---

## 6.1.3 Advantages of EtherChannel

| Benefit | Description |
| --- | --- |
| Simpler Management | Configure the EtherChannel interface once instead of individual ports. |
| Increased Bandwidth | Combines bandwidth of all links (e.g., 8x1Gbps = 8Gbps). |
| Load Balancing | Traffic spread across links (MAC-based or IP-based methods). |
| Redundancy | No spanning tree recalculation if a single link fails. |
| STP Efficiency | STP treats EtherChannel as **one logical link**. |

> 📈 EtherChannel improves network resiliency and performance.
> 

---

## 6.1.4 EtherChannel Implementation Restrictions

- Cannot mix different types of ports (e.g., no mixing Fast Ethernet and Gigabit Ethernet).
- Up to **8 links** can be bundled in a single EtherChannel.
- On Catalyst 2960 switches:
    - Supports up to **6 EtherChannels**.
- Consistency rules:
    - All member ports must have the same:
        - Speed
        - Duplex
        - VLAN information
        - Trunk or access mode

**Important**:

Configure EtherChannel settings on the **Port-Channel Interface** (affects all physical members).

---

## 6.1.5 AutoNegotiation Protocols

EtherChannel can be established in three ways:

- **PAgP** (Cisco-proprietary)
- **LACP** (IEEE 802.3ad / 802.1AX)
- **Static ("on" mode)** — No negotiation

---

## 6.1.6 PAgP Operation (Cisco Proprietary)

- **PAgP** automatically negotiates EtherChannel formation between Cisco devices.
- Sends PAgP packets every **30 seconds**.
- Checks for port consistency (speed, duplex, VLAN).

### PAgP Modes:

| Mode | Behavior |
| --- | --- |
| **on** | Forces EtherChannel without negotiation |
| **desirable** | Actively sends PAgP to form EtherChannel |
| **auto** | Listens but does not initiate |

**Tip**:

- `desirable` + `auto` = Form a channel.
- `auto` + `auto` = **No channel** forms!

---

## 6.1.7 PAgP Mode Settings Example

| S1 Mode | S2 Mode | Channel Formed? |
| --- | --- | --- |
| desirable | desirable | Yes |
| desirable | auto | Yes |
| auto | auto | No |
| on | on | Yes (manual, no PAgP) |

> ⚡ Both sides must be compatible for PAgP negotiation to succeed.
> 

---

## 6.1.8 LACP Operation (IEEE Standard)

- **LACP** (Link Aggregation Control Protocol) is **multi-vendor**.
- Negotiates bundle formation automatically.
- Supports **8 active** and **8 standby** links.

### LACP Modes:

| Mode | Behavior |
| --- | --- |
| **on** | Forces EtherChannel without negotiation |
| **active** | Actively sends LACP packets |
| **passive** | Listens but does not initiate |

**Tip**:

- `active` + `passive` = Channel forms.
- `passive` + `passive` = **No channel** forms!

---

## 6.1.9 LACP Mode Settings Example

| S1 Mode | S2 Mode | Channel Formed? |
| --- | --- | --- |
| active | active | Yes |
| active | passive | Yes |
| passive | passive | No |
| on | on | Yes (manual, no LACP) |

> 🌐 LACP is critical when working in mixed vendor environments.
> 

# Cisco CCNA 6.2 Notes

## 6.2.1 Configuration Guidelines

**Before configuring EtherChannel, follow these important rules:**

| Requirement | Description |
| --- | --- |
| EtherChannel Support | All Ethernet interfaces must support EtherChannel (no need for contiguous ports). |
| Speed and Duplex | All interfaces must have **the same speed and duplex** settings. |
| VLAN Match | Interfaces must be assigned to the **same VLAN** or be configured as **trunk ports**. |
| Allowed VLAN Range | In trunk mode, the allowed VLAN range must match across all interfaces. |

---

### Key Tips:

- Configure port settings **at the port-channel interface** (recommended).
- Changes to the **port-channel interface** affect all bundled ports.
- Changes made directly to physical interfaces can cause **inconsistencies**.

### EtherChannel Modes:

- Access mode
- Trunk mode (most common)
- Routed port mode (for Layer 3)

> 🛠️ Always configure EtherChannel settings consistently across all member ports!
> 

---

## 6.2.2 LACP Configuration Example

**Scenario**: Create an EtherChannel between S1 and S2 using **LACP**.

---

### Step-by-Step Configuration:

**Step 1: Select Interface Range**

```bash
interface range f0/1 - 2
```

- Selects multiple interfaces at once.

---

**Step 2: Create EtherChannel with LACP**

```bash
channel-group 1 mode active
```

- `1` is the **Port-Channel ID**.
- `mode active` enables **LACP** on these interfaces.

---

**Step 3: Configure Port-Channel Interface**

```bash
interface port-channel 1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
```

- Sets the port-channel as a **trunk**.
- Specifies **allowed VLANs** on the trunk.

---

✅ **Now the EtherChannel is up using LACP**, providing redundancy and increased bandwidth.

---

### Quick LACP Mode Recap:

| Mode | Action |
| --- | --- |
| active | Actively tries to form LACP EtherChannel |
| passive | Listens for LACP messages, responds but does not initiate |

# Cisco CCNA 6.3 Notes

## 6.3.1 Verify EtherChannel

After configuring EtherChannel, always verify your setup!

### Useful Verification Commands:

| Command | Purpose |
| --- | --- |
| `show interfaces port-channel` | Shows general status of the port channel. |
| `show etherchannel summary` | Displays all EtherChannels and their status. |
| `show etherchannel port-channel` | Shows detailed info about a specific port-channel. |
| `show interfaces etherchannel` | Shows the role of a specific physical interface within the EtherChannel. |

---

### Important Indicators:

- **SU** next to port-channel number:
    - **S** = Layer 2 EtherChannel
    - **U** = In use
- **Example**:
    - Port-Channel 1:
        - Bundles `FastEthernet0/1` and `FastEthernet0/2`
        - Uses LACP in **active mode**
        - Port-Channel is **operational** if configured correctly.

> 🔍 Always verify both the port-channel interface and the member interfaces!
> 

---

## 6.3.2 Common Issues with EtherChannel Configurations

| Issue | Description |
| --- | --- |
| VLAN mismatch | Ports must be in the same VLAN or all trunking with identical native VLANs. |
| Trunking mismatch | Some ports trunk, others don't — breaks EtherChannel formation. |
| Allowed VLAN mismatch | Trunked ports must allow the **same VLAN range**. |
| PAgP/LACP mismatch | Dynamic negotiation modes must match between both ends. |

---

> ⚡ Tip: Always configure EtherChannel first, then trunk settings if needed.
> 

**Reminder**:

- **PAgP** and **LACP**: Used for **link aggregation** (EtherChannel).
- **DTP**: Used to **automate trunking** (separate protocol).

---

## 6.3.3 Troubleshoot EtherChannel Example

### Scenario:

- `F0/1` and `F0/2` on switches **S1** and **S2** are supposed to form an EtherChannel.
- Problem: EtherChannel is **not operational**.

---

### Troubleshooting Steps:

**Step 1: View EtherChannel Summary**

```bash
show etherchannel summary
```

- Reveals EtherChannel is **down**.

---

**Step 2: View Port-Channel Configuration**

```bash
show run | begin interface port-channel
```

- Found **incompatible PAgP modes** on S1 and S2.

---

**Step 3: Correct the Misconfiguration**

- Change the PAgP mode to **desirable** on both sides.

**Important**:

- Remove and re-add the port-channel group when making changes to avoid STP errors.

---

**Step 4: Verify EtherChannel is Operational**

- After fixing:

```bash
show etherchannel summary
```

- Now shows EtherChannel is **up and operational**.

# Cisco CCNA 7.1 Notes

## 7.1.1 DHCPv4 Server and Client

- **DHCPv4 (Dynamic Host Configuration Protocol for IPv4)** automatically assigns:
    - IPv4 addresses
    - Subnet mask
    - Default gateway
    - Other network information
- **Deployment Options**:
    - **Dedicated DHCP Server**: Scalable, easier to manage for larger networks.
    - **Router DHCP Service**: Cisco routers can provide DHCP in small or SOHO networks.

### DHCP Lease:

- IPv4 addresses are **leased** for a defined period.
- Common lease durations:
    - **24 hours** to **1 week or more**.
- Clients must **renew** leases periodically.
- **Expired leases** return the address to the pool.

---

## 7.1.2 DHCPv4 Operation

- **Client/Server Model**:
    - DHCP server **leases** an IP address to a client.
    - Clients must **periodically contact** the DHCP server to extend leases.

> 🔄 Dynamic leasing ensures efficient use of IP addresses as devices move or disconnect.
> 

---

## 7.1.3 Steps to Obtain a Lease

When a client boots or joins a network, it follows **four steps**:

| Step | Description |
| --- | --- |
| **1. DHCP Discover (DHCPDISCOVER)** | Client broadcasts to find DHCP servers. |
| **2. DHCP Offer (DHCPOFFER)** | DHCP server offers an IP address and configuration info. |
| **3. DHCP Request (DHCPREQUEST)** | Client requests offered address (broadcasts acceptance). |
| **4. DHCP Acknowledgment (DHCPACK)** | Server confirms lease; client starts using IP address. |

---

### Step Details:

**1. DHCP Discover (DHCPDISCOVER)**:

- Client broadcasts a discovery message at Layer 2 and Layer 3.
- Contains the client’s MAC address.

**2. DHCP Offer (DHCPOFFER)**:

- Server reserves an IP address.
- Server sends an offer including:
    - IP address
    - Subnet mask
    - Gateway info
- Server creates an ARP entry for the client.

**3. DHCP Request (DHCPREQUEST)**:

- Client sends a **broadcast** to accept the offer.
- Tells all servers it accepted one offer and declined others.

**4. DHCP Acknowledgment (DHCPACK)**:

- Server verifies IP is free (optional ICMP ping).
- Server sends DHCPACK to confirm lease.
- Client does ARP lookup to ensure IP is not in use before finalizing.

---

## 7.1.4 Steps to Renew a Lease

**Two-step lease renewal process**:

| Step | Description |
| --- | --- |
| **1. DHCP Request (DHCPREQUEST)** | Client sends DHCPREQUEST to the original server. |
| **2. DHCP Acknowledgment (DHCPACK)** | Server responds with a DHCPACK to extend the lease. |

### Notes:

- If the DHCP server does not respond:
    - Client broadcasts a new DHCPREQUEST.
    - Other DHCP servers can offer a lease extension.
- DHCP communications can be sent as **unicast or broadcast** (per RFC 2131).

# Cisco CCNA 7.2 Notes

## 7.2.1 Cisco IOS DHCPv4 Server

- A Cisco router running IOS can be configured as a **DHCPv4 server**.
- The router:
    - Assigns IPv4 addresses.
    - Manages leases dynamically from **predefined pools**.

> 🛠️ Useful for small offices (SOHO) or branch offices without a dedicated DHCP server.
> 

---

## 7.2.2 Steps to Configure a Cisco IOS DHCPv4 Server

### Step 1: Exclude IPv4 Addresses

- Prevent specific addresses (like static IPs for routers, servers) from being assigned dynamically.

**Command Example**:

```bash
ip dhcp excluded-address <low-address> [high-address]
```

- Exclude a single address or a range.

---

### Step 2: Define a DHCPv4 Pool Name

- Create a pool with a recognizable name.

**Command Example**:

```bash
ip dhcp pool <pool-name>
```

- Enters **DHCP pool configuration mode**.

---

### Step 3: Configure the DHCPv4 Pool

Inside DHCP pool configuration:

| Command | Purpose |
| --- | --- |
| `network <network-address> <subnet-mask>` | Defines the address pool. |
| `default-router <gateway-ip>` | Sets default gateway. |
| `dns-server <dns-ip>` | (Optional) Sets DNS server. |
| `domain-name <domain>` | (Optional) Sets domain name. |
| `lease <days> <hours> <minutes>` | (Optional) Sets lease duration. |
| `netbios-name-server <NBNS-ip>` | (Optional) Sets NetBIOS name server. |

> ⚡ Microsoft recommends using DNS instead of NetBIOS/WINS.
> 

---

## 7.2.3 DHCPv4 Configuration Example

**Scenario**:

- Router R1 serves DHCPv4 for the **192.168.10.0/24** network.

**Configuration**:

```bash
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp pool LAN-POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
```

---

## 7.2.4 DHCPv4 Verification Commands

| Command | Purpose |
| --- | --- |
| `show running-config | section dhcp` |
| `show ip dhcp binding` | Lists all active DHCP leases. |
| `show ip dhcp server statistics` | Shows DHCP message statistics. |

---

## 7.2.5 Verify DHCPv4 is Operational

**Commands to verify**:

- **Check DHCP pool setup**:
    
    ```bash
    show running-config | section dhcp
    ```
    
- **Check DHCP bindings**:
    
    ```bash
    show ip dhcp binding
    ```
    
- **Check DHCP server statistics**:
    
    ```bash
    show ip dhcp server statistics
    ```
    
- **Verify IP configuration on PC**:
    
    ```bash
    ipconfig /all
    ```
    
- Forces renew on PC if needed:
    
    ```bash
    ipconfig /renew
    ```
    

---

## 7.2.7 Disable the Cisco IOS DHCPv4 Server

- **Disable DHCPv4 server**:
    
    ```bash
    no service dhcp
    ```
    
- **Re-enable DHCPv4 server**:
    
    ```bash
    service dhcp
    ```
    

> ⚠️ Clearing DHCP bindings or restarting DHCP may cause temporary duplicate IP addresses!
> 

---

## 7.2.8 DHCPv4 Relay

In large networks, clients may be on **different subnets** from DHCP servers.

- **Problem**:
    
    Routers **do not forward** DHCP broadcasts by default.
    
- **Solution**:
    
    Use the **ip helper-address** command to relay DHCP requests.
    

**Command Example**:

```bash
interface <interface-id>
ip helper-address <dhcp-server-ip>
```

- The router relays broadcasts as unicasts to the specified DHCP server.

---

### Example Steps:

**1. Release Current IP Address (on PC)**:

```bash
ipconfig /release
```

**2. Renew IP Address (after relay config)**:

```bash
ipconfig /renew
```

- If `ip helper-address` is configured correctly, PC obtains IP from DHCP server.
- **Verify**:
    
    ```bash
    ipconfig /all
    ```
    

---

## 7.2.9 Other Service Broadcasts Relayed

By default, **ip helper-address** also forwards these UDP services:

| Port | Service |
| --- | --- |
| 37 | Time |
| 49 | TACACS authentication |
| 53 | DNS |
| 67 | DHCP/BOOTP server |
| 68 | DHCP/BOOTP client |
| 69 | TFTP |
| 137 | NetBIOS name service |
| 138 | NetBIOS datagram service |

> 🌐 Routers can relay more than just DHCP — useful for enterprise network services.
> 

# Cisco CCNA 7.3 Notes

## 7.3.1 Cisco Router as a DHCPv4 Client

- Some Cisco routers (especially in **SOHO** or **branch offices**) need to **obtain an IPv4 address dynamically** from an ISP.
- The router acts **just like a computer DHCP client**.
- Typically used when:
    - Connecting to **cable** or **DSL modems**.
    - Accessing **dynamic public IP addresses** from an ISP.

**Command to configure** the interface as a DHCP client:

```bash
interface <interface-id>
ip address dhcp
no shutdown
```

---

### Example Scenario:

- ISP provides IP addresses from the `209.165.201.0/27` range.
- Router interface `G0/0/1` is configured as a DHCP client using:
    
    ```bash
    ip address dhcp
    ```
    
- **Verification**:
    
    ```bash
    show ip interface g0/0/1
    ```
    
    - Confirms:
        - Interface is **up**.
        - IP address assigned **via DHCP**.

---

## 7.3.2 Configuration Example

**Step-by-Step**:

```bash
interface g0/0/1
ip address dhcp
no shutdown
```

- This ensures the router interface:
    - Requests an IP address from the ISP.
    - Brings the interface up.
- **Verify address assignment**:
    
    ```bash
    show ip interface g0/0/1
    ```
    

---

## 7.3.3 Home Router as a DHCPv4 Client

- Most **consumer-grade routers** are **pre-configured** to obtain an IPv4 address from the ISP automatically.
- Setting is often labeled as:
    - **"Automatic Configuration - DHCP"**.

### Example:

- In Packet Tracer or real devices:
    - WAN interface set to **DHCP** mode.
    - Automatically requests and receives an IP address when connected to a modem.

**Note**:

- Different brands (Linksys, Netgear, TP-Link, etc.) may have slightly different UI, but the **principle is the same**.

> 🛠️ DHCPv4 client setup simplifies installation and connection to the internet without manual IP configuration.
> 

# Cisco CCNA 8.1 Notes

## 8.1.1 IPv6 Host Configuration

- To use **SLAAC** (Stateless Address Autoconfiguration) or **DHCPv6**, it's important to understand:
    - **Global Unicast Addresses (GUAs)**
    - **Link-Local Addresses (LLAs)**

### Manually Configuring IPv6 GUA on a Router:

```bash
interface <interface-id>
ipv6 address <ipv6-address>/<prefix-length>
```

### IPv6 on Windows Hosts:

- Can be **manually configured**.
- Or **dynamically acquired** (recommended to avoid errors).

> 🛠️ Most Windows machines default to dynamic IPv6 acquisition for ease and reliability.
> 

---

## 8.1.2 IPv6 Host Link-Local Address (LLA)

- **Link-Local Address (LLA)**:
    - Automatically created when the Ethernet interface is **activated**.
    - Used for **local link communication** only (not routable).
- Hosts use **ICMPv6 Router Advertisement (RA)** messages to learn how to obtain IPv6 addressing.

### Important Notes:

- If no router is present or configured:
    - Host will have only a **link-local address**.
    - No Global Unicast Address (GUA) assigned.
- **Windows Hosts**:
    - Might show an LLA with a **Zone ID** or **Scope ID** (e.g., `%5`).
    - This identifies the specific interface associated with the LLA.

> 🌐 LLA is always present, even if no router or DHCPv6 server is available.
> 

---

## 8.1.3 IPv6 GUA Assignment

IPv6 simplifies IP address assignment for hosts.

- By default, IPv6-enabled routers **advertise** network information.
- Hosts can then **dynamically create or acquire** IPv6 configurations using:
    - **Stateless methods** (SLAAC)
    - **Stateful methods** (DHCPv6)

### Key Points:

- Hosts **listen to RA messages**.
- Hosts then **decide** how to configure based on RA information.
- Some host operating systems can **override RA suggestions**.

> 🚀 IPv6 allows flexible, scalable network setup without manual IP assignment.
> 

---

## 8.1.4 Three RA Message Flags

The method by which a client obtains an IPv6 GUA depends on **three flags** in the Router Advertisement (RA):

| Flag | Name | Meaning |
| --- | --- | --- |
| **A flag** | Address Autoconfiguration | Use **SLAAC** to self-generate IPv6 GUA. |
| **O flag** | Other Configuration | Use **stateless DHCPv6** to get additional info (DNS, etc.). |
| **M flag** | Managed Address Configuration | Use **stateful DHCPv6** to get a full IPv6 address. |

---

### How the Flags Work:

- **Only A flag set**:
    - Host uses **SLAAC** only.
- **A + O flags set**:
    - Host uses **SLAAC** for IP address.
    - Host uses **stateless DHCPv6** for other info (e.g., DNS).
- **M flag set** (regardless of A and O):
    - Host uses **stateful DHCPv6** for complete IP assignment.

> 🎯 ICMPv6 RA messages guide the host, but the host makes the final decision on how to configure.
> 

# Cisco CCNA 8.2 Notes

## 8.2.1 SLAAC Overview

- **SLAAC (Stateless Address Autoconfiguration)** allows hosts to create their own **Global Unicast Address (GUA)** without a DHCPv6 server.
- **Stateless**: No server tracks IP address allocations.

**SLAAC Process**:

- Uses **ICMPv6 Router Advertisement (RA)** messages.
- Routers send RAs every **200 seconds**.
- Hosts can also send **Router Solicitation (RS)** messages to request an immediate RA.

> 🚀 SLAAC enables dynamic IPv6 addressing without needing DHCP infrastructure.
> 

---

## 8.2.2 Enabling SLAAC

**Basic Setup**:

- Router interface needs an IPv6 GUA and LLA.
- **Enable IPv6 routing** globally:
    
    ```bash
    ipv6 unicast-routing
    ```
    
- Router joins the **IPv6 all-routers multicast group (ff02::2)**.

**Verification Command**:

```bash
show ipv6 interface <interface-id>
```

- Displays IPv6 addresses and multicast group memberships.

---

## 8.2.3 SLAAC Only Method

- Enabled automatically when `ipv6 unicast-routing` is active.
- **Flags in the RA message**:
    - **A flag = 1** (Address Autoconfiguration)
    - **O flag = 0** (Other config not needed)
    - **M flag = 0** (No managed DHCP address needed)

**Result**:

- Host generates its own GUA using:
    - The prefix from RA.
    - A self-generated Interface ID (EUI-64 or random).

**Important**:

- Default gateway = LLA of the router (from RA message).
- DHCPv6 does **not** provide default gateway information.

---

## 8.2.4 ICMPv6 RS Messages

- Hosts don't always wait for a periodic RA.
- Upon boot, a host can send a **Router Solicitation (RS)** to **ff02::2** (all-routers multicast).
- Routers respond immediately with an **RA** to **ff02::1** (all-nodes multicast).

> 🛡️ RS messages ensure quicker IPv6 configuration when devices boot up.
> 

---

## 8.2.5 Host Process to Generate Interface ID

After receiving the network prefix from an RA, the host must generate a **64-bit Interface ID** using one of two methods:

| Method | Description |
| --- | --- |
| **Randomly generated** | Host randomly creates a 64-bit ID. (Windows 10 default method) |
| **EUI-64** | Host uses its MAC address, inserts `FFFE` in the middle, and flips the 7th bit. |

**Example**:

- **EUI-64** uses a MAC address to build a predictable interface ID.
- **Random generation** enhances **privacy** by not exposing MAC addresses.

> 🔒 Modern systems (Windows, Linux, macOS) prefer random Interface IDs for privacy.
> 

---

## 8.2.6 Duplicate Address Detection (DAD)

**Problem**:

- Even randomly or EUI-64 generated addresses might **conflict** with another device.

**Solution**:

- **Duplicate Address Detection (DAD)** ensures the address is unique.

**DAD Process**:

- Host sends an **ICMPv6 Neighbor Solicitation (NS)** to a **solicited-node multicast address** based on its IPv6 address.
- If no **Neighbor Advertisement (NA)** response is received, the address is assumed to be **unique**.

**Key Points**:

- DAD is recommended but **not mandatory** by the IETF.
- Almost all operating systems perform DAD **before finalizing any IPv6 address**.

> 🎯 DAD prevents IPv6 address conflicts automatically, keeping networks stable.
> 

# Cisco CCNA 8.3 Notes

## 8.3.1 DHCPv6 Operation Steps

This section explains **stateless** and **stateful** DHCPv6.

- **Stateless DHCPv6**:
    - Uses SLAAC to generate addresses.
    - DHCPv6 provides additional info (e.g., DNS).
- **Stateful DHCPv6**:
    - Provides full IPv6 addressing from the server.

**Key Points**:

- DHCPv6 is defined in **RFC 3315**.
- Server-to-client: UDP port **546**.
- Client-to-server: UDP port **547**.

---

### DHCPv6 Lease Process:

| Step | Action |
| --- | --- |
| 1 | Host sends **RS** (Router Solicitation). |
| 2 | Router sends **RA** (Router Advertisement) indicating DHCPv6 use. |
| 3 | Host sends **SOLICIT** to DHCPv6 multicast `ff02::1:2`. |
| 4 | DHCPv6 server responds with **ADVERTISE** message. |
| 5 | Host responds based on mode: • Stateless = INFORMATION-REQUEST • Stateful = REQUEST |
| 6 | Server sends **REPLY** message. |

---

### Step-by-Step Details:

- **Step 1**: Host (PC1) sends RS to all routers.
- **Step 2**: Router (R1) replies with RA indicating stateless or stateful DHCPv6.
- **Step 3**: Client sends SOLICIT to all-DHCPv6-servers address (`ff02::1:2`).
- **Step 4**: Server responds with ADVERTISE (unicast).
- **Step 5**:
    - Stateless: Host uses SLAAC, requests extra info via INFORMATION-REQUEST.
    - Stateful: Host requests full address info via REQUEST.
- **Step 6**: Server REPLY finalizes the lease.

> 🛡️ Default gateway is learned from the router’s Link-Local Address (LLA), not from DHCPv6.
> 

---

## 8.3.2 Stateless DHCPv6 Operation

- Stateless DHCPv6 only provides **additional configuration** (e.g., DNS servers).
- No tracking of address allocation by the server.

**Flags in RA**:

- **A = 1** → Use SLAAC for IPv6 address.
- **O = 1** → Get additional info from DHCPv6 server.

**Summary**:

- Host builds address with SLAAC.
- Host contacts DHCPv6 server for extras like DNS.

---

## 8.3.3 Enable Stateless DHCPv6 on an Interface

**Command**:

```bash
interface <interface-id>
ipv6 nd other-config-flag
```

- Sets the **O flag** to 1.

**To revert**:

```bash
no ipv6 nd other-config-flag
```

- Resets to SLAAC-only (O = 0).

> ⚙️ Use stateless DHCPv6 for networks where manual IP addressing is unnecessary, but DNS and other info are required.
> 

---

## 8.3.4 Stateful DHCPv6 Operation

- Provides **full IPv6 addressing** (similar to DHCPv4).
- Server **tracks** IPv6 leases (maintains client state).

**Flags in RA**:

- **M = 1** → Managed address configuration.
- **O = 0** → (Optional) No other info needed if full config is given.

**Host Actions**:

- Host sends SOLICIT to find a server.
- Receives IPv6 address, DNS info, etc., from the DHCPv6 server.

---

### Special Note:

- If **A = 1** and **M = 1**:
    - Some OS (e.g., Windows) **may use both** SLAAC and DHCPv6 addresses.
    - Recommended: manually set **A = 0** to avoid dual IP behavior.

---

## 8.3.5 Enable Stateful DHCPv6 on an Interface

**Commands**:

- Enable Managed DHCPv6:
    
    ```bash
    interface <interface-id>
    ipv6 nd managed-config-flag
    ```
    
- Disable SLAAC (optional for full DHCPv6 reliance):
    
    ```bash
    interface <interface-id>
    ipv6 nd prefix default no-autoconfig
    ```
    

**Summary**:

- M flag set to 1 → Use stateful DHCPv6.
- A flag set to 0 → Disable SLAAC.

# Cisco CCNA 8.4 Notes

## 8.4.1 DHCPv6 Router Roles

A Cisco IOS router can perform **three DHCPv6 roles**:

| Role | Description |
| --- | --- |
| DHCPv6 Server | Provides stateful or stateless DHCPv6 services. |
| DHCPv6 Client | Obtains IPv6 configuration from a DHCPv6 server. |
| DHCPv6 Relay Agent | Forwards DHCPv6 messages when server and client are on different networks. |

> 🛠️ No need for separate devices — Cisco routers are versatile for small and medium networks!
> 

---

## 8.4.2 Configure a Stateless DHCPv6 Server

**Stateless DHCPv6 Server Setup**:

### 5 Steps:

1. **Enable IPv6 Routing**:
    
    ```bash
    ipv6 unicast-routing
    ```
    
2. **Define a DHCPv6 Pool**:
    
    ```bash
    ipv6 dhcp pool <POOL-NAME>
    ```
    
3. **Configure DHCPv6 Pool**:
    - Provide **DNS server address**, **domain name**, and **other parameters**.
4. **Bind Pool to Interface**:
    
    ```bash
    interface <interface-id>
    ipv6 dhcp server <POOL-NAME>
    ipv6 nd other-config-flag
    ```
    
    - The `other-config-flag` tells clients to get **additional info** from DHCPv6.
5. **Verify Host Configuration**:
    - Use `ipconfig /all` on client.
    - Confirm GUA, default gateway (router LLA), DNS, and domain info.

> 🌐 Host creates its IPv6 address using SLAAC and gets DNS/domain info from DHCPv6.
> 

---

## 8.4.3 Configure a Stateless DHCPv6 Client (Router)

A router can act as a **stateless DHCPv6 client** too.

### 5 Steps:

1. **Enable IPv6 Routing**:
    
    ```bash
    ipv6 unicast-routing
    ```
    
2. **Create a Link-Local Address (LLA)**:
    
    ```bash
    interface <interface-id>
    ipv6 enable
    ```
    
3. **Enable SLAAC**:
    
    ```bash
    interface <interface-id>
    ipv6 address autoconfig
    ```
    
4. **Verify Assigned GUA**:
    
    ```bash
    show ipv6 interface brief
    ```
    
5. **Verify Other DHCPv6 Info**:
    
    ```bash
    show ipv6 dhcp interface <interface-id>
    ```
    

> 📈 Router now has SLAAC GUA + extra config learned via stateless DHCPv6.
> 

---

## 8.4.4 Configure a Stateful DHCPv6 Server

**Stateful DHCPv6** provides **full IPv6 addressing** to clients (like DHCPv4).

### 5 Steps:

1. **Enable IPv6 Routing**:
    
    ```bash
    ipv6 unicast-routing
    ```
    
2. **Define a DHCPv6 Pool**:
    
    ```bash
    ipv6 dhcp pool <POOL-NAME>
    ```
    
3. **Configure DHCPv6 Pool**:
    - Include **address prefix**, **DNS server**, **domain name**, etc.
4. **Bind Pool to Interface**:
    
    ```bash
    interface <interface-id>
    ipv6 dhcp server <POOL-NAME>
    ipv6 nd managed-config-flag
    ipv6 nd prefix default no-autoconfig
    ```
    
    - `managed-config-flag` sets **M flag** to 1.
    - `no-autoconfig` disables SLAAC.
5. **Verify Client Addressing**:
    - `ipconfig /all` shows IPv6 GUA assigned by DHCPv6 server.

> 🔥 Stateful DHCPv6 completely controls IPv6 addressing without SLAAC.
> 

---

## 8.4.5 Configure a Stateful DHCPv6 Client (Router)

A router can also be a **stateful DHCPv6 client**.

### 5 Steps:

1. **Enable IPv6 Routing**:
    
    ```bash
    ipv6 unicast-routing
    ```
    
2. **Create a Link-Local Address (LLA)**:
    
    ```bash
    interface <interface-id>
    ipv6 enable
    ```
    
3. **Use DHCPv6 to Obtain Address**:
    
    ```bash
    interface <interface-id>
    ipv6 address dhcp
    ```
    
4. **Verify Assigned GUA**:
    
    ```bash
    show ipv6 interface brief
    ```
    
5. **Verify Other DHCPv6 Info**:
    
    ```bash
    show ipv6 dhcp interface <interface-id>
    ```
    

---

## 8.4.6 DHCPv6 Server Verification Commands

| Command | Purpose |
| --- | --- |
| `show ipv6 dhcp pool` | Shows DHCPv6 pool details and active clients. |
| `show ipv6 dhcp binding` | Lists clients' link-local addresses and assigned GUAs. |
- **Stateful servers** maintain a binding database.
- **Stateless servers** do **not** maintain lease information.

> 🔍 Always verify DHCP bindings to confirm address assignment!
> 

---

## 8.4.7 Configure a DHCPv6 Relay Agent

When the DHCPv6 server is on a **different network**, use a **relay agent**.

**Relay Agent Command** (on client-facing interface):

```bash
interface <interface-id>
ipv6 dhcp relay destination <server-address> [egress-interface]
```

- **Egress Interface** needed if next hop is a Link-Local Address (LLA).

---

## 8.4.8 Verify the DHCPv6 Relay Agent

| Command | Purpose |
| --- | --- |
| `show ipv6 dhcp interface` | Confirms relay mode on client interface. |
| `show ipv6 dhcp binding` | Verifies DHCPv6 leases and address bindings. |
- Confirm DHCPv6 relay forwarding.
- Verify Windows clients using:
    
    ```bash
    ipconfig /all
    ```
    

> 📡 Proper relay agent configuration ensures devices on remote subnets get DHCPv6 services!
> 

# Cisco CCNA 9.0 – 9.1 Notes

## 9.0.1 Why Should I Take This Module?

- Your network is operational with Layer 2 redundancy and dynamic addressing.
- However, **if the default gateway router fails**, hosts cannot communicate outside their network.
- Without redundancy at Layer 3 (default gateway), users will be isolated and frustrated.

**Solution**:

- **First Hop Redundancy Protocols (FHRP)** prevent a single point of failure at the gateway.

**What You’ll Learn**:

- Understand FHRP.
- Discover types of FHRP (including Cisco's **HSRP**).
- Configure and verify HSRP via Packet Tracer.

> 🚀 FHRPs ensure your network remains reachable, even during failures.
> 

---

## 9.0.2 What Will I Learn to Do in This Module?

**Module Title**: FHRP Concepts

**Objective**:

- Explain how FHRPs provide **default gateway redundancy** in a network.

---

# 9.1 FHRP Concepts

## 9.1.1 Default Gateway Limitations

- **Issue**:
    - If the router or router interface acting as the default gateway fails, hosts are **stranded**.
    - Even if another router exists, hosts can’t automatically switch gateways.
- **In IPv4 networks**:
    - Clients are manually assigned a **single static default gateway**.
    - No dynamic failover without an FHRP.
- **In IPv6 networks**:
    - Clients get the default gateway dynamically via **ICMPv6 Router Advertisements**.
    - IPv6 with FHRP offers **faster failover**.

**Key Point**:

- Without FHRP, failure of a gateway results in **network isolation** for connected hosts.

> ⚠️ End devices don’t know about redundant routers unless an FHRP is implemented.
> 

---

## 9.1.2 Router Redundancy

- **Solution**: Use a **virtual router** shared by multiple physical routers.

### How it Works:

- Multiple routers share:
    - One **virtual IP address**.
    - One **virtual MAC address**.
- Hosts send traffic to the virtual router (gateway).
- Active router processes traffic transparently.
- If active router fails:
    - **Standby router** takes over **seamlessly**.

> 🔄 The transition between active and standby routers is invisible to hosts.
> 

---

### Important:

- This logic applies whether the device is a **router** or a **Layer 3 switch**.
- The goal is to **hide failover events** from end devices.

---

## 9.1.3 Steps for Router Failover

When the active router fails, FHRP ensures smooth transition:

| Step | Action |
| --- | --- |
| 1 | Standby router stops receiving Hello messages. |
| 2 | Standby router assumes the active forwarding role. |
| 3 | Standby router takes over the **IPv4 address** and **MAC address** of the virtual router. |
| 4 | Host devices experience **no disruption**. |

> 🛡️ FHRP enables high availability by maintaining network connectivity during router failures.
> 

---

## 9.1.4 FHRP Options

| FHRP Option | Description |
| --- | --- |
| **HSRP** (Hot Standby Router Protocol) | Cisco proprietary; most common FHRP. |
| **VRRP** (Virtual Router Redundancy Protocol) | Open standard equivalent of HSRP. |
| **GLBP** (Gateway Load Balancing Protocol) | Cisco proprietary; supports load balancing across gateways. |

**Choosing the right FHRP** depends on:

- Network size and design.
- Equipment vendor compatibility (Cisco or multi-vendor).
- Need for load balancing (GLBP).

> 📚 Mastering FHRP options is critical for designing resilient enterprise networks.
> 

# Cisco CCNA 9.2 Notes

## 9.2.1 HSRP Overview

- **HSRP (Hot Standby Router Protocol)** provides **first-hop redundancy** to prevent losing outside network access if the default router fails.
- Cisco proprietary FHRP designed for **transparent failover** of the default gateway.
- Ensures **high availability** by selecting:
    - One **active router** (forwards packets).
    - One **standby router** (takes over if the active fails).

**Key Points**:

- Standby router constantly **monitors** the active router.
- If the active router fails or meets preset failure conditions, the standby takes over **immediately**.

> 🚦 HSRP ensures continuous network operation even during router failures.
> 

---

## 9.2.2 HSRP Priority and Preemption

### HSRP Priority

- Determines **which router becomes the active router**.
- Router with the **highest priority** becomes active.
- **Default priority**: 100
- Range: **0 to 255**

**If priorities are equal**:

- The router with the **highest IPv4 address** wins.

**Command to set priority**:

```bash
standby <group-number> priority <value>
```

---

### HSRP Preemption

- By default, the active router stays active even if a higher priority router comes online.
- **Preemption** must be **enabled** to allow a higher-priority router to **take over**.

**Command to enable preemption**:

```bash
standby <group-number> preempt
```

### How Preemption Works:

| Event |  |
| --- | --- |
| R1 has higher priority (150) and preemption enabled. |  |
| R2 has lower priority (100). |  |
| R1 fails → R2 becomes active. |  |
| R1 comes back → R1 **reclaims** active role via preemption. |  |

> 🔄 Preemption ensures the router with the best priority is always the active router.
> 

---

**Note**:

- Preemption **only** allows takeover if priority is higher.
- Higher IP address **alone** is **not enough** without higher priority.

---

## 9.2.3 HSRP States and Timers

When HSRP is configured, a router interface goes through several states to determine its role.

| State | Description |
| --- | --- |
| **Initial** | HSRP not running yet on the interface. |
| **Learn** | Waiting to hear Hello messages to learn group IP address. |
| **Listen** | Knows virtual IP address, listening for Hello messages. |
| **Speak** | Sending and receiving HSRP Hello messages. |
| **Standby** | Ready to become active if needed. |
| **Active** | Currently forwarding packets for the virtual router. |

---

### HSRP Timers

- **Hello Timer**:
    - Interval for sending Hello messages.
    - **Default**: 3 seconds.
- **Hold Timer**:
    - Maximum time without Hello before standby takes over.
    - **Default**: 10 seconds.

### Important Timer Adjustments:

- You **can lower** timers to speed up failover.
- **Recommended minimums**:
    - Hello timer: **No lower than 1 second**.
    - Hold timer: **No lower than 4 seconds**.

> ⚡ Careful tuning of Hello and Hold timers improves network failover without overloading CPU.
> 

# Cisco CCNA 10.0 – 10.1 Notes

## 10.0.1 Why Should I Take This Module?

Welcome to **LAN Security Concepts**!

- In IT careers today, you’re not just building and maintaining networks—you are **responsible for securing them**.
- Network security is a **top priority** for architects and administrators.
- Many IT careers now specialize **exclusively** in network security.

**Key Questions**:

- What makes a LAN secure?
- What actions can threat actors take to compromise security?
- What can you do to defend the network?

> 🚀 This module is your first step into the essential world of network security!
> 

---

## 10.0.2 What Will I Learn in This Module?

**Module Title**: LAN Security Concepts

**Module Objective**:

- Explain how **vulnerabilities** compromise LAN security.

---

# 10.1 LAN Security Concepts

## 10.1.1 Network Attacks Today

Common types of attacks seen today:

| Attack Type | Description |
| --- | --- |
| **Distributed Denial of Service (DDoS)** | Coordinated attack from many devices (zombies) to degrade or halt access to websites and resources. |
| **Data Breach** | Attackers compromise servers/hosts to steal confidential information. |
| **Malware** | Malicious software (e.g., ransomware like WannaCry) infects hosts, causing data encryption and demanding ransom for access. |

> 🛡️ Threats are real, constant, and evolving!
> 

---

## 10.1.2 Network Security Devices

Essential network perimeter security devices:

| Device | Function |
| --- | --- |
| **VPN-enabled Router** | Provides secure remote connections over public networks; may include firewall integration. |
| **Next-Generation Firewall (NGFW)** | Stateful inspection, application control, NGIPS, malware protection, URL filtering. |
| **Network Access Control (NAC)** | Manages user/device access with authentication, authorization, and accounting (AAA); Cisco ISE is a common NAC solution. |

> 🔒 Layered defenses create strong protection against external threats.
> 

---

## 10.1.3 Endpoint Protection

- **LAN devices** (switches, WLCs, APs) interconnect endpoints.
- Threats can come from **internal hosts** if compromised.
- **Endpoints** include:
    - Laptops, desktops, servers, IP phones, BYOD devices.

**Security Solutions**:

- Traditional: Antivirus, host-based firewalls, HIPS (Host Intrusion Prevention Systems).
- Modern:
    - **Network Access Control (NAC)**
    - **Advanced Malware Protection (AMP)** (e.g., Cisco AMP for Endpoints)
    - **Email Security Appliances (ESA)**
    - **Web Security Appliances (WSA)**

> 🎯 A combination of endpoint and network protections is critical for comprehensive security.
> 

---

## 10.1.4 Cisco Email Security Appliance (ESA)

- **Content security appliances** offer fine-grained email and web control.

**Email Threats**:

- According to Cisco Talos (June 2019):
    - **85%** of all email is spam.
- **Phishing**:
    - Entices users to click malicious links or attachments.
- **Spear Phishing**:
    - Targets high-value individuals (executives, admins).

**Cisco ESA Features**:

- Blocks known threats.
- Remediates stealth malware.
- Discards emails with bad links.
- Blocks access to newly infected websites.
- Encrypts outgoing emails to prevent data loss.

**ESA Mechanism**:

- Pulls real-time threat intelligence from Cisco Talos every 3–5 minutes.

> 📬 ESA acts as your first line of defense against email-borne attacks.
> 

---

## 10.1.5 Cisco Web Security Appliance (WSA)

- **WSA** mitigates web-based threats.

**Features**:

- Advanced malware protection.
- Application visibility and control.
- Acceptable use policy enforcement.
- Detailed reporting and analytics.

**Traffic Control**:

- Allow, restrict, or block services like:
    - Chat
    - Messaging
    - Video
    - Audio
- URL blacklisting and filtering.
- Web traffic encryption/decryption.

**How WSA Works**:

- A user tries to access a website.
- The firewall forwards the request to WSA.
- WSA evaluates the URL:
    - If blacklisted, the packet is dropped.
    - An access denied message is sent to the user.

> 🌐 WSA ensures safe and compliant internet access for all users.
> 

# Cisco CCNA 10.2 Notes

## 10.2.1 Authentication with a Local Password

- **Simplest authentication** method:
    - Configure a login and password on **console, VTY lines**, and **AUX ports**.

### Example (VTY lines setup):

```bash
R1(config)# line vty 0 4
R1(config-line)# password ci5c0
R1(config-line)# login
```

**Weaknesses**:

- Password sent in **plaintext**.
- No accountability (anyone with the password can log in).

---

### SSH for More Secure Access

- **SSH** provides:
    - Username and password encryption.
    - Local database authentication.
    - User accountability (logs usernames).

**SSH + Local User Database Example**:

```bash
R1(config)# ip domain-name example.com
R1(config)# crypto key generate rsa general-keys modulus 2048
R1(config)# username Admin secret Str0ng3rPa55w0rd
R1(config)# ip ssh version 2
R1(config)# line vty 0 4
R1(config-line)# transport input ssh
R1(config-line)# login local
```

> 🛡️ SSH is strongly recommended for remote access.
> 

---

**Limitations of Local Database Authentication**:

- Must configure user accounts **on every device** individually.
- **No fallback** if username/password is forgotten.
- Best for **small networks**.

---

## 10.2.2 AAA Components

**AAA** = **Authentication, Authorization, Accounting**

| Component | Function |
| --- | --- |
| **Authentication** | Verifies user identity. |
| **Authorization** | Defines what users are allowed to do. |
| **Accounting** | Tracks and records user activity. |

> 💳 Similar to a credit card system: verify user, limit spending, track purchases.
> 

---

## 10.2.3 Authentication

Two common AAA authentication types:

### 1. Local AAA Authentication

- **Usernames and passwords** stored **locally** on each network device.
- Suitable for **small networks**.

### 2. Server-Based AAA Authentication

- Router uses a **central AAA server**.
- Communicates with AAA server via **RADIUS** or **TACACS+**.
- Ideal for **large enterprise networks**.

---

**Authentication Flow**:

| Step | Local AAA | Server-Based AAA |
| --- | --- | --- |
| 1 | Client connects to router. | Client connects to router. |
| 2 | Router prompts for credentials. | Router prompts for credentials. |
| 3 | Router authenticates locally. | Router forwards to AAA server for authentication. |
| 4 | Access granted based on credentials. | Access granted based on server response. |

---

## 10.2.4 Authorization

- After authentication, **authorization** controls **what the user can do**.
- Uses user attributes stored on the AAA server.

**Authorization Flow**:

- Client authenticates successfully.
- Router requests service authorization from AAA server.
- AAA server responds PASS/FAIL based on permissions.

> 🎯 Authorization defines roles, permissions, and access levels.
> 

---

## 10.2.5 Accounting

- **AAA accounting** tracks network usage and user actions.
- Useful for:
    - **Auditing**
    - **Troubleshooting**
    - **Billing**

**Accounting Records Include**:

- Connection start/stop times.
- Executed commands.
- Packet and byte counts.
- Usernames, timestamps, and detailed logs.

**Typical Process**:

- Authentication triggers a **start** accounting message.
- Disconnect triggers a **stop** accounting message.

> 🔍 Accounting provides detailed evidence of user activity — critical for security and auditing.
> 

---

## 10.2.6 802.1X

**IEEE 802.1X** is a **port-based authentication** standard.

- Restricts unauthorized devices from connecting to the LAN.
- Uses a **central authentication server** (e.g., Cisco ISE).

### 802.1X Roles:

| Role | Description |
| --- | --- |
| **Client (Supplicant)** | Device requesting network access (wired or wireless). |
| **Switch (Authenticator)** | Enforces access control between client and network. |
| **Authentication Server** | Validates the client’s identity and authorizes network access. |

---

**802.1X Process**:

- Client connects to switch.
- Switch requests credentials.
- Credentials are forwarded to the authentication server.
- Server grants or denies network access.

> 🛡️ 802.1X strengthens LAN security by enforcing user/device authentication at the network edge.
> 

# Cisco CCNA 10.3 Notes

## 10.3.1 Layer 2 Vulnerabilities

- Previous topics focused on securing endpoints.
- Now the focus shifts to **Layer 2 security** (Data Link Layer and switching).

### Why Layer 2 Security Matters:

- OSI Model divides networking into 7 layers; each layer is vulnerable independently.
- Most network security tools (VPNs, firewalls, IPS) focus on **Layer 3–7**.
- If **Layer 2** is compromised:
    - All upper-layer security becomes ineffective.
    - Attackers capturing Layer 2 frames can bypass higher-layer protections.

> 🚨 Securing Layer 2 is critical to maintaining the entire network's security posture.
> 

---

## 10.3.2 Switch Attack Categories

### Why Layer 2 Is Vulnerable:

- LANs were traditionally **trusted** environments.
- Modern networks (BYOD, remote work) introduce **untrusted devices**.
- Sophisticated attackers now target **Layer 2 weaknesses**.

### Common Layer 2 Attack Types:

| Attack Type | Description |
| --- | --- |
| **MAC Address Table Attack** | Overload the switch’s MAC table to make it flood all frames (like a hub). |
| **VLAN Attacks** | Exploit VLAN configurations to hop between VLANs. |
| **DHCP Attacks** | Rogue DHCP servers assigning incorrect IP configs. |
| **ARP Attacks** | ARP spoofing or poisoning to intercept network traffic. |
| **STP Attacks** | Manipulate Spanning Tree Protocol to take over network topology. |

> 🔥 A single compromised switch can severely impact the whole LAN.
> 

---

## 10.3.3 Switch Attack Mitigation Techniques

Cisco provides several strategies to **mitigate Layer 2 attacks**:

| Attack | Mitigation Technique |
| --- | --- |
| **MAC Address Table Attack** | Use **port security** to limit MAC addresses allowed per port. |
| **VLAN Attack** | Disable **Dynamic Trunking Protocol (DTP)**; manually configure trunks; use **VLAN hopping defenses**. |
| **DHCP Attack** | Deploy **DHCP Snooping** to allow only trusted ports to offer DHCP services. |
| **ARP Attack** | Implement **Dynamic ARP Inspection (DAI)** to validate ARP packets against trusted databases. |
| **STP Attack** | Use **BPDU Guard** and **Root Guard** to protect STP topology. |

---

### Important Management Protocol Security Tips:

- Many common management protocols (Syslog, SNMP, TFTP, Telnet, FTP) are **insecure by default**.

**Recommended Actions**:

- Always use **secure variants**:
    - SSH instead of Telnet
    - SCP or SFTP instead of FTP
    - SNMPv3 instead of SNMPv1/v2
    - HTTPS (SSL/TLS) instead of HTTP
- Consider an **out-of-band management network** dedicated only to device management.
- Use a **dedicated management VLAN** (separated from user data traffic).
- Apply **Access Control Lists (ACLs)** to **filter** and **restrict** management access.

> 🛡️ Securing management access is just as important as securing data plane traffic.
> 

# Cisco CCNA 10.4 Notes

## 10.4.1 Switch Operation Review

- Layer 2 switches use a **MAC address table** to forward frames efficiently.
- The switch learns MAC addresses from incoming frames and stores them in memory.

**Command Example**:

```bash
show mac address-table dynamic
```

| VLAN | MAC Address | Type | Port |
| --- | --- | --- | --- |
| 1 | 0001.9717.22e0 | DYNAMIC | Fa0/4 |
| 1 | 000a.f38e.74b3 | DYNAMIC | Fa0/1 |
| 1 | 0090.0c23.ceca | DYNAMIC | Fa0/3 |
| 1 | 00d0.ba07.8499 | DYNAMIC | Fa0/2 |

> 📖 Switches rely heavily on accurate MAC address tables for forwarding decisions.
> 

---

## 10.4.2 MAC Address Table Flooding

- **MAC tables have a finite size**.
- **MAC Flooding Attack**:
    - Attacker bombards the switch with **fake source MAC addresses**.
    - MAC address table fills up.
    - Switch cannot learn new addresses → floods all frames out all ports.
- **Impact**:
    - Traffic intended for one host gets broadcast to all.
    - Attacker captures traffic using packet sniffers.

---

### Attack Scenario Example:

- Threat actor uses the tool **macof** to generate thousands of random MAC addresses rapidly.
- When MAC table is full:
    - Switch behaves like a **hub**, flooding all frames.
    - Traffic is exposed within the same **VLAN** only.

**Important**:

- If the attack stops, the switch will **age out** old entries and restore normal operation.

> 🔥 MAC flooding exposes confidential traffic and degrades performance.
> 

---

## 10.4.3 MAC Address Table Attack Mitigation

**Why It’s Dangerous**:

- A tool like **macof** can flood a switch with **8,000 bogus frames per second**.
- Example:
    - Catalyst 6500 switch supports 132,000 MAC entries.
    - Attack fills it up in **seconds**.

**Sample macof Output**:

```bash
# macof -i eth1
36:a1:48:63:81:70 15:26:8d:4d:28:f8 0.0.0.0.26413 > 0.0.0.0.49492: S ...
16:e8:08:00:4d:9c da:4d:bc:7c:ef:be 0.0.0.0.61376 > 0.0.0.0.47523: S ...
...
```

---

### Mitigation Strategy:

**Use Port Security**:

- Restrict the **number of MAC addresses** allowed on a port.
- Only allows known, trusted MAC addresses.
- Prevents the switch from learning unlimited bogus addresses.

> 🛡️ Port security is a fundamental defense against MAC flooding attacks.
> 

# Cisco CCNA 10.5 Notes

## 10.5.1 VLAN and DHCP Attack Overview

This section explores common **Layer 2 attacks** targeting VLANs and DHCP services and the mitigation techniques used to prevent them.

---

## 10.5.2 VLAN Hopping Attacks

- **VLAN hopping** allows a threat actor to send traffic between VLANs **without a router**.
- **Method**:
    - Attacker configures their host to **act like a switch**.
    - Spoofs **802.1Q** and **DTP** messages.
    - Forms an unauthorized **trunk link** with the switch.
    - Gains access to traffic across **multiple VLANs**.

---

## 10.5.3 VLAN Double-Tagging Attack

- A **double-tagging attack** embeds **two VLAN tags** in a frame.
- **Steps**:
    1. Attacker sends frame with outer VLAN tag matching the **native VLAN** (e.g., VLAN 10) and inner VLAN tag for **target VLAN** (e.g., VLAN 20).
    2. First switch strips off the **outer VLAN 10 tag** and forwards frame over the trunk.
    3. Second switch reads the **inner VLAN 20 tag** and forwards the frame to VLAN 20 devices.
- **Limitation**:
    
    Only works if attacker is on the **same VLAN** as the **native VLAN**.
    

---

### VLAN Attack Mitigation

- Disable trunking on all access ports:
    
    ```bash
    switchport mode access
    ```
    
- Disable DTP auto trunking:
    
    ```bash
    switchport nonegotiate
    ```
    
- Ensure native VLANs are only used on trunk links.
- Do not allow user devices on the native VLAN.

> 🛡️ Manual trunk configuration + isolating the native VLAN = strong VLAN security.
> 

---

## 10.5.4 DHCP Messages Recap

**Standard DHCPv4 exchange** (DORA):

| Step | Description |
| --- | --- |
| Discover | Client broadcasts DHCP Discover. |
| Offer | DHCP server unicasts an Offer. |
| Request | Client broadcasts acceptance of the Offer. |
| Acknowledge | Server unicasts an Acknowledgment to finalize lease. |

---

## 10.5.5 DHCP Attacks

Two common DHCP attacks:

### 1. DHCP Starvation

- Attacker floods the DHCP server with **bogus DHCP requests** using **random MAC addresses**.
- DHCP pool is exhausted — no IP addresses left for legitimate clients.

### 2. DHCP Spoofing

- Attacker sets up a **rogue DHCP server**.
- Clients accept IP settings (gateway, DNS) from the attacker.
- Enables **man-in-the-middle (MITM)** attacks and redirects traffic.

---

### DHCP Attack Mitigation

- Implement **DHCP Snooping**:
    - Trust only authorized ports.
    - Block rogue DHCP servers.

---

## 10.5.6 ARP, STP, and CDP Attacks

### ARP Attacks (Gratuitous ARP Spoofing)

- Attacker sends **unsolicited ARP replies** with **spoofed MAC addresses**.
- Updates ARP caches incorrectly → enables **MITM attacks**.

> Example:
> 
> 
> PC2 claims to be both the default gateway and PC1.
> 

---

### STP Attacks

- Attacker sends **fake BPDUs** with a **low bridge priority**.
- Forces network switches to **elect the attacker** as the **root bridge**.
- Manipulates traffic flow for interception.

---

### CDP Reconnaissance

- **Cisco Discovery Protocol (CDP)** broadcasts:
    - Device IP address
    - IOS version
    - Hardware platform
- Unencrypted and periodic broadcasts.
- Attackers can **map the network** and find vulnerabilities.

---

### CDP and LLDP Mitigation

- Disable CDP globally:
    
    ```bash
    no cdp run
    ```
    
- Disable CDP per interface:
    
    ```bash
    no cdp enable
    ```
    
- Disable LLDP globally:
    
    ```bash
    no lldp run
    ```
    
- Disable LLDP per interface:
    
    ```bash
    no lldp transmit
    no lldp receive
    ```
    

---

## 10.5.7 ARP Attacks in Depth

- **Gratuitous ARP**:
    - Allows attackers to **poison ARP tables** by announcing fake MAC-IP mappings.
- Leads to:
    - MITM attacks
    - Network traffic interception

**Mitigation**:

- Enable **Dynamic ARP Inspection (DAI)** to validate ARP packets.

---

## 10.5.8 Address Spoofing Attacks

| Type | Description |
| --- | --- |
| **IP Spoofing** | Attacker uses a stolen or random IP address. |
| **MAC Spoofing** | Attacker uses a spoofed MAC address to hijack legitimate traffic. |
- Spoofed frames trick the switch into forwarding traffic to the attacker.

**Mitigation**:

- Use **IP Source Guard (IPSG)**:
    - Maps MAC and IP addresses to trusted ports.
    - Drops unmatched traffic.

---

## 10.5.9 STP Attack Mitigation

- **Spanning Tree Protocol (STP)** can be attacked by manipulating BPDUs.
- **Solution**:
    - Implement **BPDU Guard** on all access ports:
        
        ```bash
        spanning-tree bpduguard enable
        ```
        

---

## 10.5.10 CDP Reconnaissance Details

- CDP reveals too much info (IP, platform, IOS version).
- **Mitigation**:
    - Disable CDP where not needed.
    - Carefully control device discovery traffic.

> ⚡ Limiting CDP and LLDP protects network topology information from attackers.
> 

# Cisco CCNA 11.0 – 11.1 Notes

## 11.0.1 Why Should I Take This Module?

- **Network security** isn’t only about protecting from external threats.
- **Internal threats** are real:
    - Innocent errors (e.g., employees adding switches).
    - Malicious attacks (e.g., disgruntled insiders).
- As a network professional, **you are responsible** for securing the LAN **internally**.

> 🛡️ This module introduces how to secure switches against internal vulnerabilities.
> 

---

## 11.0.2 What Will I Learn in This Module?

**Module Title**: Switch Security Configuration

**Objective**:

- Configure switch security to **mitigate LAN attacks**.

---

# 11.1 Switch Security

## 11.1.1 Secure Unused Ports

- **Best Practice**: Disable all **unused switch ports**.
- **Why**: Prevent unauthorized device connections.

**Command Example**:

```bash
Switch(config)# interface range fa0/8 - 24
Switch(config-if-range)# shutdown
```

- Ports can be reactivated later with `no shutdown`.

> 🔒 Disabled ports reduce internal attack surface.
> 

---

## 11.1.2 Mitigate MAC Address Table Attacks

- **MAC Flooding Attack**: Overloads switch MAC table → switch floods frames like a hub.
- **Mitigation**: Use **Port Security**.

**Port Security**:

- Limits **number of valid MAC addresses** on a port.
- Optionally allow:
    - Static configuration.
    - Dynamic learning (with limits).

---

## 11.1.3 Enable Port Security

### Step-by-Step:

```bash
S1(config)# interface f0/1
S1(config-if)# switchport mode access
S1(config-if)# switchport port-security
```

**Verification**:

```bash
S1# show port-security interface f0/1
```

**Key Outputs**:

- **Secure-down**: No device attached.
- **Secure-up**: Device attached.
- **Violation Mode**: Shutdown (default).

> 🚀 Always make ports access ports first (switchport mode access) before applying port security.
> 

---

## 11.1.4 Limit and Learn MAC Addresses

- **Limit MAC addresses**:
    
    ```bash
    Switch(config-if)# switchport port-security maximum 2
    ```
    
- **Manually assign MAC addresses**:
    
    ```bash
    Switch(config-if)# switchport port-security mac-address aaaa.bbbb.1234
    ```
    
- **Dynamically learn and "stick" MAC addresses**:
    
    ```bash
    Switch(config-if)# switchport port-security mac-address sticky
    ```
    

**Verification Commands**:

```bash
show port-security interface fa0/1
show port-security address
```

> 📋 Sticky addresses can be saved to running-config for persistence.
> 

---

## 11.1.5 Port Security Aging

- Remove secure MAC addresses after a set time:
    - **Absolute Aging**: Delete after set time.
    - **Inactivity Aging**: Delete if no traffic within set time.

**Configuration**:

```bash
Switch(config-if)# switchport port-security aging time 10
Switch(config-if)# switchport port-security aging type inactivity
```

> ⏳ Aging helps recycle secure MAC slots automatically.
> 

---

## 11.1.6 Port Security Violation Modes

| Mode | Behavior |
| --- | --- |
| **Protect** | Drops violating traffic silently. |
| **Restrict** | Drops traffic + increments violation counter. |
| **Shutdown** | (Default) Disables the port (err-disabled state). |

**Set Violation Mode**:

```bash
Switch(config-if)# switchport port-security violation restrict
```

> 🚦 "Shutdown" mode is safest but most disruptive.
> 

---

## 11.1.7 Ports in Error-Disabled State

- A port transitions to **err-disabled** if a security violation occurs under **shutdown mode**.

**Recovery**:

1. Admin investigates cause.
2. Use `shutdown` then `no shutdown` commands on the interface:

```bash
Switch(config)# interface fa0/1
Switch(config-if)# shutdown
Switch(config-if)# no shutdown
```

**Symptoms**:

- Interface shows `err-disabled`.
- Port LED is off.

> 🛠️ Always investigate before re-enabling!
> 

---

## 11.1.8 Verify Port Security

**Global Summary**:

```bash
show port-security
```

**Interface-Specific**:

```bash
show port-security interface fa0/1
```

**View MAC Addresses**:

```bash
show port-security address
```

**Sticky MAC in Running Config**:

```bash
show run interface fa0/1
```

> 🔍 Always verify secure MAC learning and port states after configuring security.
> 

# Cisco CCNA 11.2 – 11.3 Notes

## 11.2 VLAN Attacks

### 11.2.1 VLAN Attacks Review

Three types of **VLAN hopping attacks**:

| Attack Type | Description |
| --- | --- |
| **Spoofing DTP** | Host pretends to be a switch, causing the real switch to form a trunk, allowing VLAN hopping. |
| **Rogue Switch** | Attacker introduces their own switch to form trunk links and access all VLANs. |
| **Double-Tagging** | Frame carries two VLAN tags. Switch strips first tag (native VLAN) and forwards using the second tag (target VLAN). |

> 🚨 VLAN hopping allows an attacker to access multiple VLANs without routing.
> 

---

### 11.2.2 Steps to Mitigate VLAN Hopping Attacks

| Step | Action |
| --- | --- |
| 1 | Disable DTP on non-trunk ports: `switchport mode access` |
| 2 | Disable unused ports and assign them to an unused VLAN. |
| 3 | Manually configure trunk links: `switchport mode trunk` |
| 4 | Disable DTP on trunk ports: `switchport nonegotiate` |
| 5 | Change native VLAN on trunks to a different VLAN (not VLAN 1): `switchport trunk native vlan <vlan-id>` |

---

**Example VLAN Hopping Mitigation Configuration**:

```bash
S1(config)# interface range fa0/1 - 16
S1(config-if-range)# switchport mode access
S1(config-if-range)# exit

S1(config)# interface range fa0/17 - 20
S1(config-if-range)# switchport mode access
S1(config-if-range)# switchport access vlan 1000
S1(config-if-range)# shutdown
S1(config-if-range)# exit

S1(config)# interface range fa0/21 - 24
S1(config-if-range)# switchport mode trunk
S1(config-if-range)# switchport nonegotiate
S1(config-if-range)# switchport trunk native vlan 999
S1(config-if-range)# end
```

> 🛡️ Separate user VLANs from trunk-native VLANs and lock down unused ports to reduce attack vectors.
> 

---

## 11.3 DHCP Attacks

### 11.3.1 DHCP Attack Review

| Attack Type | Description |
| --- | --- |
| **DHCP Starvation** | Exhausts DHCP server's address pool by flooding it with fake DHCP requests. |
| **DHCP Spoofing** | Rogue server offers fake IP settings to clients, redirecting traffic. |

**DHCP Starvation**:

- Tools like **Gobbler** automate attacks.
- Mitigation: **Port security** limits MAC addresses per port.

**DHCP Spoofing**:

- Harder to block because it may use real MAC addresses.
- Mitigation: **DHCP Snooping**.

---

### 11.3.2 DHCP Snooping

- Inspects DHCP traffic.
- Allows DHCP messages only from **trusted ports**.
- Blocks DHCP traffic from **untrusted ports** (e.g., user access ports).

**Trust Definitions**:

- **Trusted Ports**: Ports connected to official DHCP servers or uplinks.
- **Untrusted Ports**: Ports connected to end devices.

**DHCP Snooping Binding Table**:

- Tracks MAC address ↔ IP address relationships learned from DHCP assignments.

---

### 11.3.3 Steps to Implement DHCP Snooping

| Step | Action |
| --- | --- |
| 1 | Enable DHCP snooping: `ip dhcp snooping` |
| 2 | Trust specific ports: `ip dhcp snooping trust` |
| 3 | Set rate limits for untrusted ports: `ip dhcp snooping limit rate <rate>` |
| 4 | Enable snooping on specific VLANs: `ip dhcp snooping vlan <vlan-id>` |

---

### 11.3.4 DHCP Snooping Configuration Example

**Reference Scenario**:

- F0/5 = Untrusted port (PC connection).
- F0/1 = Trusted port (DHCP server connection).

**Configuration**:

```bash
S1(config)# ip dhcp snooping
S1(config)# interface f0/1
S1(config-if)# ip dhcp snooping trust
S1(config-if)# exit

S1(config)# interface range f0/5 - 24
S1(config-if-range)# ip dhcp snooping limit rate 6
S1(config-if-range)# exit

S1(config)# ip dhcp snooping vlan 5,10,50-52
S1(config)# end
```

---

**Verification Commands**:

- Check DHCP snooping status:
    
    ```bash
    show ip dhcp snooping
    ```
    
- View DHCP snooping bindings:
    
    ```bash
    show ip dhcp snooping binding
    ```
    

**Example Output**:

```
Switch DHCP snooping is enabled
DHCP snooping is configured on VLANs: 5,10,50-52

Interface         Trusted   Rate Limit (pps)
----------------- --------- ----------------
FastEthernet0/1    yes       unlimited
FastEthernet0/5    no        6
...
```

**Binding Table Example**:

```
MacAddress        IpAddress        Lease(sec) Type           VLAN Interface
00:03:47:B5:9F:AD 192.168.10.11     193185     dhcp-snooping   5    FastEthernet0/5
```

> 🔒 DHCP snooping not only blocks rogue DHCP servers but also builds security foundations for further protection (e.g., Dynamic ARP Inspection).
> 

# Cisco CCNA 11.4 Notes

## 11.4.1 Dynamic ARP Inspection (DAI)

- **ARP attacks** involve sending unsolicited ARP replies to corrupt ARP tables.
- **DAI** protects against ARP spoofing and poisoning by:

| DAI Actions | Description |
| --- | --- |
| Intercepts all ARP Requests and Replies on untrusted ports. |  |
| Verifies ARP packets against the DHCP snooping binding table. |  |
| Drops and logs invalid or gratuitous ARP packets. |  |
| Error-disables the interface if the ARP rate limit is exceeded. |  |

> 🛡️ DAI ensures that only valid, trusted ARP communications occur on the network.
> 

---

## 11.4.2 DAI Implementation Guidelines

To properly deploy DAI:

| Step | Action |
| --- | --- |
| 1 | Enable DHCP snooping globally. |
| 2 | Enable DHCP snooping on selected VLANs. |
| 3 | Enable DAI on selected VLANs. |
| 4 | Configure trusted interfaces for DHCP and ARP inspection. |

**Best Practice**:

- **Access ports** = Untrusted
- **Uplink ports** (to switches, routers) = Trusted

---

## 11.4.3 DAI Configuration Example

**Scenario**: S1 connects two PCs in VLAN 10.

**Configuration**:

```bash
S1(config)# ip dhcp snooping
S1(config)# ip dhcp snooping vlan 10
S1(config)# ip arp inspection vlan 10
S1(config)# interface fa0/24
S1(config-if)# ip dhcp snooping trust
S1(config-if)# ip arp inspection trust
```

**Explanation**:

- DHCP snooping enabled for VLAN 10.
- DAI enabled for VLAN 10.
- Interface `fa0/24` (uplink) trusted for DHCP and ARP.

---

## 11.4.4 DAI Validation Methods

DAI can be configured to validate:

| Validation Type | Description |
| --- | --- |
| **Destination MAC** | Ethernet destination MAC matches ARP body target MAC. |
| **Source MAC** | Ethernet source MAC matches ARP body sender MAC. |
| **IP Address** | Checks for invalid or unexpected IPs (e.g., 0.0.0.0, multicast addresses). |

**Command to configure validation**:

```bash
S1(config)# ip arp inspection validate src-mac dst-mac ip
```

**Important**:

- Use **one command line** to specify multiple validations.
- Entering a new `ip arp inspection validate` command **overwrites** the previous one.

**Example Validation Setup**:

```bash
S1(config)# ip arp inspection validate src-mac dst-mac ip
S1(config)# do show run | include validate
ip arp inspection validate src-mac dst-mac ip
```

> 🎯 DAI validation enhances security by ensuring ARP packets match expected MAC and IP information.
> 

# Cisco CCNA 11.5 Notes

## 11.5.1 PortFast and BPDU Guard

- **STP Manipulation Attacks**:
    - Attackers spoof BPDUs to become the root bridge and disrupt network topology.

**Mitigation Techniques**:

| Feature | Description |
| --- | --- |
| **PortFast** | Immediately brings an access port to the forwarding state, skipping STP listening and learning phases. |
| **BPDU Guard** | Shuts down a port that receives unexpected BPDUs, putting it into an error-disabled state. |

> 🛡️ Use PortFast and BPDU Guard on all end-user ports, not on switch-to-switch links.
> 

---

## 11.5.2 Configure PortFast

**Why PortFast?**

- Reduces delay when connecting end devices by bypassing STP states.

---

### Interface-Specific PortFast Configuration:

```bash
S1(config)# interface fa0/1
S1(config-if)# switchport mode access
S1(config-if)# spanning-tree portfast
```

**Warning**:

- If a switch or hub is mistakenly connected, it could cause temporary loops.

---

### Global PortFast Configuration:

```bash
S1(config)# spanning-tree portfast default
```

- Enables PortFast by **default on all access ports**.

**Verification**:

- Show if PortFast is globally enabled:
    
    ```bash
    show spanning-tree summary
    ```
    
- Show PortFast per interface:
    
    ```bash
    show running-config interface fa0/1
    show spanning-tree interface fa0/1 detail
    ```
    

**Example Output**:

```
spanning-tree portfast default
interface FastEthernet0/1
 switchport mode access
 spanning-tree portfast
```

> ⚡ Always manually disable PortFast on ports connected to other switches or hubs if needed.
> 

---

## 11.5.3 Configure BPDU Guard

- **Why BPDU Guard?**
    - Even if PortFast is enabled, an interface still listens for BPDUs.
    - If a BPDU is received, **BPDU Guard immediately shuts down the port** (err-disabled).

---

### Interface-Specific BPDU Guard Configuration:

```bash
S1(config)# interface fa0/1
S1(config-if)# spanning-tree bpduguard enable
```

---

### Global BPDU Guard Configuration:

```bash
S1(config)# spanning-tree portfast bpduguard default
```

- Automatically applies BPDU Guard to all **PortFast-enabled ports**.

---

### BPDU Guard Recovery (Optional):

If a port goes into err-disabled state due to BPDU Guard, you can configure automatic recovery:

```bash
S1(config)# errdisable recovery cause bpduguard
S1(config)# errdisable recovery interval 30
```

- **30 seconds** is the time before the port attempts to recover.

---

### Verification Commands:

- View STP summary:
    
    ```bash
    show spanning-tree summary
    ```
    

**Example Output**:

```
Portfast Default             is enabled
PortFast BPDU Guard Default   is enabled
Portfast BPDU Filter Default  is disabled
```

# Cisco CCNA 12.0 – 12.1 Notes

## 12.0.1 Why Should I Take This Module?

Welcome to **WLAN Concepts**!

- Wireless connections are everywhere: homes, work, schools.
- Different wireless types fit different situations and require specific devices.
- Wireless networks are vulnerable to specific attacks — but there are solutions.

**Goal**:

- Understand what WLANs are, how they function, and how to protect them.

> 🚀 Ready to explore how wireless LANs empower mobility and flexibility? Let’s go!
> 

---

## 12.0.2 What Will I Learn in This Module?

**Module Title**: WLAN Concepts

**Module Objective**:

- Explain how **WLANs** enable network connectivity.

---

# 12.1 WLAN Concepts

## 12.1.1 Benefits of Wireless

- **WLAN (Wireless LAN)**: Provides network access for mobile devices inside homes, offices, campuses.
- Supports devices like:
    - Laptops
    - Smartphones
    - Tablets

### Business Benefits:

- Reduces costs when moving employees, reorganizing spaces, or setting up temporary sites.
- Adapts quickly to new needs and technologies.

> 🌐 WLANs make mobility simple and cost-effective within enterprises and homes.
> 

---

## 12.1.2 Types of Wireless Networks

Wireless networks (based on IEEE standards) are categorized into four major types:

| Network Type | Description |
| --- | --- |
| **WPAN** (Wireless Personal Area Network) | Short-range (20–30 ft); Bluetooth, ZigBee (IEEE 802.15, 2.4 GHz). |
| **WLAN** (Wireless LAN) | Medium-range (~300 ft); home, office, campus (IEEE 802.11, 2.4/5 GHz). |
| **WMAN** (Wireless MAN) | Covers a city or district; licensed frequencies. |
| **WWAN** (Wireless WAN) | Covers national/global areas; used by cellular providers. |

---

## 12.1.3 Wireless Technologies

Wireless technologies use the **unlicensed radio spectrum** for data transmission.

| Technology | Description |
| --- | --- |
| **Bluetooth** | 802.15 WPAN standard for device-to-device pairing (up to 300 ft). |
| **Bluetooth Low Energy (BLE)** | Supports mesh networks and low-power devices. |
| **Bluetooth BR/EDR** | Optimized for audio streaming (point-to-point links). |
| **WiMAX** | 802.16 WWAN standard; broadband wireless over large areas (~30 miles). |
| **Cellular Broadband** | 4G/5G mobile networks for data/voice; GSM (global) and CDMA (US). |
| **Satellite Broadband** | Provides remote access via geostationary satellites; used in rural areas. |

---

## 12.1.4 802.11 Standards

- **IEEE 802.11 standards** define how radio frequencies are used for wireless communication.
- Devices transmit and receive over **2.4 GHz** or **5 GHz** bands.
- Newer standards use **MIMO** (Multiple-Input, Multiple-Output) technology:
    - Multiple antennas improve performance and throughput.

> 📡 MIMO enables faster speeds and higher reliability by using multiple transmission/reception paths.
> 

---

## 12.1.5 Radio Frequencies

Wireless LANs operate primarily in:

| Frequency | Standards |
| --- | --- |
| **2.4 GHz (UHF)** | 802.11b / g / n / ax |
| **5 GHz (SHF)** | 802.11a / n / ac / ax |
- Devices must be tuned to specific frequencies to communicate properly.

> 🎯 2.4 GHz = longer range, more interference.
> 
> 
> 🎯 5 GHz = shorter range, less interference, faster speeds.
> 

---

## 12.1.6 Wireless Standards Organizations

Three key organizations regulate and certify wireless networking:

| Organization | Role |
| --- | --- |
| **ITU-R** | Regulates radio spectrum allocations globally (through the ITU Radiocommunication Sector). |
| **IEEE** | Defines how radio signals are modulated for wireless communication (802 standards). |
| **Wi-Fi Alliance** | Promotes interoperability and certifies 802.11 WLAN products for compliance. |

> 🌍 Standards ensure devices from different vendors can communicate seamlessly!
> 

# Cisco CCNA 12.2 Notes

## 12.2.1 WLAN Components

**Wireless LAN Components Include**:

- **End devices** with wireless NICs (laptops, smartphones, tablets).
- **Network devices** like wireless routers or wireless access points (APs).

### Key Points:

- **Integrated antennas** are often built into laptops and smartphones (e.g., in the laptop screen).
- **Desktop computers** can use:
    - PCIe wireless cards with external antennas.
    - USB wireless adapters (with built-in antennas).
- **Antennas**:
    - **Dipole antennas**: Omnidirectional 360° coverage.
    - External antennas for desktops or access points.

> 📡 End devices and network devices communicate using matched radio frequencies.
> 

---

## 12.2.2 Wireless NICs

- Every wireless-capable device has a **wireless NIC** (Network Interface Card).
- If no internal NIC exists, a **USB wireless adapter** can be used.
- Many wireless devices have **hidden internal antennas** (e.g., smartphones, laptops).

---

## 12.2.3 Wireless Home Router

- A **wireless router** integrates three functions:
    
    
    | Function | Purpose |
    | --- | --- |
    | **Access Point** | Provides wireless (802.11) connectivity. |
    | **Switch** | Wired 4-port Ethernet switch for local devices. |
    | **Router** | Connects local network to external networks (like the internet). |
- **Wireless routers**:
    - Broadcast **beacons** containing **SSID** (network name).
    - Allow **association** and **authentication** with end devices.
    - Support features like:
        - IPv6
        - Quality of Service (QoS)
        - USB ports (for printers or external drives)
- **Wi-Fi Range Extenders**:
    - Boost wireless coverage but are limited in scalability.
    - Better scalability is achieved with **additional APs**.

---

## 12.2.4 Wireless Access Points (APs)

- **Wireless Access Points (APs)** provide dedicated wireless access for devices.
- Clients use their wireless NICs to **discover** nearby APs and **associate** and **authenticate** to connect.
- Example:
    - **Cisco Meraki Go APs** for small/medium businesses.

> 🛡️ APs provide secure, managed wireless entry points for client devices.
> 

---

## 12.2.5 AP Categories

| Type | Description |
| --- | --- |
| **Autonomous APs** | Standalone devices configured individually (CLI or GUI). Example: Home router. |
| **Controller-Based APs (Lightweight APs)** | Managed automatically by a **WLAN Controller (WLC)**. |

**Controller-Based APs**:

- Use **LWAPP** (Lightweight Access Point Protocol) to connect to WLCs.
- WLC manages:
    - Configuration
    - Security
    - Updates
- **Link Aggregation Group (LAG)**:
    - Bundles multiple WLC connections into a single logical link for redundancy and load balancing.
    - **Note**: LAG on WLC is not true EtherChannel (no PAgP or LACP).

---

## 12.2.6 Wireless Antennas

| Antenna Type | Purpose |
| --- | --- |
| **Omnidirectional** | 360° coverage, ideal for homes, open offices, conference rooms. |
| **Directional** | Focuses signal in one direction for stronger coverage (e.g., Yagi, parabolic antennas). |
- **MIMO (Multiple Input Multiple Output)**:
    - Uses multiple antennas to increase throughput.
    - Supported in 802.11n/ac/ax standards.
    - Up to **8 transmit/receive antennas** for better speed and reliability.

> 🚀 MIMO technology significantly boosts wireless performance in modern networks.
> 

# Cisco CCNA 12.3 Notes

## 12.3.1 WLAN Operation

**Two WLAN Operation Modes**:

| Mode | Description |
| --- | --- |
| **Infrastructure Mode** | Devices connect through a wireless router or AP tied into a wired distribution system. |
| **Ad Hoc Mode** | Devices connect directly peer-to-peer without infrastructure (e.g., Bluetooth, Wi-Fi Direct). |

**Tethering**:

- A smartphone or tablet acts as a mobile hotspot.
- Provides wireless access to other devices through its cellular connection.

> 📡 Infrastructure mode = larger networks.
> 
> 
> 📱 Ad hoc or tethering = small, quick setups.
> 

---

## 12.3.2 802.11 Wireless Topology Modes

| Topology Mode | Description |
| --- | --- |
| **Ad Hoc (IBSS)** | Peer-to-peer wireless connections without APs. |
| **Infrastructure Mode** | Wireless clients connect via APs to a wired LAN. |
| **Tethering** | Cellular device shares internet with other devices by acting as a Wi-Fi hotspot. |

---

## 12.3.3 BSS and ESS

| Concept | Description |
| --- | --- |
| **BSS (Basic Service Set)** | One AP interconnecting wireless clients. |
| **BSA (Basic Service Area)** | Coverage area of a single AP. |
| **BSSID** | MAC address of the AP; identifies the BSS. |
| **ESS (Extended Service Set)** | Multiple BSSs joined by a wired DS to create a larger roaming area. |
| **ESA (Extended Service Area)** | Total wireless coverage provided by the ESS. |

> 🌐 ESS allows seamless roaming between APs for wireless clients.
> 

---

## 12.3.4 802.11 Frame Structure

Wireless frames are similar to Ethernet frames but contain **additional fields**:

| Field | Description |
| --- | --- |
| **Frame Control** | Frame type, protocol version, power management, security info. |
| **Duration** | Remaining time for the next transmission. |
| **Address 1** | Receiver MAC address (usually AP). |
| **Address 2** | Transmitter MAC address (client device). |
| **Address 3** | Destination MAC or BSSID. |
| **Sequence Control** | Sequencing and fragmentation info. |
| **Address 4** | Used only in ad hoc mode. |
| **Payload** | Actual data being transmitted. |
| **FCS (Frame Check Sequence)** | Layer 2 error control. |

> 📋 802.11 frames handle mobility and wireless network-specific needs.
> 

---

## 12.3.5 CSMA/CA

**Carrier Sense Multiple Access with Collision Avoidance (CSMA/CA)**:

| Step | Description |
| --- | --- |
| 1 | Device listens for idle channel. |
| 2 | Sends RTS (Request to Send). |
| 3 | Receives CTS (Clear to Send) from AP. |
| 4 | Transmits data. |
| 5 | Awaits acknowledgment (ACK). |
| 6 | Retries if no ACK received (assumes collision). |

**Why CSMA/CA?**

- WLANs are half-duplex; devices can't detect collisions while transmitting.
- CSMA/CA helps avoid collisions before sending.

> 🛡️ Prevents signal collisions and data loss in wireless networks.
> 

---

## 12.3.6 Wireless Client and AP Association

**Association Steps**:

| Stage | Description |
| --- | --- |
| 1 | Discover an AP (SSID broadcasted or manually entered). |
| 2 | Authenticate to the AP (usually via password). |
| 3 | Associate (finalize connection and begin communication). |

**Key Parameters to Match**:

- **SSID**: Wireless network name.
- **Password**: For authentication and encryption.
- **Network Mode**: 802.11 standards (a/b/g/n/ac/ad).
- **Security Mode**: WEP, WPA, WPA2 (prefer highest).
- **Channel Settings**: Frequency band and specific channel.

> 📶 Proper matching ensures successful wireless connections.
> 

---

## 12.3.7 Passive and Active Discovery Modes

| Discovery Mode | Description |
| --- | --- |
| **Passive Mode** | APs send beacon frames broadcasting SSID, supported standards, and security settings. |
| **Active Mode** | Wireless client sends a probe request to discover networks if beaconing is disabled. |
- **Passive Mode** is default and easier (automatic discovery).
- **Active Mode** is used when APs do not broadcast SSIDs.

> 🔍 Understanding both modes is important for troubleshooting wireless connections.
> 

# Cisco CCNA 12.4 Notes

## 12.4.1 Introduction to CAPWAP

- **CAPWAP** = **Control and Provisioning of Wireless Access Points**.
- An IEEE standard protocol allowing a **WLC (Wireless LAN Controller)** to manage multiple APs (Access Points) and WLANs.
- Responsible for:
    - **Encapsulating** and **forwarding** WLAN client traffic between APs and the WLC.
    - **Managing** AP configurations remotely.

### Key Features:

- Based on **LWAPP** but adds **security enhancements** with **DTLS (Datagram Transport Layer Security)**.
- Works over **IPv4** and **IPv6**.
- **UDP Ports Used**:
    - **5246** for CAPWAP control messages.
    - **5247** for encapsulated client data.

---

## 12.4.2 CAPWAP Operation and Security

| Feature | Description |
| --- | --- |
| **Control Plane** | CAPWAP encrypts control messages between AP and WLC using DTLS. |
| **Data Plane** | CAPWAP does **not encrypt** client data traffic by default (optional with DTLS license). |
- **IP Protocol Numbers**:
    - **IPv4** uses **Protocol 17** (UDP).
    - **IPv6** uses **Protocol 136**.

**DTLS Encryption**:

- **Control Traffic**: Encrypted by default.
- **Client Data Traffic**: Not encrypted by default — can be enabled **per AP** if a DTLS license is installed on the WLC.

> 🔐 CAPWAP prevents man-in-the-middle (MITM) attacks on control traffic.
> 

---

## 12.4.3 Split MAC Architecture

CAPWAP introduces a **Split MAC architecture** where WLAN functions are divided between the AP and WLC:

| AP MAC Functions | WLC MAC Functions |
| --- | --- |
| Beacons and probe responses | Client authentication |
| Packet acknowledgements and retransmissions | Client association and roaming management |
| Frame queuing and prioritization | Frame translation (802.11 to 802.3) |
| MAC layer encryption and decryption | Termination of wireless traffic onto wired networks |

> 🛠️ This division improves performance, scalability, and central management.
> 

---

## 12.4.4 FlexConnect APs

**FlexConnect** is a solution for **branch offices** and **remote sites**.

### Two Modes of Operation:

| Mode | Description |
| --- | --- |
| **Connected Mode** | FlexConnect AP maintains CAPWAP connectivity with the WLC across the WAN. All functions handled by WLC. |
| **Standalone Mode** | WLC is unreachable. FlexConnect AP locally switches client traffic and performs local client authentication. |

**Use Cases**:

- Reduces the need for deploying a controller at every branch office.
- Allows local traffic switching during WAN outages.

---

### FlexConnect Benefits:

- WAN optimization for remote sites.
- Continued local network access even if WAN link to corporate WLC is down.

> 🌐 FlexConnect increases wireless network resilience and flexibility for distributed environments.
> 

# Cisco CCNA 12.5 Notes

## 12.5.1 Frequency Channel Saturation

- Wireless LAN devices are tuned to specific **frequency ranges** called **channels**.
- **Channel Saturation**:
    - Happens when too many devices use the same channel.
    - Results in degraded communication quality.

---

### Techniques to Improve Channel Usage

| Technique | Description |
| --- | --- |
| **DSSS** (Direct-Sequence Spread Spectrum) | Spreads a signal across a wider band to make interception/jamming harder. Used by 802.11b. |
| **FHSS** (Frequency-Hopping Spread Spectrum) | Rapidly switches frequencies according to a shared pattern. Used in early 802.11, Bluetooth. |
| **OFDM** (Orthogonal Frequency-Division Multiplexing) | Uses multiple orthogonal sub-channels within a single channel. Used in 802.11a/g/n/ac. |
| **OFDMA** (Orthogonal Frequency-Division Multiple Access) | A variant of OFDM, used in 802.11ax (Wi-Fi 6), improves efficiency further. |

> 📡 These techniques reduce channel congestion and improve wireless reliability.
> 

---

## 12.5.2 Channel Selection

### 2.4 GHz Channels (802.11b/g/n)

- 2.4 GHz band = **11 channels** (North America).
- Each channel:
    - **22 MHz** wide.
    - **5 MHz** separation between adjacent channels → overlaps occur!

### Best Practice:

- Use **non-overlapping channels**: **1, 6, and 11**.
- Reduces interference when deploying multiple APs.

**Example**:

| Channel | Overlap? |
| --- | --- |
| 1 | No interference |
| 6 | No interference |
| 11 | No interference |

> ⚡ Modern APs often auto-select non-overlapping channels.
> 

---

### 5 GHz Channels (802.11a/n/ac)

- 5 GHz band = **24 channels**.
- Each channel:
    - **20 MHz** apart.
- **Little to no overlap** compared to 2.4 GHz.

**Advantages**:

- More channels = less interference.
- Higher speeds possible.

**Example of First 8 Non-Interfering Channels**:

- 36, 40, 44, 48, 149, 153, 157, 161

> 🛡️ Use non-interfering channels even at 5 GHz for adjacent APs to maximize performance.
> 

---

## 12.5.3 Plan a WLAN Deployment

### Key Considerations:

| Planning Factor | Why Important |
| --- | --- |
| **Geography/Building Layout** | Walls, floors, and furniture affect coverage. |
| **Device Density** | More users = more bandwidth and APs needed. |
| **Data Rate Expectations** | Higher throughput needs more careful AP placement. |
| **Non-Overlapping Channels** | Reduces interference among APs. |
| **Transmit Power** | Set appropriately to avoid overlap and dead zones. |

---

### AP Placement Tips:

- **Use existing wiring** efficiently.
- **Avoid** sources of interference (microwaves, cameras, fluorescent lights, motion detectors).
- **Mount APs above obstructions** (higher = better signal).
- **Center APs** in coverage areas.
- **Place APs** where users are (e.g., conference rooms > hallways).

> 📶 Always check AP specifications for expected coverage based on standard and transmit power.
> 

# Cisco CCNA 12.6 Notes

## 12.6.1 WLAN Threats

Wireless LANs (WLANs) are vulnerable because **anyone within range** can attempt to connect if they have the proper credentials.

### Common Wireless Threats:

| Threat | Description |
| --- | --- |
| **Interception of Data** | Data can be eavesdropped if not encrypted. |
| **Wireless Intruders** | Unauthorized users attempting access; deterred with strong authentication. |
| **Denial of Service (DoS) Attacks** | Intentional or accidental disruption of WLAN services. |
| **Rogue APs** | Unauthorized APs installed without permission. |

---

## 12.6.2 Wireless Security Overview

- **Wireless NICs** + **Cracking techniques** allow attackers to breach networks without physical entry.
- Attackers can be:
    - Outsiders
    - Disgruntled employees
    - Unintentionally negligent employees

> 🛡️ Strong encryption and authentication are essential for WLAN protection.
> 

---

## 12.6.3 Denial of Service (DoS) Attacks

| DoS Source | Description |
| --- | --- |
| **Improperly Configured Devices** | Admin mistakes or intentional sabotage disabling wireless services. |
| **Malicious Interference** | Attackers intentionally jam or flood wireless channels. |
| **Accidental Interference** | Common devices (microwave ovens, baby monitors, cordless phones) disrupt 2.4 GHz signals. |

### Mitigation:

- Harden all devices.
- Secure passwords.
- Maintain backups.
- Apply configuration changes **off-hours**.
- Monitor and shift to **5 GHz** where possible to avoid 2.4 GHz interference.

> ⚡ Careful device management and spectrum planning minimize DoS risks.
> 

---

## 12.6.4 Rogue Access Points

- A **rogue AP** is an unauthorized device connected to the corporate network.
- Anyone with physical access could set up a cheap wireless router or enable hotspot features on a device.

### Rogue AP Risks:

- Capture MAC addresses.
- Sniff traffic.
- Unauthorized network access.
- Launch **Man-in-the-Middle (MITM)** attacks.

### Prevention:

- Configure **Wireless LAN Controllers (WLCs)** with rogue AP detection policies.
- Use monitoring software to actively scan for unauthorized devices.

> 🚨 Rogue APs are a major security gap if left unmanaged!
> 

---

## 12.6.5 Man-in-the-Middle (MITM) Attack

### Evil Twin Attack:

| Step | Description |
| --- | --- |
| 1 | Attacker sets up a rogue AP using the **same SSID** as the real AP. |
| 2 | Victims connect to the stronger (rogue) AP. |
| 3 | Rogue AP intercepts and forwards data between the client and the real AP. |
| 4 | Attacker captures sensitive data (passwords, personal info). |

### Common Locations:

- Airports
- Coffee shops
- Restaurants (open Wi-Fi)

### Defense Strategy:

- **Authenticate** all wireless clients.
- **Identify legitimate devices**.
- **Monitor** network traffic for abnormal behavior.

> 🔒 The best defense against Evil Twin attacks is a vigilant, authenticated, and monitored WLAN infrastructure.
> 

# Cisco CCNA 12.7 Notes

## 12.7.1 Securing WLANs Overview

Wireless signals travel through walls, floors, and ceilings, making it easy for outsiders to access networks without proper security.

**Early security methods**:

- **SSID Cloaking**: Hide the network name (SSID); clients must manually enter SSID.
- **MAC Address Filtering**: Allow only devices with approved MAC addresses.

**Limitations**:

- SSID cloaking can be bypassed (attackers can discover hidden SSIDs).
- MAC addresses can be **spoofed**.

> 🛡️ Real WLAN security relies on strong authentication and encryption.
> 

---

## 12.7.2 802.11 Authentication Methods

| Authentication Type | Description |
| --- | --- |
| **Open System Authentication** | No password needed. Anyone can connect. Suitable for public Wi-Fi (cafes, airports). |
| **Shared Key Authentication** | Pre-shared key (password) used for both authentication and encryption. |

---

## 12.7.3 Shared Key Authentication Techniques

| Method | Details |
| --- | --- |
| **WEP** | Wired Equivalent Privacy; weak security; obsolete. |
| **WPA** | Wi-Fi Protected Access; better than WEP, but uses TKIP, which is less secure. |
| **WPA2** | Industry standard; uses AES encryption; strong and recommended. |
| **WPA3** | Latest; uses SAE for stronger authentication and encryption. |

> 🔒 Use WPA2 or WPA3 for modern network security.
> 

---

## 12.7.4 Authenticating a Home User

**Two WPA2 Modes**:

| Mode | Description |
| --- | --- |
| **WPA2-Personal (PSK)** | Home/Small office; all users share a single pre-shared password. |
| **WPA2-Enterprise** | Larger networks; uses a RADIUS server to authenticate each user individually with username and password. |

**Enterprise Mode Details**:

- **Requires RADIUS server** (IP, ports 1812 for authentication, 1813 for accounting).
- **Shared secret** between AP and RADIUS server ensures secure requests.
- Uses **802.1X** framework and **EAP (Extensible Authentication Protocol)** for authentication.

---

## 12.7.5 Encryption Methods

| Encryption | Description |
| --- | --- |
| **TKIP** | Used by WPA; better than WEP but weaker than AES. |
| **AES** | Used by WPA2 and WPA3; strongest encryption, based on CCMP protocol. |

> 🛡️ Always choose AES encryption when setting up WPA2 or WPA3.
> 

---

## 12.7.6 WPA3 Improvements

| WPA3 Feature | Description |
| --- | --- |
| **WPA3-Personal** | Uses Simultaneous Authentication of Equals (SAE) — stronger password-based authentication. |
| **WPA3-Enterprise** | Enforces 192-bit encryption using the Commercial National Security Algorithm (CNSA) Suite. |
| **Open Networks** | Opportunistic Wireless Encryption (OWE) encrypts even open Wi-Fi traffic. |
| **IoT Onboarding** | Device Provisioning Protocol (DPP) replaces vulnerable WPS for easily adding IoT devices using QR codes. |

> 🚀 WPA3 closes many of the gaps that existed in WPA2 and earlier protocols.
> 

# Cisco CCNA 13 Notes

## 13.0 WLAN Configuration

### 13.0.1 Why Should I Take This Module?

- Early internet connections used **dial-up** over landlines — slow and stationary.
- Modern connections are **wireless**, enabling device mobility (phones, laptops, tablets).
- Setting up wireless networks requires:
    - Special end devices (wireless NICs).
    - Intermediate devices (wireless routers, APs).
    - A good understanding of wireless protocols.

> 📡 Wireless networking offers freedom, but demands careful setup and security.
> 

---

### 13.0.2 What Will I Learn?

**Module Title**: WLAN Configuration

**Objective**:

- Implement a WLAN using a wireless router and WLC.

---

# 13.1 Wireless Router Configuration

## 13.1.2 The Wireless Router

- **Wireless Routers** (e.g., Cisco Meraki MX64W) combine:
    - Switch (for wired clients)
    - WAN port (for internet)
    - Wireless components (for WLAN clients)
- Provide services like:
    - WLAN security
    - DHCP
    - NAT
    - QoS
- **Note**: Cable/DSL modem setup usually managed by the ISP.

---

## 13.1.3 Log in to the Wireless Router

Steps:

1. Connect via a web browser to default IP (e.g., **192.168.0.1**).
2. Default credentials are usually **admin/admin** (must be changed!).
3. Change default IP, SSID, passwords for better security.

---

## 13.1.4 Basic Network Setup

| Step | Action |
| --- | --- |
| 1 | Log in via web browser. |
| 2 | Change the default admin password. |
| 3 | Re-login with new password. |
| 4 | Change default DHCP IPv4 settings. |
| 5 | Renew IP (`ipconfig /renew`). |
| 6 | Log in using the new IP address. |

---

## 13.1.5 Basic Wireless Setup

| Step | Action |
| --- | --- |
| 1 | View WLAN defaults (SSID/password). |
| 2 | Set network mode (e.g., Legacy/mixed). |
| 3 | Configure SSID (e.g., OfficeNet). |
| 4 | Choose a non-overlapping channel (1, 6, or 11 for 2.4GHz). |
| 5 | Configure security mode (WPA2 Personal + AES). |
| 6 | Set a strong passphrase. |

---

## 13.1.6 Wireless Mesh Networks (WMN)

- Extends coverage using **multiple APs**.
- Set APs with the same SSID, but different non-overlapping channels.
- Modern systems are easily configured via smartphone apps.

---

## 13.1.7 NAT for IPv4

- **NAT** converts private IPv4 addresses (e.g., 10.10.10.x) to public IPv4 addresses.
- Allows multiple devices to share one public IP.
- NAT tracks each session using **source port numbers**.

---

## 13.1.8 Quality of Service (QoS)

- Prioritize traffic types:
    - Voice/Video > Web browsing/Email.
- Configurable under advanced settings (sometimes called "bandwidth control").

---

## 13.1.9 Port Forwarding and Triggering

| Feature | Description |
| --- | --- |
| **Port Forwarding** | Directs inbound traffic on a specific port (e.g., HTTP port 80) to a device inside the LAN. |
| **Port Triggering** | Dynamically opens ports based on outgoing traffic, then closes them automatically. |

---

# 13.2 Wireless LAN Controller (WLC) Configuration

## 13.2.2 WLC Topology

- Controller-based APs (Lightweight APs) use **LWAPP** to communicate with WLC.
- APs are **PoE-powered** (Power over Ethernet).

---

## 13.2.3 Log in to the WLC

- Access WLC GUI with initial credentials.
- **Network Summary Dashboard** displays:
    - WLANs
    - APs
    - Clients
    - Rogue devices

---

## 13.2.4 View AP Information

- View AP system information (e.g., IP address, connection port).
- CDP reveals AP's wired connection.
- Use CLI commands for troubleshooting (e.g., `ping`).

---

## 13.2.5 Advanced Settings

- Access full WLC settings via **Advanced** page.
- Includes features for WLAN configuration, security, QoS, policies.

---

## 13.2.6 Configure a WLAN

| Step | Action |
| --- | --- |
| 1 | Create WLAN (SSID and Profile Name). |
| 2 | Enable the WLAN. |
| 3 | Associate it with an Interface (VLAN). |
| 4 | Secure it with WPA2-PSK. |
| 5 | Verify WLAN is operational. |
| 6 | Monitor WLAN usage. |
| 7 | View wireless client details. |

---

# 13.3 WLC Management

## 13.3.2 SNMP and RADIUS Overview

- **SNMP**: For monitoring and trap forwarding.
- **RADIUS**: For AAA (authentication, authorization, accounting).
    - Users log in with unique credentials instead of shared PSK.

---

## 13.3.3 Configure SNMP Server Information

| Step | Action |
| --- | --- |
| 1 | MANAGEMENT → SNMP → Trap Receivers. |
| 2 | Create New Trap Receiver (community name + server IP). |

---

## 13.3.4 Configure RADIUS Server Information

| Step | Action |
| --- | --- |
| 1 | SECURITY → RADIUS → Authentication. |
| 2 | Add new RADIUS server (IP and shared secret). |

---

## 13.3.6 VLAN 5 Addressing

- WLC needs virtual interfaces for each WLAN.
- Router (R1) has VLAN 5 configured at 192.168.5.1/24.

---

## 13.3.7 Configure a New Interface (VLAN)

| Step | Action |
| --- | --- |
| 1 | CONTROLLER → Interfaces → New. |
| 2 | Name VLAN Interface (e.g., vlan5) and assign VLAN ID 5. |
| 3 | Configure IP (e.g., 192.168.5.254/24) and gateway. |
| 4 | Set DHCP server address (router IP). |
| 5 | Apply and verify. |

---

## 13.3.9 Configure DHCP Scope

| Step | Action |
| --- | --- |
| 1 | INTERNAL DHCP SERVER → DHCP Scope → New. |
| 2 | Name the Scope (e.g., Wireless_Management). |
| 3 | Configure start/end IPs, subnet mask, router IP. |
| 4 | Enable Scope. |
| 5 | Verify configuration. |

---

## 13.3.11 Configure a WPA2 Enterprise WLAN

| Step | Action |
| --- | --- |
| 1 | WLANs → New WLAN. |
| 2 | Set Profile Name and SSID. |
| 3 | Enable WLAN and link to VLAN 5. |
| 4 | Verify WPA2-AES and 802.1X settings. |
| 5 | Select previously configured RADIUS server. |
| 6 | Verify WLAN is active and secured. |

---

# 13.4 Troubleshooting WLANs

## 13.4.1 Troubleshooting Approach

**Steps**:

1. Identify the problem.
2. Establish a theory of probable cause.
3. Test the theory.
4. Establish a plan of action.
5. Implement the solution.
6. Verify and document the solution.

---

## 13.4.2 Wireless Client Not Connecting

Checklist:

- Verify DHCP/static IP (`ipconfig`).
- Test wired connection.
- Check NIC drivers and compatibility.
- Match SSID and security settings.
- Check coverage area.
- Investigate interference sources (microwaves, cordless phones).
- Confirm AP power and cabling.
- Verify AP status if all else fails.

---

## 13.4.3 Troubleshooting Slow Networks

Solutions:

- **Upgrade wireless clients** to newer standards (e.g., 802.11ac, 802.11ax).
- **Split traffic**:
    - 2.4 GHz: Basic browsing.
    - 5 GHz: Streaming, high-bandwidth tasks.
- Rename networks for easier management.
- Minimize physical obstructions.

---

## 13.4.4 Updating Firmware

- Regularly check for firmware updates.
- Update wireless routers, APs, and WLCs.
- Be prepared for device reboots and brief downtime.

---

# 13.5 Module Summary

- Wireless routers bundle routing, switching, DHCP, NAT, WLAN functions.
- Always change router default passwords and IPs.
- Configure QoS, port forwarding as needed.
- WLCs manage multiple APs using LWAPP or CAPWAP.
- Use RADIUS for secure WPA2 Enterprise authentication.
- Troubleshoot systematically, considering both hardware and wireless-specific issues.
- Firmware updates help patch vulnerabilities and enhance performance.

# Cisco CCNA 14 Notes

## 14.0 Routing Concepts Overview

### 14.0.1 Why Should I Take This Module?

- Even in well-designed networks, things **break**. Understanding how routers work internally is crucial for troubleshooting.
- This module dives deep into **how routers make forwarding decisions**.

---

### 14.0.2 What Will I Learn?

**Module Title**: Routing Concepts

**Objective**:

- Explain how routers use packet information to make forwarding decisions.

---

# 14.1 Router Fundamentals

## 14.1.1 Two Functions of a Router

- Routers:
    - **Connect multiple networks** (each interface belongs to a different IP network).
    - **Determine best path** to forward packets based on their **routing table**.

---

## 14.1.2 Router Functions Example

- Routers like **R1** and **R2** use their IP routing tables to determine:
    - Best route
    - Forward the packet to the next hop or destination

---

## 14.1.3 Best Path = Longest Match

- **Longest Match**:
    - The router chooses the route with the **most matching leftmost bits** with the packet’s destination IP address.
    - **Prefix Length** determines how many bits must match.
- The longest match is **always the preferred route**.

---

## 14.1.4 IPv4 Longest Match Example

Given a destination IP of **172.16.0.10**, routes match:

| Route | Prefix Length | Match? |
| --- | --- | --- |
| 172.16.0.0/12 | Yes |  |
| 172.16.0.0/18 | Yes |  |
| 172.16.0.0/26 | Yes (longest match, chosen) | X |

---

## 14.1.5 IPv6 Longest Match Example

Destination IPv6: **2001:db8:c000::99**

| Route | Prefix Length | Match? |
| --- | --- | --- |
| 2001:db8:c000::/40 | Matches first 40 bits |  |
| 2001:db8:c000::/48 | Matches first 48 bits (longest match, chosen) | X |
| 2001:db8:c000:5555::/64 | No match |  |

---

## 14.1.6 Building the Routing Table

| Type | Description |
| --- | --- |
| **Directly Connected Networks** | Interface configured with IP and active (up/up). |
| **Remote Networks** | Learned via Static Routes or Dynamic Routing Protocols. |
- **Default Route**:
    - IPv4: `0.0.0.0/0`
    - IPv6: `::/0`
    - Used when no more specific route exists.

---

# 14.2 Packet Forwarding Process

## 14.2.1 Packet Forwarding Decision

Steps:

1. Frame arrives on **ingress** interface.
2. Router examines destination IP.
3. Finds **longest matching prefix**.
4. Encapsulates and forwards packet through **egress** interface.
5. Drops packet if no match exists and no default route.

---

**If destination is directly connected**:

- Router uses **ARP** (IPv4) or **ICMPv6 Neighbor Discovery** (IPv6) to find the MAC address.

**If forwarding to next-hop router**:

- Similar ARP/Neighbor Discovery for the next-hop IP, **not** the destination IP.

---

## 14.2.2 End-to-End Packet Forwarding Example

- PC1 sends packet ➔ Default gateway ➔ R1 ➔ R2 ➔ R3 ➔ PC2.
- Various Layer 2 encapsulations occur (Ethernet, PPP, HDLC).

---

## 14.2.3 Packet Forwarding Mechanisms

| Method | Description |
| --- | --- |
| **Process Switching** | Slowest; CPU handles every packet. |
| **Fast Switching** | First packet is process-switched; subsequent packets use cached forwarding information. |
| **Cisco Express Forwarding (CEF)** | Fastest; builds **Forwarding Information Base (FIB)** and **adjacency tables** ahead of time. |

> 📋 CEF is the default on modern Cisco routers.
> 

---

# 14.3 Basic Router Configuration Review

## 14.3.2 Configuration Commands (R1 Example)

```bash
enable
configure terminal
hostname R1
enable secret class
line console 0
 password cisco
 login
line vty 0 4
 password cisco
 login
 transport input telnet ssh
service password-encryption
banner motd #
***********************************************
WARNING: Unauthorized access is prohibited!
***********************************************
#
ipv6 unicast-routing
interface gigabitethernet 0/0/0
 description Link to LAN 1
 ip address 10.0.1.1 255.255.255.0
 ipv6 address 2001:db8:acad:1::1/64
 no shutdown
...
copy running-config startup-config
```

---

## 14.3.3 Verification Commands

- `show ip interface brief`
- `show running-config interface`
- `show interfaces`
- `show ip route`
- `ping`

(Replace `ip` with `ipv6` for IPv6 versions.)

---

## 14.3.4 Filter Command Output

**Filtering `show` output** using `|`:

| Command | Use |
| --- | --- |
| `section` | Displays a section starting with a match. |
| `include` | Displays lines matching the expression. |
| `exclude` | Excludes lines matching the expression. |
| `begin` | Displays lines starting from a match onward. |

**Example**:

```bash
show running-config | section line vty
```

---

# 14.4 Routing Table Details

## 14.4.1 Route Sources

| Code | Meaning |
| --- | --- |
| **L** | Local (assigned to interface) |
| **C** | Directly connected |
| **S** | Static route |
| **O** | OSPF dynamic route |
| ***** | Default route candidate |

---

## 14.4.2 Routing Table Principles

- Routing relies on **prefix length** and **next-hop** determination.
- The **longest match** wins.

---

## 14.4.3 Routing Table Entries

- Contains network prefix, prefix length, route source, AD, metric, next-hop, and outgoing interface.

---

## 14.4.4 Directly Connected Networks

- Automatically added to the routing table when an interface is **up/up**.

| Code | Meaning |
| --- | --- |
| C | Connected network |
| L | Local address (router's own interface IP) |

---

## 14.4.5 Static Routes

- Manually configured.
- Common for:
    - Small/stable networks
    - Default routes
    - Stub networks
- Benefit: No protocol overhead.
- Downside: No automatic failover.

---

## 14.4.7 Dynamic Routing Protocols

- Allow routers to **discover remote networks** automatically.
- Respond to network **topology changes**.

**Examples**:

- OSPF
- EIGRP
- BGP

---

## 14.4.9 Default Route

- Used when no specific route matches.
- Format:
    - IPv4: `0.0.0.0/0`
    - IPv6: `::/0`

---

## 14.4.10 Routing Table Structure

- IPv4 table: Retains **classful indentation** (parent/child routes).
- IPv6 table: **Flat structure**, no indentation.

---

## 14.4.12 Administrative Distance (AD)

- Determines **trustworthiness** of a routing source.
- Lower AD = Higher trust.

| Routing Source | AD |
| --- | --- |
| Connected | 0 |
| Static | 1 |
| EIGRP | 90 |
| OSPF | 110 |

---

# 14.5 Static vs Dynamic Routing

| Type | Best When |
| --- | --- |
| Static | Small, predictable, secure networks. |
| Dynamic | Large, growing, changing networks. |

---

## 14.5.2 Dynamic Routing Evolution

| Protocol | Timeline |
| --- | --- |
| RIP | 1980s |
| RIPv2 | Improved RIP |
| OSPF | Modern scalable IGP |
| EIGRP | Cisco-proprietary advanced protocol |
| BGP | Inter-domain (ISP) routing |

---

## 14.5.3 Dynamic Routing Protocol Concepts

| Component | Description |
| --- | --- |
| Data Structures | Routing databases and tables in RAM. |
| Routing Messages | Hello packets, updates, advertisements. |
| Algorithms | Best path determination logic. |

---

## 14.5.4 Best Path

- Determined by **lowest metric**.
- Each protocol uses different metrics:
    - RIP: Hop count
    - OSPF: Cost
    - EIGRP: Bandwidth and delay

---

## 14.5.5 Load Balancing

- **Equal Cost Load Balancing**:
    - Forwarding packets over multiple equal-cost paths.
- **Only EIGRP** supports **unequal-cost load balancing**.

---

# 14.6 Module Summary

**Key Takeaways**:

- Routers determine the best path using **longest match**.
- **Directly connected**, **static**, and **dynamic routes** fill the routing table.
- **Packet forwarding** uses Process Switching, Fast Switching, or CEF.
- **Filtering commands** optimize CLI output.
- **AD** prioritizes which route source wins.
- Networks typically use a combination of **static and dynamic routing**.
- **Dynamic protocols** provide automatic network discovery and adaptation.

---

# Cisco CCNA 15 Notes

**Module Title**: IP Static Routing

**Objective**: Configure IPv4 and IPv6 static routes.

---

## 15.0 Module Introduction

### 15.0.1 Why Should I Take This Module?

- Even in networks using **dynamic routing**, **static routes** still serve critical purposes.
- Static routes:
    - Offer control.
    - Can be more efficient.
    - Are often used alongside dynamic routing.
- This module teaches how to:
    - **Configure**
    - **Verify**
    - **Troubleshoot** static routes.

> 🧠 Think of static routes like hand-washing delicate clothes – sometimes they’re the better fit!
> 

---

## 15.1 Static Route Concepts

### 15.1.1 Types of Static Routes

| Type | Description |
| --- | --- |
| **Standard Static Route** | Manually configured path to a specific network. |
| **Default Static Route** | Catch-all route used when no specific match is found. |
| **Floating Static Route** | Backup route with higher Administrative Distance. |
| **Summary Static Route** | Combines multiple routes into one. |

---

### 15.1.2 Next-Hop Options

| Route Type | Definition |
| --- | --- |
| **Next-hop route** | Only next-hop IP is specified. |
| **Directly connected static route** | Only exit interface is specified. |
| **Fully specified static route** | Both next-hop IP and exit interface are specified. |

---

### 15.1.3 IPv4 Static Route Syntax

```bash
ip route <network-address> <subnet-mask> {<ip-address> | <exit-intf> [ip-address]} [distance]
```

---

### 15.1.4 IPv6 Static Route Syntax

```bash
ipv6 route <ipv6-prefix>/<prefix-length> {<ipv6-address> | <exit-intf> [ipv6-address]} [distance]
```

🛑 Must use `ipv6 unicast-routing` to enable IPv6 routing!

---

### 15.1.5–15.1.7 Routing Table States

- At startup, routers only know **directly connected networks**.
- **Ping tests** fail for destinations outside these networks until routes are added.

---

## 15.2 Configure IP Static Routes

### 15.2.1 IPv4 Next-Hop Static Route

```bash
R1(config)# ip route 192.168.1.0 255.255.255.0 172.16.2.2
```

Verifies via `show ip route` with "S" for static.

---

### 15.2.2 IPv6 Next-Hop Static Route

```bash
R1(config)# ipv6 unicast-routing
R1(config)# ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:2::2
```

---

### 15.2.3 IPv4 Directly Connected Static Route

```bash
R1(config)# ip route 192.168.2.0 255.255.255.0 s0/1/0
```

✅ Only use for **point-to-point serial** links.

---

### 15.2.4 IPv6 Directly Connected Static Route

```bash
R1(config)# ipv6 route 2001:db8:cafe:2::/64 s0/1/0
```

Same rule: only use this for **point-to-point links**.

---

### 15.2.5 IPv4 Fully Specified Static Route

```bash
R1(config)# ip route 192.168.2.0 255.255.255.0 GigabitEthernet0/0/1 172.16.2.2
```

✅ Ideal for **Ethernet/multi-access** networks.

---

### 15.2.6 IPv6 Fully Specified Static Route (Link-local)

```bash
R1(config)# ipv6 route 2001:db8:acad:1::/64 s0/1/0 fe80::2
```

💡 Required when using a **link-local** next-hop address.

---

### 15.2.7 Verify Static Routes

| Command | Purpose |
| --- | --- |
| `show ip route static` | Lists IPv4 static routes. |
| `show ipv6 route static` | Lists IPv6 static routes. |
| `show ip route <network>` | Filters to a specific route. |
| `show run | section ip route` |

---

## 15.3 Default Static Routes

### 15.3.1 Overview

- Matches all traffic:
    - **IPv4**: `0.0.0.0 0.0.0.0`
    - **IPv6**: `::/0`
- Common in **stub routers** or **edge routers**.

---

### 15.3.2 Configure Default Routes

**IPv4**:

```bash
ip route 0.0.0.0 0.0.0.0 172.16.2.2
```

**IPv6**:

```bash
ipv6 route ::/0 2001:db8:acad:2::2
```

---

### 15.3.3 Verify Default Routes

- `S*` indicates static default route.
- Asterisk () = **candidate default** (gateway of last resort).

---

## 15.4 Floating Static Routes

### 15.4.1 What is a Floating Static Route?

- Acts as a **backup route**.
- Has a **higher administrative distance** than the primary route.
- Used **only when the preferred route fails**.

---

### 15.4.2 Configure Floating Static Routes

**IPv4 Example**:

```bash
ip route 0.0.0.0 0.0.0.0 10.10.10.2 5
```

**IPv6 Example**:

```bash
ipv6 route ::/0 2001:db8:feed:10::2 5
```

---

### 15.4.3 Test Floating Static Route

- Simulate failure of primary path (e.g., shut R2's interface).
- Check routing table:
    - **Floating route becomes active**.

---

## 15.5 Host Routes

### 15.5.1 Overview

- Host route = route to **specific host**:
    - **IPv4**: `/32`
    - **IPv6**: `/128`

---

### 15.5.2 Auto-Installed Host Routes

- IOS installs host routes for **each configured interface**.
- Identified with `L` in `show ip route`.

---

### 15.5.3–15.5.4 Manually Configure Host Routes

**IPv4 Example**:

```bash
ip route 209.165.200.238 255.255.255.255 198.51.100.2
```

**IPv6 Example**:

```bash
ipv6 route 2001:db8:acad:2::238/128 2001:db8:acad:1::2
```

---

### 15.5.6 IPv6 Host Route with Link-Local

```bash
ipv6 route 2001:db8:acad:2::238/128 serial 0/1/0 fe80::2
```

🔑 Required when using a **link-local** next hop.

---

## 15.6 Module Summary

### ✅ What You Learned

- Configure all static route types for both IPv4 and IPv6:
    - **Standard**
    - **Default**
    - **Floating**
    - **Fully specified**
    - **Host**
- Understand how **Administrative Distance** impacts route preference.
- Use verification tools:
    - `show ip route`
    - `ping`
    - `traceroute`
    - `show running-config`

---

# Cisco CCNA 16 Notes

**Module Title**: Troubleshoot Static and Default Routes

**Objective**: Troubleshoot static and default route configurations.

---

## 16.0 Module Introduction

### 16.0.1 Why Should I Take This Module?

- This is the final module in the **Switching, Routing, and Wireless Essentials v7.0 (SRWE)** course.
- You’ve learned how to configure:
    - Switches
    - Routers
    - Wireless devices
- This module focuses on **troubleshooting** — a vital skill for becoming a great network administrator.
- You’ll practice troubleshooting static and default routes using:
    - **Syntax Checker**
    - **Packet Tracer**
    - **Hands-on lab**

> 🎯 The best way to learn troubleshooting? Always be troubleshooting.
> 

---

## 16.1 Static Routes and Packet Forwarding

### 16.1.1 Packet Forwarding Process Review

Scenario: PC1 sends a packet to PC3. Here's what happens:

1. **Packet arrives at R1 (Gig0/0/0)**.
2. R1 doesn’t have a specific route to 192.168.2.0/24, so it uses its **default static route**.
3. R1 encapsulates the packet:
    - Uses “**all 1s**” (broadcast) Layer 2 destination MAC since it’s a **point-to-point link**.
4. Packet is forwarded out **Serial 0/1/0** to R2.
5. R2 de-encapsulates, checks routing table:
    - Finds a **static route** to 192.168.2.0/24 via **Serial 0/1/1**.
6. R2 forwards frame using another “all 1s” MAC address.
7. Packet arrives at R3 (Serial 0/1/1).
8. R3 sees a **connected route** to 192.168.2.0/24 via **Gig0/0/0**.
9. R3 checks ARP table for PC3’s IP.
    - If no entry: sends **ARP Request** → PC3 replies with its MAC.
10. R3 encapsulates and forwards the packet with:
    - **Source MAC**: R3’s Gig0/0/0
    - **Destination MAC**: PC3’s MAC

---

## 16.2 Troubleshoot Static and Default Routes

### 16.2.1 Network Changes

- Networks are **dynamic**. Failures or misconfigurations can break connectivity.
- Causes:
    - Interface failure
    - Provider link drop
    - Saturated links
    - Admin errors
- Goal: Use **structured troubleshooting** to quickly isolate and fix issues.

---

### 16.2.2 Common Troubleshooting Commands

| Command | Purpose |
| --- | --- |
| `ping` | Tests reachability. |
| `traceroute` | Traces route to a destination. |
| `show ip route` | Displays the routing table. |
| `show ip interface brief` | Quick overview of interface status/IPs. |
| `show cdp neighbors` | Lists directly connected Cisco devices (Layer 2 validation). |

---

### `ping` Example:

```bash
R1# ping 192.168.2.1 source 172.16.3.1
```

> Success rate: 100%
> 
> 
> Indicates connectivity from R1’s LAN to R3’s LAN.
> 

---

### `traceroute` Example:

```bash
R1# traceroute 192.168.2.1
```

> Shows hop-by-hop path to the destination. Useful for finding where packets stop.
> 

---

### `show ip route` Example:

```bash
R1# show ip route | begin Gateway
```

Example Output:

```
S     172.16.1.0/24 [1/0] via 172.16.2.2
C     172.16.2.0/24 is directly connected, Serial0/1/0
...
```

---

### `show ip interface brief` Example:

```bash
R1# show ip interface brief
```

Sample Output:

```
GigabitEthernet0/0/0   172.16.3.1   YES manual   up   up
Serial0/1/0            172.16.2.1   YES manual   up   up
```

---

### `show cdp neighbors` Example:

```bash
R1# show cdp neighbors
```

Output Example:

```
Device ID   Local Intrfce  Capability  Platform  Port ID
R2          Ser 0/1/0      R S I       ISR4221   Ser 0/1/0
```

> If CDP sees the device but ping fails, check Layer 3 configs.
> 

---

### 16.2.3 Solve a Connectivity Problem (Step-by-Step)

### Problem: PC1 cannot reach R3’s LAN.

---

**1. Ping from R1’s LAN to R3’s LAN:**

```bash
R1# ping 192.168.2.1 source g0/0/0
```

Result: **Fails** (0% success)

---

**2. Ping from R1 to R2's Serial link:**

```bash
R1# ping 172.16.2.2
```

Result: **Success** (5/5)

---

**3. Ping from R1 to R3’s LAN (without source):**

```bash
R1# ping 192.168.2.1
```

Result: **Success**

Conclusion: R1 → R3 is reachable using the **Serial interface IP**.

But PC1’s traffic (from R1’s LAN) can’t reach PC3 — there’s likely a **missing or wrong return route**.

---

**4. Check R2’s routing table:**

```bash
R2# show ip route
```

Findings:

```
S 172.16.3.0/24 [1/0] via 192.168.1.1 ❌
```

🔴 Problem: R2 sends 172.16.3.0/24 traffic to **192.168.1.1** (R3), not back to R1.

---

**5. Fix the static route on R2:**

```bash
R2(config)# no ip route 172.16.3.0 255.255.255.0 192.168.1.1
R2(config)# ip route 172.16.3.0 255.255.255.0 172.16.2.1
```

---

**6. Verify corrected route:**

```bash
R2# show ip route | begin Gateway
```

✅ New route:

```
S 172.16.3.0/24 [1/0] via 172.16.2.1
```

---

**7. Ping R3 again from R1 (source from LAN):**

```bash
R1# ping 192.168.2.1 source g0/0/0
```

🎉 Result: **Success (5/5)**

---

## 16.3 Module Summary

### Key Concepts Recap

### Static Route Forwarding:

1. Packet reaches R1.
2. R1 uses default route to forward it to R2.
3. R2 has a static route to destination → sends it to R3.
4. R3 has connected route to PC3’s LAN.
5. R3 uses **ARP** to find PC3 MAC → sends packet.

---

### Troubleshooting Tools:

| Command | Use |
| --- | --- |
| `ping` | Checks reachability. |
| `traceroute` | Shows packet path. |
| `show ip route` | Verifies routing logic. |
| `show ip interface brief` | Confirms interface status. |
| `show cdp neighbors` | Checks physical Layer 1/2 links. |