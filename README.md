![image](https://github.com/user-attachments/assets/145b11f7-f99b-405f-a558-c5346a22ba00)

# Active Directory Domain Lab Deployment

This project demonstrates the setup and configuration of an Active Directory environment using Windows Server virtual machines, including domain creation, user management, organizational units, remote access, and PowerShell scripting for automation.

---

## Environment and Technology Used

- **Platform:** Microsoft Azure (VM-based deployment)
- **Virtual Machines:**
  - `DC-1` – Domain Controller
  - `Client-1` – Domain-joined Client
- **Tools and Services:**
  - Active Directory Domain Services (AD DS)
  - Active Directory Users and Computers (ADUC)
  - PowerShell ISE (Integrated Scripting Environment)
  - Remote Desktop Protocol (RDP)
- **Network Configuration:**
  - Internal private IP addressing
  - DC-1 acting as DNS server for the domain

---

## Operating Systems Used

- **Windows Server 2019** (on both `DC-1` and `Client-1`)

- ## Prerequisites

### Part 1 – Domain Setup

- Two Windows Server VMs: `DC-1` (Domain Controller) and `Client-1`
- Virtual machines deployed in Azure or Hyper-V with internal network connectivity
- A new Active Directory forest (e.g., `mydomain.com`)
- DNS configured with `DC-1` acting as the domain's DNS server
- Administrative access to both VMs (`labuser`)
- Active Directory Domain Services (AD DS) installed on `DC-1`
- Access to Active Directory Users and Computers (ADUC)

---

## Installation Steps

1. **Create a Virtual Network in Azure**
     ![image](https://github.com/user-attachments/assets/d0813d8c-b01c-4e07-807f-93fea22cde8f)
   - Go to the Azure Portal → "Virtual networks" → **Create**.
   - Set:
     - Subscription: `Azure subscription 1`
     - Resource group: `Active Directory Lab`
     - Virtual network name: `Active Directory-VNet`
     - Region: `East US` (or your preferred region)
     - later changed to canada region.
   - Click **Review + create**, then **Create**.

2. **Deploy the Domain Controller Virtual Machine (DC-1)**
     ![image](https://github.com/user-attachments/assets/87f83de6-736f-4479-a46c-c464ff8aa58c)
   - Navigate to **Virtual Machines** → **Create**.
   - Under **Basics**, configure:
     - Subscription: `Azure subscription 1`
     - Resource group: `Active Directory Lab`
     - Virtual machine name: `dc-1`
     - Region: `Canada Central`
     - Image: `Windows Server 2022 Datacenter: Azure Edition`
     - Size: Choose a compatible and affordable instance
   - Click **Review + create**, then **Create**.

3. **Deploy the Client Virtual Machine (Client-1)**
     ![image](https://github.com/user-attachments/assets/37ff8d4a-a037-4c4f-b67d-b5e2a0ecfd21)
   - Repeat the VM creation process for the second machine.
   - Set:
     - Virtual machine name: `client-1`
     - Region: Same as DC-1 (e.g., `Canada Central`)
     - Image: `Windows Server 2022 Datacenter: Azure Edition`
     - Ensure both VMs are in the same virtual network (`Active Directory-VNet`)
   - Click **Review + create**, then **Create**.

4. **Verify Both VMs Are Running**
     ![image](https://github.com/user-attachments/assets/37ff8d4a-a037-4c4f-b67d-b5e2a0ecfd21)
   - Go to **Virtual Machines** in the Azure Portal.
   - Confirm that `dc-1` and `client-1` both show status as **Running**.
   - Check IP address assignments and OS type under the details panel.

5. **Configure IP Settings for Client-1*
        ![image](https://github.com/user-attachments/assets/2f47a13c-1728-4056-ae59-d9fcffde6c65)
   - Go to **Networking** tab → click on the **Network Interface** for `client-1`.
   - Under **IP configurations**, ensure:
     - Static private IP is assigned (if required).
     - The subnet matches the Active Directory VNet.
     - The DNS server points to the internal IP of `dc-1`.

6. **Review VM Network Overview**
     ![image](https://github.com/user-attachments/assets/5ef16986-8374-4745-be3c-ad056d7863d9)
   - Confirm both VMs appear under the Virtual Machines list.
   - Validate:
     - Private IP addresses are in the same subnet.
     - Public IP is assigned (if needed for remote access).
     - OS: `Windows Server 2022 Datacenter: Azure Edition`.

7. **Log Into DC-1 and Launch Server Manager
![image](https://github.com/user-attachments/assets/1f9c4313-59ba-4cba-83d5-31f084654c46)
   - Access `dc-1` through the Azure Portal or RDP.
   - On the desktop, open **Server Manager** to begin server configuration.
   - Review device specifications for RAM, CPU, and server name (`dc-1`).

8. **Begin Server Role Configuration**
    ![image](https://github.com/user-attachments/assets/47216e96-1734-44ce-9a00-31c35fb22b98)
   - In Server Manager, click **Add roles and features**.
   - Proceed through the wizard to install the necessary roles.
   - Use "Role-based or feature-based installation" for local server setup.

9. **Select Destination Server**
    ![image](https://github.com/user-attachments/assets/e4458fba-e325-4078-8521-b997d14a9d37)
   - Choose the server `dc-1` from the pool.
   - Confirm IP and OS information match your deployed environment.
   - Click **Next** to proceed to server role selection (e.g., AD DS).

### 10. Add Roles and Features  
![image](https://github.com/user-attachments/assets/e4458fba-e325-4078-8521-b997d14a9d37)
From Server Manager, click **"Add roles and features"** to launch the wizard that allows you to install Active Directory Domain Services (AD DS).

---

### 11. Select Destination Server  
![image](https://github.com/user-attachments/assets/17cd520c-df11-439f-af64-da2016787103)
On the **"Select destination server"** page, choose **"Select a server from the server pool"** and verify the correct server (`dc-1`) is selected. Click **Next**.

---

### 12. Select Server Roles  
![image](https://github.com/user-attachments/assets/e1d5aa9c-94a6-4e09-a072-68ad3cd59df2)
On the **"Select server roles"** screen, check the box for **Active Directory Domain Services**. A pop-up will appear asking to add required features — accept and continue by clicking **Next**.

---

### 13. Confirm Installation Selections  
![image](https://github.com/user-attachments/assets/255f94db-d673-4a44-8145-46a87d0f3016)
Review the roles and features selected. Ensure **"Restart the destination server automatically if required"** is checked. Click **Install** to begin.

---

### 14. Monitor Installation Progress  
![image](https://github.com/user-attachments/assets/dc32cb13-fb9f-40b4-bf14-b1a7b00bf6ec)
Wait for the wizard to install Active Directory Domain Services. Once complete, the results will show **Installation succeeded**. Click **Close**.

---

### 15. Promote Server to Domain Controller  
![image](https://github.com/user-attachments/assets/d6188123-b10b-4544-9c02-92936aeaee24)
In Server Manager, click the yellow exclamation icon, then select **"Promote this server to a domain controller"** to continue domain configuration.

---

### 16. Configure Deployment  
![image](https://github.com/user-attachments/assets/7594d0a4-f53c-41b7-b337-e994b9edad0a)
In the **Deployment Configuration** window, select **"Add a new forest"** and enter a domain name (e.g., `mydomain.com`). Click **Next** to proceed.

### 17. Deployment Configuration  
![image](https://github.com/user-attachments/assets/7594d0a4-f53c-41b7-b337-e994b9edad0a)
Select **"Add a new forest"** and enter your domain name (e.g., `mydomain.com`).

---

### 18. Specify Root Domain Name  
![image](https://github.com/user-attachments/assets/90bb3999-0c47-4b53-be8f-e9bb5a685456)
In the **Deployment Configuration** wizard, enter your domain name in the **Root domain name** field and click **Next**.

---

### 19. Domain Controller Options  
![image](https://github.com/user-attachments/assets/9d4444ee-828f-4087-ab0d-dcee125ce30b)
Leave the forest and domain functional levels set to **Windows Server 2016** (or appropriate version).  
Make sure the following roles are checked:  
- **Domain Name System (DNS) server**  
- **Global Catalog (GC)**  
Enter and confirm the **Directory Services Restore Mode (DSRM)** password.  
Click **Next**.

---

### 20. DNS Options  
![image](https://github.com/user-attachments/assets/7d3ab057-c533-4614-bd72-a4fac5bc91d9)
On the **DNS Options** screen, leave **"Create DNS delegation"** unchecked.  
Click **Next**.

---

### 21. Additional Options  
![image](https://github.com/user-attachments/assets/27a6df98-286b-48d1-882b-ac52951d4a2b)
Review the **NetBIOS domain name** (auto-generated as `MYDOMAIN` based on your root domain).  
Click **Next**.

---

### 22. Paths  
![image](https://github.com/user-attachments/assets/6cf7841d-606e-423e-854d-0e8d2b610456)
Accept the default locations for the AD DS database, log files, and SYSVOL.  
Click **Next**.

---

### 23. Review Options  
![image](https://github.com/user-attachments/assets/c02bdb75-71ea-4175-a2fd-ecf004fcd847)
Verify your configuration on the **Review Options** screen.  
Optionally, click **"View script"** to generate the equivalent PowerShell command.  
Click **Next**.

---

### 24. Prerequisites Check  
![image](https://github.com/user-attachments/assets/784e8b6a-c4c5-4230-937d-41b09f5172bb)
Wait for the prerequisite checks to complete.  
You may see warnings, but as long as all checks pass, click **Install** to begin the promotion.

---

### 25. Installation Progress  
![image](https://github.com/user-attachments/assets/4d93f123-d0b8-45a4-a7f2-bbed37801aa5)
The installation begins and may take several minutes.  
The system will automatically **restart** once the promotion is complete.

---

### 26. Restart and Sign Out  
![image](https://github.com/user-attachments/assets/0865252e-976f-4fa8-9c99-49e0627dd6ce)
After installation, a prompt appears saying:  
**"You're about to be signed out"**  
This confirms Active Directory Domain Services were successfully installed, and the machine is now rebooting as a Domain Controller.

---

### 27. Reconnect via Remote Desktop  
![image](https://github.com/user-attachments/assets/bbe1b2a6-ba5d-42ce-9267-a7e8fcd3e427)
Once restarted, open **Microsoft Remote Desktop** and reconnect to the VM (e.g., `dc-1`) using your domain credentials.

### 28. Verify AD DS and DNS Role Installation  
![image](https://github.com/user-attachments/assets/cefc0515-9673-490b-a8ae-574ebafd8d6d)
After reboot, return to **Server Manager**. Confirm that **AD DS** and **DNS** roles appear in the dashboard, both showing green status under **Manageability**.

---

### 29. Open Active Directory Tools  
![image](https://github.com/user-attachments/assets/ccadd6d2-44d1-4afc-b8eb-847c42fd9b56)
From the Start Menu, open **Active Directory Users and Computers** to begin managing the domain.

---

### 30. Confirm Domain in ADUC  
![image](https://github.com/user-attachments/assets/604687bb-d710-4c8b-8d0f-1540626b061f)
In the left pane of **Active Directory Users and Computers**, confirm that your domain (e.g., `mydomain.com`) is displayed under the domain controller (e.g., `dc-1.mydomain.com`).

---

### 31. View Default AD Structure  
![image](https://github.com/user-attachments/assets/8f19e605-6e69-4a04-a670-dfda3b4cc052)
Expand the domain structure. You will see default containers like:  
- **Builtin**  
- **Computers**  
- **Domain Controllers**  
- **Users**

---

### 32. Right-Click Domain to Refresh  
![image](https://github.com/user-attachments/assets/80fd10a2-776e-4ccf-84c6-de31f7a27dfb)
Right-click your domain name (e.g., `mydomain.com`) and select **Refresh** to ensure the console reflects the most recent state.

---

### 33. Create a New User  
![image](https://github.com/user-attachments/assets/d45b6c8a-d430-4ab3-828e-851d27398260)
Right-click the **Users** container or a specific OU, then go to:  
**New** > **User**  
This opens the wizard to create a new Active Directory user account.

---

### 34. Create Organizational Unit (OU): EMPLOYEES  
![image](https://github.com/user-attachments/assets/18d42bb1-ccd4-459b-8c42-2a83aeac4c64)
Right-click the domain name, go to:  
**New** > **Organizational Unit**  
Enter the name `EMPLOYEES` and click **OK** to create a new OU for user accounts.

---

### 35. Create Organizational Unit (OU): ADMINS  
![image](https://github.com/user-attachments/assets/34e42bec-7d9f-485a-b1a6-72a271384ea4)
Repeat the same process to create another OU.  
Right-click the domain, go to:  
**New** > **Organizational Unit**  
Enter the name `ADMINS` and click **OK**.

### 36. Create Another Organizational Unit: ADMINS  
![image](https://github.com/user-attachments/assets/15ce1450-0998-4bcf-95e6-12dc2ec8eb09)
> Right-click the domain name in Active Directory Users and Computers.  
> Navigate to: **New > Organizational Unit**  
> Enter the name `ADMINS` and click **OK**.

---

### 37. Verify the ADMINS OU Was Created  
![image](https://github.com/user-attachments/assets/6b63ef9f-3f8d-48e4-adb2-c7088b5f8e95)
> Confirm the new OU named `_ADMINS` now appears in the left-hand panel under your domain.

---

### 38. Log in Using Domain Admin Credentials  
![image](https://github.com/user-attachments/assets/a01e0474-18ec-4d65-9c7b-3ad652cadda4)
> Connect using Remote Desktop with the following credentials:  
> Username: `mydomain.com\jane_admin`  
> Enter the password created during the domain admin setup.

---

### 39. Refresh the ADUC Console  
![image](https://github.com/user-attachments/assets/e0907833-7753-42f1-ac33-b520dc3135a9)
> In Active Directory Users and Computers, right-click your domain name and select **Refresh**  
> Ensure that `_ADMINS` and `_EMPLOYEES` OUs are both visible.

---

### 40. Create New User in ADMINS OU  
![image](https://github.com/user-attachments/assets/a7547f6d-f15a-4ba0-b8da-984c51688699)
> Right-click the `_ADMINS` OU and select: **New > User**  
> Begin entering the user details for the domain admin account.

---

###  41. Fill in User Details for Jane Doe  
![image](https://github.com/user-attachments/assets/511d3afe-fb8d-4ea5-ac65-9cb8f017ec5e)
> Input the following:  
> First name: Jane  
> Last name: Doe  
> User logon name: `jane_admin`  
> Click **Next** to proceed.

---

### 42. Set Jane Admin's Password  
![image](https://github.com/user-attachments/assets/81eaffdf-8552-4755-8678-3a26e5ff5bdb)
> Set a strong password such as `Cyberlab123!`  
> Confirm the password, then check **Password never expires**  
> Click **Next**, then **Finish**.

---

### 43. Open User Properties  
![image](https://github.com/user-attachments/assets/28a99bc8-9f44-4f01-bcac-124e217caa87)
> Right-click on `Jane Doe` under `_ADMINS` and select **Properties**  
> Navigate to the **Member Of** tab.

---

### 44. Add to Domain Admins Group  
![image](https://github.com/user-attachments/assets/4b5fdad9-3e8d-4862-a3f2-71239f7b1851)
> In the Member Of tab, click **Add**  
> Type `Domain Admins`, click **Check Names**, then **OK**.

---

### 45. Confirm Group Membership  
![image](https://github.com/user-attachments/assets/9dfb3b92-0e09-492c-b7c7-fb100754791b)
> Ensure that `Domain Admins` and `Domain Users` both appear under group memberships  
> Click **Apply**, then **OK** to save.

---

### 46. Log Off the Current Session  
![image](https://github.com/user-attachments/assets/9514f0b3-4f7f-4940-90f6-a8ad4dd6af34)
> Use the shortcut `Win + R` to open Run  
> Type `logoff` and hit Enter to log off safely.

---

### 47. Reconnect Using Microsoft Remote Desktop  
![image](https://github.com/user-attachments/assets/0a0120c0-9165-40d8-b877-40f9a8eb2929)
> Open Microsoft Remote Desktop  
> Select the VM for `dc-1`  
> Log in as `mydomain.com\jane_admin` using the password you set.

---


![image](https://github.com/user-attachments/assets/b9a7fe98-83dd-44de-991f-75009660e8e4)

![image](https://github.com/user-attachments/assets/f79c4ac2-8e7d-4b4e-a7da-263e5f3a6bd0)

![image](https://github.com/user-attachments/assets/1743ba23-13ee-46bf-a41e-75a1a558d211)

![image](https://github.com/user-attachments/assets/033996a6-4974-4682-a2d0-49db0a2a7e54)

![image](https://github.com/user-attachments/assets/443d7c9b-4c75-4546-994e-a3416508b596)

![image](https://github.com/user-attachments/assets/ae506892-3877-4302-b11a-5fb995d8a189)

![image](https://github.com/user-attachments/assets/226e439d-c4e5-417b-8b07-b834ef813eac)

![image](https://github.com/user-attachments/assets/a8b0c2af-b60d-4db0-bf8f-d6f826d17e22)

![image](https://github.com/user-attachments/assets/407ddf52-3f5a-494e-97cb-a4fdb9e62c40)

![image](https://github.com/user-attachments/assets/2dc40268-f53f-4404-a79f-6e81a5e1f7df)

![image](https://github.com/user-attachments/assets/ccc48ecb-7263-42a2-b76f-94a5d1b630f4)

![image](https://github.com/user-attachments/assets/3358e661-145c-4959-8687-785e3f6be462)

![image](https://github.com/user-attachments/assets/b8e0c040-bc5a-4290-affc-192f6627abef)

![image](https://github.com/user-attachments/assets/f8d62266-06a0-4b17-a09f-29707651546c)

![image](https://github.com/user-attachments/assets/f51c083b-eb02-47d1-8f1b-2a5410c5d006)

![image](https://github.com/user-attachments/assets/483ea7c6-fc87-4999-9654-342e751238ed)

![image](https://github.com/user-attachments/assets/cc7b9ec6-9e9e-46e1-b00c-46532747fb1c)

![image](https://github.com/user-attachments/assets/3e8d7177-7e04-4206-8a9e-97b252af3103)


















