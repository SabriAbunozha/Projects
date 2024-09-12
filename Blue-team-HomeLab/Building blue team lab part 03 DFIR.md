2024-09-07 18:13



# SIFT workstations 

SIFT Workstation is a DFIR OS that contains a lot of tools and cheat sheets we can download the OVA file so we don't have to set anything up we are using this machine as the security team so when we set this machine up we are going to hook it to the VLAN 50 because it has to get access to the corporate LAN after we import it and run the machine we can use the default credentials to log in 

## Config
we can now change the password of the machine using the `passwd` command also we can change the machine name or hostname by changing it in both /etc/hostname and /etc/hosts 

![SIFT](/Attachment/Image16.png)

next, we have to set a static IP address for the VM by searching _Advanced Network Configuration_
if you don't have a connection already you need to add a new one and choose Ethernet then edit the IPv4 settings 

![Gateway](/Attachment/Image17.png)

*Note:* The Version I downloaded uses Netplan so you have to edit the file /etc/netplan/01-netcfg.yaml or any similar file you find in my case I had to change /etc/netplan/00-installer-config.yaml 

![NETPLAN](/Attachment/Image18.png)

After editing the file with the correct IP addresses you now have to type the command 
`Sudo netplan apply` If nothing appears in the terminal you have successfully assigned the IP addresses after changing the hostname and the IP we have to reboot the VM so that changes can be applied.

Finally, we have to create a host override record on our firewall by going to _Services>DNS Resolver>General Settings_ then go down and you will find the Host override section click on Add
and fill in the correct info then click save 

![DNS](/Attachment/Image19.png)

now to check the connectivity of the machine we can do two things first we can ping 8.8.8.8 from the VM because this particular machine has access to the internet the second method is to reach it using the FQDN (Fully qualified domain name) in my case _Sab.evil.corp_ 

![CMDPING](/Attachment/Image20.png)

also, we can do it from the GUI by going to _Diagnostics>ping_ and typing the hostname 

![GUIPING](/Attachment/Image21.png)

as you can see we have connectivity and the DNS is working and referring us to the right machine by only using the hostname 
now the DFIR machine is fully set up later we can set up the malware analysis VMs and the SIEM solutions 

# References 

[Building Blue Team Home Lab Part 4 - DFIR | facyber](https://facyber.me/posts/blue-team-lab-guide-part-4/)
