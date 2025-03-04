<p align="center">
![!Prog1_Logo](https://github.com/user-attachments/assets/a786492b-5687-4d1e-84fe-6b1aaf04397f)

</p>

<h1>Deploying and Configuring Active Directory </h1>

This entry-level tutorial walks the user through a basic implementation of Active Directory within an Azure Virtual Network.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines & Virtual Networks)
- Remote Desktop
- Active Directory Domain Services
- PowerShell & Command Prompt

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro
</br>

<p>We’ll be using the following VMs created in an earlier project:</p>


- Cyberdyne-DC-1 – running Windows Server 2022, this will become our <i>Domain Controller</i>
- Cyberdyne-Client-1 – running Windows 10 Pro, this will be our <i>Client</i>
</br>

<h2>Part 1: Setting up the Environment – Deploying and Configuring Active Directory</h2>

<p>First, we’ll install Active Directory and set up our new Domain. In the process, we’ll create Organizational Units as well as our administrator account. Finally, we’ll link our virtual Client to our new domain and then configure Remote Desktop on the Client for our future user accounts.</p>
</br>

<b>Step 1 – Install Active Directory (AD) on a Virtual Server (Cyberdyne-DC-1) </b>
- Login to the Server using <i>Remote Desktop</i> (RDP) and open “Server Manager”. Find the link for “Add roles and features” and open it.

