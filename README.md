
# Active Directory Homelab Setup (Oracle VirtualBox)

## Description

This project is a comprehensive guide to setting up an Active Directory home lab using Oracle VirtualBox. The walkthrough covers the installation of Windows Server 2019, configuration of Active Directory, user and group management, setting up Routing and Remote Access (RAS), DHCP, and integrating a Windows 10 workstation into the domain.

# Languages and Utilities Used:

* PowerShell
* VirtualBox
* Windows Server 2019
* Windows 10
* Active Directory


# Project Walkthrough

**Downloading Required Software**
* Download and install VirtualBox by Oracle.
* Download Windows Server 2019 ISO from Microsoft.
* Download Windows 10 ISO using the Media Creation Tool.

**Configuring Virtual Machine for Windows Server 2019**

* Installed VirtualBox
* Downloaded .iso images
* Launched VirtualBox
* Created a new VM named Domain Controller
    - Set memory size to 2048 MB
    - Default settings for disk size, file location, and size
    - Clicked Create
  
![App Screenshot](https://i.imgur.com/41VQ1TK.jpg)

![App Screenshot](https://i.imgur.com/4DzgPB7.jpg)
* Accessed settings after VM creation
* Configured Shared Clipboard and Drag'n'Drop under the Advanced tab in the General pane
* Under the System pane, set CPU to 1 under the Processor tab
* Under the Network pane:
    - Adapter 1: Set "Attached to:" to NAT
    - Adapter 2: Enabled and set "Attached to:" to Internal Network
* Clicked Ok to complete VM configuration.


![App Screenshot](https://i.imgur.com/vpRGpF2.jpg)

**Installing Windows Server 2019**

* Configured the VM in VirtualBox
* Initiated the VM startup
* Prompted to choose a disk/optical drive for the VM
    - Used File Explorer to locate and select Windows Server 2019 .iso file
    - Confirmed selection with Ok
* Booted into Windows Server 2019 installation
* Chose the Standard Evaluation (Desktop Experience) version for the OS installation
* Allowed Windows to reboot the VM multiple times to complete the installation

![App Screenshot](https://i.imgur.com/s6WiUXA.jpg)

**Server 2019 Setup and Configuration Steps**

* ***Administrator Account Setup***
    - Set the password for the built-in administrator account.
    - Logged in as the built-in administrator.

* ***Network Configuration:***
    - Selected Network settings.
    - Clicked on Change Adapter options.
    - Renamed the network connections:
        - Public-facing adapter: INTERNET_1
        - Internal network adapter: INTERNAL_2

![App Screenshot](https://i.imgur.com/Bzn6v81.jpg)

* ***Static IP Configuration for X_Internal_X Adapter:***
    - Assigned the following static IP configuration:
        - IP Address: 172.16.20.1
        - Subnet mask: 255.255.255.0
        - Default Gateway: the Domain Controller serves as the gateway
        - DNS: 127.0.0.1 (loopback)

* ***PC Renaming:***
    - Navigated to Control Panel > System > Advanced System Settings.
    - Renamed the PC to "Domain Controller," matching the name set in VirtualBox.

* ***Restart:***
    - Restarted the VM for changes to take effect.

**Setting Up Active Directory**

* Add Active Directory Domain Services through Server Manager.
* Promote the VM to a domain controller.
* Create a new forest with the domain "mydomain.com."

![App Screenshot](https://i.imgur.com/RrkiLYD.jpg)

![App Screenshot](https://i.imgur.com/2l7ZwJF.jpg)

![App Screenshot](https://i.imgur.com/NXIB9IS.jpg)

![App Screenshot](https://i.imgur.com/iPiXiqe.jpg)


**User and Group Management**

* Logged back into the VM.
* Accessed Active Directory Users and Computers.
* Created a new Organizational Unit named _ADMINS.
* Established a new user account for myself.
* Added myself to the Domain Admins group.
* Logged in using the new domain admin account.

![App Screenshot](https://i.imgur.com/5VOW6jd.jpg)

**Setting Up Routing and Remote Access (RAS)**
* Install Remote Access role in Server Manager.
* Configure and enable Routing and Remote Access.
* Specify NAT and select the network adapter for internet access that I previously named INTERNET_1.

![App Screenshot](https://i.imgur.com/lWbcLQn.jpg)

**DHCP Setup:**

* ***In Server Manager:***
    - Added DHCP role.
    - Installed DHCP.
* ***Accessed DHCP from Tools in Server Manager.***
* ***Created a new scope:***
    - Range: 172.16.0.100–200.
    - CIDR prefix length: 24 (subnet mask: 255.255.255.0).
    - Lease duration: 8 days. (this if for a home lab)
* ***Configured DHCP options:***
    - Default Gateway: 172.16.0.1.
    - Parent domain: mydomain.com.
    - Activated the scope.
* ***Finalized setup:***
    - Authorized DHCP server.
    - Refreshed for confirmation (green circle with checkbox).

![App Screenshot](https://i.imgur.com/ff59t8M.jpg)

**Automating User Creation with PowerShell**

I obtained Josh's PS script for adding extra users directly from the VM. The script is available on his GitHub [here](https://github.com/joshmadakor1/AD_PS).

* Downloaded and extracted a PS script in the VM from a .zip file to the desktop.
* Added my name, "Abdullah Hussein" to the top of a .txt file with randomly generated names.
* Launched PowerShell ISE as administrator and opened the PS script.
* Set execution policy to Unrestricted and allowed the script to run.
* Changed the directory in PowerShell to the script's folder.
* Successfully ran the script.

![App Screenshot](https://i.imgur.com/ehSDxhX.jpg)

* I proceeded to Active Directory to confirm the creation of the users.

![App Screenshot](https://i.imgur.com/fU4amNi.jpg)

Now that the Server VM is all set up, the next step is to create a new VM for a Workstation. I'll be installing Windows 10 on it and configuring it for use within our freshly established Active Directory Domain network.

**Windows 10 Workstation Integration**

* Create a new VM in VirtualBox named "Client1."
* Set memory size to 2048 MB and configure network settings.
* Selected VM settings, then Network, Adapter 1 tab
    - Switched to Internal Network
      
![App Screenshot](https://i.imgur.com/B1vIA30.jpg)

* Installed Windows 10 and set up a local account.
* Accessed Control Panel from the Start menu.
* Navigated to System > Advanced System Settings > Name tab.
* Selected 'Change' to rename the PC and join it to the domain.
* Renamed the VM to CLIENT1 and joined mydomain.com.
* Authenticated with domain administrator credentials.
* Prompted to reboot the VM after finalizing the setup.
* Logged back in using the newly created AD account from the PowerShell script.

![App Screenshot](https://i.imgur.com/HHK1q33.jpg)

**DNS and Internet Connectivity Verification**
* Open Command Prompt on the Windows 10 VM.
* Execute the command `ipconfig` to see if ip address work.
* Execute the command `ping`to ping a website (www.google.com) to ensure DNS resolution.
* Confirm that the entire infrastructure is operational, indicating successful communication from the VM to the default gateway (Domain Controller) and proper NATting to the internet.

![App Screenshot](https://i.imgur.com/MOoOoxT.jpg)


# Project Summary

This comprehensive project offers a hands-on experience in establishing a secure Active Directory home lab. It covers various stages, starting with the initial Virtual Machine (VM) setup, progressing through Active Directory configuration, user management, and the integration of a Windows 10 workstation. Meticulous explanations accompany each step, ensuring a thorough understanding. The incorporation of PowerShell scripting further enriches the project by automating user creation, making it an effective guide for individuals seeking practical knowledge in Active Directory setup and management. The successful verification of DNS and internet connectivity adds to the project's depth, showcasing a fully operational and secure virtual lab environment.


