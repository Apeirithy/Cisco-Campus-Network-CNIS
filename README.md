# Enterprise Campus Network Simulation (Cisco Packet Tracer)

A comprehensive multi-building Local Area Network (LAN) infrastructure designed and simulated using Cisco Packet Tracer. This project demonstrates the implementation of a hierarchical network design featuring dynamic routing, logical segmentation, and defense-in-depth network security policies.

## Network Architecture & Topology
The infrastructure is designed using a **Hierarchical Star Topology** to connect 3 separate campus buildings (Building A, B, and C) to a centralized backbone.
* **Core Layer:** Inter-building routing managed by 3 Cisco ISR 4331 Routers.
* **Distribution Layer:** Cisco 3650 Multilayer Switches handling Inter-VLAN routing (SVI) for each building.
* **Access Layer:** Cisco 2960 Switches distributing connectivity to end-user devices.

## Key Technologies & Protocols Implemented
1. Dynamic Routing (OSPF Multi-Area)
Implemented Open Shortest Path First (OSPF) to limit the blast radius of topology changes and optimize the Shortest Path First (SPF) calculations:
* **Area 0 (Backbone):** Connects the 3 core routers. Secured with MD5 Authentication to prevent unauthorized rogue routers from forming adjacency.
* **Area 10, 20, 30:** Dedicated areas for Building A, B, and C respectively.
* **Passive Interfaces: Applied to all end-user SVIs to prevent unnecessary OSPF Hello packet flooding.

2. Logical Segmentation (VLAN & VLSM)
* Segmented the network into 13 distinct VLANs across the 3 buildings based on departments (e.g., HR, Finance, IT, General Users) to reduce broadcast domains.
* Applied Variable Length Subnet Masking (VLSM) to allocate IP blocks efficiently with a 30% growth allowance per subnet. (See the IP Mapping Excel file).

3. Defense-in-Depth Security Mechanisms
Security policies were strictly enforced at both Layer 2 and Layer 3:
* **Extended Access Control Lists (ACL):** Applied to the Multilayer Switch to isolate sensitive networks (Finance and HR VLANs). Follows the Least Privilege principle by only permitting established TCP connections, DNS, and DHCP traffic, while explicitly denying unauthorized internal access.
* **DHCP Snooping:** Configured globally and per-VLAN to mitigate Rogue DHCP Server attacks. Uplink ports were set as trusted, while all access ports were set as untrusted with a strict rate limit of 15 pps.
* **Sticky Port Security:** Enforced on all access-layer switch ports, restricting access to a maximum of 1 MAC address per port with a shutdown violation mode to prevent unauthorized device connections.

📂 Repository Contents
* **(./Packet_Tracer):** The final .pkt simulation file runnable in Cisco Packet Tracer.
* **(./IP_Mapping_&_Docs):** VLSM IP addressing spreadsheet and the comprehensive project evaluation report.
* **(./CLI_Configurations):** Extracted Cisco IOS CLI running-configs for all Routers and Switches.
