# 🎯 NULL-BYTE INJECTION VULNERABILITY

---

## 📚 What is Null-Byte Injection?

**Null-byte injection** is an input-validation attack where an attacker inserts a **NUL character**  
(`\0`, `%00`, `\x00`) into user-controlled data.

💡 The trick works because:

- **Lower-level / native code (C/C++)** treats `\0` as **end of string**
- **Higher-level code (PHP, Java, Python, etc.)** may still process the **full string**

This mismatch can be abused to **bypass security checks** such as:
- File extension validation  
- Path restrictions  
- Upload filtering  

---

## ⚙️ How It Works

Many system-level APIs are written in **C/C++**, which use **NUL-terminated strings**.

### Typical flow:
1. Application validates or modifies input (e.g., appends `.php`)
2. Input is passed to a **native API**
3. Native layer **truncates input at `\0`**
4. Validation is silently bypassed 🚨

### Example:
```text
Input: shell.php%00.jpg
PHP sees: shell.php%00.jpg
C library sees: shell.php


---

## 🛡️ Is This Still a Problem Today?

### ✅ Modern PHP (7.x / 8.x)

- PHP strings are **binary-safe**
    
- Core truncation bugs were fixed around **PHP 5.3.4**
    

### ⚠️ Still risky when:

- PHP interacts with **native extensions / FFI**
    
- Web servers or proxies mishandle `%00`
    
- **Old or unpatched modules** are used
    
- File upload parsers treat metadata differently than content
    

🔐 **Conclusion:**

> Modern runtimes reduce the risk — but **defense in depth is still required**

---

## 🎯 Typical Attack Examples

- **Extension bypass**
    
    `secret_config%00.txt  → opens secret_config`
    
- **Malicious upload**
    
    `webshell.php%00.pdf`
    
- **Native command truncation**
    
    `argument%00--dangerous-flag`
    

---

## 🧪 How to Test Safely (Lab / Controlled)

⚠️ **Test only on systems you own or are authorized to test**

### Testing ideas:

- Inject `%00` or `\0` into:
    
    - Query strings
        
    - POST data
        
    - Headers
        
    - Cookies
        
- Multipart upload filename abuse:
    
    `curl -F "file=@poc.php;filename=poc.php%00.pdf" http://target/upload`
    
- Unit tests that assert **NUL input is rejected**
    

---

## 🔒 Practical Mitigations (Priority Order)

### ✅ Best Practices

1. **Reject NUL bytes early**
    
    `if (strpos($input, "\0") !== false) reject();`
    
2. **Canonicalize paths**
    
    - Use `realpath()`
        
    - Ensure file stays inside allowed directories
        
3. **Never append extensions to user input**
    
    - Use allowlists instead
        
4. **Validate uploads by content**
    
    - Magic bytes / MIME detection
        
5. **Avoid passing raw input to native code**
    
    - Use safe APIs & parameterized calls
        
6. **Keep everything updated**
    
    - PHP, web server, extensions
        
7. **WAF rules**
    
    - Helpful, but never sufficient alone
        

---

## 💻 PHP Code Examples

### ❌ Vulnerable Pattern (DON'T USE)

`<?php $page = $_GET['page'];     // attacker controls input $page = $page . '.php';    // naive append include __DIR__ . '/pages/' . $page;`

---

### ✅ Safe Pattern (Allowlist)

`<?php $allowed = [     'home'  => 'pages/home.php',     'about' => 'pages/about.php' ];  $key = $_GET['page'] ?? 'home';  if (strpos($key, "\0") !== false || !isset($allowed[$key])) {     http_response_code(400);     exit('Invalid page'); }  include __DIR__ . '/' . $allowed[$key];`

---

### ✅ Reject NULs Early

`<?php $input = $_GET['file'] ?? '';  if (strpos($input, "\0") !== false) {     http_response_code(400);     exit('Invalid input'); }`

---

## 🐝 bee-box Example Setup

`# SSH into bee-box ssh -oHostKeyAlgorithms=+ssh-rsa root@192.168.1.205  # Create directories mkdir uploads pages chown -R www-data:www-data /var/www/uploads  # View vulnerable file vim null-byte-injection.php`

---

## 🌐 Local File Inclusion (Demo)

`<!DOCTYPE html> <html> <head>     <meta charset="utf-8">     <title>Local File Inclusion</title> </head> <body>     <h1>Local File Inclusion</h1>      <?php         if (isset($_REQUEST["file"])) {             $filename = $_REQUEST['file'] . ".php";             include($filename);         }     ?> </body> </html>`

🚨 **Why vulnerable?**

- Appends `.php`
    
- No validation
    
- Susceptible to `%00` truncation in unsafe environments