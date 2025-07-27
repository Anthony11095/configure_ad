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

### Part 2 – Remote Access and User Automation

- Remote Desktop enabled on `Client-1`
- Permissions configured to allow domain users to connect via RDP
- PowerShell ISE installed and ready on `DC-1`
- Provided PowerShell script for bulk user

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

---

























![image](https://github.com/user-attachments/assets/7594d0a4-f53c-41b7-b337-e994b9edad0a)

![image](https://github.com/user-attachments/assets/90bb3999-0c47-4b53-be8f-e9bb5a685456)

![image](https://github.com/user-attachments/assets/9d4444ee-828f-4087-ab0d-dcee125ce30b)

![image](https://github.com/user-attachments/assets/7d3ab057-c533-4614-bd72-a4fac5bc91d9)

![image](https://github.com/user-attachments/assets/27a6df98-286b-48d1-882b-ac52951d4a2b)

![image](https://github.com/user-attachments/assets/6cf7841d-606e-423e-854d-0e8d2b610456)

![image](https://github.com/user-attachments/assets/c02bdb75-71ea-4175-a2fd-ecf004fcd847)

![image](https://github.com/user-attachments/assets/784e8b6a-c4c5-4230-937d-41b09f5172bb)

![image](https://github.com/user-attachments/assets/4d93f123-d0b8-45a4-a7f2-bbed37801aa5)

![image](https://github.com/user-attachments/assets/0865252e-976f-4fa8-9c99-49e0627dd6ce)

![image](https://github.com/user-attachments/assets/bbe1b2a6-ba5d-42ce-9267-a7e8fcd3e427)

![image](https://github.com/user-attachments/assets/cefc0515-9673-490b-a8ae-574ebafd8d6d)
















