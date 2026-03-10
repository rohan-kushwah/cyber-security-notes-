# 🔐 **SSH (Secure Shell) Server on CentOS**

---

## 🔍 **Check If OpenSSH Is Already Installed**

```bash
rpm -qa | grep ssh
```

---

## ❌ **Remove Existing OpenSSH Server (If Needed)**

```bash
yum remove openssh-server
```

---

## 📦 **Install OpenSSH Packages**

```bash
yum install openssh*
```

Or install them individually:

```bash
yum install openssh-server
```

```bash
yum install openssh-clients
```

---

## ℹ️ **Check Package Information**

```bash
# Retrieve detailed information about the installed OpenSSH server package:
rpm -qi openssh-server
```

```bash
# List all the files installed by the OpenSSH server package:
rpm -ql openssh-server
```

```bash
# List all the configuration files related to the OpenSSH server:
rpm -qc openssh-server
```

```bash
# Display documentation files included in the OpenSSH server package:
rpm -qd openssh-server
```

---

## ⚙️ **Manage The SSH Daemon With Systemd**

```bash
# Check the current status of the SSH daemon:
systemctl status sshd.service
```

```bash
# Start the SSH service:
systemctl start sshd.service
```

```bash
# Enable SSH to start automatically on boot:
systemctl enable sshd.service
```

---

## ✅ **Verify SSH Service Is Running**

```bash
# Check if the SSH daemon is listening on the expected ports (usually port 22):
netstat -nltup | grep sshd
```

```bash
# Tip: if `netstat` is not available, it can be installed via `yum install net-tools` or `ss` can be used as an alternative:
ss -nltup | grep sshd
```

---

## 🔀 **Change Default SSH Port**

### ✏️ Step 1: Edit The SSHD Configuration File

```bash
vim /etc/ssh/sshd_config
```

```bash
# Locate the following line and change the port number (e.g., to 2222):
Port 2222
```

> Note: Ensure this line is not commented out (remove the `#` at the beginning)

---

### 🔥 Step 2: Update The Firewall Rules (Using iptables)

```bash
vim /etc/sysconfig/iptables
```

```bash
# Add the following line to permit incoming SSH traffic on port 2222:
-A INPUT -p tcp --dport 2222 -j ACCEPT
```

---

### 🔥 Step 2: Update The Firewall Rules (Using Firewalld)

```bash
# Add the new SSH port to firewalld:
firewall-cmd --permanent --add-port=2222/tcp
```

```bash
# Remove the default port (optional):
firewall-cmd --permanent --remove-service=ssh
```

```bash
# Reload the firewall rules:
firewall-cmd --reload
```

```bash
# Verify the new port is allowed:
firewall-cmd --list-ports
```

Expected output:

```bash
2222/tcp
```

---

### 🔄 Step 3: Restart Services

```bash
# Restart the iptables firewall service:
systemctl restart iptables.service
```

```bash
# Restart the SSH service to load the updated configuration:
systemctl restart sshd.service
```

---

### 🧪 Step 4: Verify SSH Service Port

```bash
# Check that the SSH daemon is listening on the new port:
netstat -ntulp | grep sshd
```

```bash
# Alternatively, use ss if netstat is unavailable:
ss -ntulp | grep sshd
```

---

### 🔐 Step 5: Connect Using The New SSH Port

```bash
ssh -p 2222 root@192.168.1.37
```

> Replace `root` and `192.168.1.37` with the appropriate username and server IP address.

---

## 🗒️ **Example SSH Configuration Snippet (`/etc/ssh/sshd_config`)**

```bash
Port 22
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key
SyslogFacility AUTHPRIV
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication yes
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding yes
AcceptEnv LANG LC_*
Subsystem sftp /usr/libexec/openssh/sftp-server
```

> Make sure to save changes and always back up configuration files before modifying them.

---

## 🛡️ **SELinux Consideration (If Enabled)**

```bash
semanage port -a -t ssh_port_t -p tcp 2222
```

> It may be necessary to install the `policycoreutils-python` or `policycoreutils-python-utils` package to use `semanage`.

---

## 🌐 **Bind SSH To A Specific IP Address**

### 🗒️ Step 1: Edit SSH Configuration File

```bash
vim /etc/ssh/sshd_config
```

```bash
# Set ListenAddress to the desired IP address:
ListenAddress 192.168.1.38

# Change port if needed:
Port 2222
```

> You can specify multiple `ListenAddress` entries if the server should listen on more than one IP.

---

### 🔄 Step 2: Restart The SSH Service

```bash
systemctl restart sshd.service
```

---

### ✅ Step 3: Verify SSH Is Listening On The Correct IP And Port

```bash
netstat -nltup | grep sshd
```

Expected output:

```bash
tcp        0      0 192.168.1.38:2222      0.0.0.0:*               LISTEN      <PID>/sshd
```

---

## 📋 **Sample SSH Configuration Snippet**

```bash
Port 2222
ListenAddress 192.168.1.38

HostKey /etc/ssh/ssh_host_rsa_key
HostKey /c/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key

SyslogFacility AUTHPRIV
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication yes
ChallengeResponseAuthentication no

GSSAPIAuthentication yes
GSSAPICleanupCredentials no
UsePAM yes
X11Forwarding yes
AcceptEnv LANG LC_*
Subsystem sftp /usr/libexec/openssh/sftp-server
```

> You can comment out the `#ListenAddress` or `#ListenAddress 0.0.0.0` lines if you don't want the daemon to listen on all interfaces.

---

## 👤 **Disable Root Login and Use Custom User for SSH**

To improve security, you can **disable root login** via SSH and allow only specific non-root users.

---

### 👥 Step 1: Create a New User (if not already created)

Replace `yourusername` with your preferred user:

```bash
adduser yourusername
passwd yourusername
```

---

### 👮 Step 2: Add the User to the Wheel Group (for sudo access)

```bash
usermod -aG wheel yourusername
```

---

### 📝 Step 3: Edit the SSH Configuration File

```bash
vim /etc/ssh/sshd_config
```

Look for the following directive and **set it to `no`** to disable root login:

```bash
PermitRootLogin no
```

You can also explicitly **allow specific users** to connect via SSH:

```bash
AllowUsers yourusername
```

> This restricts SSH access to only `yourusername`.

---

### 🔄 Step 4: Restart the SSH Daemon

```bash
systemctl restart sshd.service
```

---

### 🧪 Step 5: Test SSH Access

```bash
ssh yourusername@192.168.1.38 -p 2222
```

> Replace `yourusername`, IP, and port as per your setup.

---

### 🔐 Optional: Set Up Key-Based Authentication for the User

1. On your **client machine**:
    
    ```bash
    ssh-keygen -t rsa
    ```
    
2. Copy the key to the server:
    
    ```bash
    ssh-copy-id -p 2222 yourusername@192.168.1.38
    ```