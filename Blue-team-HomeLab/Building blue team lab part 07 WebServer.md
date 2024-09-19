2024-09-16 23:10

Tags:[[Projects]], [[BlueTeamLab]]

# Ubuntu Installation and configuration

I am going to use Ubuntu Server 22.04 LTS for this server and i am going to install DVWA on it 

requirements :
1. dual core Processor (2 GHz)
2. 4GB RAM
3. 25GB or more disk space
4. NAT interface (to install DWVA web server later we are going to change it)


the installation is simple we are going with the default 

![[Image57.png]]

if you want to access it through SSH do not forget to download OpenSSH I am not going to do that so i will skip it (note: i will regret it later :) ...)

![[Image58.png]]
 
 now we have a server set up and ready 
 ![[image59.png]]
 
 we can run the `apt update` now 
 
![[image60.png]]

Now we are going to use the following commands to install the required tools that we are going to use and then install DVWA into the /var/www/html file 

```
sudo apt install -y apache2 mariadb-server mariadb-client php php-mysqli php-gd libapache2-mod-php nano unzip fping
cd /var/www/html
sudo wget https://github.com/digininja/DVWA/archive/master.zip
sudo unzip master.zip
mv DVWA-master DVWA
```

![[image61.png]]

Now we can change the network interface to vmnet 20
after that we are going to use the following command

`sudo nano /etc/netplan/01-network-config.yaml`

to create and edit the config file 
you need to put this inside it 

```
network:
    version: 2
    renderer: networkd
    ethernets:
        ens33:
            addresses:
                - 10.0.20.10/24
            nameservers:
                addresses: [10.0.20.254]
            routes:
                - to: default
                  via: 10.0.20.254
```

after that you have to change the permissions on the file using the following command 
`sudo chmod 600 /etc/netplan/01-network-config.yaml`
after that you can use `sudo netplan apply`
now its set use `ip a` to make sure it worked

## DVWA Installation 

first we need to start the apache service using `systemctl start apache2` and  `systemctl status apache2` to check if it started 
![[image62.png]]
after that we should use this command to change the name of the config file `cp /config/config.inc.php.dist config/config.inc.php` 
after that we want to connect to the mariaDB using `mariadb` and create a new user by executing the following command  `create database dvwa;` then `creat user dvwa@localhost identified by 'p@ssw0rd;` and `grant all on dvwa.* to dvwa@loacalhost` finally `flush privileges;` and `exit` to close the connection 

![[image63.png]]

if you want to change the user you have to change it in the *config.inc.php* file too 
i did not change it so i had to make sure that i got the password and the user correct or else it wont work, after creating the user make sure to try and log in with it by using `mysql -u dvwa -pp@ssw0rd` after that we can go to *`http://10.0.20.10/setup.php`* and click on `create/reset database` 

![[image64.png]]

now we should be able to visit *`http://10.0.20.10/DVWA/`*  
![[image65.png]]

login using username : *admin* password : *password*

## Other DVWA configuration 

on the setup page we can see that we need to change some file permissions and some settings to allow some of the labs so let's do that using this commands to change the permissions 

```
chmod 777 /var/www/html/DVWA/hackable/uploads/
chmod 777 /var/www/html/DVWA/config
```

and we should edit the php config file which we can find at /etc/php/x.x/apache2/php.ini
and allowing both *allow_url_fopen* and *allow_url_include* 

## Apache configuration 

now i want to set up DNS for my web application first i will change the Apache config file name and edit it :
```
cd /etc/apache
cp sites-available/000-default.conf sites-available/system.evil.corp.conf
cd sites-available
nano system.evil.corp.conf
```

![[image67.png]]

now we enable the new configuration by: 

```
a2ensite system.evil.corp.conf
a2dissite 000-default.conf
systemctl restart apache2
```

![[image68.png]]

now we edit the */etc/hosts* file and add our domain it should look like this after editing 

![[image69.png]]

and on my host machine we have to edit the hosts file too on windows you can find it on *`C:\Windows\System32\drivers\etc\hosts`* open the notepad as administrator and edit that file by adding the IP of the website now it should look like this 

![[image70.png]]

now we can access it by the FQDN  from our host machine

![[image71.png]]

## Active Directory configuration 

now we should configure the AD and add a new host with the correct IP 

![[image72.png]]

now we can access it from anywhere in the LAN network 

![[image73.png]]

Done
# References 

[Building Blue Team Home Lab Part 8 - Web Server | facyber](https://facyber.me/posts/blue-team-lab-guide-part-8/)
