## Kali Linux Fundamentals
***
#### 🧑‍💻 Kali Linux History and Introduction
***
##### 🔎 What is Kali Linux?
Kal Linux is a Debian-based Linux distribution developed and maintained by [Offensive Security](https://www.offensive-security.com/), designed for:
*   Penetration Testing
*   Ethical Hacking
*   Digital Forensics
*   Security Research

It comes pre-installed with hundreds of specialized tools covering various domains such as reconnaissance, exploitation, forensics, wireless attacks, and post-exploitation.

#### 🧪 BackTrack Linux (2006–2013)
***
Before Kali, there was **BackTrack Linux**, a powerful penetration testing platform created by merging:
*   WHAX (Whoppix/WHAX) – Wireless auditing distro
*   Auditor Security Collection – Focused on security tools

Initially based on [Slackware](http://www.slackware.com/), and later [Ubuntu](https://ubuntu.com/), BackTrack laid the foundation for modern offensive security Linux distributions.

#### 📦 BackTrack v1 (Released: 2006)
***
The first official release of BackTrack introduced a portable and unified toolset for security professionals.

**Key Features:**
*   Combined WHAX and Auditor tools
*   Bootable Live CD
*   Focused on wireless, network scanning, and vulnerability assessment
*   Included tools like [Aircrack-ng](https://www.aircrack-ng.org/), [Kismet](https://www.kismetwireless.net/), [Nmap](https://nmap.org/), [Metasploit](https://www.metasploit.com/)

**Limitations:**
*   No proper package management
*   Limited persistence and customization
*   Not compliant with the [Filesystem Hierarchy Standard (FHS)](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)

#### 🔁 Evolution of BackTrack Versions
***

| Version      | Release Year | Base System | Key Notes                               |
| :----------- | :----------- | :---------- | :-------------------------------------- |
| BackTrack v1 | 2006         | Slackware   | First version, Live CD, merged toolsets |
| BackTrack v2 | 2007         | Slackware   | Improved hardware detection             |
| BackTrack v3 | 2008         | Slackware   | KDE GUI improvements, more tools        |
| BackTrack v4 | 2009         | Ubuntu      | Shifted to Ubuntu; added persistence    |
| BackTrack 5  | 2011         | Ubuntu      | "Revolution", GNOME/KDE desktop options |

#### 👶 Birth of Kali Linux (2013)
In **March 2013**, Kali Linux was introduced as the successor to BackTrack.

##### 🔧 Why the Shift?
*   BackTrack's lack of package management made updates difficult
*   Custom scripts were inconsistent and hard to maintain
*   Need for a clean, modular, and standards-compliant system

##### ✅ Key Improvements in Kali:
*   Built on **Debian**
*   Uses **APT** for updates and package installation
*   Fully **FHS-compliant**
*   ISO customization via **live-build**
*   Designed for **rolling updates**
*   Originally intended for **root user by default**

##### 🛠️ Maintainers and Community
*   **Developer:** Offensive Security
*   **Website:** [https://www.kali.org](https://www.kali.org)
*   **Tools Repository:** [https://tools.kali.org](https://tools.kali.org)
*   Strong and active community with frequent updates and documentation

---

### Overview of Kali Linux
Kali Linux is a comprehensive security platform for professionals involved in:
*   Penetration Testing
*   Digital Forensics
*   Security Auditing

It bundles an extensive collection of tools including:
*   Nmap
*   Metasploit
*   Burp Suite
*   Aircrack-ng
*   John the Ripper, and more.

#### 🧰 Essential Tools in Kali Linux
| Tool             | Purpose                   |
| :--------------- | :------------------------ |
| Nmap             | Network scanning          |
| Metasploit       | Exploitation framework    |
| Burp Suite       | Web application testing   |
| Wireshark        | Packet capture and analysis |
| Aircrack-ng      | Wireless cracking suite   |
| Hydra            | Login brute-forcing       |
| John the Ripper  | Password cracking         |
| Nikto            | Web server vulnerability scan |

### 💻 Basic Linux Commands
*   Show current directory
```bash
pwd
```
*   List files in a directory
```bash
ls -l
```
*   Change directory
```bash
cd /path/to/directory
```
*   View contents of a file
```bash
cat filename.txt
```
*   Copy a file
```bash
cp file1.txt /destination/
```
*   Move or rename a file
```bash
mv oldname.txt newname.txt
```
*   Remove a file or directory
```bash
rm file.txt
rm -r folder/
```
*   Search for a string in files
```bash
grep "search_string" filename
```
*   Find files by name
```bash
find / -name "file.txt"
```

### 🛠️ System Management
***
*   View current users
```bash
who
```
*   Show logged-in user
```bash
whoami
```
*   Check IP address
```bash
ip a
```
*   Display running processes
```bash
ps aux
```
*   Kill a process
```bash
kill -9 <PID>
```

### 🔒 User and Permission Basics
***
*   Create a new user
```bash
sudo adduser hacker
```
*   Change file permissions
```bash
chmod 755 script.sh
```
*   Change ownership
```bash
chown user:user file.txt
```

### 📦 Package Management (APT)
***
*   Update repositories
```bash
apt update
```
*   Upgrade packages
```bash
apt upgrade
```
*   Install a tool
```bash
apt install nmap
```
*   Remove a tool
```bash
apt remove hydra
```
### 🧪 Service Management
*   Start a service
```bash
systemctl start apache2
```
*   Enable service at boot
```bash
systemctl enable apache2
```
*   Check status
```bash
systemctl status ssh
```

### 🌐 Networking Basics
*   Ping a host
```bash
ping google.com
```
*   Scan open ports with Nmap
```bash
nmap -sS 192.168.1.1
```
*   Display routing table
```bash
route -n
```

### 📁 File Compression
*   Create a tar.gz archive
```bash
tar -czvf archive.tar.gz folder/
```
*   Extract a tar.gz archive
```bash
tar -xzvf archive.tar.gz
```

### 🧑‍🎓 Kali-Specific Commands
*   Launch Metasploit
```bash
msfconsole
```
*   Run Social Engineering Toolkit (SET)
```bash
setoolkit
```
*   Start Burp Suite
```bash
burpsuite
```
*   Start airmon-ng for wireless monitoring
```bash
airmon-ng start wlan0
```

### Privilege Escalation Tips
*   Check for SUID binaries
```bash
find / -perm -4000 2>/dev/null
```
*   Look for world-writable files
```bash
find / -type f -perm -o+w 2>/dev/null
```
*   Enumerate OS and kernel version
```bash
uname -a
cat /etc/os-release
```

### 📌 Pro Tips
*   Run tools as **root** when needed:
```bash
sudo su
```
*   Use **VirtualBox Snapshots** before testing exploits.
*   Maintain a **notes file** to record findings during pentests.

---
---


## 💿 Choosing Your Kali Linux Version
***
Kali Linux offers a wide variety of installation images, each tailored for a specific use case. Choosing the right one depends on your hardware, your goal, and your level of expertise.

---
### 📀 Installer Images
***
This is the traditional method for a full bare-metal installation of Kali Linux onto a computer's hard drive. This gives you a permanent, persistent operating system.

*   **Installer:** This is the standard, recommended download for most users. It contains a complete image that can be installed without needing an internet connection during the process. It includes the default set of Kali tools.
*   **Weekly:** This is a "bleeding-edge" version of the installer image, updated weekly with the latest packages and fixes. It's great for users who need the newest software but may be slightly less stable than the main release.
*   **NetInstaller:** A very small image that contains only the components needed to start the installation. It downloads all the required packages from the internet during the setup process. This ensures you get the most up-to-date version of everything but requires a stable internet connection.
*   **Everything:** A massive, comprehensive image that includes every single tool available in the Kali repositories. This is designed for completely offline installations where you need access to the full suite of tools without an internet connection.

---
### 🖥️ Virtual Machines
***
These are pre-built, pre-configured images designed to run inside virtualization software. This is often the easiest and safest way to get started with Kali, as it runs in an isolated environment on top of your existing operating system.

*   **VMware:** Pre-configured virtual machine files (.vmx) optimized for VMware Workstation, VMware Player, and VMware Fusion. These images come with VMware Tools pre-installed for better performance and seamless integration.
*   **VirtualBox:** Ready-to-use OVA (Open Virtual Appliance) files specifically configured for Oracle VirtualBox. These include Guest Additions for enhanced graphics, shared folders, and clipboard functionality.
*   **Hyper-V:** Native images for Microsoft's Hyper-V virtualization platform, available on Windows Pro and Enterprise editions. These are optimized for Windows integration and enhanced session mode.
*   **QEMU:** Raw disk images suitable for QEMU/KVM virtualization on Linux hosts. These provide excellent performance for Linux-based virtualization environments.

### 🍓 ARM
***
These images are specifically compiled for ARM-based single-board computers and devices. Each image is tailored for specific hardware to ensure optimal performance and compatibility.

*   **Raspberry Pi:** Multiple versions supporting Pi 1, Pi 2/3/4, Pi Zero/Zero W, and Pi 400. These include both 32-bit and 64-bit versions where applicable, with full hardware acceleration support.
*   **Pinebook/Pinebook Pro:** Optimized images for Pine64's laptop offerings, including proper power management and hardware driver support.
*   **BeagleBone Black:** Customized for this popular development board with full GPIO and hardware interface support.
*   **ODROID:** Various images supporting ODROID-C2, XU4, and other models in the ODROID family.
*   **Generic ARM:** A more universal image that can work on various ARM devices but may require manual configuration.

### 📱 Mobile
***
This refers to **Kali NetHunter**, a version of Kali designed for mobile penetration testing on Android devices. Different versions cater to various installation methods and device capabilities.

*   **NetHunter Rootless:** Can be installed on any Android device without root access. Provides limited functionality but works on unmodified phones.
*   **NetHunter Lite:** For rooted devices that don't support custom kernels. Offers more features than Rootless but less than the full version.
*   **NetHunter Full:** The complete NetHunter experience requiring a custom kernel. Includes wireless injection, HID attacks, and BadUSB capabilities.
*   **NetHunter Pro:** Premium version for specific devices like OnePlus, Nexus, and Pixel phones with dedicated kernel patches.

### ☁️ Cloud
***
These are pre-built images optimized for major cloud service providers. They're configured to work seamlessly with cloud-specific features and billing models.

*   **AWS AMI:** Amazon Machine Images available in the AWS Marketplace. Pre-configured with cloud-init for automatic setup and AWS CLI tools.
*   **Azure:** Available through Azure Marketplace with Azure Linux Agent pre-installed for proper integration with Azure services.
*   **Google Cloud:** GCP-optimized images with Google Cloud SDK and proper network configuration for Google's infrastructure.
*   **Digital Ocean:** Available as a one-click droplet with optimized kernels and automatic security updates enabled.
*   **Linode:** Stackscripts and images configured for Linode's infrastructure with proper networking and storage drivers.

### 📦 Containers
***
Lightweight Kali Linux images designed for containerization. These provide isolated environments for specific tools without the overhead of a full operating system.

*   **Docker Official:** The official Kali Docker image maintained by Offensive Security. Minimal base system with apt repositories configured.
*   **Docker Full:** A larger Docker image that includes the default metapackage of tools, ready for immediate use.
*   **Docker Core:** An ultra-minimal image containing only essential components for building custom containers.
*   **LXC/LXD:** Linux container images optimized for LXC/LXD with proper permissions and device access configurations.
*   **Podman:** OCI-compliant images that work with Podman for rootless container deployments.

### ⚡ Live Boot
***
These images allow you to run Kali Linux directly from removable media without installation. Perfect for forensics, temporary use, or testing on different hardware.

*   **Live Standard:** The default live image with the XFCE desktop environment and standard tool selection. Supports both BIOS and UEFI boot.
*   **Live Everything:** A comprehensive live image containing all available Kali tools. Requires larger USB drives but provides offline access to the entire toolkit.
*   **Live Encrypted Persistence:** Configured to support encrypted persistent storage on USB drives, allowing you to save files and settings between boots.
*   **Live Forensics Mode:** Special live image that doesn't auto-mount drives and includes forensic tools, designed to preserve evidence integrity.

### 🪟 WSL (Windows Subsystem for Linux)
***
Kali Linux integrated into Windows through Microsoft's Windows Subsystem for Linux. Different versions cater to various WSL implementations and use cases.

*   **WSL 2:** The recommended version using a real Linux kernel for better compatibility and performance. Available through the Microsoft Store.
*   **WSL 1:** Legacy version using translation layer. Still available for older Windows versions or specific compatibility needs.
*   **Win-KeX:** Kali's desktop experience for WSL, providing a full GUI environment within Windows using VNC or RDP.
*   **WSL Enterprise:** Custom WSL images for corporate environments with pre-configured security policies and tool restrictions.

### 📜 Build Scripts
***
For advanced users who want complete control over their Kali installation. These tools allow you to create custom images tailored to specific needs.

*   **live-build:** The official Debian live-build system customized for Kali. Create custom ISOs with your choice of desktop environments and tools.
*   **kali-arm-build-scripts:** Scripts for building custom ARM images for unsupported or new ARM devices.
*   **kali-docker:** Dockerfile and build scripts for creating custom Docker containers with specific tool combinations.
*   **kali-cloud-build:** Templates and scripts for building cloud-optimized images with custom configurations.
*   **kali-nethunter-build:** Build system for creating custom NetHunter images for Android devices.

---
---