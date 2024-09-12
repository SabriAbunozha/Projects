2024-09-02 20:46

# Blue team Home Lab Part Two

## pfSense firewall

pfSense is an open-source firewall that can be used as a router and a firewall 
so we are going to use it as a firewall in our case 
we will install it on a virtual machine with the following specs: 

1. CPU: 1 
2. RAM: 512Mb or whatever you want it doesn't matter I used 2 GB just in case I want to expand the network in the future
3. HDD: minimum 30Gb for the system can be more if you want also for scalability 
4. NICs: 6x 
	- Bridged 
	- vmnet1
	- vmnet10
	- vmnet20
	- vmnet50
	- vmnet99

we have 5 networks so we have to specify them, the first one is just a bridged interface to give the security network access to the internet 

for the installation we just use the default settings for everything and at the end we will have it booted up with two configured interfaces, WAN and LAN 

![pfSense](/Attachment/Image02.png "pfSense")

now we have to assign the correct IP addresses for the two interfaces 
which following this table :


| Interface     | IP address      |
| ------------- | --------------- |
| WAN           | 192.168.1.50/24 |
| Management    | 10.0.1.50/24    |
| Corporate WAN | 10.0.10.254/24  |
| Corporate LAN | 10.0.20.254/24  |
| Security      | 10.0.50.254/24  |
| isolated LAN  | 10.0.99.254/24  |


after we set the management we can access the web configurator on 10.0.1.50
with the default credentials *Username: admin Password: pfsense*
the setup wizard is straightforward you just have to put the domain name that you want to use and the hostname also make sure to use a DNS server I used 8.8.8.8 and 8.8.4.4
also you should go to _system>general settings_ and change the protocol to HTTPS
and you should configure a secure shell server SSH
now we have to configure the interfaces by going to _interfaces_>_interface Assignments_ and clicking the add button for each interface in the list finally, we can click the save button

the next step is to enable and configure each interface with the correct description and IP addresses 

1. **Enable:** Checked
2. **Description:** $NETWORK_NAME_VLAN<VLAN_NUMBER>
3. **IPv4 Configuration Type:** Static IPv4
4. **IPv6 Configuration Type:** None
5. **MAC Address:** leave empty
6. **MTU:** leave empty
7. **MSS:** leave empty
8. **Speed and Duplex:** Default
9. **IPv4 Address:** IP address of the interface from the previous table. Mask set to _/24_
10. **IPv4 Upstream gateway:** None (only the WAN interface should have configured this field)
11. **Block private networks:** unchecked (this field should be checked for WAN interface only)
12. **Block bogon networks:** unchecked (this field should be checked for WAN interface only)


![VLANSettings](/Attachment/Image03.png "Vlan10")

finally, it would look like this 

![result](/Attachment/Image05.png)

now the rules of the firewall you can edit the rules by going to _firewall>rules_ and selecting the interface which you want to edit 
### WAN

keep it as is we don't need to change anything 

![Result](/Attachment/Image06.png)

### management

Same thing here we don't need to change anything 

![Mngmnt](/Attachment/Image07.png)

### Corporate_WAN_VLAN10

we need to allow traffic toward VLAN 20 and allow traffic inside VLAN 10 

![VLAN10](/Attachment/Image08.png)

### CORPORATE_LAN_VLAN20  

we should do the same because we are going to simulate the access to fake internet 

![VLAN20](/Attachment/Image09.png)


### Security_VLAN50 

here we have to block access to the WAN, Management, and corporate WAN VLAN10 and allow the rest of the traffic ( internet access, Isolated VLAN, and corporate LAN )

![VLAN50](/Attachment/Image11.png)

### isolation 

we just need to block all traffic 

![VLAN99](/Attachment/Image12.png)

Done now we just have to set the outbound rules to allow the security network access to the internet and simulate other networks access to the internet through the fake WAN _Firewall>NAT>outbound_

just do the same as the following rules

![OUTBOUND_RULES](/Attachment/Image13.png)

Just like that, we have the firewall set up and now we can start adding machines to the Network 
# References 
[Building Blue Team Home Lab Part 3 - Deploying a firewall | facyber](https://facyber.me/posts/blue-team-lab-guide-part-3/)


