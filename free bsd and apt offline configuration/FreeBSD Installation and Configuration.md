
## 1. Download the FreeBSD ISO
- Visit the official FreeBSD download page:  
  [FreeBSD Download](https://www.freebsd.org/where.html)

## 2. Create a New Virtual Machine in VirtualBox

### 2.1. Set Up the VM
- Open **VirtualBox** and create a new virtual machine.
- Set the **operating system type** to **BSD** and version to **FreeBSD (64-bit)**.
- Allocate **2GB of RAM** and **20GB of storage** for the virtual machine.
- Attach the **FreeBSD ISO** as the **bootable disk**.

## 3. Boot the FreeBSD Installer

### 3.1. Boot Options
- Start the VM and select **Boot Multiuser** to begin the FreeBSD installation.

## 4. Install FreeBSD

### 4.1. Keymap Selection
- Continue with the **default keymap** (usually `US`).

### 4.2. Set Hostname
- Enter the **hostname** for your system (e.g., `freebsd.local`).

### 4.3. Distribution System Selection
- Choose the **default distribution system** to install the basic FreeBSD system.

### 4.4. Partitioning
- Choose **UFS** as the filesystem type for the partitioning.
  
#### Partitioning Steps:
1. Select **Use the entire disk** to use the whole virtual disk for FreeBSD.
2. Choose the **GPT (GUID Partition Table)** scheme for partitioning.
3. After setting up, confirm the partitioning and **commit** the changes.

### 4.5. Set Root Password
- When prompted, enter a **root password** for the system.

### 4.6. Network Configuration
- Choose the **default network adapter** for your VM.
- Set **DHCP** to automatically acquire an IP address.

### 4.7. Timezone and Locale Setup
- Select your **region**.
- Choose the **timezone**.
- Set the **keyboard layout** (default `US`).

### 4.8. Create User Account
- Create an **additional user account** and set a password for it.

### 4.9. Reboot the System
- After the installation is complete, **reboot** the system.

## 5. Post-Installation Configuration

### 5.1. SSH Configuration (If Needed)
- If you're facing SSH issues, you may need to modify the following configuration files:
  - `/etc/rc.conf`
  - `/etc/pam.d/sshd`
  - `/etc/ssh/sshd_config`

#### Using `ee` or `vim` editor:
To edit files, use the following commands:

`bash
ee /etc/rc.conf


### 5.2. Configure DNS

- To configure DNS, open the **`/etc/resolv.conf`** file and add the nameservers.

#### Edit DNS with `ee` editor:

bash

Copy

`ee /etc/resolv.conf`

- Add the **nameserver** line:

text

Copy

`nameserver <DNS_IP>`

### 5.3. Restart the System

- After making changes, **restart** the system to apply the new settings:

bash

Copy

`shutdown -r now`

## 6. Additional Configuration (SSH and Network)

### 6.1. Verify SSH Service

- To ensure SSH is properly set up, start the SSH service manually if needed:

bash

Copy

`service sshd start`

### 6.2. Check Network Connectivity

- Test network connectivity to ensure your system is connected to the network and can access the internet.

bash

Copy

`ping google.com`

If the system can ping external servers, the network is working correctly.

---

### Troubleshooting Tips

- If SSH is not working:
    
    - Make sure the SSH daemon is running: `service sshd start`
    - Check if the firewall is blocking SSH: `pfctl -sr` (for packet filter configurations).
    - Verify `/etc/rc.conf`, `/etc/pam.d/sshd`, and `/etc/ssh/sshd_config` are correctly configured.
- If DNS is not resolving:
    
    - Double-check the `/etc/resolv.conf` file and ensure correct nameserver entries are added.
    - Ensure your network connection is active.