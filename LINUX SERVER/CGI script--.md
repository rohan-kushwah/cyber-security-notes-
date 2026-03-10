A **CGI script** is a program that runs on a web server and generates dynamic content for web pages. **CGI** stands for **Common Gateway Interface**, which is a standard protocol that allows web servers to execute external programs, usually to process user requests (like form submissions), and return the results to the client (browser).

### Key Points:

- **Language**: CGI scripts can be written in various programming languages like **Perl**, **Python**, **Bash**, or even **C/C++**.
    
- **Location**: They are typically stored in a special directory like `/cgi-bin/`.
    
- **Execution**: When a browser requests a CGI script, the web server runs the script and sends its output (usually HTML) back to the browser.
    

### Example (in Bash):


`#!/bin/bash echo "Content-type: text/html" echo "" echo "<html><body><h1>Hello from CGI</h1></body></html>"`

### How It Works:

1. User submits a form.
    
2. Web server runs the CGI script associated with the form's `action`.
    
3. Script processes data (e.g., save it, query a database).
    
4. Script returns output (HTML, JSON, etc.).
    

### Pros:

- Simple to implement.
    
- Language-agnostic.
    

### Cons:

- Less efficient than modern alternatives (e.g., PHP, Python frameworks, Node.js).
    
- Each request spawns a new process, which can be resource-intensive.


 
# kisi server ka load or server ki monitoring check karna hai tab cgi script use hogii==
uske liye log application banate hai toh wo to chutiyapa hai wo toh hum kuch os ki command se bhi check kar sakte hai

#### CGI Scripts (Common Gateway Interface)

CGI (Common Gateway Interface) is a standard protocol used to run external programs or scripts on a web server to generate dynamic web content. These scripts can be written in various languages like Python, Perl, PHP, or Shell, and are commonly used to deliver dynamic content based on user input.

---

### How CGI Works

- **Client Request**: A user accesses a CGI-enabled URL by clicking a link or submitting a form.
    
- **Server Execution**: The web server runs the CGI script located in the `cgi-bin` directory.
    
- **Dynamic Response**: The script processes any input and sends a dynamically generated HTML response to the client.
    

---

### Supported Technologies

CGI scripts can be written in several languages, including:

- **Perl** – Historically common for web scripting.
    
- **Python** – Readable and easy to maintain.
    
- **PHP** – Widely used for web development (though not strictly CGI).
    
- **Ruby** – Flexible and dynamic.
    
- **Shell Scripts** – Useful for small utilities and server-side tasks.


### Installation and Configuration Steps

---

#### **Install Apache and Perl CGI Module**

`yum install httpd*  yum install perl-CGI perl`

---

#### **Verify CGI Module is Enabled in Apache**


`httpd -M  httpd -M | grep cgi`

---

#### **Check SELinux Status**


`sestatus`

---

#### **Edit Apache Configuration**

Edit the main Apache config file:


`vim /etc/httpd/conf/httpd.conf`

<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options +ExecCGI
    AddHandler cgi-script .cgi .pl .py .sh
    Require all granted
</Directory>


### Creating and Deploying CGI Scripts

#### Create a Simple CGI Script (Perl)


`vim /var/www/cgi-bin/test.cgi`

#!/usr/bin/perl
print "Content-type: text/html\n\n";
print "<-h1>Server Memory Usage</-h1>";
print "<-pre>";
exec("free -h");
print "</-pre>";


**Set ownership and permissions:**

`chown apache:apache /var/www/cgi-bin/test.cgi 
 chmod 755 /var/www/cgi-bin/test.cgi`

**Restart Apache:**


`systemctl restart httpd.service`

**Access the script in your browser:**


`http://192.168.1.33/cgi-bin/test.cgi`


### Create Additional CGI Scripts

#### mem.cgi (Perl script showing memory usage)


`vim /var/www/cgi-bin/mem.cgi`

#!/bin/bash
echo
echo "<h1⠀>Server Memory Usage</h1⠀>"
echo "<**pre**>"
free -h
echo "</pre⠀>"


**Set permissions:**


`chown apache:apache /var/www/cgi-bin/mem.cgi chmod 755 /var/www/cgi-bin/mem.cgi`

---

#### ping.cgi (Shell script for ping test)
uses for  check availablity of server-

`vim /var/www/cgi-bin/ping.cgi`

#!/bin/bash
echo
echo "Ping the Server"
ping -c 4 8.8.8.8


**Set permissions:**


`chmod 755 /var/www/cgi-bin/ping.cgi chown apache:apache /var/www/cgi-bin/ping.cgi`

**Access in browser:**


`http://192.168.1.31/cgi-bin/ping.cgi`

# Here’s the `hello.cgi` snippet in text form:


`vim /var/www/cgi-bin/hello.cgi`
Here’s the `hello.cgi` snippet in text form:
#!/usr/bin/perl
print "Content-type: text/html\n\n";
print "<html><head><title>CGI Script</title></head><body>";
print "<h1>Hello, World!</h1>";
print "</body></html>";


`chmod +x /var/www/cgi-bin/hello.cgi ls -lh /var/www/cgi-bin/hello.cgi`


![[Pasted image 20250409130759.png]]

Securing the cgi-bin Directory
Restrict Access to Authenticated Users
Edit Apache config:
```
```vim /etc/httpd/conf/httpd.conf
Example secure block:
<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options ExecCGI
    AddHandler cgi-script .cgi .pl .py .sh
    AuthType Basic
    AuthName "Armour CGI"
    AuthUserFile /etc/httpd/htpasswd
    Require valid-user
</Directory>
<VirtualHost 192.168.1.43:80 >
    DocumentRoot /var/www/html/
    DirectoryIndex index.html
    <Directory /var/www/html/webdav>
        DAV On
        AuthType Basic
        AuthName "Armour - Webdav"
        AuthUserFile /etc/httpd/htpasswd
        Require valid-user
    </Directory>
</VirtualHost>
``