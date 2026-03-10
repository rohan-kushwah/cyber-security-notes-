login user hi wrote ka pai

autnenticate user ke liye hi unki home directory accessible ho
ab /etc/vsftpd/vsftpd.conf me anonyomous ko no kareneg

# Access User Home Directory on FTP Server using vsftpd

  

This guide walks through the process of setting up a basic FTP server using `vsftpd` on a Linux system. The goal is to allow local users (`u1`, `u2`, etc.) to log in and access their own home directories securely.

  

### 1. Create Local Users

  

Create two local user accounts (`u1` and `u2`). These users will be used to test FTP access.

  

```

useradd u1

```

  

```

useradd u2

```

  

Set passwords for the new users:

  

```

passwd u1

```

  

```

passwd u2

```

  

> It's important that each user has a valid password to authenticate via FTP.

  

### 2. Configure vsftpd

  

Edit the `vsftpd` configuration file:

  

```

vim /etc/vsftpd/vsftpd.conf

```

  

Example content for `/etc/vsftpd/vsftpd.conf`:

  

```

# Disallow anonymous logins

anonymous_enable=NO

  

# Enable local user login

local_enable=YES

  

# Allow FTP write commands (upload, delete, etc.)

write_enable=YES

  

# Set default permissions for uploaded files

local_umask=022

  

# Display directory-specific messages if any

dirmessage_enable=YES

  

# Enable logging of uploads/downloads

xferlog_enable=YES

  

# Ensure data connections use port 20

connect_from_port_20=YES

  

# Use standard FTP xferlog format

xferlog_std_format=YES

  

# Chroot local users into their home directories

chroot_local_user=YES

  

# Allow writeable chroot (otherwise vsftpd may reject writeable home dirs)

allow_writeable_chroot=YES

  

# Enable passive mode with defined port range

pasv_enable=YES

pasv_min_port=55000

pasv_max_port=55999

  

# PAM authentication service

pam_service_name=vsftpd

  

# Enable user list

userlist_enable=YES

  

# Enable IPv6 listener (disable `listen=YES` if this is enabled)

listen=NO

listen_ipv6=YES

```

  

> Save and exit the file after editing.

  

---

- Note:

  By default, when you enable:

            `chroot_local_user=YES`

  

    - vsftpd **jails local users to their home directories**, **but also requires** that the home directory is:

  

    - **NOT writable by the user**

    - (i.e., owned by `root`, with `chmod 755`)

    - Why?  This is to **prevent potential security bypasses** from users inside the chroot jail.

-  ❗Problem: If the user **needs to write into their home directory**, the default behavior **fails with**:

    `500 OOPS: vsftpd: refusing to run with writable root inside chroot()`

- ### Solution: `allow_writeable_chroot=YES`

    Adding this line in `/etc/vsftpd.conf`:

        `allow_writeable_chroot=YES`

- ...will **allow the user's home directory to be both writable and the root of the chroot jail.**

---

### 3. Check or Configure SELinux (If Applicable)

  

View the current SELinux mode:

  

```

cat /etc/sysconfig/selinux

```

  

Example output:

  

```

# SELinux status

SELINUX=disabled

SELINUXTYPE=targeted

```

  

> If SELinux is enforcing, you may need to enable the `ftp_home_dir` boolean:

>

> ```

> setsebool -P ftp_home_dir 1

> ```

  

### 4. Restart vsftpd Service

  

Apply configuration changes by restarting the `vsftpd` service:

  

```

systemctl restart vsftpd.service

```

  

Check if the service is active:

  

```

systemctl status vsftpd.service

```

  

Enable it at boot:

  

```

systemctl enable vsftpd.service

```

### 5. Setup Proper Home Directory Permissions

  

Ensure that user directories exist and have correct permissions:

  

```

mkdir -p /home/u1 /home/u2

```

  

```

chown u1:u1 /home/u1

```

  

```

chown u2:u2 /home/u2

```

  

```

chmod 755 /home/u1 /home/u2

```

  

> You can use `chmod 700` if you want their home directories to be private.

  

### 6. Configure the Firewall (Optional)

  

If you're running a firewall (like `firewalld`), allow the passive port range:

  

```

firewall-cmd --permanent --add-port=55000-55999/tcp

```

  

```

firewall-cmd --reload

```

  

> This ensures that FTP passive mode will work properly, especially behind NAT.

### 7. Test FTP Login

  

