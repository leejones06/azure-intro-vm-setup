![!Prog1_Logo](https://github.com/user-attachments/assets/a15cebc9-8570-4605-af75-08d7ca8c8dd8)

<p align="center">

</p>

<h1>Creating Virtual Machines on a Virtual Network in Azure</h1>

This entry-level tutorial walks the user through the creation of a desktop PC and a server on a shared network in Azure. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines & Virtual Networks)
- Remote Desktop
- PowerShell 


<h2>Operating Systems Used </h2>
- Windows Server 2022
- Windows 10 Pro
</br>

<h2>Part 1: : Create a Virtual Network (VN) in Azure </h2>

<p>After creating your Azure account, the first thing we’ll do is create the <i>virtual network</i> through which our machines will connect to one another. This will require creating a new <i>resource group</i> first. After creating the network, we’ll then create our <i>virtual machines</i> and do some basic configuring of their connections. </p>
</br>

- Go to the “All Services” page. Our focus will be on the list of icons across the top of the screen. 
- Find the icon labeled "Virtual Networks" and click through to the VN page. Once there, select “Create virtual network”.

![Part1_1 READY](https://github.com/user-attachments/assets/8b16b420-be97-4364-8a22-13ebd6854fd8)
</br> </br> 
![Part1_2 READY](https://github.com/user-attachments/assets/218f3016-c4f2-46c6-a60b-0ac39f032c6c)

- We’ll need to create a <i>Resource Group</i>. A resource group is like a “container” for our project. Select “Create new” and choose a name for your “group”. 

- Next, choose a name for your VN. 

- Choose the <i>Region</i> in which your project will be stored. You may have to play around with the region to find one that will allow you to create the type of project you’re working on. Regardless of which region you choose, you’ll want to use the same region for each resource you create.

- This is all we need to configure at this point. Select “Review + create”. It will show you an overview of some of the details of your new network. Click “Create” and allow it to deploy.

![Part1_3 READY](https://github.com/user-attachments/assets/b513224b-56d5-4b3e-9bb2-50622c2a7a55)
</br> </br> 
![Part1_4 READY](https://github.com/user-attachments/assets/373bfeca-c492-44fb-8ac5-1611fbe23e6e)
</br>

<h2>Part 2: Create the Virtual Machines. </h2>

<h4>Next, we’ll create our first VM, which will run Windows Server 2022</h4>

- Go back to the "All Services" page and select the "Virtual Machines" icon (you can also search “virtual machines” in the search box at the top). 

- From the "Virtual Machines" page, choose “Create” and then select “Azure virtual machine” from the drop-down menu. 

![Part2_1 READY](https://github.com/user-attachments/assets/006d1936-0c1d-4143-ac4d-d5347f1456d1)

- Select the “Resource Group” you created earlier and give your VM a name. Remember to create it in the same Region as your network. Continue filling in the information as required (name your machine, etc)

![Part2_2 READY](https://github.com/user-attachments/assets/6b1caae5-03db-449c-b208-1bdad5cde440)

- For the “Image”, we’re going to select “Windows Server 2022”. For “Size”, choose a setting with at least <i>2 vcpu’s</i> and <i>8 GiB’s of RAM</i>. 

- Create a “username” and “password” for the machine. 

- Make sure to check the box, “Would you like to use an existing Windows Server license?”.
  
- At the bottom, choose “Next: Disks”. Leave the “disks” page as is and choose “Next: Networking”.

- On the networking page, make sure the correct “virtual network” is selected. Then scroll down and make sure “Delete public IP and NIC…” is checked. Leave everything else as is and select “Review + create”. 
If it passes validation, choose “Create”.

![Part2_3 READY](https://github.com/user-attachments/assets/cd95bc79-f471-4388-aa3f-1a3793a0596c)

<h4>Before we create the second VM, let’s set the server’s <i>private IP address</i> to "static".</h4>

- On the VM page, open to your server's dashboard.

- On the left side of the page is a series of options; choose “Networking”, and then “Network settings”.

![Part2_4 READY](https://github.com/user-attachments/assets/de186dc6-eb1c-499c-891a-0f0e7d976023)

- When that screen opens, you’ll see a box in the middle labeled “Network interface / IP configuration”. Select it.

![Part2_5 READY](https://github.com/user-attachments/assets/a1211e80-402d-4f41-906e-89b536401dae)

- Choose the link titled “ipconfig1”. Change “Private IP” to <i>static</i> and save.

![Part2_6 READY](https://github.com/user-attachments/assets/7ba1218b-b1e3-4d0b-a69f-1bdfda56dedc)

- Next, we’re going to log into our new VM using <i>Remote Desktop</i> (RDP) and disable <i>Windows Firewall</i>. To do this, you'll need the server's public IP address (you’ll find this on the VM’s main page in Azure). 

![Part2_6 5 READY](https://github.com/user-attachments/assets/792e502c-74ba-451c-a971-aa46357e1966)

- Open Remote Desktop and copy the IP into RDP. Enter the username and press “Connect”. When prompted, enter the password and you should connect to the VM. 

![Part2_7 READY](https://github.com/user-attachments/assets/bb65ea65-3c5a-4971-8d7e-c86ca4818a99)


- Open the firewall. To do this, right-click on the start button of the VM and choose “Run”. Type "wf.msc" and “ok”. 

![Part2_8 READY](https://github.com/user-attachments/assets/f64c7c68-0d11-42a6-ac49-53a165c93cae)

- Choose “Properties” from the menu on the right. On the window that pops up, on the <i>first three</i> tabs ("Domain Profile", "Private Profile", "Public Profile"), set “Firewall State” to “Off”. Select “Apply” and “Ok”.

![Part2_9 READY](https://github.com/user-attachments/assets/7cb2761e-6fb7-473c-82b1-cd9da33a8b4d)
</br></br>
![Part2_10 READY](https://github.com/user-attachments/assets/5146c94a-57fb-43ed-af3c-cdcd10549500)


<h2>Part 3: Create the second VM, which will be the “Client”, running Windows 10, and configure the DNS.</h2>

<h4>The process to create this machine is essentially the same as the set up for the server. </h4>

- Go to "Virtual Machines" and create a new one.

- This time, for the “Image”, choose <i>Windows 10 Pro</i>. For “Size”, choose the same for the server. Make sure you’re in the same Resource Group and region. 

-	Choose a user name and password.
  
- At the bottom of the page, be sure to check the box, “I confirm I have an eligible Windows 10/11 license…”. 

- Click through to “Networking” and make sure you’re on the correct VN. Check the box, “Delete public IP and NIC…” “Review + Create”. Once validated, “Create”. 

<h4>Now that we’ve created our two machines, we’re going to set our Client’s <i>DNS</i> to our Server’s private IP address.</h4>

- Find the Server’s private IP address from the VM’s main page in Azure and copy it. 

![Part3_1 READY](https://github.com/user-attachments/assets/8a94589c-e97a-4531-8692-b61b763e3e30)

- Go to the client’s page in Azure; choose “Networking”, “Network settings”, and “network interface/ip config”.

- Choose “DNS Servers” on the left; select “Custom” and enter the Server’s private IP in the field. “Save”.

![Part3_3 READY](https://github.com/user-attachments/assets/5a1bc96a-41a0-413e-acaa-3722a04b12ee)

<h4>To see if we were successful, let’s log into our Client VM and attempt to “ping” the server’s private IP. </h4>

- From the Azure portal, <i>restart</i> our Client VM. To do this, open the Client’s main page in Azure; at the top you’ll see a series of options running horizontally. Choose “Restart”. 

![Part3_4 READY](https://github.com/user-attachments/assets/ed367d09-d5a9-4643-a0fe-9cd51aeb309d)

- Next, log into the Client VM using RDP (get the client's public IP from it's dashboard, just as you did with the server's IP). You’ll have to choose a few set-up options since it’s our first login to this new machine. 

- Once inside, open “PowerShell” as the <i>administrator</i> by searching “powershell” in the search box. Right-click on PowerShell in the search results and choose “Run as administrator”. 

- Once PowerShell opens, attempt to “ping” the Server’s private IP. Type “ping [server’s private IP]” and press “Enter”. Ensure the ping is successful. 

- Next, run “ipconfig /all” in the prompt. You should see the private IP next to the “DNS Servers” item. 

![Part3_5 READY](https://github.com/user-attachments/assets/e4f34912-a3b6-4518-a802-cf1a793c891b)

<p><b></b>Congratulations on setting up your first Virtual Network!</b></p>

<br />
