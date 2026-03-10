**FTP (File Transfer Protocol)** is a standard network protocol used to transfer files between a client and a server over a computer network, such as the internet or a private corporate intranet.

---

### 🔹 What is an **FTP Server**?

An **FTP server** is a system that uses FTP to store and share files. Users (clients) can connect to the server using FTP client software (like FileZilla or WinSCP), authenticate with a username and password, and upload or download files depending on their permissions.

---

### 🔹 Uses of FTP Server in the Corporate Field

Here are common corporate uses of an FTP server:

1. **File Sharing Between Departments**
    
    - Allows different teams (like Sales, HR, and IT) to upload and access shared documents quickly and securely.
        
2. **Data Backup & Archiving**
    
    - Automatically backs up critical data from client machines or servers to a central FTP server for disaster recovery.
        
3. **Remote File Access**
    
    - Employees working remotely or at branch offices can access files hosted on the company’s FTP server.
        
4. **Large File Transfers**
    
    - Useful for transferring large files that are too big for email (e.g., design files, reports, videos).
        
5. **Software & Patch Distribution**
    
    - IT teams use FTP to distribute updates, patches, or custom software builds to multiple users or machines.
        
6. **Web Hosting Support**
    
    - Web developers use FTP to upload website files to a web server.
        
7. **Secure Data Exchange with Clients or Partners**
    
    - With user authentication and directory restrictions, companies can securely share files with external stakeholders.
        

---

### 🔹 Types of FTP

- **FTP (Basic)** – Unencrypted, not secure.
    
- **FTPS (FTP Secure)** – Adds SSL/TLS encryption.
    
- **SFTP (SSH File Transfer Protocol)** – A different protocol using SSH, more secure and commonly used in modern environments.

# configuration-

ftp do type me atta hai ek client side ftp aur sever side vsftp


## **Anonymous-Access-on-FTP-Server**

### **Anonymous FTP Access Configuration on a Linux Server**

This guide walks through installing, configuring, and enabling anonymous access on an FTP server using `vsftpd` on a Red Hat-based system. It also includes firewall setup using `firewalld`.

---

### **1. Check for Existing FTP Packages**

Check if any FTP-related packages are already installed:


`rpm -qa | grep ftp`

---

### **2. Install vsftpd and ftp Client**

Install the FTP server and client if they aren’t already installed:


`yum install vsftpd ftp`

**Verify installation:**


`rpm -qa | grep ftp`

---

### **3. Inspecting `vsftpd` Package Details**

Check detailed package information:


`rpm -qi vsftpd`

**List installed files:**


`rpm -ql vsftpd`

use qc and qd for configuration and documnetation file'

## **4. Configure vsftpd**

Edit the main configuration file:


`vim /etc/vsftpd/vsftpd.conf`

### **Example configuration:**

anonymous_enable=YES
local_enable=YES
write_enable=YES
local_umask=022
anon_upload_enable=YES
anon_mkdir_write_enable=YES
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd
userlist_enable=YES
pasv_min_port=55000
pasv_max_port=55999
pasv_enable=YES
"PASV ports" (short for **Passive FTP ports**) refer to the **range of ports used by an FTP server in passive mode** for data transfers.

---

### **This configuration enables:**

- Anonymous and local user login
    
- Upload and directory creation for anonymous users
    
- Passive mode (required for many FTP clients)
## **5. Manage vsftpd Service**

Check if the service is running:


`systemctl status vsftpd.service`

Start the service:


`systemctl start vsftpd.service`

Restart the service (e.g., after configuration changes):


`systemctl restart vsftpd.service`

Enable the service at boot:


`systemctl enable vsftpd.service`

---

## **6. Verify FTP Listening Ports**

Check if the FTP service is listening on the correct ports:


`netstat -nltup | grep ftp`

---

## **7. Set Up Anonymous Content**

Navigate to the default FTP root:


`cd /var/ftp/`

Copy content for anonymous access:


`cp -vr /var/www/html/site/* /var/ftp/pub`

chmod -Rv 777 /var/ftp/pub/


