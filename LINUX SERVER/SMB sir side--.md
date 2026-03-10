# 📂 SMB Client

  

This section documents commands and utilities for interacting with SMB/CIFS shares on Linux, especially in RHEL/CentOS systems using rpm and yum.

  

## 📦 Package Management

### 🔍 Check Installed Samba Packages

  

    rpm -qa | grep samba

  

### 🔧 Install Samba Client

  

    yum install samba-client

  

### 📝 Verify Installation

  

Verify Installation

  

    rpm -qa | grep samba-client

  

Package info

  

    rpm -qi samba-client

  

Docs provided by the package

  

    rpm -qd samba-client

  

List all installed files

  

    rpm -ql samba-client

  

## 🌐 Browsing Shares with **smbtree**

  

### 🔍 Discover Network Shares

  

```

smbtree

```

  

Show only shares

  

```

smbtree -b

```

  

Authenticate as user 'win7'

  

```

smbtree -U win7

```

  

Show a specific workgroup

  

```

smbtree -D workgroup

```

  

List all available workgroups

  

```

smbtree -D workgroups

```

  

**Note**:  on smbtree limitations

```

main: This utility doesn't work if NetBIOS name resolution is not configured.

If you are using SMB2 or SMB3, network browsing uses WSD/LLMNR, which is not yet supported by Samba.

SMB1 is disabled by default on the latest Windows versions for security reasons.

```

Explanation:

* Modern SMB (v2/v3) no longer uses NetBIOS for browsing.

* WSD/LLMNR used by Windows for network discovery isn't supported by Samba.

* SMB1 is disabled on Windows by default due to security risks.

-  smbtree is mainly for browsing SMB1 networks but can still be useful for modern SMB shares, depending on network setup.

  

## 📂 Accessing Shares with smbclient

  

smbclient is an FTP-like command-line utility for interacting with SMB shares.

  

### Explore Available Shares

  

```

smbclient

```

Starts the tool without connecting to a share.

  

```

smbclient --help

```

Shows available options and usage.

  

```

smbclient -L 192.168.1.80

```

Lists available shares on a remote host anonymously.

  

```

smbclient -L //192.168.1.51

```

Uses UNC format to list shares.

  

```

smbclient -L //192.168.1.51 -U win11

```

Lists shares using a specific user.

  

```

smbclient -L 192.168.1.51 -U "" -N

```

Access with no username or password (guest mode).

  

### Connect to a Share

  

```

smbclient //192.168.1.80/data

```

Connects to the share using anonymous credentials.

  

```

smbclient //192.168.1.80/data -U Armour

```

Connect using a specified username.

  

### Example smbclient Session

  

```

smb: \> get runasroot.sh

```

Downloads a single file from the remote share.

  

```

smb: \> mget templated-roadtrip.zip VBoxDarwinAdditions.pkg

```

Downloads multiple files.

  

```

smb: \> put mysql80.community-release-el7.3.noarch.rpm

```

Uploads a single file.

  

```

smb: \> mput html5up-paradigm-shift.zip latest.zip

```

Uploads multiple files to the share.

  

## 🔽 Download Files with smbget

  

smbget allows file downloads from SMB shares via command line, similar to wget.

  

```

smbget -U Armour smb://192.168.1.80/data/OS2/VBoxReplaceDll.exe

```

Downloads a file using SMB credentials and a full path.

## 📦 Backup and Restore with smbtar

  

smbtar backs up or restores entire SMB shares using the tar format.

  

```

smbtar --help

```

Displays usage help for smbtar.

  

```

smbtar -s 192.168.1.80 -x data -u Armour -p 123 -t data.tar -v

```

  

* -s: Server address

* -x: Share name

* -u / -p: Username and password

* -t: Output tar archive

* -v: Verbose output

## 🔗 Mounting SMB Shares with mount.cifs

  

Linux systems can mount SMB shares into the filesystem using the cifs kernel module.

  

