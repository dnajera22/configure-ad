<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1 align="center">Setting up Active Directory using Azure</h1>

<p>In this tutorial we will set up and configure Active Directory in a virtualized Azure environment. Creating a Domain Controller, joining a client machine to the domain, and managing user accounts. We will also set up Remote Desktop access for non-administrative users.</p>

<h2>Environments and Technologies Used</h2>
<ul>
  <li>Microsoft Azure </li>
  <li>Remote Desktop Connection</li>
  <li>Windows Powershell</li>
  <li>Active Directory</li>
  <li>Windows Server 2022 (Domain Controller)</li>
  <li>Windows 10 (Client)</li>

<h2>Step 1: Creating Virtual Machines using Azure</h2>
<ul>
  <li>Create a new VM, name it “DC-1”. For image select ‘Windows Server 2022”. Take note of the Resource Group, Location, and Vnet selected.</li>
  <li>Create another VM, named “Client-1”. For image select “Windows 10”. Use the same Resource Group, Location, and Vnet as DC-1.</li>
  <li>Select DC-1 VM. Go to Network>Network Settings, select the NIC>ipconfig1 and change the Private IP to static.</li>
  <li>Select Client-1 VM. Go to Network>Network Settings, select the NIC>DNS Servers, select custom and paste in DC-1’s Private IP.</li>
</ul>
<br/>
<img src="https://i.imgur.com/LgzmPmy.png"/>

<h2>Step 2: Testing connection between the VMs</h2>
<ul>
  <li>Using Remote Desktop, Log in to both VMs by using the Private IPs found in Azure.</li>
  <li>On DC-1, hit start>Run, and type ‘wf.msc’. Click Windows Defender Firewall Properties</li>
  <li>Under Domain Profile set Firewall state to Off. Do the same for ‘Private Profile’ and ‘IPsec’ tabs.</li>
  <li>On Client-1, open Windows Powershell, type “ping” then DC-1’s Private IP. Make sure the connection is successful.</li>
</ul>
<br/>
<img src="https://i.imgur.com/GqDDHH7.png"/>
<h2>Step 3: Install Active Directory</h2>
<ul>
  <li>On DC-1, go to Server Manager, select add roles and features, check Active Directory
Domain Services, and install.</li>
  <li>After installation, select ‘promote this server to a domain controller’. Select add a new forest, for this tutorial we will use ‘mydomain.com’. Finish installation.</li>
  <li>Close the VM and restart it through Azure.</li>
  <li>Log back into DC-1. For username put “mydomain.com\” in front of your username.</li>
</ul>
<br/>
<img src="https://i.imgur.com/pplI1sz.png"/>
<h2>Step 4: Set up Organizational Units (OUs) and Admin User</h2>
<ul>
  <li>On DC-1, open Active Directory Users and Computers. Click mydomain.com on the left.</li>
   <li>Right click mydomain.com>New>Organizational Unit. name is “_EMPLOYEES”</li>
   <li>Make two more OUs named “_ADMINS” and “_CLIENTS”</li>
   <li>Under _ADMINS folder right click>New>User. We will use Jack Smith for this example. Give her a username and password. Ex. jack_admin</li>
  <li>Right click user Jack Smith. hit ‘Member of’ and type in “domain admin” and hit check names.</li>
  <li>Log out of DC-1 and relog in using mydomain.com\jack_admin. Use this username when logging into DC-1 from now on.</li>
</ul>

<h2>Step 5: Joining Client-1 to the Domain</h2>
<ul>
  <li>On Client- 1, right click start menu> System> rename this PC(advanced)>Change, select domain and enter mydomain.com.</li>
  <li>On DC-1, open Active Directory Users and Computers, select mydomain.com>Computers, drag Client-1 into _CLIENTS folder.</li>
</ul>
<br/>
<img src="https://i.imgur.com/3nVxyML.png"/>
<h2>Step 6: Allowing Non-Administrative Users remote access to Client-1</h2>
<ul>
  <li>Log onto Client-1 using the admin account, mydomain.com\jack_admin.</li>
  <li>Right click start>System>Remote Desktop>’Select users that can remotely access this
PC’> Add> type in domain users>Check names.</li>
</ul>
