# Wazuh Setup Tutorial

## Introduction

In this guide, we’ll set up **Wazuh** — an open-source **SIEM** and **XDR** platform used to monitor systems and detect security events.  
We’ll also do a quick test at the end on **File Integrity Monitoring (FIM)** demo to see how Wazuh detects and reports file changes in real time.

---

## What Are SIEM and XDR?

- **SIEM (Security Information and Event Management):** Collects and analyzes security logs from multiple sources such as routers, firewalls, servers, and switches.  
- **XDR (Extended Detection and Response):** Expands on SIEM by automatically detecting and responding to threats across endpoints, cloud services, and networks.

Together, they give visibility into your entire infrastructure and help identify security issues quickly.

## Objective

We’ll perform the following steps:

1. **Install the Wazuh Manager** on **Ubuntu 24.04 LTS** (running inside a virtual machine).  
2. **Install the Wazuh Agent** on a **Windows 11 Pro** machine to send logs to the server.

That’s it — a simple setup that gives you powerful monitoring capabilities.

---

## Step 1: Check Your System Requirements

Before starting, make sure your computer meets the requirements for this setup.

### Minimum Requirements
- **RAM:** 8 GB or more  
- **Virtualization:** Must be enabled in your BIOS or UEFI settings  

Virtualization allows your computer to run virtual machines efficiently. Most modern systems support it — it just needs to be turned on if not already on.

### How to Check Your RAM?
1. Press **Windows + R**, type `msinfo32`, and press **Enter**.  
2. Look for **Installed Physical Memory (RAM)** to see your total memory.

### How to Check Virtualization?
1. Press **Ctrl + Shift + Esc** to open **Task Manager**.  
2. Go to the **Performance** tab and select **CPU**.  
3. Look at the bottom-right corner — it should say **Virtualization: Enabled**.

If it says **Disabled**, restart your computer, open your BIOS or UEFI settings, and enable virtualization.  
You can find guides by searching:  
`enable virtualization on [your PC model] BIOS` on Google or YouTube.

Once your computer is ready, proceed to the next step to begin the setup.

---

## Step 2: Install VirtualBox and Download Ubuntu 24.04 LTS ISO

Now that your computer is ready, it’s time to set up the lab environment.
We’ll use **VirtualBox** to create and run our Ubuntu virtual machine.
It’s free, easy to use, and doesn’t require any registration or payment.

If you already have **VMware**, you can use that instead — it's even better.

### What Is a Hypervisor (like Hyper-V)?
A **hypervisor** (for example, **Hyper-V**, **VirtualBox**, or **VMware**) allows you to run multiple operating systems on a single computer.
It does this by creating isolated virtual environments that share your computer’s hardware resources. 