```

mount -t cifs -o username=win10,password=123 //192.168.1.71/data /mnt

```

Mounts a share to /mnt using specified credentials.

  

```

mount -t cifs -o username=Armour,password=123 //192.168.1.80/data /mnt/d1

```

  

In Debian mount by :

```

mount -t cifs //192.168.1.28/smb1 /mnt/smb/ -o username=smb1,password=123,vers=3.0

```

  

Mounts another share to a custom directory.

  

```

umount /mnt

```

Unmounts the mounted share.

## 📝 Notes

  

* Install cifs-utils for full CIFS support

  

* You can persist mounts via /etc/fstab, e.g:

```

//192.168.1.80/data /mnt cifs username=Armour,password=123 0 0

```

  

* For better security, store credentials in a file:

```

# /etc/smb-credentials

username=Armour

password=123

```

  

Then mount like:

  

```

mount -t cifs -o credentials=/etc/smb-credentials //192.168.1.80/data /mnt

```

  

* Consider using autofs for dynamic mounts.

* Avoid SMB1; use SMB2 or SMB3 for better performance and security.

* Use sec=ntlmssp or vers=3.0 if compatibility issues arise:

  

```

mount -t cifs -o username=Armour,password=123,vers=3.0 //192.168.1.80/data /mnt

```

  

---

# Samba Server Setup

  

This guide outlines how to install, configure, manage, and test a Samba server on RHEL or CentOS using yum and related tools. Samba allows Linux systems to share files and printers with Windows clients over the SMB/CIFS protocol.

  

## Installation

  

### Install Samba Packages

  

Install the base Samba package:

```

yum install samba

```

  

Install all related Samba packages (clients, tools, etc.):

```

yum install samba*

```

  

## Verifying Samba Installation

  

### List Installed Samba Packages

```

rpm -qa | grep samba

```

  

### Get Detailed Info On Samba Package

```

rpm -qi samba

```

  

### List Samba Configuration Files

```

rpm -qc samba

```

### List Samba Documentation Files

```

rpm -qd samba

```

  

### List All Files Installed By Samba Package

```

rpm -ql samba

```

  

---

  

## Configuration

  

### Edit Samba Configuration File

  

Open the main Samba configuration file using a text editor:

```

vim /etc/samba/smb.conf

```

  

Refer to `/etc/samba/smb.conf.example` or the `man smb.conf` for detailed syntax.

  

### Example Minimal Configuration

  

```

[global]

    workgroup = SAMBA

    security = user

    passdb backend = tdbsam

    printing = cups

    printcap name = cups

    load printers = yes

    cups options = raw

  

[homes]

    comment = Home Directories

    valid users = %S, %D%w%S

    browseable = No

    read only = No

    inherit acls = Yes

  

[printers]

    comment = All Printers

    path = /var/tmp

    printable = Yes

    create mask = 0600

    browseable = No

  

[print$]

    comment = Printer Drivers

    path = /var/lib/samba/drivers

    write list = @printadmin root

    force group = @printadmin

    create mask = 0664

    directory mask = 0775

```

  

### Verify the config is correct after

  

```

testparm

```

## Managing Samba Users

  

### Create A Linux User

```

useradd smb-user1

```

  

### View Samba Password Command Help

```

smbpasswd -h

```

  

Or

```

smbpasswd --help

```

  

### Add Users To Samba

  

Each user must first exist as a Linux user:

```

smbpasswd -a armour

```

  

```

smbpasswd -a infosec

```

  

```

smbpasswd -a thw

```

  

## Managing Samba Services

  

### Check Samba Service Status

```

systemctl status smb.service

```

  

### Start Samba Service

```

systemctl start smb.service

```

  

### Enable Samba Service On Boot

```

systemctl enable smb.service

```

  

## Firewall Configuration With Firewalld

  

Samba uses ports 137/udp, 138/udp, 139/tcp, and 445/tcp.

  

### Start And Enable Firewalld

