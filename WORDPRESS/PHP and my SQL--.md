
### **PHP and MySQL in Brief**

#### **1️⃣ PHP (Hypertext Preprocessor)**

- **PHP** is a **server-side scripting language** used for web development.
- It helps create **dynamic web pages** by processing user input, handling forms, and interacting with databases.
- PHP code runs on the **server** and sends HTML to the browser.

Example:
```php
<?php echo "Hello, World!"; ?>
```

**Common Uses:**
- ✅ Dynamic websites  
- ✅ User authentication (login systems)  
- ✅ File handling (uploads, downloads)  
- ✅ Interacting with databases (MySQL)  

---

#### **2️⃣ MySQL (Structured Query Language Database)**

- **MySQL** is an **open-source relational database management system (RDBMS)**.
- It stores, retrieves, and manages data efficiently.
- Works with **PHP** to store user info, posts, products, etc.

Example Query:
```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE
);
```

**Common Uses:**
- ✅ Storing user details (login, profiles)  
- ✅ Managing product catalogs  
- ✅ Handling large-scale data efficiently  

---

### **🔗 PHP + MySQL Together**

- PHP processes user requests and **fetches/saves** data in MySQL.

Example: PHP code to get user data from MySQL:
```php
<?php
$conn = new mysqli("localhost", "username", "password", "database");
$result = $conn->query("SELECT * FROM users");
while ($row = $result->fetch_assoc()) {
    echo $row['name'];
}
?>
```

**Why Use Them Together?**
- ✅ **PHP handles logic** (forms, authentication)  
- ✅ **MySQL stores data** (users, products, transactions)  

---

# ✨ **PHP and MySQL Installation and Configuration** ✨

## 🔹 **PHP Installation and Configuration**

### 📌 **Install Required Repositories**
```bash
yum install epel-release yum-utils
yum repolist all
```

---

### 🌐 **Add Remi Repository**
```bash
yum install http://rpms.remirepo.net/enterprise/remi-release-9.rpm
yum repolist all
```

### ⚙️ **Verify and Configure Remi Repository**
```bash
rpm -qc remi-release
rpm -ql remi-release
vim /etc/yum.repos.d/remi.repo
```

### 🔎 **Search for Available PHP Versions**
```bash
yum search php
yum info php.x86_64
yum info php81.x86_64
yum info php84.x86_64
```

### 🔧 **Install PHP and Required Extensions**
```bash
yum install php php-common.x86_64 php-cli.x86_64 php-gd.x86_64 php-opcache.x86_64 php-curl php-mysqlnd.x86_64 php-xml.x86_64 php-mbstring.x86_64 php-pear php-mbstring php-pecl-http php-session
```

### ✅ **Verify PHP Installation**
```bash
php -v
```

### 🔄 **Restart Apache**
```bash
systemctl restart httpd.service
```

### 📄 **Create `phpinfo.php` for Testing**
```bash
vim /var/www/html/phpinfo.php
```

Add the following content:
```php
<?php
    phpinfo();
?>
```

Access via:
```
http://192.168.1.32/phpinfo.php
```

---

## **MySQL Installation and Configuration**

### 📥 **Download and Install MySQL Repository**
```bash
wget https://repo.mysql.com/mysql80-community-release-el7-3.noarch.rpm
yum install ./mysql84-community-release-el9.1.noarch.rpm
yum repolist all
```

### 🔄 **Enable and Disable MySQL Versions**
```bash
yum-config-manager --disable mysql57.community
```

### 🔎 **Search for MySQL Packages**
```bash
yum search mysql
```

### 📌 **Install MySQL Server and Development Packages**
```bash
 yum install mysql-community-server mysql-community-devel
```

### 📡 **Check MySQL Service Status**
```bash
  systemctl status mysqld.service\nsystemctl enable mysqld.service\nnetstat -nltup | grep mysql
10404  reboot
```

### 🔄 **Start and Restart MySQL Service**
```bash
systemctl start mysqld.service
systemctl restart mysqld.service
netstat -nltup | grep mysql
```

### 🔑 **Retrieve MySQL Root Password**
```bash
ls -lh /var/log/mysqld.log
grep 'temporary password' /var/log/mysqld.log
```

zl+31?#W5qaw
Example Output:
```
A temporary password is generated for root@localhost: s;r=oAt25UwR
```

### 🔐 **Secure MySQL Installation**
```bash
mysql -u root -p
```
Enter the temporary password when prompted.
```bash
mysql_secure_installation
```

Example new password:
```
p@ssW0rd@1234\
```


![[Pasted image 20250403155930.png]]
# gui of![[Pasted image 20250403160104.png]] my sql-
![[Pasted image 20250403160104.png]]



php admin php pe bani ek particular application  hai jise my sql  manage hota hai

 ![[Pasted image 20250403160450.png]]
