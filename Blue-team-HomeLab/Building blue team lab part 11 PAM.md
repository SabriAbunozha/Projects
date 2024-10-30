2024-10-30 16:06


# Privileged Access Management (PAM)

**Privileged Access Management (PAM)** is a security framework that helps organizations control and monitor access to critical systems, applications, and sensitive data. PAM solutions are designed to restrict and log access for privileged accounts—those with elevated permissions—preventing unauthorized access and potential misuse. PAM is essential for secure IT environments, especially where sensitive data and critical infrastructure are involved, as it provides a secure gateway for privileged access and helps mitigate insider threats.
# JumpServer

**JumpServer** is an open-source Privileged Access Management (PAM) tool that gives DevOps and IT teams secure, on-demand access to SSH, RDP, Kubernetes (K8s), remote applications, and database endpoints through a web browser interface. As an alternative to proprietary PAM solutions like CyberArk, JumpServer provides similar secure access control with an open-source license, making it an excellent choice for our lab.
## System Requirements

To install and configure JumpServer, we first need to set up an Ubuntu server with the following specifications:

- **40 GB Hard Disk**
- **4-Core Processor**
- **8 GB RAM**
- **VLAN 50**: We can access it from outside our network later.

now just continue the default installation 

## Network Configuration 

I forgot to plug it into VLAN 50 on setup so I did it after so we have to edit the netplan file which is in my case */etc/netplan/50-cloud-init.yaml* 

![[Image106.png]](/Attachment/Image106.png)

After we have used `Sudo netplan apply` Now we have access to the network while being on our VLAN 50
## Installing JumpServer

To install JumpServer, execute the following command. This will handle the installation and initial configuration: 

```
curl -sSL https://github.com/jumpserver/jumpserver/releases/latest/download/quick_start.sh | bash

```
> **Note**: Before starting the installation, you must switch to the `root` user by running `sudo su`.

![[Image107.png]](/Attachment/Image107.png)
![[Image108.png]](/Attachment/Image108.png)


After installation, JumpServer will be accessible at http://10.0.50.77:80.
Use the following default credentials to log in for the first time:

Username: `admin`
Password: `ChangeMe`

## Initial Setup

1. **First Login and Password Change**  
    after logging in, you’ll be prompted to change the default admin password to enhance security.

2. **Enable MFA (Multi-Factor Authentication)**  
    Since the server will be exposed to network traffic, it’s recommended to enable MFA for additional security. To enable MFA, go to the **Settings > MFA** section and follow the prompts.

![[Image109.png]](/Attachment/Image109.png)


you will be prompted to change the password of the admin user 

![[Image110.png]](/Attachment/Image110.png)
![[Image111.png]](/Attachment/Image111.png)


3. **Add Users**  
	To add new users who will access specific resources through JumpServer, navigate to **Users > Add User**. Here, you can specify usernames, roles, and permissions for each user.

![[Image112.png]](/Attachment/Image112.png)


4. **Add Assets (Servers and Endpoints)**  
	After users are added, you can register servers, databases, or remote applications as assets within JumpServer.
	- Go to **Assets > Create** and fill in the required details for each asset, such as IP address, hostname, and protocols (e.g., SSH for Linux servers or RDP for Windows servers).
	- Define the credentials (username and password) that users will need to authenticate to these assets.

![[Image113.png]](/Attachment/Image113.png)


5. **Enable SSH and RDP on Assets**  
    Ensure SSH (for Linux servers) and RDP (for Windows servers) are enabled on the target machines so that JumpServer can initiate connections.

6. **Set Up Authorization**  
    Access control is managed by creating authorization rules that dictate which users or groups have access to which assets.
    - Navigate to **Authorization > Create** and specify the users and assets involved.
    - Define the level of access each user or group has (e.g., full access, read-only) to prevent unauthorized access.


![[Image114.png]](/Attachment/Image114.png)

## Testing Access

With the configuration complete, you can now test the setup:
1. Log in with a user account.
2. From the JumpServer web interface, select the asset you want to access.
3. JumpServer will establish the connection to the endpoint using SSH, RDP, or the relevant protocol, as per the asset's configuration.

![[Image115.png]](/Attachment/Image115.png)


You should now be successfully connected to the asset, with session activity being monitored and recorded as per JumpServer's configuration.

This setup ensures secure and auditable access to critical servers and applications in the home lab, helping you practice and test PAM in a controlled environment.
# References 

[Quickstart Guide - JumpServer](https://www.jumpserver.com/docs/quickstart)
