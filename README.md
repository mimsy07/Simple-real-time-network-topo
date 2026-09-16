# Simple-Small-Company-network
# Main Objective

Create a multi-branch network that optimize traffic, isolates broadcast domains, and ensure uninterrupted WAN connectivity. Deployment utilizing VLANs and DTP for internal traffic control, while connecting offices seamlessly through implementation of OSPF and GRE Tunnel with a HSRP gateway redundancy. 

- Create GRE tunnel Establishing virtual point-to-point tunnels through a public network to connect multi-branch company
- Execute NAT (network address translation) to allow the company's private IP to access public IP address
- Configure OSPF for fast-converging internal routing across core, distribution and remote layers
- Implement HSRP to configure VIRTUAL IP per VLANs that would be shared by MAIN and BACKUP switches. This provides layer 3 gateway redundancy to eliminate single point of failure for local users.
- VLANs and DTP (Dynamic Trunking Protocol) that segregates internal department traffic at layer 2 to enhance security and automatically establishes trunk links between switches.

<h3>Key Skill </h3>
