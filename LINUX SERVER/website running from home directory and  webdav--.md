# **Enable-Apache-User-Home-Directories**

## **Enable Apache User Home Directories**

Apache allows individual users to host web content from their home directories using the `mod_userdir` module. This setup is useful for development, personal websites, or segregated hosting.


ab ek server se multiple website run karni hai toh client ko toh mujhe root ka password dena padega ye toh dikkat hai toh toh me sites ko user ki home directory me place kar dunga aur user ka password client ko de dunga aur yadi nhi bhi dunga toh ek user dusre user ki home directory me nhi ja sakta
phir wo secure way se apni website chala payega


---

### **1. Enable UserDir in Apache Configuration**

**Edit the `userdir.conf` File**


`vim /etc/httpd/conf.d/userdir.conf`

---

### **Sample Configuration**


<IfModule mod_userdir.c>
    UserDir enabled
    UserDir public_html
</IfModule>

<Directory "/home/*/public_html">
    AllowOverride FileInfo AuthConfig Limit Indexes
    Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
    Require method GET POST OPTIONS
</Directory>

---

### **This configuration:**

- **Enables** `public_html` inside each user’s home directory.
    
- **Allows** basic options like indexing, symbolic links, and overrides.
    
- **Limits** requests to safe HTTP methods.
## **2. Configure Home Directory for User: `armour`**

### **Create Public Web Directory**


`mkdir /home/armour/public_html`

---

### **Set Correct Permissions**


`chmod 711 /home/armour/ chown -R armour:armour /home/armour/public_html/ chmod -R 755 /home/armour/public_html/`

---

### **Note:**

`711` on the home folder ensures the Apache process can enter it,  
while `755` on the web folder allows content access.


## **3. Create VirtualHost for `armour`**

### **Edit the VirtualHost File**

`vim /etc/httpd/conf.d/armour.conf`

<VirtualHost 192.168.1.37:80>
    DocumentRoot /home/armour/public_html
    DirectoryIndex index.html
</VirtualHost>
**Explanation:** This binds the site to the server IP `192.168.1.37` and serves content from `armour`’s public directory.


## **4. Configure Home Directory for Another User: `infosec`**

### **Create Public Web Directory**


`mkdir /home/infosec/public_html`

---

### **Set Correct Permissions**


`chmod 711 /home/infosec/ chown -R infosec:infosec /home/infosec/public_html/ chmod -R 755 /home/infosec/public_html/`


## **5. Create VirtualHost for `infosec`**

### **Edit the VirtualHost File**


`vim /etc/httpd/conf.d/infosec.conf`

---

### **Example HTTP and HTTPS VirtualHosts**

<VirtualHost 192.168.1.37:80>
    ServerName infosec.com
    DocumentRoot /home/infosec/public_html/
    DirectoryIndex index.html
    ServerAlias www.infosec.com
    <Directory /home/infosec/public_html/>
        Options -Indexes
    </Directory>
</VirtualHost>
## **6. Restart Apache to Apply Changes**

`systemctl restart httpd`

---
ab public html me page ya vim karke kuch bhi dal do

## **Notes**

- Make sure **SELinux** (if enabled) is configured to allow home directory access by Apache.
    
- Ensure **mod_userdir** is loaded in Apache (enabled by default on most systems).
    
- Consider firewall rules or DNS mappings to test domain-based VirtualHosts on a local network.
    

---

Let me know if you’d like to expand this further with `.htaccess`, custom logs, or per-user authentication setups!


# webdav--

**WebDAV** (Web Distributed Authoring and Versioning) is an extension of the **HTTP** protocol that allows users to **create, edit, move, or delete files on a remote web server**—basically turning a web server into a shared drive.

### 🔧 Key Features:

- **Remote File Management** – Upload, download, rename, and delete files remotely.
    
- **Collaborative Editing** – Multiple users can work on the same files.
    
- **Directory Listings** – Like viewing folders and files in a browser or file explorer.
    
- **Locks** – Prevents overwriting by letting users "lock" files while editing.
    

### 📌 Example Use Case:

- Mounting a WebDAV folder on Windows or Linux, so it appears like a network drive:
    
    `\\webdav.example.com\files`
    
- Users can drag and drop files like they would on a USB or network share.
    

### 🧠 Think of it as:

A way to use a **web server like a cloud storage platform**—simple, lightweight, and based on HTTP.



## WebDAV with Apache

**WebDAV** (Web Distributed Authoring and Versioning) extends the HTTP protocol to allow users to collaboratively edit and manage files on a remote web server. Apache HTTP Server supports WebDAV using the `mod_dav` and `mod_dav_fs` modules.

---

### Features of WebDAV

- Remote file management via HTTP/S.
    
- Supports file creation, upload, editing, and deletion.
    
- Useful for collaborative development and web file hosting.
    
- Easily integrates with tools like SVN and Git.
    
- Clients can mount WebDAV shares as network drives or local filesystems.
    

---

## 1. Install and Verify Apache

### Check if Apache is Installed


`rpm -qa | grep httpd`

> This command checks if the Apache HTTP Server is already installed.

### Install Apache (if not installed)


`yum install httpd`

> Installs Apache and all associated packages on RHEL/CentOS.


#same protocol ke jariye site chalani hai aur usi protocol se upload bhi karni hai jab webdav use hoga--

### List Loaded Apache Modules


`httpd -M`

> Displays all currently enabled Apache modules.

---


`httpd -M | grep fs`

> Expected output should include:


`dav_fs_module (shared)`

> Indicates `mod_dav_fs` is enabled for WebDAV functionality.

 ### 2. Create and Set Up WebDAV Directory

#### **Create WebDAV Directory**