```

systemctl start firewalld

```

  

```

systemctl enable firewalld

```

  

### Check Firewall State

```

firewall-cmd --state

```

  

### Add Samba Services To The Firewall (Permanent)

```

firewall-cmd --permanent --add-service=samba

```

  

You can also add individual ports if preferred:

```

firewall-cmd --permanent --add-port=137/udp

firewall-cmd --permanent --add-port=138/udp

firewall-cmd --permanent --add-port=139/tcp

firewall-cmd --permanent --add-port=445/tcp

```

  

### Reload Firewall To Apply Changes

```

firewall-cmd --reload

```

  

### Verify Active Rules

```

firewall-cmd --list-all

```

  

## Testing Samba Shares From A Client

  

### List Available Shares (Anonymous)

```

smbclient -L 192.168.1.37

```

### List Shares With User Authentication

```

smbclient -L 192.168.1.37 -U armour

```

  

```

smbclient -L 192.168.1.37 -U infosec

```

### Connect To A Share

```

smbclient //192.168.1.37/infosec -U infosec

```

---

## 📂 SMB Client Shell Commands

  

### 📁 List Files And Directories

```

ls

```

  

### 📁 Change Directory

```

cd <foldername>

```

  

Example:

```

cd reports

```

  

### 📥 Download A File From The Share

```

get <remote_filename>

```

  

Example:

```

get confidential.txt

```

  

### 📤 Upload A File To The Share

```

put <local_filename>

```

  

Example:

```

put notes.txt

```

  

### 📥 Download Multiple Files

```

mget *.txt

```

  

You will be prompted for each file. Use `prompt` to toggle confirmation:

```

prompt

```

  

### 📤 Upload Multiple Files

```

mput *.pdf

```

  

### ⬅️ Go Back To Previous Directory

```

cd ..

```

  

### 🏠 Go To Root Of Share

```

cd /

```

  

### 📁 Create A New Directory

```

mkdir <dirname>

```

  

Example:

```

mkdir backups

```

  

### ❌ Remove A File

```

del <filename>

```

  

### 🗑️ Remove A Directory

```

rmdir <dirname>

```

  

### 🚪 Exit The SMB Shell

```

exit

```

---

## Samba Password And TDB Database Management

  

### Navigate To Samba Private Database Directory

```

cd /var/lib/samba/private

```

  

Files present:

```

passdb.tdb  secrets.ldb  secrets.tdb

```

### List all Samba users registered in the **Samba passdb** database (`tdbsam` or others).

  

```

pdbedit -L

```

### List Samba User Info Verbosely

```

pdbedit -L -v

```

  

### List Samba Users In smbpasswd Format

```

pdbedit -L -w

```

  

## TDB Tools For Database Management

  

### Install TDB Tools

```

yum install tdb-tools

```

  

### Verify TDB Tools Installation

```

rpm -qa | grep tdb-tools

```

  

```

rpm -qi tdb-tools

```

  

```

rpm -ql tdb-tools

```

## 📘 Understanding Samba's Database Files

  

### passdb.tdb

* **Purpose**: Stores Samba user account information (passwords, flags, etc.).

* **Backend**: Used when `passdb backend = tdbsam` is set in `smb.conf`.

* **Location**: Usually found in `/var/lib/samba/private/` or `/var/lib/samba/`.

  

### View Contents (Low-Level)

```

yum install tdb-tools

```

  

```

apt install tdb-tools

```

  

```

tdbdump /var/lib/samba/private/passdb.tdb

```

  

### secrets.tdb

* **Purpose**: Contains internal secrets used by Samba:

  * Domain membership keys

  * Trust passwords

  * LDAP bind credentials

* **Highly sensitive**: Should be readable by root only.

* **Location**: Typically in `/var/lib/samba/private/`

  

### View Contents

```

tdbdump /var/lib/samba/private/secrets.tdb

```

  

### secrets.ldb

* **Purpose**: Used in Samba AD DC environments.