![P2_Deploy1 - Edit](https://github.com/user-attachments/assets/8d4a3b35-2108-44eb-8d53-e04e5c8433f2)

- Scroll through “Next” to verify the <i>destination server</i> is our Domain Controller VM (verify name and IP address).

![P2_Deploy1 50 - Edit](https://github.com/user-attachments/assets/f1ad64f0-0ed1-4cba-9d25-4d5600ef2c84)

- On the “Select server roles” page, check “Active Directory Domain Services” and select “Add features” on the popup menu. 

![P2_Deploy2 - Edit](https://github.com/user-attachments/assets/f822ec3d-987a-48fa-8054-a9af55ec43d8)
</br>
</br>
</br>
![P2_Deploy3 - Edit](https://github.com/user-attachments/assets/905daacf-29ff-4f94-a213-661c120a1209)

- Click through “Next” to “Confirm installation selections”. Make sure the box for “Restart the destination” is checked and click “Install”.
</br>

<b>Step 2 - Set up a new Domain </b>
- In the top right of the AD dashboard, click the <i>flag icon</i> and press the link labeled “Promote this server to a domain controller”.

![P2_Domain1 - 425wide](https://github.com/user-attachments/assets/d7df7d97-0a58-4fb6-912e-eaa76eabf247)

- In the popup window, select the radial “Add a new forest” and enter a name in the “Root domain name” field. Let’s call it “mydomain.com”. Add a password on the next page; for ease, we’ll use “Password1” (I wouldn’t recommend this in real life but it works for our purposes here!). 

![P2_Domain2 - 425wide](https://github.com/user-attachments/assets/5c91c85b-baa8-4bb8-ba69-8c9bd5a31d39)

- On the next page, make sure “Create DNS delegation” is unchecked and click through “Next” until you're given the “Install” option; go ahead and press “Install”. 

![P2_Domain3 - 425wide](https://github.com/user-attachments/assets/b430617b-7287-4b66-a13d-4bdd59e9bd89)
</br>

<b>Step 3 – Create <i>Organizational Units</i> within our new Domain </b>
- Restart the <i>Domain Controller</i> from the Azure dashboard and log back in thru RDP as user: “mydomain.com\labuser” (Password: Cyberdyne123 [from our virtual network" project]). 

- Once back in, press the “start” button; find the “Windows Administrative Tools” folder, and choose “Active Directory Users and Computers” in the drop-down menu. 

![P2_OU1 - 425wide](https://github.com/user-attachments/assets/8fb43fa0-7bc9-440f-a850-d2fbc1a5e042)

- Right-click on “mydomain” in the left column of the new window; choose “New” and “Organizational Unit”. Give it the name “_EMPLOYEES”.

![P2_OU2 - 425wide](https://github.com/user-attachments/assets/852ee771-788d-41e2-8b67-4e02c3bac3b2)
</br>
</br>
</br>
![P2_OU3 - 425wide](https://github.com/user-attachments/assets/3b468459-b4b9-4924-af79-3b06fc7da577)


- Do the same to create another <i>Organizational Unit</i> called “_ADMINS”. 
</br>

<b>Step 4 – Add an employee to "_ADMINS" to serve as our <i>Admin Account</i></b>

- Right-click on _ADMINS and choose “New”, then “User”. Fill in the new user’s information (Name – Miles Dyson, username – miles_dyson), then choose a password on the next page. <i>Make sure to uncheck the box “User must change password…”</i> before you “Finish”. 

- Next, we’ll add Miles to the <i>domain security group</i>. Right-click on his name and choose “Properties”

- In the Properties window, choose the “Member of” tab and choose “Add”. 

![P2_Admin1 5 - 425wide](https://github.com/user-attachments/assets/dc936571-76bb-4bfd-9f16-c89a94539b8f)


- In the new window, use “Check names” to find “Domain Admins”; click “Ok”. 

![P2_Admin2 1 - 425wide](https://github.com/user-attachments/assets/cee5acac-3b45-4d17-92d7-718bf1691bdf)

- Choose “Apply”. This account is now a <i>domain admin account</i>. We’ll use Miles as our <i>admin account</i> from now on. 
</br>

<b>Step 5 – Join our <i>Client VM</i> to our Domain</b>
- Log into the Client VM using RDP and open the “About this PC” window. On the right side, choose the link “Rename this PC (Advanced)”. On the “Computer name” tab, choose “Change”. Select the “Domain” radial and enter “mydomain.com”. Press “Ok”. 

![P2_Join1](https://github.com/user-attachments/assets/20273ed5-97df-4f73-bd8b-0699cd228007)

- In the next window, enter the user as “mydomain.com\miles_dyson” and enter the password you created for him earlier. “Ok”. Restart the Client from the Azure dashboard.

![P2_Join2](https://github.com/user-attachments/assets/4fbd2849-8106-4ea7-9ccd-3a09b0730162)

- To verify that the new user shows up in Active Directory, login to the Server and open “Active Directory Users and Computers”. Click on the “Computers” folder; you should see the Client VM listed in the box. 

![P2_Join3](https://github.com/user-attachments/assets/a12c3da9-f796-4cdc-ba08-3139cc3c2b0c)

- Finally, let’s create a new <i>Organizational Unit</i> called “_CLIENTS” using the procedure from Step 3. Once created, drag the Client VM into the new “_CLIENTS” folder. 
</br>

<b>Step 6 – Set up RDP within our <i>Client VM</i> for non-admin users </b>
- First, login to the <i>Client VM</i> as “mydomain.com\miles_dyson”.

- Once in, open the “About this PC” window and choose “Remote Desktop” from the right side. At the bottom of the next window, choose the link “Select users that can remotely access this PC”. Then choose “Add”.

![P2_RDP1](https://github.com/user-attachments/assets/e6274fff-1bfd-413c-a35b-ce69a06ca694)
</br>
</br>
</br>
![P2_RDP2](https://github.com/user-attachments/assets/8fcccf1e-ef80-499e-acfd-704cb8cdbed3)

- In new window, type “domain users” in the typing field and press “Check Names”. Press “ok”. This will allow all users to access this computer.

![P2_RDP3](https://github.com/user-attachments/assets/1fd2081e-ae62-4d9e-ac60-fcfa7d22d2f3)
</br>
</br>



<h2>Part 2: Adding Users and Demonstrating Common Troubleshooting Issues</h2>

<p>We can now create some users and configure access to their accounts. We’ll then demonstrate some basic troubleshooting by unlocking a user’s locked account as the administrator, as well as resetting their password. We’ll also introduce the security logs, which will allow you as the administrator to view account activity in detail.</p>
</br>

<b>Step 1 – Set up user accounts</b>
</br>

- Login to our Server as the admin (mydomain.com\miles_dyson). 
- You can create multiple user accounts manually or by running an appropriate script in PowerShell ISE. We’re going to attempt the latter using a script from <a href=https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1>Josh Madakor</a>. This particular script will create a series of completely random (and ridiculous!) names, all with the same password (Password1).
- Open PowerShell ISE as admin. Open a “New Script”; copy the script we fouind online into the field and run it.

![P2_User1](https://github.com/user-attachments/assets/87f30ec2-a9d6-451c-9f5c-0ccb9089f155)

- Open “Active Directory users and computers”, select the “_EMPLOYEES” folder, and you should be able to see the new accounts.

![P2_User2](https://github.com/user-attachments/assets/b0edc4b6-13a4-4e7b-9aaa-3875a5ff63bb)

- Pick one of the accounts and attempt to login to the Client as that user. In my example, I chose the user “bad.texi”, username: “mydomain.com\bad.texi” in RDP (Password1).  
</br>

<b>Step 2 – Run through some basic settings/troubleshooting issues using the new user accounts</b>
</br>

- First, let’s configure the login settings for one of our new users. Open the “Group Policy Management Console” by typing <i>gpmc.msc</i> in the search field. 
- In the menu to the left, expand “Forest: mydomain.com”, expand the “Domains” folder, and then expand “mydomain.com”. Underneath this, you should see the folder “Default Domain Policy”; right-click on it and select “Edit”.

![P2_Lock1](https://github.com/user-attachments/assets/41903e75-8973-42cd-901c-e3f04952aa30)

- In the new window, expand “Policies”, then “Windows Settings”, then “Security Settings”, then “Account Policies”, and choose “Account Lockout Policy”. You should see the parameters for the lockout policy in the window to the left.
- The first is <i>Account Lockout Duration</i>; this is the amount of time an account stays locked before unlocking. To set it, double-click on it and select “Define this policy setting”. I’ll choose 30 minutes and press “Apply”. When you do this, it may suggest changes to some of the other settings. For our purposes, say “Ok”.

![P2_Lock2](https://github.com/user-attachments/assets/f90d7b31-6667-4925-9cf8-586cdd97f2da)

- Make any other changes you’d like by following the same procedure above.

- The changes in <i>Group Policy</i> will eventually populate to the computers. However, if you don’t want to wait, you can force the update using the <i>gpupdate /force</i> command in the command prompt. To do this, open the command prompt and execute this command. 

![P2_Lock3](https://github.com/user-attachments/assets/901471cc-0542-4f7a-9810-1435dbf87481)
</br>

<b>Step 3 – Practice unlocking a locked account</b>
</br>

- Attempt to login with the wrong password to the <i>Client</i> as our random user 6 times. On the sixth try, you should get a notice that the account is locked. 
- Back on the server, go into the “Active Directory Users and Computers” and right-click on “mydomain.com”.

![P2_Unlock1](https://github.com/user-attachments/assets/e924f4a7-6cf2-42ad-a7bf-0218e8c6d97f)

- Chose “Find” and search for the user. Open their account window, choose the “Account” tab and select “Unlock account…”.

![P2_Unlock2](https://github.com/user-attachments/assets/5e590fac-cfe3-499d-84cc-86712947b136)

- Now you can reset the user’s password. Right-click on the user and choose “Reset password”. Type new password and choose “Ok”.
- Attempt to login to the Client again as the user with the new password. 
- We can also <i>disable</i> this account by simply right-clicking on the user (like before) and choosing “Disable account”. It’s now disabled. To reenable it, right-click and choose “Enable account”.
</br>

<b>Step 4 – Observe Logs</b>
</br>

- Open “Event Viewer” in the server by searching <i>eventvwr.msc</i>. Expand “Windows Logs” in the top left and choose “Security”.

![P2_Log1](https://github.com/user-attachments/assets/833ad546-eb99-4415-82e1-0fb67e988f5d)

- Right-click on “Security” and choose “Find”. If you type the name of the user, it will show you the relevant entries in the log. Click on one of the entries and you can see the details of the event.

![P2_Log2](https://github.com/user-attachments/assets/423fdb65-3b07-41e6-9aba-c4f4356b2994)
</br>
</br>
</br>

<p><b>Congratulations on completing your first project in Active Directory!</b> </p>



<br />
