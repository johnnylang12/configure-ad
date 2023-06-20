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


<h2>Deployment and Configuration Steps</h2>

<p>
</p>
<p>
In this lab we will create two VMs in the same VNET. One will be a Domain Controller, the other will be a Client machine. Please see overview diagram below.
</p>
<br />
<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h3>Step 1: Create Virtual Machine & Resources</h3>
<p>
First we will setup two Virtual Machines. One will be "DC-1" and second will be "Client-1" <br/>
We will change the DC to a static IP because its offering Active Directory services to the client machine. The client machine will be joined to the domain. We will configure the DNS settings on the client machine, the client machine will use the DC as its DNS server. Ensure both VMs are in the same Vnet<br/>
DC-1 has to have a static Private IP Address. Client one will connect to DC-1 to ensure connectivity we will try to ping DC-1 from Client-1. 
 </p>
<img src="https://i.ibb.co/3zhmJ2C/Active-Directory-1.jpg" height="80%" width="60%" alt="AD-Lab"/>
<img src="https://i.ibb.co/DRQqNBc/Active-Directory-2.jpg" height="80%" width="60%" alt="AD-Lab"/>
<img src="https://i.ibb.co/fFWy7Qt/Active-Directory-3.jpg" height="80%" width="60%" alt="AD-Lab"/>
<img src="https://i.ibb.co/ZG9hYYg/Active-Directory-4.jpg" height="80%" width="60%" alt="AD-Lab"/>
<br/>
At first the ping will not work correctly. We have to enable ICMPv4 on the firewall on DC-1. 
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="80%" alt="Disk Sanitization Steps"/>
</p>
Now we can ping DC-1 successfully from Client-1.
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<h3>Step 2: Install Active Directory, Users & Computers</h3>
<p>
Now we will log back into DC-1 to install AD Users & Computers. Promote the VM to DC, setup a new forest as "mydomain.com" afterwards restart then log back into DC-1 as user: "mydomain.com\labuser". If executed properly, you should be able to run AD Users & Computers as shown below.
</p>
<img src="https://i.ibb.co/KxktnNT/Active-Directory-5.jpg" height="80%" width="60%" alt="Active Directory"/>
<img src="https://i.ibb.co/BjZ4gLn/Active-Directory-6.jpg" height="80%" width="60%" alt="Active Directory"/>


<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
We can start creating Organizational Units (OU). Let's first create an OU named _EMPLOYEES. Create another OU named _ADMINS. In order to do that, right click on the domain area. Select new->Organizational Unit and fill out the field. Then click inside of your OU and right click, select new and select user and fill out the information for your new user. The user should be named Jane Doe, she is going to be an Admin so her username will be Jane_admin. Lastly add Jane to the domain admins security group. 
</p>
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
From now on you can use Jane_admin as the administrator account. Now we will join Client-1 to the domain (mydomain.com). From the azure portal we will change client-1's DNS settings to the DC's Private IP address. After you do that restart Client-1 from within the Azure portal. Our picture below shows verification that client-1 is using DC-1 as its DNS. 
</p>
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
We have to join Client-1 to the domain. Navigate to your system settings and go to "About." Select Rename this PC (advanced). From there select to change the domain. Enter "mydomain.com." Enter your credentials from mydomain.com\labuser. Your computer will restart and then client-1 will be a part of mydomain.com.
</p>
<br />

<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<p>
Client-1 is now a part of the domain. Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open System Properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing those steps you should be able to log into Client-1 as a normal user.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly to verify that normal users can RDP into Client-1, we will use a script to generate thousands of users into the domain. We will input the script in powershell. After the users are created we will select one and RDP into Client-1.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
As you can see the Powershell script created a user with the username "bab.hubo" We were able to login to Client-1 with his credentials as a normal user. <br/>
That concludes this tutorial!
</p>