* **Part of**: Samba's LDB (LDAP-like) database when acting as an Active Directory domain controller.

* **Stores**: Domain controller secrets, machine credentials, replication metadata, etc.

* **Location**: Usually in `/var/lib/samba/private/`

  

> If you're using Samba as an AD DC (with `samba-tool domain provision`), `secrets.ldb` replaces or supplements `secrets.tdb`.

  

### View Contents (Advanced Tool Required)

```

ldbsearch -H /var/lib/samba/private/secrets.ldb

```

  

### 🔒 Best Practices

* Do not edit these files manually.

* Always backup before upgrading or migrating Samba.

* Restrict permissions:

```

chmod 600 /var/lib/samba/private/*

chown root:root /var/lib/samba/private/*

```

## TDB Tools For Database Management

  

### Install TDB Tools

```

yum install tdb-tools

```

  

### Verify TDB Tools Installation

```

rpm -qa | grep tdb-tools

```

  

```

rpm -qi tdb-tools

```

  

```

rpm -ql tdb-tools

```

  

### Dump Samba TDB Databases

```

tdbdump /var/lib/samba/private/passdb.tdb

```

  

```

tdbdump /var/lib/samba/private/secrets.tdb

```

  

Or from current working directory:

  

```

tdbdump passdb.tdb

```

  

```

tdbdump secrets.tdb

```

---

# 📚 Full Explanation of Default `smb.conf` File

## 🏛️ Overall Structure of `smb.conf`

  

In Samba, the configuration file `/etc/samba/smb.conf` is structured as:

  

1. A **[global]** section — which configures **global server settings**.

2. Several **[share]** sections — each defining **one share** (file, printer, etc.).

  

👉 Syntax:

  

- Square brackets `[section-name]` define sections.

- Inside a section, settings are written as `parameter = value`.

- Parameters are **case-insensitive** (`workgroup` = `Workgroup` = `WORKGROUP`).

- Comments start with `#` or `;`.

  

---

  

# 🔵 [global] Section (Server-Wide Settings)

  

```ini

[global]

    workgroup = SAMBA

    security = user

    passdb backend = tdbsam

    printing = cups

    printcap name = cups

    load printers = yes

    cups options = raw

```

  

|Parameter|Meaning|

|:--|:--|

|`workgroup = SAMBA`|Name of the **workgroup/domain** the server belongs to. Clients must match this to find the server easily. Default is "WORKGROUP" on Windows.|

|`security = user`|Defines the **authentication mode**. `"user"` means each client must authenticate individually (username + password).|

|`passdb backend = tdbsam`|Backend database storing usernames/passwords. `tdbsam` is a local file-based database (`/var/lib/samba/private/passdb.tdb`).|

|`printing = cups`|Defines the print system type. `cups` = Common UNIX Printing System is used for printing support.|

|`printcap name = cups`|Instructs Samba to get list of printers from CUPS.|

|`load printers = yes`|Auto-load printers from the system into Samba so they can be shared automatically.|

|`cups options = raw`|Send raw (unfiltered) print jobs to CUPS.|

  

---

  

# 🟡 [homes] Section (Per-User Home Directories)

  

```ini

[homes]

    comment = Home Directories

    valid users = %S, %D%w%S

    browseable = No

    read only = No

    inherit acls = Yes

```

  

This is a **special share** that **dynamically maps each user’s home directory**. When a user logs in, they see their own directory.

  

|Parameter|Meaning|

|:--|:--|

|`comment = Home Directories`|Comment visible in network browser (Windows Explorer).|

|`valid users = %S, %D%w%S`|Only allow the **current user** (`%S`) or **domain\user** (`%D%w%S`) to access their own share.|

|`browseable = No`|This share will **not appear** in the list of shares; users must manually connect.|

|`read only = No`|Share is **writable**. Users can read/write files.|

|`inherit acls = Yes`|Inherit file permissions (ACLs) from the parent directory (good for UNIX permission compatibility).|

  

