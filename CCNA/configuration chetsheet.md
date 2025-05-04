# CIDR cheatsheet

Created by: Tobias
Created time: March 26, 2025 9:47 AM
Category: Cybersecurity, Netværk, Notes
Last updated time: May 4, 2025 10:21 PM

# ⚙️ Cisco Device Configuration Cheat Sheet (Router, L3 Switch, Switch)

---

## 🛜 Router Configuration (R-1)

### 🔧 Global Configuration

```bash
enable
configure terminal
hostname R-1
no ip domain-lookup
enable secret class
service password-encryption
```

### 🧑 Console & VTY Access

```bash
line console 0
 password cisco
 login
exit

line vty 0 4
 password cisco
 login
exit
```

### 🖼 Banner

```bash
banner motd #Authorized Access Only!#
```

---

### 🌐 Static Routes

```bash
ip route 10.0.10.0 255.255.255.0 GigabitEthernet0/0/0
ip route 10.0.20.0 255.255.255.0 GigabitEthernet0/0/0
ip route 10.0.30.0 255.255.255.0 GigabitEthernet0/0/0
ip route 10.0.99.0 255.255.255.240 GigabitEthernet0/0/0
```

---

### 🌍 Interface Configuration

### G0/0/0 – Uplink

```bash
interface g0/0/0
 description "R1 G0/0/0"
 ip address 10.0.0.1 255.255.255.0
 no shutdown
exit
```

### S0/1/0 – WAN Link

```bash
interface s0/1/0
 description "R1 S0/1/0"
 ip address 209.165.201.2 255.255.255.252
 no shutdown
exit
```

### G0/0/1 – Trunk to VLANs

```bash
interface g0/0/1
 no shutdown
```

### Subinterfaces:

```bash
interface g0/0/1.40
 description "Gateway for VLAN40"
 encapsulation dot1q 40
 ip address 10.0.40.1 255.255.255.0
exit

interface g0/0/1.50
 description "Gateway for VLAN50"
 encapsulation dot1q 50
 ip address 10.0.50.1 255.255.255.0
exit

interface g0/0/1.60
 description "Gateway for VLAN60"
 encapsulation dot1q 60
 ip address 10.0.60.1 255.255.255.0
exit

interface g0/0/1.99
 description "Gateway for VLAN99"
 encapsulation dot1q 99 native
 ip address 10.0.99.17 255.255.255.240
exit
```

---

## 🛡 Layer 3 Switch Configuration

### 🆔 VLANs + SVI Interfaces

```bash
vlan 10
 name SALES
interface vlan 10
 description SALES
 ip address 10.0.10.1 255.255.255.0
exit

vlan 20
 name ACCT
interface vlan 20
 description ACCT
 ip address 10.0.20.1 255.255.255.0
exit

vlan 30
 name EXEC
interface vlan 30
 description EXEC
 ip address 10.0.30.1 255.255.255.0
exit

vlan 99
 name ADMIN
interface vlan 99
 description ADMIN
 ip address 10.0.99.2 255.255.255.240
exit
```

### 🌐 Routing & Trunking

```bash
ip routing

interface g1/1/1
 no switchport
 ip address 10.0.0.2 255.255.255.0
exit
```

### 🔀 EtherChannel Setup

```bash
interface range g1/0/1-2
 channel-group 1 mode active
exit

interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
exit

interface range g1/0/3-4
 channel-group 2 mode active
exit

interface port-channel 2
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
exit
```

---

## 🧱 Access Switch Configuration

### 🧾 VLANs

```bash
vlan 10
 name users
vlan 999
 name unused
exit
```

### 🔌 Access Ports (Users)

```bash
interface range f0/1-5, g0/1
 switchport mode access
 switchport access vlan 10
```

### 🔐 Port Security

```bash
interface range f0/1-5
 switchport port-security
 switchport port-security maximum 4
 switchport port-security violation restrict
 switchport port-security aging time 10
 switchport port-security mac-address sticky
exit

interface f0/1
 switchport port-security mac-address 00D0.D3DC.2825
exit
```

---

### 🚫 DHCP Snooping

```bash
ip dhcp snooping
ip dhcp snooping vlan 10,999

interface range f0/1-5, g0/1
 ip dhcp snooping limit rate 5
exit

interface g0/1
 ip dhcp snooping trust
exit
```

---

### 🛡️ Dynamic ARP Inspection (DAI)

```bash
ip arp inspection vlan 10,999

interface g0/1
 ip arp inspection trust
exit
```

---

### 🌲 STP Enhancements

```bash
interface range f0/1-5
 spanning-tree portfast
 spanning-tree bpduguard enable
```

---

### ❌ Unused Ports

```bash
interface range f0/6-24, g0/2
 switchport mode access
 switchport access vlan 999
 shutdown
```