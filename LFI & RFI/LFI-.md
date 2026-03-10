# Local File Inclusion (LFI) Security Notes

## Definition

Local File Inclusion (LFI) is a web security vulnerability where an attacker tricks a web application into including files that exist on the same server. This happens when user-supplied input is used unsafely to specify files for inclusion, often via PHP functions like `include()` or `require()`.

## How LFI Works

Developers sometimes dynamically include files based on user input to load different pages or content. If this input is not properly validated or sanitized, attackers can manipulate paths to include unintended files such as sensitive configuration files or system files.

### Example Vulnerable Code

```php
<?php
$file = $_GET['file'];
include('pages/' . $file);
?>
```

### Attack Example

If an attacker requests:

```
http://example.com/index.php?file=../../../etc/passwd
```

They might be able to cause the server to include and reveal the contents of `/etc/passwd`, which is a critical system file.

## Differences Between LFI and Directory Traversal

- **LFI**: Includes and executes files **within** the web server's directory or its subdirectories
- **Directory Traversal**: Reads files from **outside** the web root but usually without executing them

## Risks of LFI

- Reading sensitive files (passwords, config files)
- Remote code execution if attacker can include uploaded malicious files or use PHP wrappers
- Privilege escalation and full server compromise

## Common LFI Attack Techniques

### Path Traversal

Using sequences like `../` to move up directories.

### Null Byte Injection (Historical)

To truncate string extensions (less relevant in modern PHP versions).

### PHP Wrappers

Using wrappers like `php://filter` to read source code as plain text.

### Log Poisoning

Injecting PHP code into logs and including them.

## Example Scripts Analysis

### Script 1: local-file-inclusion.php (Basic Vulnerable Example)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Local File Inclusion</title>
</head>
<body>

<h1>Local File Inclusion</h1>

<?php
if (isset($_REQUEST["file"])) {
    include($_REQUEST["file"]);
}
?>

</body>
</html>
```

**Vulnerability**: Direct inclusion of user input without any validation.

### Script 2: local-file-inclusion-2.php (Attempted Mitigation - Still Vulnerable)

```html
<!DOCTYPE html>

<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Local File Inclusion</title>
</head>
<body>

<h1>Local File Inclusion</h1>

<?php
if (isset($_REQUEST["file"])) {
    // Prepend the directory to the user-supplied filename
    $local_file = "includes/" . $_REQUEST["file"];
    include($local_file);
}
?>

</body>
</html>
```

**Issue**: Still vulnerable to path traversal attacks like `../../../etc/passwd`.

### Script 3: local-file-inclusion-3.php (Absolute Path - Still Vulnerable)

```html
<!DOCTYPE html>

<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Local File Inclusion</title>
</head>
<body>

<h1>Local File Inclusion</h1>

<?php
if (isset($_REQUEST["file"])) {
    // Absolute path concatenated with user input
    $local_file = "/var/www/html/webpentest/includes/" . $_REQUEST["file"];
    
    include($local_file);
}
?>

</body>
</html>
```

### How Script 3 Works:

- The script receives the `file` parameter from the HTTP request
- Concatenates it with a fixed absolute directory path: `/var/www/html/webpentest/includes/`
- Includes the resulting file

### Security Considerations for Script 3:

**Issue**: Path traversal can still be used to escape the intended directory using sequences like `../../../etc/passwd`.

## Mitigation Best Practices

### Input Validation and Sanitization

- Validate and sanitize all user inputs used in file paths
- Use strict allowlists of permitted files

### Use Whitelists

- Use whitelist of allowed files instead of user input directly
- Example: Map user input to predefined safe file names

### Disable Dangerous PHP Settings

- Disable `allow_url_include=off` in PHP configuration
- Consider disabling `allow_url_fopen` if not needed

### Least Privilege

- Employ least privilege for web server user
- Restrict file system permissions

### Keep Software Updated

- Keep software and dependencies updated
- Apply security patches promptly

### Additional Security Measures

- Implement proper access controls
- Use Web Application Firewalls (WAF)
- Regular security audits and penetration testing
- Monitor logs for suspicious file inclusion attempts

## Security Testing Considerations

When testing for LFI vulnerabilities:

- Test various path traversal sequences (`../`, `..\\`, etc.)
- Try different file types and system files
- Test with URL encoding and double encoding
- Check for null byte injection (in older systems)
- Test PHP wrappers if PHP is used

## Key Takeaways

1. Never trust user input when constructing file paths
2. Always validate and sanitize input
3. Use whitelists instead of blacklists when possible
4. Implement defense in depth with multiple security layers
5. Regular security testing is essential
6. Keep systems and dependencies updated