---

  

# 🟢 [printers] Section (Printer Sharing)

  

```ini

[printers]

    comment = All Printers

    path = /var/tmp

    printable = Yes

    create mask = 0600

    browseable = No

```

  

This defines a **special printer share** to automatically offer **all system printers** to clients.

  

|Parameter|Meaning|

|:--|:--|

|`comment = All Printers`|Description for users.|

|`path = /var/tmp`|Temporary location where Samba spools print jobs before sending to printer.|

|`printable = Yes`|Declares that this is a **printer share**, not a file share.|

|`create mask = 0600`|Set file permissions for new print job files — only owner can read/write.|

|`browseable = No`|Hide the printers from the browse list.|

  

---

  

# 🟣 [print$] Section (Printer Drivers)

  

```ini

[print$]

    comment = Printer Drivers

    path = /var/lib/samba/drivers

    write list = @printadmin root

    force group = @printadmin

    create mask = 0664

    directory mask = 0775

```

  

This share is used by **Windows clients to download printer drivers** automatically when connecting to Samba printers.

  

|Parameter|Meaning|

|:--|:--|

|`comment = Printer Drivers`|Description.|

|`path = /var/lib/samba/drivers`|Directory where printer drivers are stored.|

|`write list = @printadmin root`|Only users in group `printadmin` or root can upload new drivers.|

|`force group = @printadmin`|Files created inside will have group ownership `printadmin`.|

|`create mask = 0664`|New files permissions: rw-rw-r--|

|`directory mask = 0775`|New directories permissions: rwxrwxr-x|

  

---

  

# 🧠 Available Options Per Section

  

Each section (`[global]`, `[share-name]`) can have specific **available parameters**.

  

|Section Type|Options You Can Use|

|:--|:--|

|**[global]**|server role, security, workgroup, interfaces, hosts allow, log level, smb ports, encrypt passwords, max protocol, min protocol, dns proxy, etc.|

|**File Share**|path, valid users, read only, browseable, create mask, directory mask, guest ok, force user, force group, etc.|

|**Printer Share**|printable, printer name, print command, lpq command, lprm command, etc.|

  

👉 **Total:** Samba has **hundreds** of available options. You can see them with:

  

```bash

man smb.conf

```

  

or online official Samba documentation.

  

---

  

# 🚨 Important Things to Remember

  

- `[global]` affects the **entire server**.

- Each `[share]` defines one **accessible resource** (folder, printer, driver, etc.).

- Special placeholders like `%S`, `%U`, `%D` are **dynamic substitutions**.

- Default file and directory permissions (`create mask`, `directory mask`) are important to control file access.

- **Guest access** can be allowed with `guest ok = yes` if needed.

- **User authentication** is tied to the backend selected (`tdbsam`, `ldapsam`, etc.).

  

---

  

# 📂 To Create Your Own Share in `smb.conf`

  

You need to **add this block** at the bottom of `/etc/samba/smb.conf`:

  

```ini

[sharename]

    comment = Any description you want

    path = /full/path/to/folder

    browseable = yes

    read only = no

    guest ok = no

    valid users = username

    create mask = 0664

    directory mask = 0775

```

  

---

  

# 🧠 Explanation of Each Line

  

|Option|Purpose|

|:--|:--|

|`[sharename]`|The name that will appear to clients.|

|`comment`|Description shown in network browsers.|

|`path`|The real directory to share. Must exist and be accessible.|

|`browseable = yes`|Whether the share appears when browsing (like Windows Network Neighborhood).|

|`read only = no`|Allow clients to write files (upload, edit).|

|`guest ok = no`|Allow anonymous access (no password)? Usually set to `no` for secure shares.|

|`valid users = username`|Who can access the share. Use real Samba users.|

|`create mask = 0664`|Permissions for new files (rw-rw-r--).|

|`directory mask = 0775`|Permissions for new folders (rwxrwxr-x).|

  

