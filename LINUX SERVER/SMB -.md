SMB 

|   |   |   |   |   |
|---|---|---|---|---|
|Term|Type|Purpose|Exists Where|Notes|
|SMB|Protocol (Language)|File sharing, printer sharing, IPC communication over a network|Windows (native), Linux (clients via software)|SMB2/3 are modern versions, secure|
|CIFS|Old Implementation of SMB1|Microsoft's old "Internet-ready" version of SMB1|Windows NT/2000/XP, old NAS devices|Obsolete, insecure, avoid use|
|Samba|Linux Software Implementation|Makes Linux act as SMB server or client|Linux, Unix systems|Uses SMB protocol underneath|

SMB Protocol 

(Server Message Block - Complete Deep-Dive Reference) 

1. Introduction to SMB: What and Why? 

SMB (Server Message Block) is a network file sharing protocol designed to allow applications and users to read, write, and execute files and access resources like printers and serial ports over a network. 

- Why SMB Exists: Computers need to share resources such as files and printers over a local or wide-area network. SMB provides a standardized way to do this, ensuring consistent communication, authentication, authorization, and data sharing among different systems. 
    
- Who created SMB: SMB was originally designed by IBM in the 1980s and then heavily modified and popularized by Microsoft as part of its Windows networking systems. 
    
- Where SMB is Used: 
    
    - Windows networks (most native usage) 
        
    - Samba on Linux/UNIX systems (open-source SMB implementation) 
        
    - Embedded systems like printers and NAS devices 
        

2. Fundamental Working Principle of SMB 

SMB works based on the client-server model: 

- The SMB client (e.g., a user’s machine) sends requests to an SMB server (e.g., a file server) asking to perform operations such as "open file," "read file," "write to file," "list directory contents," etc. 
    
- The server processes the request and sends back responses with data or status information. 
    

⚡ Important: SMB is stateful — it maintains a session between the client and server (meaning they keep track of the connection status, opened files, etc.) 

3. Architecture of SMB 

When you use SMB, it operates across several layers: 

|   |   |
|---|---|
|Layer|Role|
|Application Layer|Handles file/printer sharing, session establishment, authentication.|
|Presentation Layer|Encoding/decoding data, format translation.|
|Session Layer|Handles session management (start/stop/maintain).|
|Transport Layer|Typically TCP/IP, specifically TCP port 445 (older versions also used TCP/NetBIOS over ports 137, 138, 139).|

4. SMB Versions and Evolution 

|   |   |   |
|---|---|---|
|SMB Version|Features|Year|
|SMB1|Original version, weak security, chatty protocol (many messages).|1980s|
|SMB2|Improved performance, reduced commands (from ~100 to ~19), pipelining support.|2006 (Windows Vista/Server 2008)|
|SMB2.1|Leasing, larger buffers.|2008|
|SMB3.0|Encryption, multichannel (multiple TCP connections), better resiliency.|2012|
|SMB3.1.1|Pre-authentication integrity, stronger encryption (AES-GCM).|2015|

🔴 Note: SMB1 is considered highly insecure and is disabled by default in modern systems. 

5. SMB Packet Flow (In Real-World Binary Terms) 

When you capture SMB traffic (e.g., using Wireshark), you will see exact packet structures. Here's what typically happens step-by-step: 

5.1 TCP Handshake (Underlying Transport Layer) 

Before SMB packets even begin, there’s a TCP 3-Way Handshake: 

|   |   |   |   |
|---|---|---|---|
|Step|Source|Destination|Details|
|SYN|Client → Server|Initiates connection to TCP 445||
|SYN-ACK|Server → Client|Acknowledges||
|ACK|Client → Server|Finalizes connection||

5.2 SMB Session Setup (SMB-Specific Packets) 

Once TCP is established, SMB packets start to flow: 

(1) SMB Negotiation Protocol Request 

- Client sends a Negotiate Protocol Request listing the SMB versions it supports. Packet content example (hex-binary view): Header (4 bytes) | Protocol ID ("SMB") | Command: Negotiate Protocol | Flags | Session ID 
    
