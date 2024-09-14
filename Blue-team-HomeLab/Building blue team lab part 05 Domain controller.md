2024-09-12 18:17



# Windows 2019 server 

Download the iso file from [here](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019) which is going to give you a free server for 180 days after installing you can take a snapshot 
start the configuration by changing the adapter to VLAN 20 from VMware then we can set the static IP addresses :

![staticIP](/Attachment/Image24.png)

then we can change the server name 

![ServerName](/Attachment/Image25.png)

Afterward, you can restart the machine so the changes can take effect

## Roles and Features 

Go to _Server Manager>Manage>Add Roles and Features_ skip everything till you get the server roles where you gonna select the following:
1. Active Directory Domain service 
2. DHCP Server
3. DNS Server
4. File and Storage Services 
5. Remote Access 
after selecting them also skip the rest we don't need to change anything it's gonna take a bit of time to install the new features after it finishes 

![Dashboard](/Attachment/Image26.png)

now click on Promote your server to a domain controller 
add a new forest and enter the domain name sadly I forgot to take screenshots so I am going to use the pictures from Facyber blog :

![Image27](/Attachment/Image27.png)


after that hit next enter the password of the admin and skip the rest of the configuration 

![Image28.png](/Attachment/Image28.png)

during the configuration, the server will restart and if everything is set up correctly you should get this login page 

![Image29.png](/Attachment/Image29.png)

## DHCP

now we can configure the DHCP server by going to _Tools>DHCP_ right click on _IPv4_ and click new scope 

![Image30.png](/Attachment/Image30.png)

Enter the name of the network and a description of it 

![Image31.png](/Attachment/Image31.png)

Now we have to specify the range of IP addresses we can not give it the full range because in a real environment, you need IP ranges that are reserved for servers and any devices that require a static IP address so we gonna give it a range from 51 - 151

![Image32.png](/Attachment/Image32.png)

Do not exclude any IPS skip also keep the lease duration the same 
Now we configure the default gateway DNS server and WINS server

![Image33](/Attachment/Image33.png)

![[Image34.png]](/Attachment/Image34.png)

![[Image35.png]](/Attachment/Image35.png)

Now we can see a pool in the address pool

![[Image36.png]](/Attachment/Image36.png)

## DNS 

First, we need to set up a reverse zone by going to _tools>DNS_ and right-clicking on reverse lookup zones then clicking on create a new zone skip everything till you come to the Network ID part and enter our network IP 10.0.20

![[Image37.png]]
Now we can add our devices manually but we are going to do the firewall only and if we needed we can do new entries later go to _Forward lookup Zones > evil.corp_ and right click and _Net host (A or AAAA)_
Enter the name and the IP address and check the _create associate pointer_ 

![[Image38.png]](/Attachment/Image38.png)

TADAAAA Now we can use nslookup to check if everything is right  

![[Image39.png]](/Attachment/Image39.png)

;)

## Static Routing 

We need the DC route for the endpoints so we can access SIEM on specific ports to have access to the logs later soooo now we have to set up static routing go to _Tools>Routing and remote access_ right click the DC (local) and select configure and enable routing and remote access select _Custom_ and then _LAN routing_
now we go to _EDC01> IPv4 > Static routes_ and add a route toward the SIEM IP which is going to be 10.0.50.55 

![[Image40.png]](/Attachment/Image40.png)

later we will give this IP to our SIEM 

## Users and Groups

for our users we gonna use bad designs and security practices like weak passwords and other poor practices, Microsoft does enforce some practices by default so we have to disable them  first 

after we can go to _tools>active directory users and Computer_ right click on Users and then click on _New>User_. type first and last name and user logon name I will make 6 users we gonna need them later 

![[Image41.png]](/Attachment/Image41.png)

after that we should make a new Organizational unit and name it Groups and inside of it we are creating 3 groups or more: HR, Marketing, and IT OPS 

![[Image42.png]](/Attachment/Image42.png)
![[Image43.png]](/Attachment/Image43.png)
now we assign the users to the groups and assign one of them as an Admin 

## Local Admin Policy

we have to set a local admin policy so we can have a local admin account to manage host devices 
go to _Server Manager_ and open _Group policy management_ create a new policy and name it Local Admin GPO
![[Image44.png]](/Attachment/Image44.png)

then edit it by going to _Configuration > Policies > Windows Settings > Security Settings > Restricted Groups_ and create a new group inside of it and name it **Local Administrators - Workstations**

![[Image45.png]](/Attachment/Image45.png)

then I added the IT ops group to it 

## Shared Folders

we are going to create the following folders in our C: drive  
```
"C:"
|-- "Evilcorp"
    |-- "HR"
    |-- "IT OPS"
    |-- "Marketing"
```

After that, we are going to give the right permissions to Every group all of them being read/write to the correct folder 

![[Image46.png]](/Attachment/Image46.png)

Done
# References 

[Building Blue Team Home Lab Part 6 - Corporate LAN (Domain Controller) | facyber](https://facyber.me/posts/blue-team-lab-guide-part-6/)
