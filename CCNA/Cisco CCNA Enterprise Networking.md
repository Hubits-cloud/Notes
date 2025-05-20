
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

## 2.0 Overview

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