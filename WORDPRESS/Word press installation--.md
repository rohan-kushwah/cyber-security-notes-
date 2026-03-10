 WordPress is a **popular content management system (CMS)** used to create and manage websites. It is open-source, free, and highly customizable, making it a favorite choice for bloggers, businesses, and developers.

### **Key Features of WordPress:**

✅ **User-Friendly:** No coding required; easy-to-use interface.  
✅ **Customizable:** Thousands of themes and plugins for added functionality.  
✅ **SEO-Friendly:** Built-in features and plugins for better search engine rankings.  
✅ **Secure & Scalable:** Regular updates and security plugins available.  
✅ **Community Support:** Large developer and user community for help and guidance.

### **Common Uses of WordPress:**

- **Blogs & News Websites** 📰
    
- **Business Websites** 🏢
    
- **E-Commerce (WooCommerce)** 🛒
    
- **Portfolios & Personal Websites** 🎨
    
- **Online Learning (LMS)** 📚

### **WordPress Installation and Configuration**

WordPress is a popular content management system (CMS) used to create and manage websites.

#### **Step 1: Download and Extract WordPress**

Download the latest version of WordPress from the official website:


`wget https://wordpress.org/latest.zip`

Extract the downloaded package:


`unzip latest.zip`

Move WordPress files to the Apache web directory:


`cp -vr wordpress/ /var/www/html/`

Navigate to the web root directory:


`cd /var/www/html/`

Set the correct ownership to allow Apache to manage WordPress files:


`chown -Rv apache:apache wordpress/`

Navigate to the WordPress directory:


`cd wordpress/`

### **Step 2: Configure Apache for WordPress**

Open the Apache configuration file for editing:



`vim /etc/httpd/conf/httpd.conf`

Restart Apache to apply changes:


`systemctl restart httpd.service`

Restart firewall services if necessary:


`systemctl restart iptables.service`

Restart DNS service if applicable:


`systemctl restart named.service`

Test domain resolution to ensure WordPress is accessible:


`dig armour.local`


![[Pasted image 20250404152121.png]]
change karna hai document root me /var/html/wordpress



phpmyadmin browse karke  jao new database banao phir wordpress browse karke usme dal do
admin login ke liye wo -admin
username password admin
# directory browsing--

IN LINUX DIRECTORY BROWWSING BY DEFAULT ENABLE HOTI HAI

## 🗂️ Directory-Listing

### 🔵 Directory Listing

---

### ✅ Change Directory and Create an Empty File

bash

CopyEdit

`cd /var/www/html/wordpress/wp-content/uploads touch index.php`

---

### ❌ Disable Directory Listing for All Directories in Apache

1. **Edit the Apache configuration file:**
    


`vim /etc/httpd/conf/httpd.conf`


jake us site ki binding me ye  line add kar do
<Directory /var/www/html/phpmyadmin>
        options -Indexes
        
        </Directory>

2. **Restart the Apache service:**
    


`systemctl restart httpd.service`

3. **Edit the specific site configuration file:**
    


`vim /etc/httpd/conf.d/wp-site.conf`

4. **Restart Apache again:**
    


`systemctl restart httpd.service`

indexing disable hai par file puri location hai toh browse kar sakte hai