- Fields inside the packet: 
    
    - Client capabilities (e.g., signing, large MTU support) 
        
    - SMB dialects (supported versions like SMB2.1, SMB3.0) 
        

(2) SMB Negotiation Protocol Response 

- Server replies with the chosen SMB dialect (e.g., SMB2.1) and security features (like signing required, encryption supported). 
    

(3) SMB Session Setup Request 

- Client sends authentication information, typically: 
    
    - NTLM or Kerberos tokens (if integrated into a domain) 
        
- Fields: 
    
    - Security blob (the authentication info) 
        
    - Requested capabilities 
        
    - Client GUID 
        

(4) SMB Session Setup Response 

- Server responds indicating: 
    
    - Success (session established) or 
        
    - Request for further authentication steps 
        

(5) SMB Tree Connect Request 

- Client maps to a specific share like: [\\server-ip\sharename](file:///\\server-ip\sharename) 
    
- This allows the client to operate under a Tree ID (TID) which is used in further operations. 
    

(6) SMB File/Directory Operations 

Once a Tree Connect is done: 

- Client can open, read, write, delete, list directories, etc., by sending SMB commands inside packets. 
    

Each operation is a separate SMB message over the TCP session, often consisting of: 

|   |   |
|---|---|
|Header Field|Purpose|
|Command|Which operation: e.g., READ, WRITE, CREATE|
|Session ID|Identifies which session the request belongs to|
|Tree ID|Identifies which shared resource|
|File ID|Identifies the file inside the share|
|Credit Charge|How many operations the client can pipeline|

5.3 SMB Specific Packet Headers (Byte-Level Details) 

Every SMB packet (SMB2+) has a standard header: 

|   |   |   |
|---|---|---|
|Field Name|Size|Meaning|
|Protocol ID|4 bytes|Always 0xFE 'S', 'M', 'B', '\0'|
|Structure Size|2 bytes|Size of the header structure|
|Credit Charge|2 bytes|Flow control|
|Status|4 bytes|Return codes|
|Command|2 bytes|Type of SMB operation|
|Credits Requested|2 bytes|For pipelining|
|Flags|4 bytes|Various status flags|
|Next Command|4 bytes|Chaining operations|
|Message ID|8 bytes|Identify requests|
|Process ID|4 bytes|Identify client's process|
|Tree ID|4 bytes|Identify share|
|Session ID|8 bytes|Identify session|
|Signature|16 bytes|Packet signature (security)|

6. SMB Commands List (Real-World View) 

|   |   |
|---|---|
|Command|Purpose|
|NEGOTIATE|Choose protocol version|
|SESSION_SETUP|Authenticate and create session|
|TREE_CONNECT|Connect to share|
|CREATE|Open file or directory|
|CLOSE|Close file or directory|
|READ|Read file data|
|WRITE|Write file data|
|QUERY_DIRECTORY|List files and folders|
|DELETE|Delete file|
|ECHO|Check connection is alive|
|LOGOFF|End session|

7. SMB Security Mechanisms 

Authentication: 

- NTLMv1 (old, insecure) 
    
- NTLMv2 (improved) 
    
- Kerberos (best for domain environments) 
    

Message Signing: 

- Integrity check to detect tampering. 
    
- SMBv2/3 require or allow optional signing. 
    

Encryption: 

- SMB3+ supports end-to-end encryption (AES-128-CCM, AES-128-GCM, AES-256-GCM). 
    

8. SMB Vulnerabilities (Very Important for Hackers) 

|   |   |
|---|---|
|Vulnerability|Description|
|EternalBlue|SMBv1 exploit (CVE-2017-0144), enabled WannaCry ransomware.|
|SMBGhost|SMBv3 compression flaw leading to RCE (CVE-2020-0796).|
|Null Sessions|Anonymous SMB logins (SMB over NetBIOS) can leak info.|

⚠️ Hence, never expose SMB directly to the internet without strict control. 

9. SMB Ports 

|   |   |   |
|---|---|---|
|Port|Protocol|Purpose|
|445/TCP|Direct SMB over TCP (modern usage)||
|137/UDP|NetBIOS Name Service (old SMB1)||
|138/UDP|NetBIOS Datagram Service (browsing)||
|139/TCP|NetBIOS Session Service (SMB over NetBIOS)||

10. Practical SMB Communication Example 

Let's walk through a real example (flow of packets): 

1. TCP 3-way handshake on port 445 2. Client → Server: SMB NEGOTIATE request 3. Server → Client: SMB NEGOTIATE response 4. Client → Server: SMB SESSION SETUP request (NTLM token) 5. Server → Client: SMB SESSION SETUP response (authentication success) 6. Client → Server: TREE CONNECT to [\\192.168.1.10\share](file:///\\192.168.1.10\share) 7. Server → Client: TREE CONNECT OK (with Tree ID) 8. Client → Server: CREATE request to open "document.txt" 9. Server → Client: CREATE response with File ID 10. Client → Server: READ request on File ID 11. Server → Client: READ response with file data 12. Client → Server: CLOSE File ID 13. Client → Server: LOGOFF 14. TCP connection closed  

Each step above maps exactly to a packet you can capture and analyze. 

--------------------------------------------------------------------------------------------------- 

SMB, Samba, and CIFS 

1. SMB — Server Message Block (Protocol) 

First, understand: 

SMB is a protocol — a language that defines how two computers communicate to share files, printers, and other resources across a network. 

- It defines: 
    
    - Commands: like OPEN FILE, READ FILE, WRITE FILE 
        
    - Responses: like SUCCESS, ACCESS DENIED 
        
    - Packet formats: how requests and replies are structured bit-by-bit 
        
    - Authentication methods: like NTLM or Kerberos 
        
    - Transport rules: how to send over TCP, port 445 
        

SMB does not refer to any specific software. It’s only the standard. 

In short: 

👉 SMB = Communication protocol (language) 

Analogy: SMB is like English language → Any two people speaking English can understand each other, regardless of their nationality (Windows, Linux, etc.). 

2. Samba — Software Implementation of SMB 

Now, Samba is a real software suite — an open-source implementation of the SMB protocol for Linux and UNIX systems. 

- Written originally by: Andrew Tridgell (1992) 
    
- Purpose: 
    
    - Allow Linux machines to share files and printers with Windows machines 
        
    - Allow Linux machines to join Windows Active Directory domains (Domain Controller support) 
        
    - Allow Linux machines to authenticate against Windows systems 
        
- What Samba does: 
    
    - Listens on TCP port 445 
        
    - Parses SMB requests 
        
    - Replies according to SMB protocol standards 
        
    - Can act as: 
        
        - A file server 
            
        - A printer server 
            
        - A domain controller (for authentication) 
            

🔵 In short: 

👉 Samba = Software that speaks SMB on Linux/UNIX 

Analogy: Samba is like an interpreter who learned to speak fluent English to communicate with Americans (Windows machines). 

3. CIFS — Common Internet File System 

Now, CIFS is a specific version (and Microsoft extension) of SMB — specifically, an old extension of SMB1. 

- Microsoft took SMBv1 and extended it in the mid-1990s and renamed it CIFS. 
    
- CIFS adds things like: 
    
    - Larger file support 
        
    - More robust support for symbolic links and file locks 
        
    - Performance optimizations at that time 
        

But CIFS is still considered just "an SMB dialect", specifically SMB1-based, and now is deprecated and considered unsafe. 

In fact, when you mount a Windows share in Linux (using mount -t cifs), you are using a client that speaks "CIFS/SMB protocol." 

Today, when people say CIFS, they often mean SMB1 or old SMB in general. 

🔴 CIFS is very insecure today! 

- No encryption 
    
- Weak authentication 
    
- Easily vulnerable to replay attacks, man-in-the-middle attacks (MITM), etc. 
    

Summary Table: 

|   |   |   |
|---|---|---|
|Term|Nature|Details|
|SMB|Protocol|Defines how network file sharing works (standard)|
|Samba|Software|A Linux/Unix implementation of SMB protocol|
|CIFS|Old SMB1 Dialect|Microsoft's SMB1 extensions, deprecated now|

4. How They Relate Together 

Here’s the full logical relationship: 

Protocol (Standard)      -> SMB (Server Message Block) Specific Old Version     -> CIFS (SMB1 + Microsoft extensions) Software Implementation  -> Samba (Linux/Unix program that speaks SMB) 

Visual Relationship: 

SMB (protocol)   ├── SMB1 (original version)        ├── CIFS (Microsoft enhanced SMB1)   ├── SMB2 (rewritten protocol - faster, fewer commands)   ├── SMB3 (encrypted, resilient connections)  

Samba (software)   ├── Supports SMB1   ├── Supports SMB2   ├── Supports SMB3  

5. Real-World Examples 

Example 1: Windows Machine Shares a Folder 

- Windows acts as an SMB server. 
    
- Windows machine listens on port 445. 
    
- Another Windows client or Linux client (with Samba or cifs-utils) connects via SMB. 
    

Example 2: Linux Machine with Samba 

- smbd daemon (Samba server process) listens on TCP port 445. 
    
- A Windows machine can see shared folders from Linux when browsing "Network." 
    

Example 3: Mounting a Windows Share on Linux using CIFS driver 

- Command: mount -t cifs //192.168.1.10/share /mnt/share -o username=admin,password=pass 
    
    - -t cifs indicates that the Linux kernel should use the CIFS (SMB) driver to talk to the Windows server. 
        
    - This driver supports SMB1, SMB2, SMB3 based on negotiation. 
        

6. Important Notes 

- Modern SMB clients (Windows 10/11, modern Samba versions) prefer SMB2/SMB3 by default. 
    
- Linux Samba server can enforce: 
    
    - Only allow SMB2/3 
        
    - Disable insecure SMB1/CIFS 
        
- SMB1 (and CIFS) are DISABLED by default in Windows 10/11 and Server 2016/2019/2022. 
    

Example: Samba server config to disable SMB1: 

[global]    server min protocol = SMB2    server max protocol = SMB3


List of Things You Can Do Using SMB (Server Message Block): 

  

1. Access Remote Files (Read, write, open, modify, delete files on another computer) 
    
2. Access Remote Directories (List, create, delete, rename folders) 
    
3. File Locking (Lock parts of files for multi-user safety — prevents conflicting writes) 
    
4. Share Printers (Access, install, and send print jobs to remote printers) 
    
5. Interprocess Communication (IPC) (Named pipes, mail slots — send data between applications over the network) 
    
6. Authentication and Authorization (Login using username/password or Kerberos, enforce file permissions) 
    
7. Session Management (Keep persistent sessions with session IDs — no need to reconnect again and again) 
    
8. Directory Service Integration (Access Active Directory objects, policies, shares) 
    
9. File and Directory Metadata Access (Get file attributes: size, timestamps, security descriptors, ownership) 
    
10. Search and Browse Remote Shares (Find shared folders/printers on network without knowing exact names) 
    
11. Remote File Caching (Offline Files — cache server files locally for use when offline) 
    
12. Opportunistic Locking (Oplocks) (Clients can cache file data locally for performance, server notifies if file changes) 
    
13. Change Notifications (Server tells client when a file or folder changes) 
    
14. SMB Signing (Ensure data integrity by digitally signing SMB packets) 
    
15. SMB Encryption (Encrypt SMB traffic over the network — SMB 3.0 and above) 
    
16. Multiple Channel Communication (Use multiple network connections for higher speed — SMB 3.0 "Multichannel") 
    
17. Clustered Shared Volumes (CSV) Support (Access same file system cluster-wide — important for failover and scaling) 
    
18. Transport Named Pipe Calls (Remote procedure call (RPC) communication over named pipes) 
    
19. Map Network Drives (Mount remote SMB shares as local drives, like Z: drive) 
    
20. Directory Junctions and Symbolic Links over Network (Support for symlinks across network shares)

# SMB server--
Samba-SMB-CIFS-Server 

Samba / SMB / CIFS Server 

Common Internet File System (CIFS) is a dialect of the Server Message Block (SMB) protocol, used to provide shared access to files, printers, and serial ports between nodes on a network. 

- SMB (Server Message Block): A protocol used for network file sharing. 
    
- CIFS: A specific implementation/dialect of SMB, often associated with Windows-based systems. 
    
- Samba: An open-source implementation of SMB/CIFS that allows Unix/Linux systems to share files and printers with Windows clients. 
    

🔧 Use Cases 

- File sharing across Linux, macOS, and Windows. 
    
- Printer sharing in mixed-OS environments. 
    
- Integration with Windows Active Directory (AD) for centralized authentication. 
    

📂 Common Paths and Configs 

Main config file: /etc/samba/smb.conf 

Example basic share configuration: 

[shared]     path = /srv/samba/shared     browseable = yes     read only = no     guest ok = yes


Common Commands 

- Install Samba (Debian/Ubuntu): sudo apt install samba 
    
- Check Samba version: smbd --version 
    
- Add a Samba user: sudo smbpasswd -a <_username> 
    
- Restart Samba services: sudo systemctl restart smbd nmbd 
    
- Test Samba config for syntax errors: testparm

Security & Permissions 

- Ensure proper UNIX file permissions on shared directories (chmod, chown). 
    
- Configure valid users, read only, write list options in smb.conf for access control. 
    
- Use encrypt passwords = yes and enable SMBv2 or SMBv3 for enhanced security. 
    

🌐 Mounting SMB Shares (Client-side) 

On Linux: 

sudo mount -t cifs //192.168.1.100/shared /mnt/shared \     -o username=user,password=pass,iocharset=utf8,vers=3.0  

Or configure in /etc/fstab: 

//192.168.1.100/shared /mnt/shared cifs

username=user,password=pass,iocharset=utf8,vers=3.0 0 0

# smb client-


📂 SMB Client 

This section documents commands and utilities for interacting with SMB/CIFS shares on Linux, especially in RHEL/CentOS systems using rpm and yum. 

📦 Package Management 

🔍 Check Installed Samba Packages 

rpm -qa | grep samba 

🔧 Install Samba Client 

yum install samba-client 

📝 Verify Installation 

Verify Installation 

rpm -qa | grep samba-client 

Package info 

rpm -qi samba-client 

Docs provided by the package 

rpm -qd samba-client 

List all installed files 

rpm -ql samba-client 

🌐 Browsing Shares with smbtree 

🔍 Discover Network Shares 

smbtree 

Show only shares 

smbtree -b 

Authenticate as user 'win7' 

smbtree -U win7 

Show a specific workgroup 

smbtree -D workgroup 

List all available workgroups 

smbtree -D workgroups 

Note: on smbtree limitations 

main: This utility doesn't work if NetBIOS name resolution is not configured. If you are using SMB2 or SMB3, network browsing uses WSD/LLMNR, which is not yet supported by Samba. SMB1 is disabled by default on the latest Windows versions for security reasons. 

Explanation: 

- Modern SMB (v2/v3) no longer uses NetBIOS for browsing. 
    
- WSD/LLMNR used by Windows for network discovery isn't supported by Samba. 
    
- SMB1 is disabled on Windows by default due to security risks. 
    
- smbtree is mainly for browsing SMB1 networks but can still be useful for modern SMB shares, depending on network setup. 
    

📂 Accessing Shares with smbclient 

smbclient is an FTP-like command-line utility for interacting with SMB shares. 

Explore Available Shares 

smbclient 

Starts the tool without connecting to a share. 

smbclient --help 

Shows available options and usage. 

smbclient -L 192.168.1.80 

Lists available shares on a remote host anonymously. 

smbclient -L //192.168.1.51 

Uses UNC format to list shares. 

smbclient -L //192.168.1.51 -U win11 

Lists shares using a specific user. 

smbclient -L 192.168.1.51 -U "" -N 

Access with no username or password (guest mode). 

Connect to a Share 

smbclient //192.168.1.80/data 

Connects to the share using anonymous credentials. 

smbclient //192.168.1.80/data -U Armour 

Connect using a specified username. 

Example smbclient Session 

smb: \> get runasroot.sh 

Downloads a single file from the remote share. 

smb: \> mget templated-roadtrip.zip VBoxDarwinAdditions.pkg 

Downloads multiple files. 

smb: \> put mysql80.community-release-el7.3.noarch.rpm 

Uploads a single file. 

smb: \> mput html5up-paradigm-shift.zip latest.zip 

Uploads multiple files to the share. 

🔽 Download Files with smbget 

smbget allows file downloads from SMB shares via command line, similar to wget. 

smbget -U Armour smb://192.168.1.80/data/OS2/VBoxReplaceDll.exe 

Downloads a file using SMB credentials and a full path. 

📦 Backup and Restore with smbtar 

smbtar backs up or restores entire SMB shares using the tar format. 

smbtar --help 

Displays usage help for smbtar. 

smbtar -s 192.168.1.80 -x data -u Armour -p 123 -t data.tar -v 

- -s: Server address 
    
- -x: Share name 
    
- -u / -p: Username and password 
    
- -t: Output tar archive 
    
- -v: Verbose output 
    

🔗 Mounting SMB Shares with mount.cifs 

Linux systems can mount SMB shares into the filesystem using the cifs kernel module. 

mount -t cifs -o username=win10,password=123 //192.168.1.71/data /mnt 

Mounts a share to /mnt using specified credentials. 

mount -t cifs -o username=Armour,password=123 //192.168.1.80/data /mnt/d1 

Mounts another share to a custom directory. 

umount /mnt 

Unmounts the mounted share. 

📝 Notes 

- Install cifs-utils for full CIFS support 
    
- You can persist mounts via /etc/fstab, e.g: 
    

//192.168.1.80/data /mnt cifs username=Armour,password=123 0 0 

- For better security, store credentials in a file: 
    

# /etc/smb-credentials username=Armour password=123 

Then mount like: 

mount -t cifs -o credentials=/etc/smb-credentials //192.168.1.80/data /mnt 

- Consider using autofs for dynamic mounts. 
    
- Avoid SMB1; use SMB2 or SMB3 for better performance and security. 
    
- Use sec=ntlmssp or vers=3.0 if compatibility issues arise: 
    

mount -t cifs -o username=Armour,password=123,vers=3.0 //192.168.1.80/data /mnt



📜 How to Use SMB from Linux Client Side 

  

1. 🔥 First, Understand What You Are Trying To Do 

In SMB client usage, you can do three kinds of things: 

|   |   |
|---|---|
|Task|Meaning|
|Browse SMB shares|See what shared folders or printers are available on a server|
|Mount SMB share|Connect a remote share as if it were a local folder (permanent usage)|
|Access SMB share manually|Without mounting, just access it temporarily|

To perform these, Linux uses special SMB client tools: 

- smbclient → Like FTP client for SMB. 
    
- mount.cifs → Mount SMB share permanently. 
    
- gvfs-smb → For GUI browsing (in GNOME, KDE desktops). 
    

Internally, smbclient and mount.cifs talk SMB protocol over TCP port 445 exactly like Windows does. 

  

2. ⚙️ Install Required Packages (Client Tools) 

Depending on your Linux distro: 

For RHEL / CentOS / Fedora: 

sudo dnf install samba-client cifs-utils  

For Ubuntu / Debian: 

sudo apt install smbclient cifs-utils  

Why? 

- smbclient: To connect, browse, interact with shares manually (like a shell). 
    
- cifs-utils: To mount SMB shares into Linux filesystem. 
    

✅ 

Now client side is ready. 

  

3. 🧠 Behind the Scenes Protocol Stack 

When you use smbclient or mount.cifs: 

- Linux opens TCP connection to server port 445. 
    
- Then negotiates SMB 1.0/2.0/2.1/3.0. 
    
- Then authenticates (user/pass or guest). 
    
- Then uses Tree Connect to a share. 
    
- Then accesses files/printers. 
    

Exactly the same TCP/IP + SMB packet flow I taught earlier. 

  

4. 🏁 Practical Usage 

  

📂 (A) Accessing SMB Share with smbclient (Shell-like) 

Syntax: 

smbclient //<server-ip>/<share-name> -U <username>  

Example: 

smbclient //192.168.1.10/data_share -U admin  

Then it asks for password. 

After login, you get an interactive prompt like: 

smb: \>  

Inside this prompt, you can run commands: 

- ls → list files 
    
- cd → change directory 
    
- get <filename> → download file 
    
- put <filename> → upload file 
    
- exit → quit 
    

Behind the scenes: 

- Every command (ls, get, put) becomes an SMB request packet (like QUERY_DIRECTORY, READ, WRITE). 
    

  

If the server supports guest access: 

smbclient //<server-ip>/<share-name> -U guest -N  

(-N = No password prompt) 

  

If you want to list available shares on a server: 

smbclient -L //<server-ip> -U <username>  

Example: 

smbclient -L //192.168.1.10 -U admin  

This shows output like: 

Sharename       Type      Comment ---------       ----      ------- data_share      Disk      Project files print$          Disk      Printer drivers IPC$            IPC       IPC Service (server IPC)  

✅ 

Each of these shares can be connected individually. 

  

🏔️ (B) Mount SMB Share Permanently with mount.cifs 

Suppose you want /data_share from 192.168.1.10 to appear as a local directory /mnt/data_share. 

Step 1: Create a mount point 

sudo mkdir -p /mnt/data_share  

Step 2: Mount manually: 

sudo mount -t cifs //<server-ip>/<share-name> /mnt/data_share -o username=<username>,password=<password>  

Example: 

sudo mount -t cifs //192.168.1.10/data_share /mnt/data_share -o username=admin,password=123456  

✅ 

Now if you cd /mnt/data_share, you will directly see remote files like local ones! 

  

If you want to automount on boot, you can add an entry into /etc/fstab: 

Example /etc/fstab line: 

 //192.168.1.10/data_share /mnt/data_share cifs username=admin,password=123456,iocharset=utf8,vers=3.0 0 0  

  

Important mount options: 

- vers=3.0 → Force SMB protocol version. 
    
- iocharset=utf8 → Handle filenames in UTF-8. 
    
- rw / ro → Read-write or Read-only mount. 
    

  

🌟 (C) Using GUI to Browse SMB Shares 

If your Linux has a graphical desktop (GNOME, KDE), 

you can directly open: 

smb://<server-ip>/<share-name>  

in Files application (Nautilus, Dolphin etc). 

It uses gvfs-smb module internally. 

✅ 

  

🧠 Some Real Behind-the-Scenes Facts 

|   |   |
|---|---|
|Concept|Meaning|
|CIFS vs SMB|In Linux world, CIFS driver was old, SMB2/3 is now used. Still called cifs-utils|
|NTLM / Kerberos|Used for authentication over SMB|
|Encryption|SMB3 can encrypt traffic between client and server|
|Signing|SMB2/3 can sign packets for integrity check|
|Sessions|Linux establishes a full SMB session (with Session ID, Tree IDs)|

  

📚 Full Concept Summary: 

smbclient = like FTP client → temporary browsing 

mount.cifs = mount remote share as a local folder → permanent access 

gvfs-smb = GUI access 

Internally all use: 

TCP 445 ➔ SMB negotiation ➔ session setup ➔ tree connect ➔ access