2024-09-14 16:36

# Windows 7 VM

after downloading the Windows image I configured it with the following settings :

1. Memory 4Gb
2. Processors 1 CPU
3. HDD 40Gb
4. Network adapter VMnet20
### Windows firewall

Windows 7 blocks ICMP by default so we have to enable it so we can ping the machine 
we can enable it by going to _control panel > system and security > windows firewall > Advanced settings_ Then you can find *File and Printer Sharing (Echo Request - ICMPv4-In)* rules which there are two one for the private/public and one for the domain enable them both

![[Image47.png]](/Attachment/Image47.png)

### Computer Name

now we will change the name of the computer and give it a proper description to simulate real live corporate devices after naming it we can add it to the domain by checking the Domain and typing the domain name it will prompt us to login we can use the Administrator to log in

![[Image48.png]](/Attachment/Image48.png)

the machine will restart now we can use one of the accounts that we made earlier and log in with it 
if everything works you will see the background change to this 

![[Image49.png]](/Attachment/Image49.png)

now you can check the file sharing which is working too

![[Image50.png]](/Attachment/Image50.png)

we can ping the machine from the DC(domain controller) just to make sure 

![[Image51.png]](/Attachment/Image51.png)

## Windows 10

now we have to do the same thing here as ICMP is also disabled we are going to enable it.

![[Image52.png]](/Attachment/Image52.png)

after that we can go to _system > change settings_
to rename the machine and join the domain

![[Image53.png]](/Attachment/Image53.png)
![[Image54.png]](/Attachment/Image54.png)

I had a problem where I couldn't see the EDC01 machine i did some research and found out that I have to enable the function discovery resource publication on the server and make sure that it is set to start automatically after I did that the problem is fixed.  

![[Image55.png]](/Attachment/Image55.png)

and here it is after the modification we have successfully joined the domain and we have access to the files shared by the server 

![Image56](/Attachment/Image56.png)


# References 

[Building Blue Team Home Lab Part 7 - Corporate LAN (End Devices) | facyber](https://facyber.me/posts/blue-team-lab-guide-part-7/)
