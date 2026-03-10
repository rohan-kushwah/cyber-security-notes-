# DVWA Lab Setup

---

## 1. Download and Transfer DVWA

  

Download the DVWA release from GitHub:

https://github.com/digininja/DVWA/releases/tag/2.2.2

  

Transfer the zip file to the server using `scp`:

  

```bash

$ scp Downloads/DVWA-2.2.2.zip root@192.168.1.32:/root/

```

  

**Note:** Install PHP and MySQL if not already installed.

## 2. Install DVWA

  

SSH into the server and unzip the DVWA package:

  

```bash

unzip DVWA-2.2.2.zip

```

  

Move the extracted DVWA folder to the web directory:

  

```bash

mv -v DVWA-2.2.2 /var/www/html/dvwa

```

  

Navigate to the web directory:

  

```bash

cd /var/www/html

```

  

Change ownership of the DVWA directory to the web server user ( `www-data` ):

  

```bash

chown -Rv www-data:www-data /var/www/html/dvwa

```

  

## 3. Configure DVWA

---

  

Navigate to the configuration directory:

  

```bash

cd dvwa/config

```

  

Copy the default configuration file:

  

```bash

cp -v config.inc.php.dist config.inc.php

```

  

Change ownership of the configuration file to `www-data`:

  

```bash

chown -Rv www-data:www-data config.inc.php

```

  

## 4. Create dvwa database and user

  

Login to MySQL as root:

```bash

sudo mysql -u root -p

```

  

Create the `dvwa` database:

```sql

CREATE DATABASE dvwa;

```

  

Create the user `dvwa` with password `dvwapass`:

```sql

CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'dvwapass';

```

  

Grant all privileges on the `dvwa` database to the user:

```sql

GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';

```

  

Apply the privilege changes:

```sql

FLUSH PRIVILEGES;

```

  

(Optional) Verify grants:

```sql

SHOW GRANTS FOR 'dvwa'@'localhost';

```

  

Edit the `config.inc.php` file:

```bash

vim config.inc.php

```

And set the database credentials:

```php

$_DVWA = array();

$_DVWA[ 'db_server' ]   = '127.0.0.1';

$_DVWA[ 'db_database' ] = 'dvwa';

$_DVWA[ 'db_user' ]     = 'dvwa';

$_DVWA[ 'db_password' ] = 'dvwapass';

$_DVWA[ 'db_port' ]     = '3306';

$_DVWA[ 'default_security_level' ] = 'low';

$_DVWA[ 'disable_authentication' ] = false;

```

  

## 5. Configure PHP for DVWA

---

Edit the PHP configuration file to enable certain options:

  

```bash

vim /etc/php/8.2/apache2/php.ini

```

  

Add/modify the following settings in `php.ini`:

  

```ini

allow_url_include = on

allow_url_fopen = on

display_errors = on

display_startup_errors = on

```

  

## 6. Restart Apache

---

Restart the Apache service to apply changes:

  

```bash

systemctl restart apache2.service

```

  

## 7. Access DVWA

---

Open your browser and navigate to:

  

```http

http://192.168.1.31/dvwa/

```

  

On the setup page

it will run:

```http

http://192.168.1.31/dvwa/setup.php

```

Review all checks displayed on the screen.

Resolve any issues indicated (items marked in red) before proceeding.

 Once all requirements are met,

 click the **Create / Reset Database** button at the bottom of the page.

  

## 8. Login

---

After the database is created, you will be redirected to the login page. You can now log in with the default credentials:

  

Username: `admin`  

Password: `password`

  

---

---