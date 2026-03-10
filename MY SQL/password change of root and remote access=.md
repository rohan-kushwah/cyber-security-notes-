# 📘 MySQL, MariaDB & PostgreSQL Administration and Configuration Guide

---

## 🧠 Overview

**MySQL**, **MariaDB**, and **PostgreSQL** are the three most popular open-source relational database management systems (RDBMS).  
They are used for web apps, enterprise systems, and data analytics.

|Feature|MySQL|MariaDB|PostgreSQL|
|---|---|---|---|
|Developer|Oracle Corporation|MariaDB Foundation|PostgreSQL Global Dev Group|
|License|GPL (Community) / Proprietary|GPL|PostgreSQL License (liberal)|
|Speed|Fast for read-heavy workloads|Slightly faster with new optimizations|Slightly slower, but more feature-rich|
|Best For|Web apps (e.g. WordPress, PHP apps)|Drop-in MySQL replacement|Complex data, JSON, analytics|
|Replication|Master–Slave|Master–Master supported|Streaming replication|
|JSON Support|Yes|Yes|Excellent (native JSONB type)|

---

## ⚙️ 1. Installation and Service Management

### On Debian/Ubuntu:

`sudo apt update sudo apt install mysql-server mariadb-server postgresql -y`

### On CentOS/RHEL/Fedora:

`sudo dnf install mysql-server mariadb-server postgresql-server -y`

### Start and Enable Service:

`sudo systemctl start mysql sudo systemctl enable mysql sudo systemctl status mysql`

> 🧩 Explanation:
> 
> - `start`: launches the service now
>     
> - `enable`: ensures it starts on boot
>     
> - `status`: checks if it’s running
>     

---

## 💻 2. Accessing the Database Shell

### MySQL / MariaDB:

`sudo mysql -u root -p`

### PostgreSQL:

`sudo -i -u postgres psql`

### Check databases:

`SHOW DATABASES;`

or for PostgreSQL:

`\l`

---

## 🔐 3. Resetting the Root Password (MySQL / MariaDB)

If you forget your MySQL root password:

1. Stop the MySQL service:
    
    `sudo systemctl stop mysql`
    
2. Start MySQL in safe mode:
    
    `sudo mysqld_safe --skip-grant-tables &`
    
3. Log in without password:
    
    `mysql -u root`
    
4. Update the password:
    
    `FLUSH PRIVILEGES; ALTER USER 'root'@'localhost' IDENTIFIED BY 'newpassword';`
    
5. Restart MySQL:
    
    `sudo systemctl restart mysql`
    

> ✅ Explanation: `--skip-grant-tables` allows access without authentication temporarily.

---

## 🌐 4. Enabling Remote Access (MySQL)

1. Edit configuration file:
    
    `sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf`
    
2. Find the line:
    
    `bind-address = 127.0.0.1`
    
    Change to:
    
    `bind-address = 0.0.0.0`
    
3. Save and restart:
    
    `sudo systemctl restart mysql`
    
4. Allow port 3306 in firewall:
    
    `sudo ufw allow 3306`
    Steps To Create Remote Access For Root

1. Create the user (allow root to connect from any host)
    

`CREATE USER 'root'@'%' IDENTIFIED BY 'root';`

2. Grant all privileges
    

`GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;`

3. Apply changes
    

`FLUSH PRIVILEGES;`

4. Confirm user creation
    

`SELECT user, host FROM mysql.user;`

**Notes / cautions**

- Run these inside the MySQL client (e.g. `mysql -u root -p`).
    
- Allowing `root`@`%` is a serious security risk — it permits root logins from any IP. Prefer creating a less-privileged user for remote access, or restrict host/IPs (e.g. `'root'@'192.168.1.%'`).
    
- On MySQL 8+, `CREATE USER ... IDENTIFIED BY 'password'` is correct; if you use other authentication plugins you may need to adjust (e.g. `IDENTIFIED WITH caching_sha2_password BY 'password'`).

rom client side this command-
mysql -u root -h 192.168.1.80 -p --ssl=false


---

## 👥 5. Granting Privileges to Remote Users

Login to MySQL:

`mysql -u root -p`

Then:

`CREATE USER 'dbuser'@'%' IDENTIFIED BY 'StrongPass123!'; GRANT ALL PRIVILEGES ON *.* TO 'dbuser'@'%' WITH GRANT OPTION; FLUSH PRIVILEGES;`

