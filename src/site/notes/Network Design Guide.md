---
{"dg-publish":true,"permalink":"/network-design-guide/"}
---

# Two-Tier / Collapsed Core Design
Core and Distribution in Single block
and access switch in each block if needed
![Pasted image 20240806170800.png](/img/user/Attachments/Pasted%20image%2020240806170800.png)
**WAN EDGE**
Connect to:
- Remote data centers
- Remote Branches
- Other campus networks
- Connectivity to cloud providers

**Data Center / Server Room**
for business critical servers are placed

**Internet Edge**
- to ISP
- remote branches (ipsec/sd-wan)
- Remote VPN
- non-dedicated cloud provider connection

**Network Services**
- WLC
- Radius
- NAC
- DHCP/DNS Server

# Three-Tier Design
- Components: Core, Distribution, Access
- Each block deployed with a pair of distribution switches connected to the core
- Recommended for north-south traffic flows
- Data center use leaf-spine design to fulfill east-west traffic flow within data center
![Pasted image 20240806171744.png](/img/user/Attachments/Pasted%20image%2020240806171744.png)

# Simplified Campus Design
![Pasted image 20240806172405.png](/img/user/Attachments/Pasted%20image%2020240806172405.png)
![[Network Design.pdf]]