---

  

# 🔥 Minimal Example

  

Suppose you want to share `/data/myshare`, allowing only user `dev`.

  

You would add:

  

```ini

[myshare]

    comment = My Personal Share

    path = /data/myshare

    browseable = yes

    read only = no

    guest ok = no

    valid users = dev

    create mask = 0664

    directory mask = 0775

```

  

---

  

# ⚡ Important after Editing `smb.conf`

  

After adding your block:

  

1. Check config is valid:

  

```bash

testparm

```

  

2. Reload Samba service:

  

```bash

systemctl restart smb

```

  

or

  

```bash

systemctl restart smb.service

```

  

---
# Anonymous Samba Share

  

Anonymous or guest shares allow users to access shared directories without authentication. This setup is useful for public access within a trusted network.

  

## 🔧 Prepare Public Directory

  

Create a public directory with full access permissions:

```

mkdir /Public

```

  

Set open permissions for read, write, and execute access:

```

chmod 777 /Public/

```

  

Or recursively apply to all contents inside:

```

chmod -R 777 /Public/

```

  

Verify directory exists and has correct permissions:

```

ls -lh / | grep Public

```

  

Set ownership to nobody user and group:

```

chown -R nobody:nobody /Public/

```

## ⚙️ Configure Samba For Anonymous Access

  

Edit the Samba configuration file:

```

vim /etc/samba/smb.conf

```

  

Append or modify the following share definition:

```

[Public]

    path = /Public

    writable = yes

    guest ok = yes

    guest only = yes

    read only = no

    create mode = 0777

    directory mode = 0777

    force user = nobody

```

  

This configuration:

* Enables guest-only access

* Allows full read/write/delete capabilities

* Forces all operations as nobody user

  

## 🔄 Restart Samba Service

  

Apply the configuration by restarting the Samba service:

```

systemctl restart smb.service

```

  

## 🌐 Test Anonymous Share Access

  

Check the list of available shares on the Samba server:

```

smbclient -L 192.168.1.37

```

  

Try accessing the public share anonymously:

```

smbclient //192.168.1.25/Public/

```

  

Explicitly use guest access with no credentials:

```

smbclient -L 192.168.1.37 -U "" -N

```

  

Or connect directly to the public share as a guest:

```

smbclient //192.168.1.37/Public -U "" -N

```

  

Another equivalent form:

```

smbclient -U "" //192.168.1.27/Public -N

```

## 🧩 Additional Sample Configuration

  

Here's an extended smb.conf snippet with additional shares:

  

```

[data]

    comment = Data

    path = /opt/data

    writable = yes

  

[backup]

    comment = Server Backup

    path = /backup

    writable = yes

    valid users = smb1, armour

  

[Public]

    path = /Public

    writable = yes

    guest ok = yes

    guest only = yes

    read only = no

    create mode = 0777

    directory mode = 0777

    force user = nobody

```

  

> Make sure the directories `/opt/data` and `/backup` exist with proper ownership and permissions for listed users.

  

## 🔒 Security Note

  

Anonymous shares should only be used in trusted networks. Avoid enabling guest access on internet-facing systems or in environments where data sensitivity is a concern.


# Shared Common Directories With Samba

  
  

This setup demonstrates how to share directories such as `/opt/data` and `/backup` over the network using Samba, with appropriate permissions and configurations.

  

## 📁 Create And Prepare Shared Directories

  

Create the data directory under `/opt`:

```

cd /opt/

mkdir data

```

  

Set wide-open permissions (read/write/execute for all users):

```

chmod 777 /opt/data/

```

  

Create the backup directory:

```

mkdir /backup/

```

  

Set sticky bit and full permissions to preserve file ownerships while allowing write access:

```

chmod -R 1777 /backup/

```

  

> The 1777 permission allows anyone to write to the directory but only the owner can delete their files — commonly used for directories like `/tmp`.

  

## ⚙️ Configure Samba Shared Folders

  

