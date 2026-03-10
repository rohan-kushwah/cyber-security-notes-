**Enable Directory Listing for a Selected Directory**

1. Create a new directory:
    


`mkdir /var/www/html/wordpress/backup`

2. Change ownership:
    


`chown -R apache:apache /var/www/html/wordpress/backup`

3. Edit the Apache configuration:
    


`vim /etc/httpd/conf/httpd.conf`


<Directory /var/www/html/wordpress/backup >
    Options Indexes
</Directory>

<Directory /var/www/html/wordpress >
    Options -Indexes
</Directory>


4. Restart Apache:
    

`systemctl restart httpd.service`


##  Allow Selected IP Addresses**

1. Edit Apache configuration:
    


`vim /etc/httpd/conf/httpd.conf`

<VirtualHost 192.168.1.35:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    DocumentRoot /var/www/html/wordpress/
    DirectoryIndex index.php

    <Directory /var/www/html/wordpress/backup >
        Options Indexes
        Order allow,deny
        Allow from 192.168.1.6 192.168.1.51
    </Directory>

    <Directory /var/www/html/wordpress >
        Options -Indexes
    </Directory>
</VirtualHost>


2. Restart Apache:
    


`systemctl restart httpd.service`

 ## Deny Selected IP Addresses 

1. Edit Apache configuration:
    


`vim /etc/httpd/conf/httpd.conf`

<VirtualHost 192.168.1.35:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    DocumentRoot /var/www/html/wordpress/
    DirectoryIndex index.php
<VirtualHost 192.168.205.143:443>
    ServerName hardia.local
    ServerAlias www.hardia.local

    DocumentRoot /var/www/html/wordpress

    <Directory /var/www/html/wordpress/backup>
        Options Indexes
        Order allow,deny
        Allow from all
        Deny from 192.168.205.239
    </Directory>

    DirectoryIndex index.php
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/ca.crt
    SSLCertificateKeyFile /etc/pki/tls/private/ca.key

    <Directory /var/www/html/wordpress>
        Options -Indexes
    </Directory>
</VirtualHost>



2. Restart Apache:
    


`systemctl restart httpd.service`


### **Secure Directory Hosting with User Authentication**

**Using `.htaccess  -configuration hold karega folder or site ki**

1. **Edit the `.htaccess` file:**
    


`vim /var/www/html/wordpress/backup/.htaccess`


![[Pasted image 20250405162655.png]]


2. **Create a password file and add users:**
    


`htpasswd -c /etc/httpd/htpasswd user1   htpasswd /etc/httpd/htpasswd user2`

3. **Edit the Apache configuration:**
    


`vim /etc/httpd/conf/httpd.conf`

 ![[Pasted image 20250405163320.png]]
 

4. **Restart Apache:**
    


`systemctl restart httpd.service`

### **User Authentication Without .htaccess**

1. **Edit the Apache configuration:**
    


`vim /etc/httpd/conf/httpd.conf`
<VirtualHost 192.168.1.35:443>
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
    DocumentRoot /var/www/html/wordpress/
    DirectoryIndex index.php

    <Directory /var/www/html/wordpress/backup>
        AuthName "Private"
        AuthType Basic
        AuthBasicProvider file
        AuthUserFile /etc/httpd/htpasswd
        Require valid-user
        Options +Indexes
    </Directory>

    <Directory /var/www/html/wordpress>
        Options -Indexes
    </Directory>
</VirtualHost>


**2. Restart Apache:**


`systemctl restart httpd.service`

 

So unless you directly visit:

arduino

CopyEdit

`https://192.168.205.143:8090/backup/`

you won’t get prompted for credentials.