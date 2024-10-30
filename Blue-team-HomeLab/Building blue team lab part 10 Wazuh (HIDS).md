2024-10-14 16:21

Tags:[[BlueTeamLab]], [[Projects]]

# Firewall changes

First we need to allow Wazuh traffic from the agents to SO Wazuh manager. using SSH we can login to the SO CLI and use `so-allow` command then we should select W *Wazuh agent* and we should add our VLAN 20 here to allow the traffic and then do the same thing for R *Wazuh registration service* 

![[Image96.png]]

Now from the pfSense we should set a rule to allow Wazuh TCP/UDP traffic on ports 1514-1515 towards the SIEM IP which is 10.10.50.55

![[Image99.png]]

Done now we can start adding agents 
# Deploying Wazuh agents 

## Host registration

Before installing the agents we need to register them in Wazuh manger using the command `so-wazuh-agent-manage` and selecting A *Add an agent* add the host name and the IP of the agent 
after that we should obtain the key for the agent by selecting E *Extract agent key* now we just repeat for every agent we have ( Linux server, AD, Windows 7 and 10) make sure to save every key because we need them later.

![[image100.png]]

## Windows installation 

For windows we will use Sysmon in combination with Wazuh so we need to Download [Sysmon - Sysinternals](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)  and we should Download the configuration from Neo23x0 's [GitHub](https://github.com/Neo23x0/sysmon-config/blob/master/sysmonconfig-trace.xml)  after downloading them both extract the Sysmon file and use this command to install it 
`sysmon64.exe -accepteula -i sysmonconfig-export.xml` 

![[Image97.png]]

Now from the SO SIEM web Dashboard we should Go to Downloads and Download the appropriate Agent install it and then we should add the authentication key and the Manager IP and save the changes 

![[Image98.png]]

Now we edit the configuration file by going to *View>View Config* and add this code to the config file right after the last localfile block then we should save and restart the agent 

![[Image101.png]]

## Linux Installation 

For Linux we only need the DEB file so download it from the SO dashboard after that use the following command `sudo dpkg -i wazuh-agent_3.13.1-1_amd64.deb` 

![[image.png]]

Also we should edit the config file and change the Manager IP address 

![[Image102.png]]

we also need to change client.keys file which we can find at */Var/ossec/etc/client.keys* we should extract the key and then using cyberchef decode it from base64

![[Image103.png]]

after saving both files we should use the following 

```
systemctl daemon-reload
systemctl enable wazuh-agent
systemctl start wazuh-agent
systemclt status wazuh-agent
```

## Data verification 

Now we could verify if we receive our logs in Security Onion by opening Kibana page form the main SO page and selecting Host select the first Wazuh module of host data and see if there is any logs 

![[Image104.png]]

Now select Sysmon module 

![[Image105.png]]

Tada!!! everything is working 
# References 

[Building Blue Team Home Lab Part 11 - SIEM Part 2 | facyber](https://facyber.me/posts/blue-team-lab-guide-part-11/)