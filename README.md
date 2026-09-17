# Simple-Small-Company-network
<h2>Main Objective</h2>

Create a multi-branch network that optimize traffic, isolates broadcast domains, and ensure uninterrupted WAN connectivity. Deployment utilizing VLANs and DTP for internal traffic control, while connecting offices seamlessly through implementation of OSPF and GRE Tunnel with a HSRP gateway redundancy. 

- Create GRE tunnel Establishing virtual point-to-point tunnels through a public network to connect multi-branch company
- Execute NAT (network address translation) to allow the company's private IP to access public IP address
- Configure OSPF for fast-converging internal routing across core, distribution and remote layers
- Implement HSRP to configure VIRTUAL IP per VLANs that would be shared by MAIN and BACKUP switches. This provides  gateway redundancy to eliminate single point of failure for local users.
- VLANs and DTP (Dynamic Trunking Protocol) that segregates internal department traffic at layer 2 to enhance security and automatically establishes trunk links between switches.

<h2>Skill Demonstrated</h2>

- Configure GRE tunnel to connect two different networks
- Implementing HSRP providing a backup default gateway for the local area network
- Execute NAT rules allowing the certain private network to access internet
- Configuration of DTP for the inter-switch connections.
- Implementation of VLANs to logically separates the traffic of department to the layer 2 and core devices.
- Configuring general routing configurations
- Assigning IP address and default gateway for switches, routers, and end user devices.

<h2>Project Walk through</h2>

<p align="center">
Network Diagram: <br/>
<img src="https://github.com/mimsy07/Simple-real-time-network-topo/blob/main/images/Topo.png" height="80%" width="80%"/>
<br />

<h3>Routing table</h3>

<img src="https://github.com/mimsy07/Simple-real-time-network-topo/blob/main/images/output/Routing%20table.png" height="80%" width="80%"/>

<p>The HQ router's routing table shows the remote networks learned through OSPF and GRE Tunnel. The 10.1.1.0/24 network is a remote branch network learned dynamically through OSPF, with 172.16.1.2 as the next-hop IP address over Tunnel 0. The 172.16.1.0/30 network is directly connected to the GRE tunnel and is labeled with the "C" that means directly connected route. The HQ router also has a Static default route using serial 0/0/0 as the exit interface for destinations that's not found in the routing table.</p>

