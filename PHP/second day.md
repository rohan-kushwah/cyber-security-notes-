# 🧠 PHP Concepts for Cybersecurity Students (Simplified Guide)

These are **important PHP concepts** that help cybersecurity students understand **data handling, validation, encoding, encryption, and secure coding**.

---

## 🔹 1. Regular Expressions (RegEx)

Used to **match patterns** in strings (for validation and filtering).

`<?php $username = "Cyber_123"; if (preg_match("/^[a-zA-Z0-9_]+$/", $username)) {     echo "✅ Valid username<br>"; } else {     echo "❌ Invalid username<br>"; } ?>`

---

## 🔹 2. `preg_replace()` & `preg_split()`

Used to **replace or split** text using regex.

`<?php $text = "Hackers love exploiting vulnerabilities!"; $clean = preg_replace("/hackers/i", "Security Experts", $text); echo $clean . "<br>";  $words = preg_split("/\s+/", $text); print_r($words); ?>`

---

## 🔹 3. Encoding & Decoding

Used to **hide data format**, not for encryption.

`<?php $text = "CyberSecurity@2025"; $encoded = urlencode($text); $decoded = urldecode($encoded);  echo "Encoded: $encoded <br>"; echo "Decoded: $decoded <br>"; ?>`

---

## 🔹 4. HTML Entities / Special Characters

Prevents **XSS attacks** by escaping HTML.

`<?php $input = "<script>alert('Hacked!');</script>"; echo htmlspecialchars($input) . "<br>";  echo htmlentities($input) . "<br>"; echo html_entity_decode("&lt;b&gt;Secure&lt;/b&gt;") . "<br>"; ?>`

---

## 🔹 5. Base64 Encoding & Decoding

Used to store data safely in text form (not secure encryption).

`<?php $data = "Sensitive Data"; $encoded = base64_encode($data); $decoded = base64_decode($encoded);  echo "Encoded: $encoded <br>"; echo "Decoded: $decoded <br>"; ?>`

---

## 🔹 6. Hashing (MD5, SHA1, SHA256, SHA512)

Converts data into **fixed-length secure strings**.

`<?php $password = "MySecret123"; echo "MD5: " . md5($password) . "<br>"; echo "SHA1: " . sha1($password) . "<br>"; echo "SHA256: " . hash('sha256', $password) . "<br>"; echo "SHA512: " . hash('sha512', $password) . "<br>"; ?>`

---

## 🔹 7. Reversible vs Non-Reversible Hash

- **Reversible** – base64_encode(), you can decode it.
    
- **Non-Reversible** – md5(), sha256(), cannot be reversed.  
    ✅ Always use **non-reversible hashes** for passwords.
    

---

## 🔹 8. `password_hash()` & 9. `password_verify()`

Modern, secure way to **store and verify passwords**.

`<?php $pass = "Secure@123"; $hash = password_hash($pass, PASSWORD_DEFAULT); echo "Password Hash: $hash <br>";  if (password_verify($pass, $hash)) {     echo "✅ Password matched!<br>"; } else {     echo "❌ Invalid password!<br>"; } ?>`

---

## 🔹 10. Include & Require

Used to **add external PHP files**.

`<?php include 'header.php';       // Includes even if missing require 'config.php';       // Fatal error if missing include_once 'menu.php';    // Includes only once require_once 'footer.php';  // Includes only once ?>`

⚠️ Avoid user-controlled paths — prevents **LFI (Local File Inclusion)** attacks.

---

## 🔹 11. OS Functionality

Runs system commands from PHP.

`<?php $cmd = "whoami"; // Shows current user echo shell_exec($cmd); ?>`

⚠️ Dangerous if user input is unsanitized — may lead to **Command Injection**.

---

## 🔹 12. File System Functionality

Used to read/write files. Be careful with **file uploads**.

`<?php $file = "log.txt"; if (file_exists($file)) {     echo "File size: " . filesize($file) . " bytes<br>";     echo "Content: " . file_get_contents($file); } else {     echo "File not found!"; } ?>`

---

## 🔹 13. PHP Error Display

Shows all PHP errors (useful for debugging).

`<?php error_reporting(E_ALL); ini_set('display_errors', 1); echo "Error display enabled.<br>"; ?>`

⚠️ Disable in production — hides system info from attackers.

---

## 🔹 14. OS Command Execution Example (Practical)

Run a real system command securely (never take input directly!).

`<?php $command = escapeshellcmd("ls -l"); // Sanitized $output = shell_exec($command); echo "<pre>$output</pre>"; ?>`

---

## 🔹 15. Learn More

👉 Visit **W3Schools PHP Tutorial** for examples, syntax, and safe coding practices.

---

# 🧩 Combined Example – Everything Together

Here’s a **big code sample** showing many of the concepts working together:

`<?php error_reporting(E_ALL); ini_set('display_errors', 1);  // --- 1. Input validation --- $user = "Cyber_Student01"; if (preg_match("/^[a-zA-Z0-9_]+$/", $user)) {     echo "✅ Valid username<br>"; } else {     echo "❌ Invalid username<br>"; }  // --- 2. Encoding & decoding --- $data = "Cyber@2025"; $encoded = base64_encode($data); echo "Encoded data: $encoded<br>"; echo "Decoded data: " . base64_decode($encoded) . "<br>";  // --- 3. Hashing and password security --- $plain = "MySecurePassword!"; $hash = password_hash($plain, PASSWORD_DEFAULT); echo "Hash: $hash<br>";  if (password_verify($plain, $hash)) {     echo "✅ Password verified<br>"; } else {     echo "❌ Wrong password<br>"; }  // --- 4. HTML entity encoding (prevent XSS) --- $input = "<script>alert('XSS');</script>"; echo "Safe output: " . htmlspecialchars($input) . "<br>";  // --- 5. File and OS functions --- $file = "test.txt"; file_put_contents($file, "This is a test log entry.\n"); echo "File created.<br>";  if (file_exists($file)) {     echo "File size: " . filesize($file) . " bytes<br>"; }  // --- 6. OS Command (safe example) --- $cmd = escapeshellcmd("whoami"); echo "Running as: " . shell_exec($cmd) . "<br>"; ?>`

---

# ✅ Quick Summary

|Concept|Purpose|Security Relevance|
|---|---|---|
|RegEx|Validate inputs|Prevent injection attacks|
|HTML Entities|Escape HTML|Prevent XSS|
|Base64|Encode data|Not secure encryption|
|Hashing|Data integrity|Non-reversible|
|password_hash()|Secure password storage|Recommended|
|include/require|Modular code|Avoid LFI/RFI|
|File functions|Read/write|Prevent file upload abuse|
|OS commands|System interaction|Avoid injection|
|Error reporting|Debugging|Hide in production|