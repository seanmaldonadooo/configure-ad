<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create the Domain Controller VM
- Set Domain Controller’s NIC Private IP address to be static
- Create the Client VM (Windows 10) named “Client-1”
- Enable ICMPv4
- 
- 
- 
- 
- 

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://github.com/seanmaldonadooo/configure-ad/assets/149026184/9be67066-1d19-4569-b14d-a7188a76f61c" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a domain controller VM named "DC-1" (Windows Server 2022). Take note of the Resource Group and Virtual Network (Vnet) that is created while creating.
</p>
<br />

<p>
<img src="https://github.com/seanmaldonadooo/configure-ad/assets/149026184/2ddbd1eb-ce1f-404f-b4f5-498089d52ac3" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Set Domain Controller’s NIC Private IP address to be static. After this, create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created with the first Domain Controller VM. Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher)
</p>
<br />

<p>
<img src="https://github.com/seanmaldonadooo/configure-ad/assets/149026184/055c053c-97b0-49ea-b82f-bf52b804743e" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping). Notice that the request times out.
</p>
<br />

<p>
<img src="https://github.com/seanmaldonadooo/configure-ad/assets/149026184/fcbaaad5-9cbd-4915-8ca9-64cf40a272ab" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall.
</p>
<br />

<p>
<img src="https://github.com/seanmaldonadooo/configure-ad/assets/149026184/fc63d7d2-4202-4862-b7d1-0f986af22cd2" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Check back at Client-1 to see the ping succeed.
</p>
<br />

<p>
<img src="https://github.com/seanmaldonadooo/configure-ad/assets/149026184/75b4c19f-b612-4d85-9cdb-ce978085341c" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to DC-1 and install Active Directory Domain Services Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is) Restart and then log back into DC-1 as user: mydomain.com\labuser
</p>
<br />

<p>
<img src="https://github.com/seanmaldonadooo/configure-ad/assets/149026184/567aa52d-bc19-4335-9959-70cba799fd7b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”. Create a new OU named “_ADMINS” 
</p>
<br />

<p>
<img src="https://github.com/seanmaldonadooo/configure-ad/assets/149026184/094af85a-cd88-4186-a018-1965e07aaf52" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”. Add jane_admin to the “Domain Admins” Security Group. Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”. Use jane_admin as your admin account from now on.
</p>
<br />

<p>
<img src="https://github.com/seanmaldonadooo/configure-ad/assets/149026184/9679ee12-bf88-4ed0-8361-bbe0b09b4336" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address.
From the Azure Portal, restart Client-1.
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart). Also, afterwards make sure to rename the Client-1 PC and join it to the domain. Then re-login as the original lab user “mydomain.com\labuser”. Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.
</p>
<br />

<p>
<img src="https://github.com/seanmaldonadooo/configure-ad/assets/149026184/5b81cc5f-8ef1-4f2a-9bca-e1043ad6d666" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log into Client-1 as mydomain.com\jane_admin and open system properties.
Click “Remote Desktop”.
Allow “domain users” access to remote desktop.
You can now log into Client-1 as a normal, non-administrative user now.
</p>
<br />

<p>
<img src="https://github.com/seanmaldonadooo/configure-ad/assets/149026184/05096471-7009-44f7-8e07-9ebfca5ace8d" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to DC-1 as jane_admin.
Open PowerShell_ise as an administrator.
Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).
Run the script and observe the accounts being created.
When finished, open ADUC and observe the accounts in the appropriate OU.
attempt to log into Client-1 with one of the accounts (take note of the password in the script).

</p>
<br />
