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

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users
  

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First we must set up our resources in Azure, starting with the domain controller. To do that we sould create a resource group in azure which we should be prompted to do when we create a new virtual machine. When nameing the virtual machine we should call it DC-1 and we we can name the resource group whatever we want (ex: AD). Make sure to use a Windows server as the image and and pick a size of at least two cpus to make sure we run smoothly. We can use whatever we prefer for the username and password as long as we remember it for later. Once we check the boxes and click create it should automatically create our virtual network and subnet.

  Next we need to create the client by making another virtual machine. Make sure that it is in the same resource group as the preivous VM we created. Also make sure it is using the same region and image. Our username and passwpord can be whatever as long as we remember it. On the networking tab make sure that we are on the same virtual network that we have created previously. As long as the boxes are ticked we should now be good to create. 

  Once these are made we should be able to go back to DC-1 and scroll to the networking tab and click on the Network Interface section. From there we can go to the IP configuration and we should be able to see that it is set up to be dynamic, we want to change this to static.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Our next step will be to ensure connectivity between the client and the domain controller. To do this we will need to first login on the client vm. We get the the ip address from the azure menu for client one and then plug it into remote desktop connection and then the username and password we saved from earlier. Next we are going to need DC-1's private IP address which we should be able to get from yhe azure menu.
  
  Once we have this we can go back to the client vm and open the command prompt. we can then type ping -t and then the private IP address we got from DC-1. This should result in a timeout error. In order to fix this we need to allow the client's IP address through the DC-1's firewall. To do this we need to connect to DC-1 the same way we connected to the client vm. Once we are connected we can type wf.msc into the windows search section in order to get the firewall. From here we should head to the inbound rules section and find the ICMPv4 protocols. We should enable both the echo requests networking diagnostics. Once these are enabled we should be able to tab over to the client and see that the ping request is now working. To stop the pings just hit control and c in the command prompt window.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

