 Employee Leaves Management System (ELMS)

  

## Source

*   [PHP Projects](https://phpgurukul.com/php-projects-free-downloads/)

*   [Employee Leaves Management System (ELMS)](https://phpgurukul.com/employee-leaves-management-system-elms/)

  

---

  

## Installation Steps

  

### 1. Upload to Server

Transfer the ELMS package to your server:

```bash

scp ~/Downloads/Employee-Leave-Management-System-PHP.zip root@192.168.1.31:/root/

```

  

### 2. Extract Files

```bash

unzip Employee-Leave-Management-System-PHP.zip

```

```bash

cd Employee-Leave-Management-System-PHP

```

  

### 3. Move Files to Web Directory

```bash

cp -vr elms /var/www/html/

```

```bash

cd /var/www/html/

```

```bash

chown -Rv www-data:www-data elms

```

  

---

  

## Database Configuration

  

### 1. Create the Database

Log in to MySQL:

```bash

mysql -u root -p

```

Inside the MySQL shell, run the following commands:

```sql

CREATE USER 'elms'@'localhost' IDENTIFIED BY 'your_secure_password';

```

```sql

GRANT ALL PRIVILEGES ON elmsdb.* TO 'elms'@'localhost';

```

```sql

CREATE DATABASE elmsdb;

```

```sql

SHOW DATABASES;

```

```sql

EXIT;

```

  

### 2. Import Database Schema

```bash

cd /root/Employee-Leave-Management-System-PHP/SQL\ File/

```

```bash

mysql -u root -p elmsdb < elmsdb.sql

```

  

---

  

## Configure Database Connection

  

Edit the configuration file in both the main application and admin directories.

  

**For Main Application:**

```bash

vim /var/www/html/elms/includes/config.php

```

  

**For Admin Panel:**

```bash

vim /var/www/html/elms/admin/includes/config.php

```

  

### Configuration Template

```php

<?php

// DB credentials

define('DB_HOST','localhost');

define('DB_USER','root');

define('DB_PASS','Armour@1234');

define('DB_NAME','elmsdb');

  

// Database connection

try {

    $dbh = new PDO("mysql:host=".DB_HOST.";dbname=".DB_NAME,

        DB_USER,

        DB_PASS,

        array(PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES 'utf8'")

    );

}

catch (PDOException $e) {

    exit("Error: " . $e->getMessage());

}

?>

```

  

---

  

## Default Credentials

  

### Admin Panel

*   **Username:** `admin`

*   **Password:** `Test@12345`

*   **Admin URL:** `http://<server-ip>/elms/admin/`

  

### User Panel

*   **Username:** `jhn12@gmail.com`

*   **Password:** `Test@123`

*   **EmpID (for password recovery):** `7856214`

*   **User URL:** `http://<server-ip>/elms/`

  

---

  

## Important Notes

*   Always run ELMS in an **isolated lab or internal testing environment**.

*   The default credentials are for **testing purposes only** – change passwords if deploying in production.

*   Ensure **Apache, PHP, and MySQL/MariaDB** are properly installed and configured.

  

---

  

## Access URLs

  

| Panel | URL Example                       |

| :---- | :-------------------------------- |

| Admin | `http://192.168.1.30/elms/admin/`  |

| User  | `http://192.168.1.30/elms/`       |

  

---

  

## License & Credits

*   Project by PHPGurukul

*   https://phpgurukul.com/

  

---
# Gym Management System

  

A guide for setting up the Gym Management System from PHPGurukul.

  

*   **Source:** [GYM Management System - PHPGurukul](https://phpgurukul.com/gym-management-system-php-with-source-code/)

  

---

  

## Installation Steps

  

### 1. Upload to Server

Transfer the downloaded ZIP file to your server.

```bash

scp ~/Downloads/GYM-Management-System-using-PHP.zip root@192.168.1.30:/root/

```

  

### 2. Extract Files

Log in to your server and extract the application files.

```bash

unzip GYM-Management-System-using-PHP.zip

```

```bash

cd GYM-Management-System-using-PHP

```

  

### 3. Move Files to Web Directory

Move the `gym` directory to your web server's root directory (e.g., `/var/www/html/`) and set the correct permissions.

```bash

cp -vr gym /var/www/html/

```

```bash

cd /var/www/html/

```

```bash

chown -Rv www-data:www-data gym

```

  

---

  

## Database Configuration

  

### 1. Create the Database

Log in to your MySQL or MariaDB server to create a new database.

```bash

mysql -u root -p

```

  

Inside the MySQL shell, run the following commands:

  

```sql

CREATE USER 'gym'@'localhost' IDENTIFIED BY 'your_secure_password';

```

  

```sql

GRANT ALL PRIVILEGES ON gymdb.* TO 'gym'@'localhost';

```

  

```sql

CREATE DATABASE gym;

```

  

```sql

SHOW DATABASES;

```

  

```sql

EXIT;

```

  

### 2. Import the Database Schema

Navigate to the SQL file directory and import the provided `.sql` file into your newly created database.

```bash

cd /root/GYM-Management-System-using-PHP/SQL\ File/

```

  

```bash

mysql -u root -p gymdb < gymdb.sql

```

  

---

  

## Configure Database Connection

  

You need to update the database connection details in two separate configuration files.

  

#### For the Main Application:

```bash

vim /var/www/html/gym/include/config.php

```

  

#### For the Admin Panel:

```bash

vim /var/www/html/gym/admin/config.php

```

  

### Configuration Template

Update the following lines in both `config.php` files with your database credentials.

  

```php

<?php

// DB Connection

define('DB_HOST','localhost');

define('DB_USER','root');

define('DB_PASS','Armour@1234'); // <-- Change this to your database password

define('DB_NAME','gymdb');

  

// Establish database connection

try {

    $dbh = new PDO("mysql:host=".DB_HOST.";dbname=".DB_NAME,

        DB_USER,

        DB_PASS,

        array(PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES 'utf8'")

    );

}

catch (PDOException $e) {

    exit("Error: " . $e->getMessage());

}

?>

```

  

---

  

## Access & Default Credentials

  

### Access URLs

| Panel    | URL Example                          |

| :------- | :----------------------------------- |

| Admin    | `http://192.168.1.30/gym/admin/`     |

| Employee | `http://192.168.1.30/gym/`           |

  

### Admin Panel

*   **Username:** `admin@gmail.com`

*   **Password:** `Test@12345`

  

### Employee Panel

*   **Username:** `john@test.com`

*   **Password:** `Test@123`

  

---

  

## Notes & Disclaimers

> **Important:**

> * This setup is for **testing and educational purposes only**.

> * **Never expose this application to the public internet** without proper security hardening (e.g., updating default passwords, implementing firewalls, using HTTPS).

> * Ensure that **Apache, PHP, and MySQL/MariaDB** are installed and correctly configured on your server.

  

## License & Credits

*   Project by **PHPGurukul**

*   [https://phpgurukul.com/](https://phpgurukul.com/)

  

---

---