### Download VirtualBox:
Download the latest version of VirtualBox from the official website:  
[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
Install it by running the setup file and following the instructions.

### Download Ubuntu 24.04 LTS ISO:
Next, download the Ubuntu 24.04 LTS ISO file, which acts as the installation media for the virtual machine.

You can download it from the official Ubuntu website:  
[https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)

Save the ISO file in a dedicated folder for your virtual machines.  

Once both are ready, proceed to the next step to begin creating your virtual machine.

---

## Step 3: Set Up the Virtual Machine

### a. Fix the Network First

This step is crucial to ensure your virtual machine (VM) connects properly to your local network.

When you configure the VM in **Bridged Mode**, it will use your real network adapter, allowing the VM and your computer to be on the same network.  
This setup makes it easier for them to communicate with each other.

If this configuration is skipped, the VM may receive an IP address from a different subnet, preventing proper communication between devices.

### How to Configure the Bridged Network?

1. Open the **Start Menu**, search for **Virtual Network Editor**, and run it as **Administrator**.  
2. If you are connected through Wi-Fi, select **VMnet0**.  
3. Under **VMnet Information**, locate **Bridged to** and choose your **wireless adapter**.  
4. Click **Apply**, then **OK** to save the

### How to Find Your Wireless Adapter Name?

There are several ways to check it:

- Open **System Information → Components → Network → Adapter**  
- Or open **Device Manager**, then expand **Network adapters** to see the list

### b. Build Your Virtual Machine

Now that your network is ready, it’s time to create your virtual machine.

1. Open **VirtualBox**.  
2. Click **Machine → New** at the top.

#### Basic Setup

1. **Name:** Choose a name for your VM.
2. **Folder:** Select a clean folder to store your VM files — avoid using used folders like Downloads.  
3. **ISO File:** In the “Name and Operating System” section, choose the **Ubuntu 24.04 LTS ISO** you downloaded earlier.

#### Unattended Guest OS Installation

During this step:
- Set a **password** and confirm it.  
- You can also change the **username** if you want.  
Keep these credentials safe — you’ll need them to log in later.

#### Hardware Configuration

Let’s allocate the necessary resources:

- **Base Memory (RAM):** 4 GB  
- **CPUs:** 2 cores  
- **Storage:** Create a new virtual hard disk (recommended size: **40-60 GB**)

When done, click **Finish** to complete the setup.

> **Note:**  
> If you skip or rush through the configuration, your VM may run slowly.  
> Adjusting resources properly (2 CPU cores and 60 GB storage) ensures smooth performance.

---

#### Expanding Storage (Optional)!

If your VM only has the default 25 GB disk, don’t worry — you can expand it anytime.

1. Open **Settings → Storage**.  
2. Under **Controller: SATA**, select the `.vdi` file (your virtual disk).  
3. Click the **Remove Attachment** icon (bottom right).  
4. Then click **Add Attachment → Hard Disk → Create**.  
5. Choose your desired size, then click **Finish**.

Your virtual machine will now have additional storage capacity.

---

#### Final Network Check

Before booting up:

1. Right-click your VM → **Settings → Network**.  
2. Make sure the **Adapter** is set to **Bridged Adapter**.  
3. Confirm it shows your **wireless network adapter name**.

Once verified, your VM is fully configured and ready to start.

---

### c. Run Your Virtual Machine and Set Up the Operating System

After configuring the virtual machine, you can now start it and begin installing the operating system.

1. In **VirtualBox**, double-click your newly created VM to start it.  
2. The Ubuntu installation should begin automatically.

---

### Handling Common Errors!

If you see an error message such as:
"MWGFX seems to be running on an unsupported hypervisor..."

do not worry. This issue sometimes appears when VirtualBox encounters compatibility problems with graphics drivers.  
Wait a moment to see if the installation continues.  

If the screen stays black but you can hear Ubuntu starting up, follow these steps to fix it.

### Fixing the Black Screen Issue

1. Power off the VM:  
   - Go to **Machine → Stop → Power Off**.  
2. Select your VM and open **Settings → Display**.  
3. Under **Graphics Controller**, select either **VMSVGA** or **VBoxSVGA**.  
4. (Optional) Increase **Video Memory** for smoother display, but avoid maxing it out to prevent VM issues.
5. Click **OK**, then start the VM again.

After applying these changes, Ubuntu should start correctly and display the installation screen.

---

Once Ubuntu is visible and running, you can proceed with the installation process inside the virtual machine.

### d. Complete the Ubuntu Installation

Before starting the installation, plug in your laptop if the battery is low to avoid interruptions during setup.

Follow the on-screen installation steps:

1. Choose your **language** and **location**.  
2. Continue through the installation wizard by clicking **Next** or **Skip** where appropriate.  
   Take a moment to read each screen before proceeding.  
3. Enter your **computer name**, **username**, and **password**.
4. If the installer prompts you to remove any connected devices, just press **Enter** to continue.  
5. When the installation completes, select **Restart Now** to reboot your virtual machine.

After the restart, Ubuntu 24.04 LTS will load for the first time.  
You now have a fully functional virtual machine running Ubuntu — ready for your next steps.

---

## Step 4: Setting Up Wazuh Server and Agent

Before installing the **Wazuh Manager** on your Ubuntu VM and the **Wazuh Agent** on your local machine, we need to check once more that both systems can communicate over the network.

### Check Your Windows Local Machine’s IP

1. Open the **Start Menu**, type **cmd**, and press **Enter** to open the Command Prompt.
2. Type:
   
       ipconfig
   
4. Scroll down to **Wireless LAN adapter Wi-Fi** (or whichever adapter you’re using).
5. Find the line labeled **IPv4 Address** — it should look like 192.168.1.x.

Note that address; it’s your computer’s IP on the local network.

### Check Your Ubuntu VM’s IP

1. Open the **Terminal** from **Show Applications** → **Terminal**, or press **Ctrl + Alt + T**.
2. Type:

       ifconfig

3. If the command is not found, install it with:

       sudo apt install net-tools

5. Look for the adapter named *enp0s3* — that’s usually your main network interface.
6. Find the line starting with *inet* (for example, inet 192.168.1.x).

That’s your VM’s IP address, save it as well.

Ignore 127.0.0.1, which refers only to the local machine.

### c. Confirm the Connection with a Ping Test

To ensure your host and VM can communicate even more, perform a simple ping test.

### From the Ubuntu VM:
1. Open the **Terminal**.  
2. Type the following command, replacing the IP with your Windows local machine’s IP address:
   
       ping 192.168.1.x

3. If you see replies like "64 bytes from 192.168.1.x...", the connection is working.
4. To stop the ping, press *Ctrl + C*.

### From Windows Host:
1. Open *Command Prompt*.
2. Type the following, replacing the IP with your VM’s IP address:

       ping 192.168.1.x

3. If you receive replies from the VM, the connection is confirmed.

If both ping tests succeed, your local machine and VM are properly connected on the same network and ready for the Wazuh setup.

---

## Enable Copy and Paste

Before installing Wazuh, it is helpful to enable copy and paste between your host machine and the Ubuntu VM.

1. Make sure the VM is powered off.  
2. In VirtualBox, select your Ubuntu VM and go to **Settings → General → Advanced**.  
3. Set **Shared Clipboard** to **Bidirectional**.  
4. If you changed this setting while the VM was running, reboot it to apply the change:   

       reboot
   
---

## a. Add the Wazuh GPG Key

Before installing Wazuh, you need to add its GPG key.  
This key ensures the packages that you're about to download are verified and safe.

Open your terminal and run the following command:

    curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --dearmor -o /usr/share/keyrings/wazuh-archive-keyring.gpg

If you receive an error indicating that **curl** is not installed, install it with:

    sudo apt install curl
   
## b. Download and Install Wazuh

Next, download and run the Wazuh installation script.

    curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i
   
### Command options:

- -a → Installs all Wazuh components (Manager, Indexer, and Dashboard).

- -i → Runs the installer in interactive mode.

This step may take several minutes to complete.
When finished, the installer will display your **admin username** and a **generated password** for accessing the Wazuh dashboard.

---

## Save Your Credentials:

To keep your credentials safe, open a text file and save them:

    nano wazuh_credentials.txt

Paste your credentials inside, then press:

**Ctrl + X* → to exit

**Y** → to save changes

**Enter** → to confirm







File Integrity Monitoring (FIM) Demo