Edit the Samba configuration file:

```

vim /etc/samba/smb.conf

```

  

Update or append the following sections in the configuration:

  

```

[global]

    workgroup = SAMBA

    security = user

    passdb backend = tdbsam

    printing = cups

    printcap name = cups

    load printers = yes

    cups options = raw

  

[homes]

    comment = Home Directories

    valid users = %S, %D%w%S

    browseable = No

    read only = No

    inherit acls = Yes

  

[printers]

    comment = All Printers

    path = /var/tmp

    printable = Yes

    create mask = 0600

    browseable = No

  

[print$]

    comment = Printer Drivers

    path = /var/lib/samba/drivers

    write list = @printadmin root

    force group = @printadmin

    create mask = 0664

    directory mask = 0775

  

[data]

    comment = Data

    path = /opt/data

    public = yes

    writable = yes

    guest ok = no

    guest only = no

  

[backup]

    comment = Server Backup

    path = /backup

    public = yes

    writable = yes

```

  

## 🔄 Restart Samba Service

  

Apply the configuration changes:

```

systemctl restart smb.service

```

  

## 🌐 Accessing The Shares

  

You can test the shares using smbclient from another Linux machine:

```

smbclient -L //192.168.1.37 -U your_samba_user

```

  

Or mount the share using:

```

sudo mount -t cifs //192.168.1.37/data /mnt/data -o username=your_samba_user,password=your_password,vers=3.0

```

  

For the backup share:

```

sudo mount -t cifs //192.168.1.37/backup /mnt/backup -o username=your_samba_user,password=your_password,vers=3.0

```

  

## 🔒 Access Control Note

  

Although `public = yes` is set, `guest ok = no` ensures only authenticated users can access these shares. You can manage permissions more tightly by adding `valid users` or `write list` directives as needed.

  

Let me know if you'd like to add user-specific access, Active Directory integration, or auto-mount via `/etc/fstab`.

# Share With Selected Users

  

This setup enables controlled access to the Samba share (`/backup`) by specifying selected users.

  

## ⚙️ Configure Samba Share For Selected Users

  

Edit the Samba configuration:

```

vim /etc/samba/smb.conf

```

  

Add or modify the following configuration:

  

```

[global]

    workgroup = SAMBA

    security = user

    passdb backend = tdbsam

    printing = cups

    printcap name = cups

    load printers = yes

    cups options = raw

  

[homes]

    comment = Home Directories

    valid users = %S, %D%w%S

    browseable = No

    read only = No

    inherit acls = Yes

  

[printers]

    comment = All Printers

    path = /var/tmp

    printable = Yes

    create mask = 0600

    browseable = No

  

[print$]

    comment = Printer Drivers

    path = /var/lib/samba/drivers

    write list = @printadmin root

    force group = @printadmin

    create mask = 0664

    directory mask = 0775

  

[backup]

    comment = Server Backup

    path = /backup

    public = yes

    writable = yes

    valid users = infosec, armour

```

  

> The `valid users` directive ensures only `infosec` and `armour` can access the backup share.

  

## 🔄 Restart Samba Service

  

Apply the configuration changes:

```

systemctl restart smb.service

```

## 👤 Access The Restricted Share

  

Access as thw (should be denied if not listed in valid users):

```

smbclient //192.168.1.37/backup -U thw

```

  

Access as allowed users:

```

smbclient -L 192.168.1.37 -U infosec

```

  

```

smbclient -L 192.168.1.37 -U armour

```

  

## 🔑 Additional Notes

  

* Make sure the users (infosec, armour) exist as both Linux system users and Samba users.

* Add Samba users with:

```

smbpasswd -a infosec

smbpasswd -a armour

```

  

* Ensure the shared directory /backup has proper ownership/permissions:

```

mkdir -p /backup

chmod -R 770 /backup

chown root:users /backup

```

  

You can also create a custom group (e.g., sambashare) and add infosec and armour to it for easier permission management.