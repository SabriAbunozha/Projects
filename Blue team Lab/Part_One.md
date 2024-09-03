# Blue Team Home lab part one 

## Network 


This project is aimed at building a full blue team including Firewalls, Network topology, Antivirus, SIEM, EDR, and more 
using virtualization we can do all of that in this project we are going to have 
We are going to have 5 different VLANs for this project 

1. VLAN 1: management VLAN which we can access the firewall from 
2. VLAN 10: This is the corporate WAN which will include a Kali machine to simulate attacks, this VLAN will not have access to the real internet or my machine and will only have access to VLAN 20.
3. VLAN 20: this is the corporate LAN network here we can find the servers and end devices and will only have access to the fake internet network
4. VLAN 50: This is the security VLAN with security tools and access to the real internet so we can use public tools for analysis and finally the corporate network to retrieve logs 
5. VLAN 99: this will be an isolated LAN network for malware analysis
I am going to name this domain (E Corp) ;)

Here is the Network Information : 

| Name          | Domain | VLAN | Subnet       | Gateway     |
| ------------- | ------ | ---- | ------------ | ----------- |
| Management    | N/A    | 1    | 10.0.1.0/24  | 10.0.1.1    |
| Corporate WAN | N/A    | 10   | 10.0.10.0/24 | 10.0.10.254 |
| Corporate LAN | E corp | 20   | 10.0.20.0/24 | 10.0.20.254 |
| Security      | E corp | 50   | 10.0.50.0/24 | 10.0.50.254 |
| Isolated LAN  | N/A    | 99   | 10.0.99.0/24 | 10.0.99.254 |

And here is the Network Topology   

![NetworkTopology](/Attachment/Image14.png "E Corp Network topology")

## VLAN creation 

Now we are going to create these networks in our VMware

In VMware go to _Edit > Virtual Network Editor_ then we can add the 4 networks *vmnet10*, **vmnet20**, **vmnet50**, and **vmnet99** all four network types should be *Host-only*.

Next for each network, we gonna uncheck *use local DHCP* cuz we are going to use our firewall as a DHCP server, finally we should set up the subnet IPs as in the table above 

here is the final result : 

![Virtual Network Editor](/Attachment/Image01.png "Virtual Networks")
### Tools Used

- VMware workstation for building and managing the machines and the VLANs
- Drow.io to build the Network topology
- pfSense which is a free open-source router/firewall
- Virtual machines
  
Just like that we made the virtual networks and assigned them the correct subnet addresses next we are going to set up pfSense and set our rules for the Firewall