cp -vr /var/www/html/site/* /var/ftp/pub




## **8. Check Open Files Used by FTP**

Verify FTP-related open files and sockets:


`lsof | grep ftp`

---

## **Additional Tips**

- If you're running **SELinux**, make sure to set proper booleans like `allow_ftpd_anon_write` using `setsebool`.
    
- Ensure the `/var/ftp/pub` directory has correct permissions for anonymous uploads (e.g., `chmod 777` for testing).
    
- Use an FTP client like **FileZilla** or the `ftp` command-line client to verify anonymous access.
    

---

## **Configuring `firewalld` for FTP Access**

By default, `firewalld` is the firewall management tool on many modern Linux distributions (like CentOS 7+, RHEL 7+, Fedora, etc.). To support **FTP**, especially **passive FTP**, you need to configure it.

---

### **1. Start and Enable `firewalld`**

Ensure the service is running:


`systemctl start firewalld systemctl enable firewalld`

Check its status:


`systemctl status firewalld`

## **2. Allow FTP Service**

This opens **port 21**, used by FTP control connections:


`firewall-cmd --permanent --add-service=ftp`

---

## **3. Allow Passive FTP Ports**

If passive mode is configured in `/etc/vsftpd/vsftpd.conf` (e.g., **55000–55999**), you must allow this range explicitly:


`firewall-cmd --permanent --add-port=55000-55999/tcp`

---

## **4. Reload `firewalld` to Apply Changes**


`firewall-cmd --reload`

---

## **5. Verify Rules**

List all allowed services and ports:


`firewall-cmd --list-all`

### **6. Optional: Allow Access Only in Specific Zones**

If you're using a specific zone (e.g., `public`, `internal`), specify it like this:


`firewall-cmd --zone=public --permanent --add-service=ftp firewall-cmd --zone=public --permanent --add-port=55000-55999/tcp firewall-cmd --reload`

---

### **7. SELinux Consideration (If Enforcing)**

If SELinux is enabled, you may also need to allow FTP passive mode with these:


`setsebool -P ftp_home_dir=1 setsebool -P allow_ftpd_full_access=1`

You can confirm settings with:


`getsebool -a | grep ftp`

---

### **Summary of Required Ports**

|Service|Protocol|Port(s)|Description|
|---|---|---|---|
|FTP|TCP|21|Control connection|
|FTP-PASV|TCP|55000-55999|Passive data connections|


# client tool==

 apt install ftp 
 then ab browse karemnge-
 ftp then ftp server ip address

anonymous write kar sakta hi delete nbhi
FTP-Client-Usage  
FTP Client Usage

This guide provides a step-by-step walkthrough of using the ftp command-line client to connect to an FTP server, switch modes, and manage remote files.

Connect to an ftp server  
To begin an FTP session, use the following command:  
`ftp 192.168.1.6`

login credentials  
When prompted, enter a valid username and password. For anonymous access, try one of the following usernames:

- ftp
- anonymous
- anon
### 🔌 **Connection and Login**

|Command|Description|
|---|---|
|`ftp <hostname>`|Connect to the FTP server|
|`open <hostname>`|Open a connection to the FTP server|
|`user <username>`|Specify username if not prompted automatically|
|`bye` or `quit`|Disconnect from the FTP server and exit|

---

### 📂 **Navigation Commands**

|Command|Description|
|---|---|
|`ls` or `dir`|List files in the current directory on server|
|`cd <directory>`|Change directory on the server|
|`lcd <directory>`|Change local directory on your machine|
|`pwd`|Show current directory on the server|
|`lpwd`|Show current directory on the local machine|

---

### 📤 **Uploading Files**

|Command|Description|
|---|---|
|`put <filename>`|Upload a file to the FTP server|
|`mput <files>`|Upload multiple files (wildcards allowed)|

---

### 📥 **Downloading Files**

|Command|Description|
|---|---|
|`get <filename>`|Download a file from the FTP server|
|`mget <files>`|Download multiple files (wildcards allowed)|

---

### ⚙️ **Transfer Modes**

|Command|Description|
|---|---|
|`binary`|Set transfer mode to binary (for files like .zip, .exe)|
|`ascii`|Set transfer mode to ASCII (for text files)|

---

### 🛠️ **Other Useful Commands**

|Command|Description|
|---|---|
|`delete <filename>`|Delete file on the server|
|`mkdir <directory>`|Create a new directory on the server|
|`rmdir <directory>`|Remove a directory on the server|
|`status`|Show current status|
|`help` or `?`|Show available commands|