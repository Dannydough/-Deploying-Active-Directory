<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic


<h2>Actions and Observations</h2>

Install and Configure Active Directory on DC-1

1) Log into DC-1:

Use Remote Desktop (RDP) to access the DC-1 VM. Install Active Directory Domain Services (AD DS) by going to Server Manager within the virtual machine. Open Server Manager and click Add Roles and Features

<p>
<img src="https://imgur.com/Dd5vn1i.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>

2) Select Role-based or feature-based installation.
Choose Active Directory Domain Services, and proceed with the installation.
Promote DC-1 to a Domain Controller:

<p>
<img src="https://imgur.com/01cb1Ky.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>

<p>
<img src="https://imgur.com/H78Ye1b.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
  
<p>
<img src="https://imgur.com/sCy8hJo.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>

4) After installation, click the notification flag in Server Manager and select Promote this server to a domain controller.
Choose Add a new forest and enter the domain name (e.g., mydomain.com).
Set the Directory Services Restore Mode (DSRM) password, and complete the configuration wizard.
Restart DC-1:

<p>
<img src="https://imgur.com/zMnomgz.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>

<p>
<img src="https://imgur.com/sBHU6AZ.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
  
<p>
<img src="https://imgur.com/aqf6uyJ.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>

<p>
<img src="https://imgur.com/ddJ4wSM.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
  
5) Allow the server to restart automatically after the domain configuration.
Log Back into DC-1:

Use the following credentials to log in:
Username: mydomain.com\labuser
Password: Cyberlab123!

<p>
<img src="https://imgur.com/2c3UAUe.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>

6) Create Organizational Units (OUs) in Active Directory

7) Access Active Directory Users and Computers (ADUC):

Log into DC-1 using mydomain.com\labuser.
Open Active Directory Users and Computers from the Start menu or by running dsa.msc in the Run dialog (Win + R).
Create the “_EMPLOYEES” Organizational Unit:
<p>
<img src="https://imgur.com/s9jveoN.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>

<p>
<img src="https://imgur.com/XHT3vpR.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
  
8) Right-click on the domain name (e.g., mydomain.com) in the left-hand pane.
Select New > Organizational Unit.
Enter the name _EMPLOYEES and click OK.
Create the “_ADMINS” Organizational Unit:
<p>
<img src="https://imgur.com/qrp25vx.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
9) Right-click on the domain name again.
Select New > Organizational Unit.
Enter the name _ADMINS and click OK.
Verify Creation:
<p>
<img src="https://imgur.com/XhuzwCM.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
10) Ensure both OUs (_EMPLOYEES and _ADMINS) appear under the domain in the ADUC console.