Test the FTP server using a client like the command-line `ftp` tool:

  

```

ftp 192.168.1.50

```

  

Example login:

  

```

Name (192.168.1.50:u1): u1

Password: 123

```

  

If the connection is successful, you will be placed in `/home/u1` and have access to read/write files (based on permissions).

  

### 8. Troubleshooting Tips

  

#### FTP Login Fails

  

* Check authentication logs:

  ```

  journalctl -xe | grep vsftpd

  ```

  

* Ensure the user is not listed in `/etc/vsftpd/ftpusers` (users in this file are denied FTP access).

  

#### Directory Listing Fails

  

* Passive ports might be blocked by a firewall.

* Ensure proper permissions on home directories.

* Ensure SELinux (if enabled) is not preventing access.

## 9. Optional: Add Banner Message

  

You can add a custom banner to display to users on login:

  

```

ftpd_banner=Welcome to the Armour FTP server.

```

  

Add this line to your `/etc/vsftpd/vsftpd.conf`.

  

## 10. Useful Resources

  

* [vsftpd man page](https://linux.die.net/man/5/vsftpd.conf)

* Logs: `/var/log/vsftpd.log`, `/var/log/xferlog`, `/var/log/secure`

  

Let me know if you'd like to add TLS encryption or isolate users further using virtual users or jail environments.

# TLS-Encryption-on-FTP

## 🔐 TLS Certificate & Key

  

### 1. Create the required directories (if missing)

```

mkdir -p /etc/ssl/private

mkdir -p /etc/ssl/certs

```

  

### 2. Generate the self-signed TLS certificate and key

```
openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout /etc/ssl/private/vsftpd.key \
  -out /etc/ssl/certs/vsftpd.pem


```

  

You'll be prompted for details like Country, Org, etc. You can leave them blank or fill them in.

  

### 3. Set the correct file permissions

```

chmod 600 /etc/ssl/private/vsftpd.key

chmod 644 /etc/ssl/certs/vsftpd.pem

```

  

Once that's done, make sure the following lines are in your `/etc/vsftpd/vsftpd.conf`:

  

```

ssl_enable=YES

rsa_cert_file=/etc/ssl/certs/vsftpd.pem

rsa_private_key_file=/etc/ssl/private/vsftpd.key

```

  

Then restart the FTP service:

  

```

sudo systemctl restart vsftpd

```

  

---

### 4. Understanding SSL/TLS in FTP

  

FTP normally transmits data in plaintext, making it vulnerable to eavesdropping. FTPS (FTP Secure) adds TLS/SSL encryption to protect:

- Authentication credentials (username/password)

- Commands sent between client and server

- Data transferred (files, listings)

  

There are two types of FTPS:

- **Explicit FTPS** (default, port 21): Client connects normally, then upgrades to secure connection with STARTTLS

- **Implicit FTPS** (port 990): Connection is encrypted from the start

  

For maximum security, you can configure your server to require encryption:

```

force_local_logins_ssl=YES  # Require encrypted authentication

force_local_data_ssl=YES    # Require encrypted data transfers

```

  

### 5. Client-Side Connection with ftp-ssl

  

#### Installing ftp-ssl

  

On Debian/Ubuntu:

```

apt-get update

apt-get install ftp-ssl

```

  

On RHEL/CentOS:

```

yum install epel-release

yum install ftp-ssl

```

  

#### Basic Connection

  

Connect to an FTPS server:

```

ftp-ssl server_ip

```

  

When prompted, enter your username and password.

  

#### Advanced Connection Options

  

Connect with specific port:

```

ftp-ssl -p 21 server_ip

```

  

Verbose mode for troubleshooting:

```

ftp-ssl -v server_ip

```

  

Disable certificate verification (for self-signed certificates):

```

ftp-ssl -z server_ip

```

  

#### Essential ftp-ssl Commands

  

List files in current directory:

```

ls

```

  

Change directory:

```

cd directory_name

```

  

Download a file:

```

get filename

```

  

Download multiple files:

```

mget *.txt

```

  

Upload a file:

```

put localfile

```

  

Upload multiple files:

```

mput *.txt

```

  

Set binary transfer mode (for non-text files):

```

binary

```

  

Set ASCII transfer mode (for text files):

```

ascii

```

  

Display current directory:

```

pwd

```

  

Create a directory:

```

mkdir new_directory

```

  

Delete a file:

```

delete filename

```

  

Delete a directory:

```

rmdir directory_name

```

  

Exit ftp-ssl:

```

bye

```

  

#### Automating ftp-ssl with Scripts

  

Create a file with ftp-ssl commands:

```

cat > ftpscript.txt << EOF

user username password

binary

cd /remote/directory

get important_file.dat

bye

EOF

```

  

Execute the script:

```

ftp-ssl -s:ftpscript.txt server_ip

```

  

### 6. Troubleshooting ftp-ssl Connections

  

#### Common Issues and Solutions

  

##### Certificate Verification Failures

If you receive SSL certificate errors:

```

ftp-ssl -z server_ip   # Disables certificate verification

```

  

##### Connection Timeouts

If connection times out:

1. Check firewall settings on both client and server

2. Verify FTPS service is running: `systemctl status vsftpd`

3. Try connecting to explicit port: `ftp-ssl -p 21 server_ip`

  

##### Data Transfer Issues

If you can connect but transfers fail:

1. Try switching between active and passive modes: `passive`

2. Check if server forces data encryption

3. Ensure data ports (high ports) are open on firewalls

  

#### Debugging Connections

  

Enable verbose mode:

```

ftp-ssl -v server_ip

```

  

Test TLS handshake directly:

```

openssl s_client -connect server_ip:21 -starttls ftp

```

  

### 7. Other FTPS Client Tools (For Reference)

  

The following tools also support FTPS connections:

- LFTP (command-line)

- cURL (command-line)

- ncFTP (command-line)

- FileZilla (GUI)

- WinSCP (GUI)

- Cyberduck (GUI)

- gFTP (GUI)

  

### 8. Additional Security Settings

  

For enhanced security, add these to vsftpd.conf:

```

# Disable older, vulnerable protocols

ssl_tlsv1=NO

ssl_sslv2=NO

ssl_sslv3=NO

ssl_tlsv1_1=NO

ssl_tlsv1_2=YES

ssl_tlsv1_3=YES

  

# Use strong ciphers only

ssl_ciphers=HIGH

  

# Prevent SSL session reuse problems

require_ssl_reuse=NO

  

# Logging configuration

xferlog_enable=YES

xferlog_std_format=YES

xferlog_file=/var/log/vsftpd.log

log_ftp_protocol=YES

```

  

These settings disable older, vulnerable protocols and enforce strong ciphers while ensuring proper logging of all FTP transactions.

  

### 9. Testing Your FTPS Configuration

  

After setting up both server and client, verify the connection is encrypted:

  

1. Connect using ftp-ssl:

```

ftp-ssl -v server_ip

```

  

2. Look for TLS negotiation messages in the output

  

3. Check server logs:

```

tail -f /var/log/vsftpd.log

```

  

4. Ensure files transfer correctly with full encryption by uploading and downloading test files

## Allow Root Login on vsftpd (FTP Server)

  

By default, `vsftpd` denies FTP access to privileged users like `root` for security reasons. However, in certain controlled environments (e.g., labs, VMs), you might want to enable root FTP login.

  

⚠️ **Warning**: Allowing `root` to log in via FTP is a major security risk and should **never** be done on a production or internet-facing server. Proceed only in isolated, secure environments.

  

## 1. Edit the `/etc/vsftpd/ftpusers` File

  

This file lists users who are always denied FTP access, even if they have a valid shell and password. You must remove or comment out the `root` entry.

  

```

vim /etc/vsftpd/ftpusers

```

  

Original example (root is blocked):

  

```

# Users that are not allowed to login via ftp

root

bin

daemon

adm

lp

sync

shutdown

halt

mail

news

uucp

operator

games

nobody

```

Updated version (root is allowed):

```

# Users that are not allowed to login via ftp

#root

bin

daemon

adm

lp

sync

shutdown

halt

mail

news

uucp

operator

games

nobody

```

## 2. Edit the `/etc/vsftpd/user_list` File

  

This file also lists users who are denied access by default (unless configured otherwise via `userlist_deny` setting). You must comment out `root` here as well.

  

```

vim /etc/vsftpd/user_list

```

  

Original:

  

```

# vsftpd userlist

# If userlist_deny=NO, only allow users in this file

# If userlist_deny=YES (default), never allow users in this file, and

# do not even prompt for a password.

# Note that the default vsftpd pam config also checks /etc/vsftpd/ftpusers

root

bin

daemon

adm

lp

sync

shutdown

halt

mail

news

uucp

operator

games

nobody

```

Updated version:

  

```

# vsftpd userlist

# If userlist_deny=NO, only allow users in this file

# If userlist_deny=YES (default), never allow users in this file, and

# do not even prompt for a password.

# Note that the default vsftpd pam config also checks /etc/vsftpd/ftpusers

#root

bin

daemon

adm

lp

sync

shutdown

halt

mail

news

uucp

operator

games

nobody

```

## 3. Restart the vsftpd Service

  

After making changes, restart the `vsftpd` service to apply them:

  

```

systemctl restart vsftpd.service

```

  

Check its status:

  

```

systemctl status vsftpd.service

```

  

## 4. Optional: Verify `root` Shell and Password

  

Make sure that the root account has a valid login shell and password:

  

```

grep root /etc/passwd

```

  

Expected output:

  

```

root:x:0:0:root:/root:/bin/bash

```

  

To set (or reset) the root password:

  

```

passwd root

```

  

## 5. Test Root FTP Login

  

You can now attempt to log in via FTP using `root`:

  

```

ftp 192.168.1.50

```

  

Credentials:

```

Username: root

Password: [your root password]

```

## 6. Jailing All Users Except Root in vsftpd

### 🔧 vsftpd.conf Settings

  

```ini

chroot_local_user=YES

chroot_list_enable=YES

chroot_list_file=/etc/vsftpd.chroot_list

````

### 📄 /etc/vsftpd.chroot_list

- List users who should **NOT** be jailed.

- Example:

    ```

    root

    ```

### 🧠 How It Works

- `chroot_local_user=YES`: Jail all users into their home directories.

- `chroot_list_enable=YES`: Enable exception list.

- Users in `vsftpd.chroot_list` are **excluded** from jail (e.g., `root`).

### ✅ Results

- All users are jailed by default.

- `root` or any listed user is **not jailed**.

## 7. Additional Notes

  

* `vsftpd.conf` settings related to user access do not need modification if `local_enable=YES` is already set.

* For secure setups, it's recommended to:

  * Use SSH/SFTP instead of FTP

  * Use virtual users with limited access if FTP is required.



# Login with Selected Users on vsftpd

  

You can restrict FTP login access to a specific list of users using the `userlist_file` directive in vsftpd.

  

### Step 1: Create the Allowed Users File

  

Edit or create the following file:

  

```

vim /etc/vsftpd/allow_users

```

  

Add the usernames you want to allow:

  

```

infosec

armour

u1

```

  

Only these users will be allowed to log in via FTP

### Step 2: Configure vsftpd.conf

  

Edit the main configuration file:

  

```

vim /etc/vsftpd/vsftpd.conf

```

  

Ensure the following lines are present or updated correctly:

  

```

# Enable local users to log in

local_enable=YES

  

# Enable write commands like STOR

write_enable=YES

  

# Allow passive FTP range

pasv_enable=YES

pasv_min_port=55000

pasv_max_port=55999

  

# Custom FTP login banner

ftpd_banner=Welcome to ARMOUR FTP service.

  

# Restrict access to only specified users

userlist_enable=YES

userlist_deny=NO

userlist_file=/etc/vsftpd/allow_users

  

# Allow chroot with write access if needed

allow_writeable_chroot=YES

  

# PAM authentication service

pam_service_name=vsftpd

  

# Use TCP Wrappers for host-based access control

tcp_wrappers=YES

  

# Enable IPv6 (vsftpd runs in standalone mode if listen=YES)

listen=NO

listen_ipv6=YES

```

  

>The key lines for restricting access to selected users are:

```

userlist_enable=YES

userlist_deny=NO

userlist_file=/etc/vsftpd/allow_users

```

  

* `userlist_deny=NO`: means only users in the list are allowed.

* If `userlist_deny=YES` (default), the users in the list are blocked instead.

## Step 3: Restart vsftpd Service

  

Apply the changes:

  

```

systemctl restart vsftpd.service

```

  

## Example Test

  

Login using FTP client:

  

```

ftp <your-server-ip>

```

  

Try logging in as one of the allowed users (e.g., `u1`) and verify access. Any other users should receive a login denial.

  

## Notes

  

* Make sure `ftpusers` and `user_list` files do not block your allowed users.

* If a user appears in `/etc/vsftpd/ftpusers` or `/etc/vsftpd/user_list`, they will be blocked even if they are listed in `allow_users`.

* To avoid this, comment out or remove those users from those two files.