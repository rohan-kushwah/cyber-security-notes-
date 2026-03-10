### 🔹 What is a TFTP Server?

**TFTP (Trivial File Transfer Protocol)** is a simple protocol used to transfer files between devices on a network. A **TFTP server** is a lightweight server application that allows clients (like routers, switches, or computers) to upload and download files using the TFTP protocol.

Unlike FTP, **TFTP doesn’t require authentication** (no username/password) and it runs over **UDP (port 69)**. It’s very basic and is mainly used in environments where simplicity and small file sizes are key.

---

### 🔹 Uses of TFTP Server

TFTP is typically used in **network environments** where devices need to **automatically send or receive configuration files or firmware updates**. Common scenarios:

- **Backing up configuration files** (e.g., from a Cisco router or switch)
    
- **Loading device firmware or OS images**
    
- **Booting diskless workstations using PXE (Preboot Execution Environment)**
    
- **Initial setup of network devices** where there's no GUI or command-line interface available yet
    

---

### 🔹 Real-Life Example

Let’s say you’re a **network admin** managing several **Cisco routers and switches**. You want to back up the configuration of a switch before making changes.

Here’s how TFTP would be used:

1. You set up a TFTP server on your PC or a dedicated machine.
    
2. You connect to the Cisco switch and run a command like:
    
    
    `copy running-config tftp:`
    
3. The switch asks for the TFTP server IP and the file name.
    
4. It sends its current config to the TFTP server as a backup.
    

Now if anything goes wrong during reconfiguration, you can easily restore the original settings from that backup using TFTP.


### **rivial-File-Transfer-Protocol (TFTP) Server**

#### **Trivial File Transfer Protocol (TFTP) Setup on RHEL/CentOS**

---

### **Package Verification and Installation**

**Check if the TFTP client or server is already installed:**

bash

CopyEdit

`rpm -qa | grep tftp`

**Install both the TFTP client and server:**

bash

CopyEdit

`yum install tftp tftp-server`

---

### **TFTP Package Information**

**View details about the installed TFTP server package:**


`rpm -qi tftp-server`

list  of files--
`rpm -ql tftp-server`

**Display the configuration files:**


`rpm -qc tftp-server`

for documentation files=
``rpm -qd tftp-server

### **Directory Setup for TFTP Boot**

**Navigate to the default TFTP root directory:**


`cd /var/lib/tftpboot/`

**Check if the directory exists:**


`ls -lha /var/lib/ | grep "tftpboot"`

**Set full permissions on the directory (for testing purposes only, not recommended for production):**


`chmod -R 777 /var/lib/tftpboot/`

**Copy test files, such as logs, into the TFTP root directory:**


`cp -v /var/log/* /var/lib/tftpboot/`


### **Systemd Configuration for TFTP**

**Copy the default systemd service and socket unit files to the custom path:**


`cp -v /usr/lib/systemd/system/tftp.service /etc/systemd/system/tftp-server.service 
``cp -v /usr/lib/systemd/system/tftp.socket /etc/systemd/system/tftp-server.socket`

**Review the TFTP daemon documentation:**


`man in.tftpd`

---

### **TFTP Server Service File (`tftp-server.service`)**




[Unit]
Description=Tftp Server
Requires=tftp-server.socket
Documentation=man:in.tftpd

[Service]
ExecStart=/usr/sbin/in.tftpd -c -p -s /var/lib/tftpboot
StandardInput=socket

[Install]
WantedBy=multi-user.target
Also=tftp-server.socket


### `[Unit]` Section

This section defines metadata and dependencies for the service.

- **`Description=Tftp Server`**  
    A short human-readable description of the service.
    
- **`Requires=tftpi-server.socket`** ❗️**(Likely typo)**  
    This indicates that the service requires a corresponding `.socket` unit to be active.  
    ✅ **Correction:** Probably meant to be `tftp-server.socket`
    
- **`Documentation=man:in.tftpd`**  
    Points to the manual page for `in.tftpd`, which is the TFTP daemon binary.
    

---

### `[Service]` Section

This section defines how the service runs.

