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
- Provided PowerShell script for bulk user creation
- Understanding of where user passwords are stored in the script


