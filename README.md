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

11) Create a New Employee Account and Assign Domain Admins Group

Log into DC-1:

Use mydomain.com\labuser credentials to log into DC-1.
Create the Employee Account:

12) Open Active Directory Users and Computers (ADUC).

<p>
<img src="https://imgur.com/58gmOz3.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
13) Right click mydomain.com > New > Organizational Unit > Create _EMPLOYEES > Create _ADMINS 
  **ATTENTION** 
  The picture at the bottom is the results of create both the employees and admins
<p>
<img src="https://imgur.com/4T2NbOP.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
 
14) Right-click on _ADMIN and select New > User.
Enter the following details:
First Name: Jane
Last Name: Doe
Username: jane_admin
Click Next and enter the password: Cyberlab123!
Uncheck User must change password at next logon and check Password never expires if desired.
Click Next, then Finish.
Add jane_admin to the Domain Admins Security Group:
In ADUC, locate the Domain Admins security group.
Right-click on Domain Admins and select Properties.
Go to the Members tab and click Add.
Enter jane_admin and click Check Names.
Click OK to add jane_admin to the group.
<p>
<img src="https://imgur.com/V6sPD0K.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
  <p>
<img src="https://imgur.com/C7vmAqy.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
  <p>
<img src="https://imgur.com/2s32Bq2.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
Log Out and Log In as jane_admin:

15) Log out of DC-1, and close the connection.
Log back into DC-1 using the following credentials:
Username: mydomain.com\jane_admin
Password: Cyberlab123!
Use jane_admin as Your Admin Account:
  <p>
<img src="https://imgur.com/MiUJahG.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>

*ATTENTION* --------> From now on, use the jane_admin account for administrative tasks on DC-1.

16) Join Client-1 to the Domain and Organize in Active Directory

Log into Client-1 as the Local Admin (labuser):

  <p>
<img src="https://imgur.com/undefined.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>

  <p>
<img src="https://imgur.com/undefined.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
  
17) Use Remote Desktop (RDP) to connect to Client-1.
Log in with the original local admin credentials:
Username: labuser
Password: Cyberlab123!
Join Client-1 to the Domain:

Open the System Properties:
Right-click on This PC or My Computer, then select Properties > Advanced System Settings.
Go to the Computer Name tab and click Change.
Select Domain, enter the domain name (e.g., mydomain.com), and click OK.
Provide domain admin credentials (e.g., mydomain.com\jane_admin, password Cyberlab123!) when prompted.
Restart Client-1 to complete the domain join process.
Verify Client-1 in ADUC:
  <p>
<img src="https://imgur.com/d5PqzEV.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
    <p>
<img src="https://imgur.com/1RnSnQx.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
    <p>
<img src="https://imgur.com/undefined.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
    <p>
<img src="https://imgur.com/BSRQojH.png" height="80%" width="80%" alt="Azure Networking Steps"/>
</p>
<p>
18) Log into DC-1 as mydomain.com\jane_admin.
Open Active Directory Users and Computers (ADUC).
Navigate to the Computers container to confirm that Client-1 appears in the list.
Organize Client-1 in ADUC:

Create a new Organizational Unit (OU):
In ADUC, right-click on the domain (e.g., mydomain.com) and select New > Organizational Unit.
Name the new OU _CLIENTS and click OK.
Move Client-1 into the new OU:
Locate Client-1 in the Computers container.
Right-click on Client-1 and select Move.
Choose the _CLIENTS OU and click OK.
Confirm Organization:

Verify that Client-1 now appears under the _CLIENTS OU in ADUC.
