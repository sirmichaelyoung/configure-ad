<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Install & Setup within Azure</h1>
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

- Setup Azure with Windows Server & Windows 10 Virtual Machines (VM)
- Install Active Directory & Setup Domain on WinServer
- Create Admin and User Accounts in Active Directory
- Connect Win10 VM to Domain
- Allow Users Remote Desktop Access on Win10 VM  

<h2>Deployment and Configuration Steps</h2>

![Screenshot 2024-04-09 at 7 19 15 PM](https://github.com/sirmichaelyoung/configure-ad/assets/163785883/43a7fb8d-f4f7-4fe3-bc16-2772c49670ea)

<h4>Setup Azure with Windows Server & Windows 10 Virtual Machines (VM)</h4>

- Create VM with Windows Server 2022
- Under > Networking, allow the new VM to create a new Virtual Network

Once the WinServer VM has finished deployment, change Private IP address settings to "Static"
- Network settings > IP configurations > [ipconfig1] > Private IP address settings [X] Static

![Screenshot 2024-04-09 at 7 39 01 PM](https://github.com/sirmichaelyoung/configure-ad/assets/163785883/b8014494-6935-426c-b2ae-dc1e059b0afd)

- Create Windows 10 VM with the existing Virtual Network

![Screenshot 2024-04-09 at 7 43 37 PM](https://github.com/sirmichaelyoung/configure-ad/assets/163785883/5bbacdc1-b5c4-43d4-9a8b-9859a51cf5cd)

<h4>Install Active Directory & Setup Domain on WinServer</h4>

- Remote access WinServer
- Server Manager > Configure this local server > Server Roles [X] Active Diectory Domain Services

![Screenshot 2024-04-09 at 8 17 24 PM](https://github.com/sirmichaelyoung/configure-ad/assets/163785883/0cf4415b-9f84-4b62-8efe-25b94184f2d0)


Promote this server to domain controller within Server Manager

![Screenshot 2024-04-09 at 8 26 10 PM](https://github.com/sirmichaelyoung/configure-ad/assets/163785883/55f50f4f-c4f5-40c2-ba41-d5b01837d159)

In Deployment Configuration, select "Add a new forest"

- Create a Root domain name, a password will be created next
- For this scenario, all other settings may be left default.

After instillation, WinServer will restart.

<h4>Create Admin and User Accounts in Active Directory</h4>

![Screenshot 2024-04-09 at 9 08 50 PM](https://github.com/sirmichaelyoung/configure-ad/assets/163785883/6775cc38-835e-4e37-a60e-a8d496a10049)

Open Actuve Directory Users and Computers
- Create a new Organizational Unit (OU) within the existing Domain
  - _EMPLOYEE
  - _ADMIN
 
Within _EMPLOYEE OU, create new Employee (example_
- Joseph | Username JosephEmployee
- Create a User Logon Name (josephemployee@winserver.com) & password (P@ssword1)

Within _ADMIN OU, create new Admin (example)
- Jazmine | Username JazmineAdmin
- Create a User Logon name (jazmineadmin@winserver.com) & password (P@ssword1)

Add Jazmine to "Domain Admins" Security Group 

![Screenshot 2024-04-09 at 9 35 23 PM](https://github.com/sirmichaelyoung/configure-ad/assets/163785883/9bc82d75-6302-4101-aabb-e4e796c581d8)

We can now log out, and sign back in using (example) Jazmine's credentials. 

<h4>Connect Win10 VM to Domain</h4>

Within Azure, set Win10 VM's DNS settings to match WinServer Private IP (example 10.0.0.4) 

![Screenshot 2024-04-09 at 10 16 43 PM](https://github.com/sirmichaelyoung/configure-ad/assets/163785883/0c3f95bd-4f22-4873-b7ad-dcd8680eea3b)

*Restart Win10 VM

- Sign in to Win10 VM (original admin user)
  - Settings > About > Rename this PC (advance)
    - Change domain or workgroup
    - Enter domain (example winserver.com)
   
*Restart Win10 VM
   
![Screenshot 2024-04-09 at 10 40 20 PM](https://github.com/sirmichaelyoung/configure-ad/assets/163785883/85ca9fa5-e9ef-412b-90c1-eb1627806eba)

Within WinServer, Win10 should now be listed inside the "Computers" container
- Create a new OU named "_CLIENTS" and add Win10

<h4>Allow Users Remote Desktop Access on Win10 VM  </h4>

- Log into Win10 VM with (example) Jazmine Admin (jazmineadmin@winserver.com)
- Open Remote desktop Settings

![Screenshot 2024-04-10 at 12 44 48 AM](https://github.com/sirmichaelyoung/configure-ad/assets/163785883/f2b9e0fa-f87b-4ad1-9434-05e28f235d32)

- Under User accounts > "Select users that can remotely access this PC"
  - Add > "Domain Users"
 
Members of Domain Users (Joseph Employee > _EMPLOYEES) may now access Win10 VM with their credentials. 

This concludes the Active Directory Install & Setup within Azure.












