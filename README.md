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


<h2>Deployment and Configuration Steps</h2>

Step 1: Firstly, create two virtual machines. The first virtual machine will be the domain controller. Its name will be DC-1, and its image type will be Windows Server 2022. The Virtual Network (Vnet) that is now established should be noted since it will be required for our second virtual machine.
</p>

<p>
<img src="https://i.imgur.com/n75PPjD.png" height="80%" width="80%" alt="Virtual Machine DC-1"/>
</p>

Furthermore, make sure that DC-1's virtual Network Interface Card (NIC) Private IP address is set to static. To achieve this, navigate to the network settings of the DC-1, select networking, and then click the link next to network interface.
<br />

<p>
<img src="https://i.imgur.com/8idIkfJ.png" height="80%" width="80%" alt="NIC IP"/>
</p>

Next, choose ipconfig1 from the IP Configurations menu. To ensure that DC-1's IP address won't change, simply select Assignment from Dynamic to Static. 
</p>
<br />

<p>
<img src="https://i.imgur.com/sRLW3o7.png" height="80%" width="80%" alt="ip config"/>
</p>

<p>
<img src="https://i.imgur.com/sswneoz.png" height="80%" width="80%" alt="Static"/>
</p>
<br />

<p>
The 2nd virtual machine will represent the Client, labeled Client-1, and Windows 10 Pro will be the image type. Use the same Vnet and resource group as DC-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/UWHi1Xn.pngg" height="80%" width="80%" alt="Client 1"/>
</p>

<p>
<img src="https://i.imgur.com/GU4ySko.png" height="40%" width="40%" alt="Vnet"/>
</p>
<br />

<p>
Step 2: Access Client-1 by logging in with Microsoft Remote Desktop. You then want look for a command line, open it, and ping DC-1's private IP Address (in our instance, 10.0.0.4). Select ping - t 10.0.0.4. 
</p>

<p>
<img src="https://i.imgur.com/qTZZ0S7.png" height="80%" width="80%" alt="timeout"/>
</p>

<p>
The request is timing out due to the firewall. On DC-1's local Windows firewall, we must activate ICMPv4 to fix issue. Utilizing Microsoft Remote Desktop, log in to DC-1, begin by selecting Start and Windows Administrative Tool, and then select Windows Defender Firewall with Advanced Security -Inbound rules. Additionally, enable the two inbound rules for Core Networking Diagnostics ICMPv4 by locating it in the list of protocols.
</p>
<br />

<p>
<img src="https://i.imgur.com/1dPUDFU.png" height="80%" width="80%" alt="Windows Defender Firewall"/>
</p>

<p>
<img src="https://i.imgur.com/3cjb7w8.png" height="80%" width="80%" alt="Enabling ICMPv4"/>
</p>
<br />
<p>
Finally, log back into Client-1, and the command line will begin to successfully ping DC-1.
</p>

<p>
<img src="https://i.imgur.com/btWubBB.png" height="80%" width="80%" alt="Replies from ICMPv4"/>
</p>
<br />

<p>
Step 3: Return to DC-1 and select "Add roles and features" in Server Manager after logging back in. Follow the guidelines. Select "Active Directory Domain Services at Server Roles.  Then click on Add Features next. Finish installation by selecting next.
</p>

<p>
<img src="https://i.imgur.com/htCmnhr.png" height="80%" width="80%" alt="Installing AD"/>
</p>

<p>
<img src="https://i.imgur.com/ZidjxWG.png" height="80%" width="80%" alt="Active Directory Domain Services"/>
</p>

<p>
Afterwards, click on the flag in the top right corner of the Server Manager Dashboard. Then select, “Promote this server to a domain controller”. 
</p>

<p>
<img src="https://i.imgur.com/Pj3CXAs.png" height="80%" width="80%" alt="Promote Server"/>
</p>

<p>
Additionally, choose "Add a new forest" - "Root domain name: mydomain.com", then choose "Next" to Create password, and lastly, select "Next" and adhere to the instructions, then click Install to complete the process. Ultimately, DC-1 will restart itself; as a result, log back in as user: mydomain.com\labuser..
</p>

<p>
<img src="https://i.imgur.com/7VCyPN4.png" height="80%" width="80%" alt="Adding a new forest"/>
</p>

<p>
<img src="https://i.imgur.com/DvwjxdK.png" height="80%" width="80%" alt="mydomain labuser"/>
</p>
<br />

<p>
Step 4: Open Server Manager on DC-1, choose tools in the upper right corner, and then choose Active Directory Users and Computers. Right-click MyDomain.com, choose "New," and then "Organizational Unit" (We will be creating 2 folders.)
</p>
  
<p>
<img src="https://i.imgur.com/xVJWHLt.png" height="80%" width="80%" alt="ADUC"/>
</p>

<p>
<img src="https://i.imgur.com/p0BZRX8.png" height="80%" width="80%" alt="New OU"/>
</p>

