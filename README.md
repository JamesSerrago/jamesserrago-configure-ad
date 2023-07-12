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
<p> Next we will have to setup active directory on the domain controller. Once we are tabbed over to domain controller we should open the server manager from the start menu if it is not already open. Go to add roles and features and keep clicking next until you see a list of unchecked boxes and make sure to check active directory domain services. Then click add services and next until it allows you to install. Once this finishes, a small exclamtion mark should appear in the corner of the server manager and once clicked it should prompt you with a line that say promate server to domain controller. Once this is clicked you should get another window and make sure to click add new forest and name the domain (ex:mydomain.com). Then make anothe password for it and clicking next until it is installed.

  Once this finishes it should disconnect you from the vm and we just need to reconnect. However since this is now a domain controller our username will be the name of our domain that we made up folloed by a / and then the user name we made up along with the password.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next up is to create a admin user account id in active directory. To do this this we must first navigate through active directory and create two new oragnization unit. The first one will be called _EMPLOYEES and the second will be called _ADMINS. We are naming them like this just so they are easier to recognise. From here we can head to the _ADMINS we just made and create a new user by right cliking and selecting that option. You will be prompted with a box that will ask for account details such as the first and last name associated with the account, which we can use whatever we want to fill. Same goes for password but for this make sure to uncheck "user must change password at next login" so we dont have to reset it later.

  In order to actuaslly make the account a admin account we will need to right click the account we made in the _ADMINS folder and go to properties. We can then go to the member of section and then add it the domain admins group. Once this is done we now log out of DC-1 and then log back in with our newly created admin account.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next up we will be joining our client account to the domain controller. So we first need to get DC-1's private IP address from the azure vm menu and copy that over to the dns servers menu of Client-1. Make sure to change it to custom and add the IP address you got from the domain controller into the dns server section. After doing this we wil be restarting client-1 from the azure menu in order to flush the dns cache. After it has restarted we now login using the original account we made and not the admin account we just made. Once logged in we can right click the start menu and go to settings. Go to rename this pc and then change. Select domain and type in the name of your domain, in our case it is mydomain.com. A domain change prompt should appear and this is where we will now login using the admin account we had just created. Make sure to put the name of your domain in front of the admin accounts name (ex:mydomain.com/adminaccount). The computer should now restart and be connected to the domain controller.
  
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Next we will be setting up the ability to remote desktop to non-administrative accounts on Client-1. So first we will have to be tabbed on Client-1 and right click the start menu and go to system. Go to remote desktop and then user accounts. This will bring up a window that allows us to allow any user to be able to do this. Instead of having to add every indidvidual user though, we can add a group with those permissions. For this we will use domain users to give everyone access.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally we can test a bunch of users and see if they can log in. So first we should go back to DC-1 and be logged in as our admin account. We should then search for powershell ISE and run it. Once oen we can create a new file and then copy and paste the following script into it.  
</p>
<br />

