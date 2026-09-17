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
  
### VLANs and DTP (Dynamic Trunking Protocol)

<img src="https://github.com/mimsy07/Simple-real-time-network-topo/blob/main/images/output/SW10-30%20VLAN.png" height="40%" width="50%"/>
- Create VLANs to logically divide network, separating department traffic and prevent unnecessary traffic going to distribution and core devices  

### Routing table

<img src="https://github.com/mimsy07/Simple-real-time-network-topo/blob/main/images/output/Routing%20table.png" height="60%" width="60%"/>

<p>The HQ router's routing table shows the remote networks learned through OSPF and GRE Tunnel. The 10.1.1.0/24 network is a remote branch network learned dynamically through OSPF, with 172.16.1.2 as the next-hop IP address over Tunnel 0. The 172.16.1.0/30 network is directly connected to the GRE tunnel and is labeled with the "C" that means directly connected route. The HQ router also has a Static default route using serial 0/0/0 as the exit interface for destinations that's not found in the routing table.</p>

<img src="https://github.com/mimsy07/Simple-real-time-network-topo/blob/main/images/output/NET%20routing%20tab..png" height="60%" width="60%"/>
<p>Routing table of Internet router, you can see that there is no remote network in here except for the directly connected network</p>
<br />
<h3>Configuration</h3>

<b>HQ Router</b>

````
conf t
router ospf 100
network 10.10.10.1 0.0.0.0 area 0
network 10.10.10.5 0.0.0.0 area 0
network 172.16.1.0 0.0.0.255 area 0
default-information originate
exit
````
NOTE: I also implement default information originate to advertise a default route to all OSPF routers in this OSPF domain.

<b>MAIN Switch</b>

````
conf t
router ospf 100
network 10.10.10.2 0.0.0.0 area 0
network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
network 192.168.30.0 0.0.0.255 area 0
passive-interface default
no passive-interface g1/3
````
Configure passive interface for those interface that I don't need to send hello message, except interface that is connected to another router g1/3, same with the configuration of backup L3SW

<b>BACKUP Switch</b>

````
router ospf 100
network 10.10.10.2 0.0.0.0 area 0
network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
network 192.168.30.0 0.0.0.255 area 0
passive-interface default
no passive-interface g1/3
````
<b>Branch Router</b>

````
conf t
router ospf 100
network 10.1.1.0 0.0.0.255 area 0
network 172.16.1.0 0.0.0.255 area 0
exit
````






















