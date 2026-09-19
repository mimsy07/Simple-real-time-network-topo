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

## Project Walk through
<h3>Network Diagram</h3> <br/>
<img src="https://github.com/mimsy07/Simple-real-time-network-topo/blob/main/images/Topo.png" height="80%" width="80%"/>
<br />
<h3>Network Overview</h3>
<img src="https://github.com/mimsy07/Simple-real-time-network-topo/blob/main/images/Overview/HQ.png" height="70%" width="70%"/>
<img src="https://github.com/mimsy07/Simple-real-time-network-topo/blob/main/images/Overview/ISP.png"/>
<img src="https://github.com/mimsy07/Simple-real-time-network-topo/blob/main/images/Overview/BRANCH.png"/>


## Configuration 
<h3>SW10, SW20, and SW30</h3>

````

SW10
conf t
vlan 10
name VLAN10
exit
!
int r f0/2 - 3
switchport mode access
switchport access vlan 10
exit
!
int r g0/1 - 2
switchport mode trunk
exit
!
!
SW20
conf t
vlan 20
name VLAN20
exit
!
int r f0/2 - 3
switchport mode access
switchport access vlan 20
exit
!
int r g0/1 - 2
switchport mode trunk
exit
!
!
SW30
conf t
vlan 30
name VLAN20
exit
!
int r f0/2 - 3
switchport mode access
switchport access vlan 30
exit
!
int r g0/1 - 2
switchport mode trunk
exit
````

<h3>MAIN</h3>

````

conf t
int r g1/4 - 6
switchport mode trunk
no shut
exit
!
vlan 10
name VLAN10
exit
!
vlan 20
name VLAN20
exit
!
vlan 30
name VLAN30
exit
!
int vlan 10
ip address 192.168.10.1 255.255.255.0
standby 10 ip 192.168.10.100 255.255.255.0
standby 10 priority 120
standby 10 preempt
standby 10 timer 1  2
no shut
exit
!
int vlan 20
ip address 192.168.20.1 255.255.255.0
standby 10 ip 192.168.20.100 255.255.255.0
standby 10 priority 120
standby 10 preempt
standby 10 timer 1  2
no shut
exit
!
int vlan 30
ip address 192.168.30.1 255.255.255.0
standby 10 ip 192.168.30.100 255.255.255.0
standby 10 priority 120
standby 10 preempt
standby 10 timer 1  2
no shut
exit
!
ip routing
int g1/3
no switchport
ip address 10.10.10.2 255.255.255.252
no shut
exit
router ospf 100
network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
network 192.168.30.0 0.0.0.255 area 0
network 10.10.10.2 0.0.0.0 area 0
passive-interface default
passive-interface g1/3

````

<h3>BACKUP</h3>

````

conf t
int r g1/4 - 6
switchport mode trunk
no shut
exit
!
vlan 10
name VLAN10
exit
!
vlan 20
name VLAN20
exit
!
vlan 30
name VLAN30
exit
!
int vlan 10
ip address 192.168.10.2 255.255.255.0
standby 10 ip 192.168.10.100 255.255.255.0
standby 10 timer 1  2
no shut
exit
!
int vlan 20
ip address 192.168.20.2 255.255.255.0
standby 10 ip 192.168.20.100 255.255.255.0
standby 10 timer 1  2
no shut
exit
!
int vlan 30
ip address 192.168.30.2 255.255.255.0
standby 10 ip 192.168.30.100 255.255.255.0
standby 10 timer 1  2
no shut
exit
!
ip routing
int g1/3
no switchport
ip address 10.10.10.6 255.255.255.252
no shut
exit
router ospf 100
network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
network 192.168.30.0 0.0.0.255 area 0
network 10.10.10.6 0.0.0.0 area 0
passive-interface default
passive-interface g1/3

````

<h3>HQ</h3>

````

conf t
int g0/0
ip address 10.10.10.1 255.255.255.252
no shut
exit
!
int g0/1
ip address 10.10.10.6 255.255.255.252
no shut
exit
!
int s0/0/0
ip address 100.1.1.1 255.255.255.252
no shut
exit
!
router ospf 100
network 10.10.10.1 0.0.0.0 area 0
network 10.10.10.5 0.0.0.0 area 0
default-information originate
exit
!
ip router 0.0.0.0 0.0.0.0 s0/0/0
!
ip nat pool c-rule 100.1.1.1 100.1.1.1 netmask 255.255.255.252
access-list 10 permit 192.168.0.0 0.0.255.255
ip nat inside source list 10 pool c-rule overload
!
! CONFIGURING GRE TUNNEL
!
int tunnel 0
ip mode gre
ip address 172.16.1.1 255.255.255.252
tunnel source s0/0/0
tunnel destination 101.1.1.1
no shut
exit
!
router ospf 100
network 172.16.1.0 0.0.0.3 area 0
exit


````

<h3>INTERNET</h3>

````
conf t
int s0/0/1
ip address 100.1.1.2 255.255.255.252
no shut
exit
!
int s0/0/0
ip address 101.1.1.2 255.255.255.252
no shut
exit
int loopback 0
ip address 8.8.8.8 255.255.255.0
no shut
exit
````

<h3>BACKUP</h3>

````

conf t
int s0/0/1
ip address 101.1.1.1 255.255.255.252
no shut
exit
int g0/0
ip address 10.1.1.1 255.255.255.0
no shut
exit
!
router ospf 100
network 10.1.1.0 0.0.0.255 area 0
exit
!
ip nat pool b-rule 101.1.1.1 101.1.1.1 netmask 255.255.255.252
access-list 1 permit 10.1.1.0 0.0.0.255
ip nat inside source list 1 pool b-rule overload
exit
!
! CONFIGURING GRE TUNNEL
!
conf t
int tunnel 0
ip mode gre
ip address 172.16.1.2 255.255.255.252
tunnel source s0/0/1
tunnel destination 100.1.1.1
no shut
exit
!
router ospf 100
network 172.16.1.0 0.0.0.3 area 0
exit

````

















