---
{"dg-publish":true,"permalink":"/digital-garden/network-enterprise-network-design-guide/","noteIcon":""}
---

When designing an enterprise network, selecting the right architecture is crucial for ensuring scalability, performance, and reliability. Two popular network designs are the Two-Tier (or Collapsed Core) and Three-Tier architectures. Each has its strengths and use cases, making them suitable for different scenarios. This guide will explore both designs, detailing their components, benefits, and ideal use cases.

### **Two-Tier / Collapsed Core Design**

The Two-Tier architecture, also known as Collapsed Core, consolidates core and distribution functions into a single layer. This design simplifies the network by reducing the number of tiers and components, making it ideal for smaller to medium-sized networks or environments where simplicity and cost are priorities.
#### **Components of Two-Tier Design**
1. **Core/Distribution Layer:**
    - **Collapsed Core:** In this design, core and distribution functions are combined into a single block of switches or routers. This reduces complexity and cost by merging these roles.
    - **Access Switches:** Access switches connect directly to the core layer. If needed, additional access switches can be deployed to provide connectivity to end-user devices or servers.
2. **WAN Edge:**
    - **Remote Data Centers:** Connectivity to remote data centers ensures business continuity and data redundancy.
    - **Remote Branches:** Connects to branch offices using technologies like IPsec VPNs or SD-WAN.
    - **Other Campus Networks:** Integrates with other campus networks for a unified network experience.
    - **Cloud Providers:** Provides connectivity to cloud services for scalability and flexibility.
3. **Data Center / Server Room:**
    - **Business-Critical Servers:** Houses critical applications and databases. It often uses high-performance switches to ensure reliability and speed.
4. **Internet Edge:**
    - **ISP Connectivity:** Connects to Internet Service Providers for external internet access.
    - **Remote Branches (IPsec/SD-WAN):** Ensures secure connections to remote offices.
    - **Remote VPN:** Supports remote user access through secure VPN connections.
    - **Cloud Providers:** Connects to non-dedicated cloud services for additional resources.
5. **Network Services:**
    
    - **Wireless LAN Controller (WLC):** Manages wireless access points and client connections.
    - **RADIUS/NAC:** Provides authentication and network access control.
    - **DHCP/DNS Server:** Assigns IP addresses and resolves domain names for network devices.

#### **Diagram**
![Pasted image 20240806170800.png](/img/user/97%20Attachments/Pasted%20image%2020240806170800.png)

### **Three-Tier Design**

The Three-Tier design is more complex but offers greater scalability and performance. It separates the network into Core, Distribution, and Access layers, each with specific roles. This design is recommended for larger networks with significant north-south and east-west traffic flows.

#### **Components of Three-Tier Design**
1. **Core Layer:**
    - **High-Speed Backbone:** Provides high-speed connectivity between Distribution layers and handles traffic aggregation.
2. **Distribution Layer:**
    - **Redundant Switches:** Each Distribution block is connected to the Core layer with redundancy to ensure high availability and reliability.
    - **Routing and Policy Enforcement:** Handles routing between VLANs and enforces network policies.
3. **Access Layer:**
    - **End-User Connectivity:** Provides connectivity to end-user devices and connects to Distribution switches.
    - **Additional Access Switches:** If required, more switches can be added for additional connectivity.
4. **Data Center:**
    - **Leaf-Spine Design:** In modern data centers, a Leaf-Spine design is used for efficient east-west traffic flow within the data center.

#### **Diagram**
![Pasted image 20240806171744.png](/img/user/97%20Attachments/Pasted%20image%2020240806171744.png)

![[Network Design.pdf]]