- **`ExecStart=/usr/sbin/in.tftpd -c -p -s /var/lib/tftpboot`**  
    Specifies the command to start the service:
    
    - `-c`: Allows new files to be created.
        
    - `-p`: Runs in "standalone" mode (as a daemon).
        
    - `-s /var/lib/tftpboot`: Sets the root directory for file access (chroot).
        
- **`StandardInput=socket`**  
    Tells systemd to use the socket that activated the service as its standard input.  
    This means the service is socket-activated and starts only when there's an incoming TFTP connection.
    

---

### `[Install]` Section

This section defines how the service behaves when enabled.

- **`Also=tftp-serevr.socket`** ❗️**(Typo)**  
    Specifies that the corresponding `.socket` unit should be installed (enabled) alongside the service.  
    ✅ **Correction:** Should be `Also=tftp-server.socket`
    
- **`WantedBY=multi-user.target`**  
    Ensures that the service is started as part of the `multi-user.target`, which is typical for network services and non-GUI environments.




### **TFTP Server Socket File (`tftp-server.socket`)**


`vim /etc/systemd/system/tftp-server.socket`


[Unit]
Description=Tftp Server Activation Socket

[Socket]
ListenDatagram=69
BindIPv6Only=both

[Install]
WantedBy=sockets.target



### **Firewall Configuration Using Firewalld**

**Check if `firewalld` is active:**


`systemctl status firewalld`

**Start and enable `firewalld` if not running:**


`systemctl start firewalld systemctl enable firewalld`

**Add the TFTP service to the public zone permanently:**



`firewall-cmd --zone=public --add-service=tftp --permanent`

**Or open UDP port 69 directly:**


`firewall-cmd --zone=public --add-port=69/udp --permanent`

**Reload the firewall configuration:**


`firewall-cmd --reload`

**Verify the rule is applied:**


`firewall-cmd --zone=public --list-all`
### **Starting and Enabling the TFTP Server**

**Restart the TFTP socket to activate the service:**


`systemctl restart tftp-server.socket`

**Enable the service to start on boot:**


`systemctl enable tftp-server`

---

### **Verifying TFTP Server Status**

**Use `netstat` or `ss` to confirm that TFTP is listening on UDP port 69:**

With `netstat`:


`netstat -nltup | grep 69`

Or with `ss`:


`ss -uln | grep 69`

**You should see** `in.tftpd` **listening on** `0.0.0.0:69` **or** `[::]:69`.

# TFTP client--


### **TFTP Transfers Using A Client Like tftp or atftp**

---

### **Installing TFTP Client Utilities**

**Install the basic TFTP client:**

For RHEL/CentOS/Fedora:

bash

CopyEdit

`yum install tftp`

For Debian/Ubuntu:

bash

CopyEdit

`apt install tftp`

**Install the Advanced TFTP client (`atftp`), which supports more features:**


`yum install atftp`

---

### **Testing File Download (GET Operation)**

To test file downloading from a TFTP server using the classic `tftp` client:


`tftp <server-ip>`

**Example interaction:**


`tftp> get testfile.txt tftp> quit`

### **TFTP File Download One-Liners and Advanced Usage**

**In a one-liner (using `tftp`):**


`tftp -v <server-ip> -c get testfile.txt`

**Using `atftp`:**


`atftp --get testfile.txt --remote-host <server-ip>`

**To specify a local file name:**


`atftp --get testfile.txt --local-file downloaded.txt --remote-host <server-ip>`

### testing File Upload (PUT Operation)**

To upload a file to the TFTP server (write permission required on server):


`tftp <server-ip>`

**Example interaction:**


`tftp> put sample.txt tftp> quit`

**Or use a one-liner:**


`tftp -v <server-ip> -c put sample.txt`

---

### **Downloading A File (GET Operation) using atftp**


`atftp -g -r messages -l messages 192.168.1.34`

- `-g`: get (download) file from server
    
- `-r messages`: remote file name on the server
    
- `-l messages`: local file name to save as
    
- `192.168.1.34`: the TFTP server IP address

![[Pasted image 20250423140959.png]]
![[Pasted image 20250423141045.png]]
![[Pasted image 20250423141214.png]]