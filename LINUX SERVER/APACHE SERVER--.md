### **Apache Server: A Web Server Explained**

#### **What is Apache?**

The **Apache HTTP Server**, commonly known as **Apache**, is an open-source web server software that **serves web pages** to users over the internet. It is one of the most widely used web servers in the world, powering millions of websites.

#### **How Does It Work?**

- When a user enters a website address (e.g., `www.example.com`), their browser sends a request to the web server.
    
- Apache **processes this request** and **serves the requested webpage** (HTML, images, PHP files, etc.).
    
- The browser then **displays the webpage** to the user.

### **Apache Web Server Setup and Configuration**

This document outlines the installation, configuration, and management of the Apache Web Server on a Linux-based system using **yum** and **rpm** package managers. It includes essential commands, configuration steps, and troubleshooting steps.

---

### **1. Check if Apache is Installed**

Use the following command to check if Apache (**httpd**) is already installed:


`rpm -qa | grep httpd`

---

### **2. Install Apache Web Server**

#### **Install Apache using yum:**


`yum install httpd`

#### **Install all related Apache packages:**


`yum install httpd*`

### **3. Verify Apache Installation**

#### **Check package information:**


`rpm -qi httpd`

#### **List installed files:**


`rpm -ql httpd`

#### **List configuration files:**


`rpm -qc httpd`

#### **List documentation files:**


`rpm -qd httpd`

---

### **4. Apache Configuration**

#### **View Apache Configuration**

**Check the main configuration file:**


`tail /etc/httpd/conf/httpd.conf`

**Find the `conf.modules.d` directive:**


`grep conf.modules.d /etc/httpd/conf/httpd.conf`

**Find the `conf.d` directive:**


`grep conf.d /etc/httpd/conf/httpd.conf`

### **5. Manage Apache Service**

#### **Check Apache Status**

Check if the Apache service is running:


`systemctl status httpd.service`

#### **Start Apache Service**

Start the Apache service:

`systemctl start httpd.service`

#### **Enable Apache to Start on Boot**

Enable Apache to start on system boot:

`systemctl enable httpd.service`

#### **Restart Apache Service**

Restart the Apache service:


`systemctl restart httpd.service`

### **6. Network and Port Verification**

#### **Check Open Ports**

Check open ports and services:


`netstat -nltup`

Check if port 80 is open:


`netstat -nltup | grep 80`

Check if Apache is listening:


`netstat -nltup | grep httpd`


### **7. Firewall Configuration**

#### **Firewalld Configuration for Apache Web Server**

Steps to configure **firewalld** to allow Apache Web Server traffic.  
**firewalld** is a firewall management tool that dynamically manages firewall rules on Linux systems.

---

### **1. Check Firewall Status**

Check if **firewalld** is running:


`systemctl status firewalld`

---

### **2. Start and Enable Firewalld**

#### **Start Firewalld**

Start the **firewalld** service:


`systemctl start firewalld`

#### **Enable Firewalld at Boot**

Enable **firewalld** to start automatically at boot:


`systemctl enable firewalld`

### **3. List Current Firewalld Rules**

#### **List All Active Zones and Rules**

Check all active zones and their associated rules:


`firewall-cmd --list-all`

#### **List Active Services and Ports**

List all services and ports currently open:


`firewall-cmd --list-services   firewall-cmd --list-ports`

### 4. Configure Firewalld for Apache

#### **Add HTTP and HTTPS Services**

Allow HTTP (port 80) and HTTPS (port 443) traffic:


`firewall-cmd --permanent --add-service=http firewall-cmd --permanent --add-service=https`

#### **Reload Firewalld**

Reload `firewalld` to apply the changes:


`firewall-cmd --reload`

#### **Verify Services are Allowed**

Verify that HTTP and HTTPS are allowed:


`firewall-cmd --list-services`

### 5. Open Specific Ports for Apache

#### **Open Port 80 Manually**

If HTTP is not defined as a service, open port 80 manually:


`firewall-cmd --permanent --add-port=80/tcp`

#### **Open Port 443 for HTTPS**

If HTTPS is not defined as a service, open port 443 manually:


`firewall-cmd --permanent --add-port=443/tcp`

#### **Open Additional Ports (Optional)**

For example, to open port 8080 for a custom Apache configuration:


`firewall-cmd --permanent --add-port=8080/tcp`

#### **Reload Firewalld After Adding Ports**

Reload the firewall to apply changes:

`firewall-cmd --reload`


ab sare pc se by ip call honi chaiyee--


### 8. Apache Process and User Management

#### **Check Running Processes**

Check Apache processes:


`ps -aux | grep httpd`

#### **Check Apache User and Group**

Find the Apache user:


`cat /etc/passwd | grep apache`

Find the Apache group:


`cat /etc/group | grep apache`

---

### 9. Configure Website Files

#### **Create or Edit Web Files**

Navigate to the document root:


`cd /var/www/html`

Create or edit an `index.html` file:


`vim /var/www/html/index.html`

### 9. Configure Website Files

#### **Create or Edit Web Files**

Navigate to the document root:


`cd /var/www/html`

Create or edit an `index.html` file:


`vim /var/www/html/index.html`

#### **Remove Existing File**

Remove the existing file:


`rm -f index.html`

#### **Set Ownership of Web Files**

Set ownership of the website files to the Apache user:


`chown -Rv apache:apache /var/www/html/*`
### 10. Load CSS Files

#### **Download and Extract Template Files**

**Download template:**


`wget https://templatemo.com/tm-zip-files-2020/templatemo_571_hexashop.zip`

**Unzip the template:**


`unzip templatemo_571_hexashop.zip`

**Remove the zip file:**


`rm -rf templatemo_571_hexashop.zip`

**Move the extracted folder to a new directory:**


`mv -v templatemo_571_hexashop site1`

**Copy site files to the Apache document root:**


`cp -vr /root/site1/* /var/www/html`


 IN the case  CSS file not load--
 
![[Pasted image 20250327144303.png]]
 sirf bich wali line add karni hai
![[Pasted image 20250327144644.png]]