<p>
Assign one the name _EMPLOYEES and the other _ADMINS. The new organizational units will be sorted to the top if you right-click mydomain.com and choose refresh. Go to the _ADMINS organizational unit and use the right-click menu to pick New, then User. Once that is done, you must enter the following data: First/Last name: jane doe and user login name: jane_ admin. After that, choose next, enter a password, then uncheck every box, click next, and finally select finish. 
</p>

<p>
<img src="https://i.imgur.com/GzCzqPN.png" height="80%" width="80%" alt="New User"/>
</p>

<p>
<img src="https://i.imgur.com/9yJNdiT.png" height="80%" width="80%" alt="Admin User"/>
</p>
  
<p>
Furthermore, go to _ADMINS organizational unit and right click Jane doe then select properties. Click the "member of" tab and select Add then type in domain admins then select Check Names and select OK and then Apply. Log out of DC-1 as "labuser" and log back in as “mydomain.com\jane_admin”
</p>

<p>
<img src="https://i.imgur.com/5nXQSeo.png" height="80%" width="80%" alt="User Properties"/>
</p>

<p>
<img src="https://i.imgur.com/3OIM1ek.png" height="80%" width="80%" alt="Domain Admin"/>
</p>

<p>
<img src="https://i.imgur.com/rWxqqAx.png" height="80%" width="80%" alt="mydomainuser"/>
</p>
<br />

<p>
Step 5: Go back to Client-1 Virtual Machine via the Azure interface. Select Networking from the menu on the left, click the link next to the NIC, choose DNS server, then click Custom. Type in the private IP address of DC-1 and click Save. Once the update is complete, choose restart and then click yes.
</p>

<p>
<img src="https://i.imgur.com/MdHiiaS.png" height="80%" width="80%" alt="Client-1 DNS"/>
</p>

<p>
<img src="https://i.imgur.com/MpUR4zK.png" height="80%" width="80%" alt="Client-1 using DC-1 Private IP Address"/>
</p>

<p>
<img src="https://i.imgur.com/4TJFGYJ.png" height="80%" width="80%" alt="Restart Client-1"/>
</p>

<p>
Re-login to Client-1 using Microsoft Remote Desktop as the original local admin (labuser), and then choose System from right clicking on the start menu. Choose “Rename this PC” (advanced), then click “Change”. Next, choose “Under Member of”. Finally, type “mydomain.com” into the domain field and click OK. The username is mydomain.com\jane_ admin. Select OK after entering the password. At last, restart the virtual machine.
</p>

<p>
<img src="https://i.imgur.com/sSBuXQa.png" height="80%" width="80%" alt="Rename this PC"/>
</p>

<p>
<img src="https://i.imgur.com/Bt1gq03.png" height="80%" width="80%" alt="Domain changes"/>
</p>
<br />

<p>
Step 6: Use mydomain.com\jane_ admin to log back into Client-1, and then use the right-click menu to choose System from the Start menu. Select "Remote Desktop"
</p>

<p>
<img src="https://i.imgur.com/nEqlc4V.png" height="80%" width="80%" alt="Remote Desktop"/>
</p>

<p>
Click "User Accounts", choose "Select users who can remotely access this PC", select Add. Type: Domain Users, and click check Names, and then click OK. Select "OK" again. 
</p>

<p>
<img src="https://i.imgur.com/xsO0nTa.png" height="80%" width="48%" alt="Domain Users"/>
</p>
<br />

<p>
Step 7: Log back into DC-1 as jane_admin, search for Powershell_ ise, right-click on it, and select "Open as administrator." 
</p
  
<p>
<img src="https://i.imgur.com/3GBhYwA.png" height="80%" width="80%" alt="Powershell_ise"/>
</p>

<p>
Choose "new script" from the menu in the upper left, then paste the script's content there. To run the script, click the green arrow button located at the top Centre.
</p>
 
<p>
<img src="https://i.imgur.com/js2WD56.png" height="80%" width="80%" alt="New Script"/>
</p>

<p>
<img src="https://i.imgur.com/DMn4a6r.png" height="80%" width="80%" alt="Run Script"/>
</p>
  
<p>
Come back to Active Directory Users and Computers after the users have been created. Click _EMPLOYEES after selecting mydomain.com. Here, you may see every account that has ever been made.
</p>

<p>
<img src="https://i.imgur.com/Kq3VMMG.png" height="80%" width="80%" alt="Users"/>
</p>
  
<p>
Using one of the newly created users, let's log in to Client-1 (in our instance "dov.judu", password will be Password1)
  
<p>
<img src="https://i.imgur.com/zJ2pE4c.png" height="80%" width="80%" alt="User login"/>
</p>

<p>
Congratulations!🎉 You have set up on-premises Active Directory and generated users inside of Azure Virtual Machines.
</p>

