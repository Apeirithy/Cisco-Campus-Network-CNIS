# Enterprise Campus Network Simulation (Cisco Packet Tracer)

A comprehensive multi-building Local Area Network (LAN) infrastructure designed and simulated using Cisco Packet Tracer. This project demonstrates the implementation of a hierarchical network design featuring dynamic routing, logical segmentation, and defense-in-depth network security policies.

## Network Architecture & Topology
The infrastructure is designed using a **Hierarchical Star Topology** to connect 3 separate campus buildings (Building A, B, and C) to a centralized backbone.
- **Core Layer:** Inter-building routing managed by 3 Cisco ISR 4331 Routers.
- **Distribution Layer:** Cisco 3650 Multilayer Switches handling Inter-VLAN routing (SVI) and local DHCP distribution for each building.
- **Access Layer:** Cisco 2960 Switches distributing connectivity to end-user devices with Layer 2 security.

> **Visual Topology:** [Click here to view the Campus Network Topology](./Topology/Network_Topology.png)

## Key Technologies & Protocols Implemented

### 1. Dynamic Routing (OSPF Multi-Area)
Implemented Open Shortest Path First (OSPF) to limit the blast radius of topology changes and optimize Shortest Path First (SPF) calculations:
- **Area 0 (Backbone):** Interconnects the 3 core routers. Secured with **MD5 Authentication** (`ip ospf message-digest-key 1 md5`) to prevent rogue routers from forming adjacency.
- **Area 10, 20, 30:** Dedicated non-backbone areas for Building A, B, and C, respectively.
- **Route Summarization (ABR):** Summarized subnets at the ABR level (`area range`) to minimize routing table overhead and LSA flooding.
- **Passive Interfaces:** Applied to end-user SVIs to suppress unnecessary OSPF Hello packets.

### 2. Logical Segmentation (VLAN & VLSM)
- Segmented into **13 distinct VLANs** across the 3 buildings categorized by department (e.g., Finance, IT, Technician, HR, Staff/General Users) to isolate broadcast domains.
- Applied **Variable Length Subnet Masking (VLSM)** with an allocated 30% growth margin for future expansion *(detailed in the IP Mapping spreadsheet)*.

### 3. Defense-in-Depth Security Mechanisms
Security policies enforced across Layer 2 and Layer 3:
- **Extended Access Control Lists (ACL):** Deployed on Multilayer Switch C to isolate sensitive departments (`PROTECT_FINANCE` and `PROTECT_HR`). Implements the *Least Privilege* principle by only permitting established TCP sessions, ICMP echo-replies, DNS, and DHCP traffic, backed by an explicit `deny ip any any`.
- **DHCP Snooping:** Configured across all Access Switches with uplink ports marked as `trusted` and access ports as `untrusted` (capped at a 15 pps rate limit to prevent DoS/flooding). Disabled Option 82 insertion (`no ip dhcp snooping information option`) to prevent packet drops from untrusted interfaces.
- **Sticky Port Security:** Enforced on all access ports, limiting MAC learning to a maximum of 1 address per port with a `shutdown` violation mode to mitigate unauthorized MAC spoofing or physical intrusion.

## Repository Contents
- **[`/Packet_Tracer`](./Packet_Tracer):** Complete `.pkt` topology files runnable in Cisco Packet Tracer.
- **[`/IP_Mapping_&_Docs`](./IP_Mapping_&_Docs):** VLSM IP addressing spreadsheet (`Mapping CNIS KELOMPOK 2.xlsx`) and the comprehensive project report.
- **[`/CLI_Configurations`](./CLI_Configurations):**
  - `Cisco_IOS_Command_Template.txt`: Complete command reference for L2 switches, L3 multilayer switches, ABR routers, ACLs, and Port Security.
  - `DHCP_Snooping_Implementation_Guide.txt`: Setup notes and Option 82 handling guidelines for DHCP Snooping.