> 💡 `'%'` means any host. Use specific IP for more security (e.g., `'192.168.1.10'`).

---

## 🌍 6. Remote Connection Example

From another client:

`mysql -h 192.168.1.100 -u dbuser -p`

PostgreSQL example:

`psql -h 192.168.1.100 -U postgres -d mydb`

---

## 🧾 7. PostgreSQL Remote Access Setup

1. Edit configuration:
    
    `sudo nano /etc/postgresql/15/main/postgresql.conf`
    
    Find:
    
    `listen_addresses = 'localhost'`
    
    Change to:
    
    `listen_addresses = '*'`
    
2. Edit client authentication:
    
    `sudo nano /etc/postgresql/15/main/pg_hba.conf`
    
    Add line:
    
    `host    all             all             0.0.0.0/0               md5`
    
3. Restart PostgreSQL:
    
    `sudo systemctl restart postgresql`
    

from client side this command-
mysql -u root -h 192.168.1.80 -p --ssl=false

---

## 🧰 8. Common Problems and Fixes

|Problem|Cause|Solution|
|---|---|---|
|Access denied for user|Wrong password / host not allowed|Check `GRANT` and user host|
|Can’t connect remotely|Bind address / Firewall|Set `bind-address=0.0.0.0`, open port 3306|
|Root password lost|Misconfigured user|Use `--skip-grant-tables` reset|
|PostgreSQL connection refused|`pg_hba.conf` or firewall issue|Add `host all all 0.0.0.0/0 md5`|

---

## 🌐 9. Real-Life Example: Hosting Web App Database

A **PHP web application** on one server needs a database hosted on another.

**Web Server (192.168.1.10)**  
**DB Server (192.168.1.20)**

On DB Server:

`CREATE DATABASE webapp; CREATE USER 'webuser'@'192.168.1.10' IDENTIFIED BY 'App@123'; GRANT ALL PRIVILEGES ON webapp.* TO 'webuser'@'192.168.1.10'; FLUSH PRIVILEGES;`

In PHP code:

`<?php $connection = new mysqli("192.168.1.20", "webuser", "App@123", "webapp"); if ($connection->connect_error) {     die("Connection failed: " . $connection->connect_error); } echo "Connected successfully!"; ?>`

---

## ⚡ 10. SQL Query Examples

### Create Database and Table

`CREATE DATABASE shopdb; USE shopdb; CREATE TABLE products (   id INT AUTO_INCREMENT PRIMARY KEY,   name VARCHAR(100),   price DECIMAL(10,2) );`

### Insert and Select

`INSERT INTO products (name, price) VALUES ('Laptop', 599.99); SELECT * FROM products;`

### Update and Delete

`UPDATE products SET price = 499.99 WHERE name='Laptop'; DELETE FROM products WHERE id=1;`

---

## 🧩 11. Quick Commands Summary (Cheat Sheet)

### 🔹 MySQL / MariaDB

`sudo systemctl start mysql sudo systemctl restart mysql mysql -u root -p SHOW DATABASES; CREATE DATABASE testdb; CREATE USER 'test'@'localhost' IDENTIFIED BY 'pass'; GRANT ALL PRIVILEGES ON testdb.* TO 'test'@'localhost'; FLUSH PRIVILEGES;`

### 🔹 PostgreSQL

`sudo -i -u postgres psql \l                     # List databases \du                    # List users CREATE DATABASE testdb; CREATE USER test WITH PASSWORD 'pass'; GRANT ALL PRIVILEGES ON DATABASE testdb TO test;`

### 🔹 Configuration Files

|Database|Config File Path|
|---|---|
|MySQL|`/etc/mysql/mysql.conf.d/mysqld.cnf`|
|MariaDB|`/etc/my.cnf` or `/etc/mysql/mariadb.conf.d/`|
|PostgreSQL|`/etc/postgresql/<version>/main/postgresql.conf`|

---

## 🧠 Final Tips

- Always **restart** services after config changes.
    
- Use **strong passwords** and **specific IPs** instead of `%`.
    
- Keep your MySQL/PostgreSQL **updated** for security patches.
    
- For backups:
    
    `mysqldump -u root -p mydb > mydb_backup.sql pg_dump -U postgres mydb > mydb_backup.sql`