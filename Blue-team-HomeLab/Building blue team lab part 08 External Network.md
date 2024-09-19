2024-09-18 18:30


# Installation and configuration 

I am going to download Kali Linux to simulate attacks from an external network  I am going to download the VMware machine from [here](https://www.kali.org/get-kali/#kali-virtual-machines) after downloading it I am going to simply import it into VMware and leave the default settings as is.

Now we can update the machine and upgrade it with `sudo apt update & sudo apt upgrade -y`

## Network

after we updated the system now we can change the network adapter to Vmnet10 to simulate attacks from external network and set up the static IP for the machine 

![[image74.png]](/Attachment/image74.png)

also, we should take a snapshot now just in case 

## Firewall configuration

 we are going to set up port forwarding in our firewall by going to _friewall>NAT>Port Forward_ then we will create rules so we redirect any traffic coming on the WAN VLAN 10 interface on ports 80,3306 and 4422 towards our Linux server IP which is 10.0.20.10 on ports 3306, 80 and 4422 
 
 ![[image75.png]](/Attachment/image75.png)

## Connectivity 

Now we can try to access the DVWA from the Kali Linux machine by going to *`HTTP://10.0.10.254`* 

![[image66.png]](/Attachment/image66.png)

also, we can try to SSH into the server 

![[image76.png]](/Attachment/image76.png)

Done
# References 

[Building Blue Team Home Lab Part 9 - Bandito | facyber](https://facyber.me/posts/blue-team-lab-guide-part-9/)
