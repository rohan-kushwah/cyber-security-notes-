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