`mkdir /var/www/html/webdav`

Creates the directory for WebDAV file storage.

---

#### **Set Ownership to Apache User**


`chown apache:apache /var/www/html/webdav/`

Ensures Apache has ownership of the directory.

---

#### **Set Permissions**

`chmod 777 /var/www/html/webdav/`

> 💡 _For production environments, use `755` or `775` with tighter controls._

---
### 3. Test WebDAV Access

#### **Send OPTIONS Request to WebDAV Directory**

`curl -v -X OPTIONS http://192.168.1.33/webdav/`

Checks available HTTP methods supported by the WebDAV-enabled directory.

---

### 4. Configure WebDAV in Apache

#### **Edit Apache Main Configuration**


`vim /etc/httpd/conf/httpd.conf`

---

#### **Add WebDAV Virtual Host**


`IncludeOptional conf.d/*.conf  <VirtualHost *:80>     DocumentRoot /var/www/html/     DirectoryIndex index.html     <Directory /var/www/html/webdav>         DAV On     </Directory> </VirtualHost>`

> This enables WebDAV functionality for the `/webdav` directory on port 80.

### 3. Test WebDAV Access

**Send OPTIONS Request to WebDAV Directory**

bash

CopyEdit

`curl -v -X OPTIONS http://192.168.1.33/webdav/`

_Checks available HTTP methods supported by the WebDAV-enabled directory._

---

### 4. Configure WebDAV in Apache

**Edit Apache Main Configuration**


`vim /etc/httpd/conf/httpd.conf`

**Add WebDAV Virtual Host**

IncludeOptional conf.d/*.conf

<VirtualHost *:80>
    DocumentRoot /var/www/html/
    DirectoryIndex index.html
    <Directory /var/www/html/webdav>
        DAV On
    </Directory>
</VirtualHost>

_This enables WebDAV functionality for the `/webdav` directory on port 80._


---
### 5. Configure SELinux and Firewall

#### Restart Apache to Apply Changes


`systemctl restart httpd.service`


`systemctl restart httpd`

#### Check SELinux Status


`sestatus`

#### Disable SELinux (for testing only)


`vim /etc/sysconfig/selinux`

Set this line:


`SELINUX=disabled`

> _Recommended to use context labeling instead of disabling SELinux._


### 6. Secure WebDAV with Authentication

#### Create .htpasswd File and Users

htpasswd -c /etc/httpd/htpasswd dev
htpasswd /etc/httpd/htpasswd admin
htpasswd /etc/httpd/htpasswd root
htpasswd /etc/httpd/htpasswd user


Creates the password file and adds users for basic authentication.

#### View Password File


`cat /etc/httpd/htpasswd`

Displays encrypted passwords.

**Example output:**


`dev:$apr1$GxmFNd4f$H8a..Fu2B1SMUncBcATuG/ admin:$apr1$NmNCnOp5$LhkZl/HVjXKV7mKXpKxi4C/ ...`


### 7. Enable Authentication in Apache Configuration

**Edit Apache Configuration Again:**


`vim /etc/httpd/conf/httpd.conf`


Add Authenticated WebDAV Block:

IncludeOptional conf.d/*.conf

<VirtualHost *:80>
    DocumentRoot /var/www/html/
    DirectoryIndex index.html
    <Directory /var/www/html/webdav>
        DAV On
        AuthType Basic
        AuthName "webdav"
        AuthUserFile /etc/httpd/htpasswd
        Require valid-user
    </Directory>
</VirtualHost>


### 8. Restart Apache After Changes


`apachectl restart`


`systemctl restart httpd`
### WebDAV Client Testing

#### Basic OPTIONS Request


`curl -v -X OPTIONS http://192.168.1.40/webdav/`


`curl -i -k -X OPTIONS https://192.168.1.40/webdav/`

---

#### Scan HTTP Methods via Nmap


`nmap -v -sT -sV -A -O -p 80 --script=http-methods.nse --script-args http-methods.url-path='/webdav/' 192.168.1.40`

---

### File Upload & Deletion with cURL

#### Upload a File


`curl -X PUT -d "hi" http://192.168.1.40/webdav/1.txt`

#### Upload a PHP File (for testing)

`curl -X PUT -d "<?php phpinfo(); ?>" http://192.168.1.25/webdav/phpinfo.php`

#### Delete Uploaded File


`curl -X DELETE http://192.168.1.22/webdav/phpinfo.php`


`curl -X DELETE http://192.168.1.22/webdav/1.txt`

 curl -v -u dev:123 -X OPTIONS http://192.168.1.36/test/
curl -u dev:123 -X PUT -d "hi" http://192.168.1.36/test/3.txt
curl -u user1:123 -X DELETE http://192.168.1.22/webdav/phpinfo.php
curl -X PUT -u user1:123 -d "<?php phpinfo(); ?>" http://192.168.1.22/webdav/phpinfo.php
curl -v -u root:root -X PUT -d "hi" http://192.168.1.31/webdav/1.txt


## Using `cadaver` CLI WebDAV Client

### Install cadaver

bash

CopyEdit

`apt install cadaver     # Debian/Ubuntu yum install cadaver     # CentOS/RHEL`

---

### Connect to WebDAV Server

bash

CopyEdit

`cadaver http://192.168.1.36/webdav/`

---

### Sample cadaver Session

bash

CopyEdit

`dav:/webdav/> ? dav:/webdav/> ls dav:/webdav/> put network.png dav:/webdav/> mkdir newdir dav:/webdav/> mput *.php dav:/webdav/> delete *`

---

### Connect to Another Location

bash

CopyEdit

`cadaver http://192.168.1.36/test/`