2024-09-19 15:14



# Security Onion installation 


We can Download the Iso for the security Onion from [here](https://github.com/Security-Onion-Solutions/securityonion/blob/master/VERIFY_ISO.md) after downloading it we should set up the virtual machine with the following specifications: 

1. 6GB RAM
2. 200GB or more HDD
3. 8 cores would be good
4. vmnet50
5. vmnet88(We Should add it to the firewall too for port mirroring)

Now start the machine and follow these steps :
We should choose STANDALONE

![[image77.png]](/Attachment/image77.png)

Accept and skip everything until you reach the hostname which can be anything I am going to name it ersiem (evil remote SIEM) note that this is not the FQDN :

![[image79.png]](/Attachment/image79.png)

select the first interface which should be the Vmnat50, The second interface is the one used for SPAN (port mirroring), to get the network traffic, and this interface will **not** have an IP address.

![[image80.png]](/Attachment/image80.png)

Now we should set up the static IP address by selecting static and then typing our IP

![[image81.png]](/Attachment/image81.png)

here we should type the IP which in my case is 10.0.50.55/24

![[image82.png]](/Attachment/image82.png)

the Gateway which is 10.0.50.254

![[image83.png]](/Attachment/image83.png)

Then continue with the default options (**Standard** manager installation, **Direct** internet connection) The next option is to choose how this manager will be installed. We will set **Standard** and continue.

Security Onion will start checking some scripts, it will not take much time.

install Zeek or Suricata wouldn't matter 

![[image84.png]](/Attachment/image84.png)

Now we should set up the admin soc user in my case socadmin@evil.corp also set up the password 

![[image85.png]](/Attachment/image.png)

Now we can set the FQDN by selecting Other 

![[image86.png]](/Attachment/image86.png)

type the FQDN that you want to use in my case it's ersiem.evil.corp we are going to access the web interface by this URL later 

![[image87.png]](/Attachment/image87.png)

Now we should allow the so-allow by pressing yes and then entering the home network gateway which you can check by doing `ipconfig` or `ifconfig` on your host machine in my case its 192.168.1.1/24

![[image88.png]](/Attachment/image88.png)

tadaaaa everything is set up now the machine should reboot and the interface should be accessible from our security vlan

![[image89.png]](/Attachment/image89.png)

if the URL is not working we should connect to the machine via SSH and try `sudo so-status`
and make sure that all dockers are running if not we should wait a bit until we can see all of them running 

![[image90.png]](/Attachment/image90.png)


wait for all containers to run and make sure to add the machine to the hosts file or you won't be able to access it from your host machine 

![[image91.png]](/Attachment/image91.png)

now we can access the web interface using the email address that we assigned for the admin and the password

![[image92.png]](/Attachment/image92.png)

## pfSense Port Mirroring 
Port mirroring allows you to get all network traffic packets that one network interface receives, and copy them to another interface, without interfering with the network traffic
Now we can configure the port mirroring by going to _Interfaces > assignment > add a new_
after that, we can change the name and enable it 

![[image93.png]](/Attachment/image93.png)

Now we should go to _Interfaces > Bridges > edit_ and create a new bridge by selecting both VLAN20 and VLAN50 and then selecting the span port as the new port that we created above 
Port mirroring will allow us to receive all the traffic that is going to VLAN20

![[image95.png]](/Attachment/image95.png)

## Basic testing 

Now we can start testing it by performing some attacks/scans on the DVWA web server we should perform them from the external network machine   

1. we can ping the web server using `ping 10.0.20.10` which is going to raise a low severity alert *GPL ICMP_INFO ping *NIX*
2. `Nmap -sV 10.0.20.10` a simple Nmap scan which would raise a few alerts with varying severities such as *ET SCAN Possible Nmap User-Agent observed, ET SCAN suspicions inbound to port 5432* 
3. `sqlmap -u http://10.0.20.10/` simple sqlmap scan which will fail but it will trigger an alert with high severity 

![[image94.png]](/Attachment/image94.png)

## Summery 

we configured security onion and configured a span port on pfSense, performed a few attacks, and detected them using security onion alerts 
next, we can deploy HIDS agents and ship their logs to the security onion 
# References 

[Building Blue Team Home Lab Part 10 - SIEM Part 1 | facyber](https://facyber.me/posts/blue-team-lab-guide-part-10/)
