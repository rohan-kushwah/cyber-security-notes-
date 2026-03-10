### **Complete LDAP Server Setup Guide**

#### **Prerequisites**

- Ubuntu/Debian Linux server
    
- Root or sudo access
    
- Static IP address recommended
    
- Fully qualified domain name (FQDN) configured
    

---

### **Step 1: System Preparation**

Update the system:


`sudo apt update && sudo apt upgrade -y`

Set hostname and FQDN:



`hostname -f sudo hostnamectl set-hostname ldap-server.tanishq.com sudo nano /etc/hosts`

Add this line:



`127.0.0.1 ldap-server.tanishq.com ldap-server localhost`

---

### **Step 2: Install OpenLDAP Server**



`sudo apt install slapd ldap-utils -y`

Set a strong administrator password during setup.

---

### **Step 3: Configure Open LDAP**



`sudo dpkg-reconfigure slapd`

Prompts:

1. Omit configuration? → No
    
2. DNS domain name → tanishq.com
    
3. Organization name → Your Organization Name
    
4. Admin password → [set password]
    
5. Database backend → MDB
    
6. Remove database on purge? → No
    
7. Move old DB? → Yes
    

---

### **Step 4: Verify LDAP Installation**



`sudo systemctl status slapd sudo systemctl enable slapd ldapsearch -x -b "dc=tanishq,dc=com" ldapsearch -x -D "cn=admin,dc=tanishq,dc=com" -W -b "dc=tanishq,dc=com"`

---

### **Step 5: Create LDAP Directory Structure**

Create `/tmp/base_structure.ldif` with:

l

`dn: ou=people,dc=tanishq,dc=com objectClass: organizationalUnit ou: people  dn: ou=groups,dc=tanishq,dc=com objectClass: organizationalUnit ou: groups  dn: ou=system,dc=tanishq,dc=com objectClass: organizationalUnit ou: system`

Add structure:


`ldapadd -x -D "cn=admin,dc=tanishq,dc=com" -W -f /tmp/base_structure.ldif`

---

### **Step 6: Create User Groups**

Create `/tmp/groups.ldif`:


`dn: cn=users,ou=groups,dc=tanishq,dc=com objectClass: posixGroup cn: users gidNumber: 10000  dn: cn=admins,ou=groups,dc=tanishq,dc=com objectClass: posixGroup cn: admins gidNumber: 10001  dn: cn=developers,ou=groups,dc=tanishq,dc=com objectClass: posixGroup cn: developers gidNumber: 10002`

Add:

b

`ldapadd -x -D "cn=admin,dc=tanishq,dc=com" -W -f /tmp/groups.ldif`

---

### **Step 7: Create Test Users**

Generate password hash:


`slappasswd -s password123`

Create `/tmp/users.ldif`:



`dn: uid=tanishq,ou=people,dc=tanishq,dc=com objectClass: inetOrgPerson objectClass: posixAccount objectClass: shadowAccount uid: tanishq cn: Tanishq Kumar sn: Kumar givenName: Tanishq mail: tanishq@tanishq.com uidNumber: 10000 gidNumber: 10000 homeDirectory: /home/tanishq loginShell: /bin/bash userPassword: {SSHA}generated_hash_here  dn: uid=jay,ou=people,dc=tanishq,dc=com objectClass: inetOrgPerson objectClass: posixAccount objectClass: shadowAccount uid: jay cn: Jay Patel sn: Patel givenName: Jay mail: jay@tanishq.com uidNumber: 10001 gidNumber: 10000 homeDirectory: /home/jay loginShell: /bin/bash userPassword: {SSHA}generated_hash_here`

Add:


`ldapadd -x -D "cn=admin,dc=tanishq,dc=com" -W -f /tmp/users.ldif`

---

### **Step 8: Configure LDAP Access Controls**

Create `/tmp/acl.ldif`:



`dn: olcDatabase={1}mdb,cn=config changetype: modify replace: olcAccess olcAccess: {0}to attrs=userPassword    by self write    by anonymous auth    by dn="cn=admin,dc=tanishq,dc=com" write    by * none  olcAccess: {1}to *    by dn="cn=admin,dc=tanishq,dc=com" write    by self read    by * read`

Apply:



`sudo ldapmodify -Q -Y EXTERNAL -H ldapi:/// -f /tmp/acl.ldif`

---

### **Step 9: Enable Logging**

Create `/tmp/logging.ldif`:


`dn: cn=config changetype: modify replace: olcLogLevel olcLogLevel: stats`

Apply:


`sudo ldapmodify -Q -Y EXTERNAL -H ldapi:/// -f /tmp/logging.ldif`

For rsyslog:


`sudo tee /etc/rsyslog.d/10-slapd.conf > /dev/null << EOF local4.*    /var/log/slapd.log & stop EOF sudo systemctl restart rsyslog`

For systemd-journald:



`sudo journalctl -u slapd -f`

---

### **Step 10: Test LDAP Setup**



`ldapsearch -x -b "dc=tanishq,dc=com" ldapsearch -x -b "ou=people,dc=tanishq,dc=com" ldapsearch -x -b "ou=groups,dc=tanishq,dc=com" ldapwhoami -x -D "uid=tanishq,ou=people,dc=tanishq,dc=com" -W`

---

### **Step 11: Secure LDAP with TLS/SSL**

Generate certificates:


`sudo mkdir /etc/ldap/ssl cd /etc/ldap/ssl sudo openssl genrsa -out ldap-server.key 2048 sudo openssl req -new -x509 -key ldap-server.key -out ldap-server.crt -days 365 sudo chown openldap:openldap /etc/ldap/ssl/* sudo chmod 600 /etc/ldap/ssl/*`

Create `/tmp/tls.ldif`:



`dn: cn=config changetype: modify add: olcTLSCertificateFile olcTLSCertificateFile: /etc/ldap/ssl/ldap-server.crt - add: olcTLSCertificateKeyFile olcTLSCertificateKeyFile: /etc/ldap/ssl/ldap-server.key`

Apply:



`sudo ldapmodify -Q -Y EXTERNAL -H ldapi:/// -f /tmp/tls.ldif`

---

### **Step 12: Firewall Configuration**



`sudo ufw allow 389/tcp  # LDAP sudo ufw allow 636/tcp  # LDAPS sudo ufw enable`

---

### **Troubleshooting**

Check logs:


`sudo tail -f /var/log/slapd.log sudo journalctl -u slapd -f sudo slaptest -u sudo netstat -tlnp | grep slapd`

---

### **Important Notes**

1. Replace `tanishq.com` with your real domain
    
2. Use strong passwords
    
3. Backup regularly:
    
    bash
    
    CopyEdit
    
    `sudo slapcat > /backup/ldap-backup-$(date +%Y%m%d).ldif`
    
4. Monitor logs
    
5. Test before integrating
    

---

### **Next Steps**

- Configure LDAP clients
    
- Integrate with PostgreSQL
    
- Configure Joomla or other web apps
    
- Enable LDAP authentication for FTP/SSH
## **Complete LDAP Setup Client Guide**

### **1. Configure Client Systems for LDAP Authentication**

#### Install Required Packages:

**On Ubuntu/Debian:**



`sudo apt update sudo apt install libnss-ldap libpam-ldap ldap-utils nscd`

**On CentOS/RHEL:**


`sudo yum install nss-pam-ldapd pam_ldap openldap-clients nscd`

#### Step 1: Configure LDAP client settings

Edit `/etc/ldap/ldap.conf`:



`BASE    dc=tanishq,dc=com URI     ldap://192.168.29.38 BINDDN  cn=admin,dc=tanishq,dc=com TIMELIMIT 120 BIND_TIMELIMIT 120 IDLE_TIMELIMIT 3600`

#### Step 2: Configure NSS (Name Service Switch)

Edit `/etc/nsswitch.conf`:



`passwd:         files ldap group:          files ldap shadow:         files ldap hosts:          files dns`

#### Step 3: Configure PAM LDAP

Edit `/etc/pam_ldap.conf`:



`host 192.168.29.38 base dc=tanishq,dc=com uri ldap://192.168.29.38/ ldap_version 3 binddn cn=admin,dc=tanishq,dc=com bindpw your_admin_password pam_filter objectclass=posixAccount pam_login_attribute uid pam_member_attribute memberuid nss_base_passwd ou=people,dc=tanishq,dc=com?one nss_base_shadow ou=people,dc=tanishq,dc=com?one nss_base_group  ou=groups,dc=tanishq,dc=com?one ssl no tls_reqcert never`

#### Step 4: Configure PAM Modules

Edit these files:

- `/etc/pam.d/common-auth`
    



`auth    [success=2 default=ignore]      pam_unix.so nullok_secure auth    [success=1 default=ignore]      pam_ldap.so use_first_pass auth    requisite                       pam_deny.so auth    required                        pam_permit.so`

- `/etc/pam.d/common-account`



`account [success=1 new_authtok_reqd=done default=ignore] pam_unix.so account requisite                       pam_deny.so account required                        pam_permit.so account sufficient                      pam_ldap.so`

- `/etc/pam.d/common-session`
    



`session required pam_mkhomedir.so skel=/etc/skel umask=077`

#### Step 5: Restart services



`sudo systemctl restart nscd sudo systemctl restart ssh`

#### Step 6: Test client authentication

\

`getent passwd tanishq getent passwd jay ssh tanishq@client-ip`

---

## **2. LDAP Integration with PostgreSQL**

#### Step 1: Install PostgreSQL



`sudo apt update sudo apt install postgresql postgresql-contrib`

#### Step 2: Configure `pg_hba.conf`

Edit `/etc/postgresql/*/main/pg_hba.conf`:



`host    all    all    192.168.29.0/24    ldap ldapserver=192.168.29.38 ldapport=389 ldapbasedn="dc=tanishq,dc=com" ldapsearchattribute=uid ldapbinddn="cn=admin,dc=tanishq,dc=com" ldapbindpasswd="your_admin_password"`

#### Step 3: Create PostgreSQL users




`sudo -u postgres psql  CREATE USER tanishq; CREATE USER jay;  GRANT CONNECT ON DATABASE postgres TO tanishq; GRANT CONNECT ON DATABASE postgres TO jay;  CREATE DATABASE testdb;  GRANT ALL PRIVILEGES ON DATABASE testdb TO tanishq; GRANT ALL PRIVILEGES ON DATABASE testdb TO jay;  \q`

#### Step 4: Restart PostgreSQL


`sudo systemctl restart postgresql`

#### Step 5: Test PostgreSQL LDAP login



`psql -U tanishq -h 192.168.29.38 -d testdb psql -U jay -h 192.168.29.38 -d testdb`

---

## **3. Configure Joomla with LDAP Authentication**

#### Step 1: Install prerequisites



`sudo apt install apache2 mysql-server php php-mysql php-xml php-gd php-curl php-mbstring php-zip php-ldap libapache2-mod-php`

#### Step 2: Install Joomla



`cd /tmp wget https://downloads.joomla.org/cms/joomla4/4-4-0/Joomla_4-4-0-Stable-Full_Package.zip  sudo mkdir /var/www/joomla sudo unzip Joomla_4-4-0-Stable-Full_Package.zip -d /var/www/joomla/ sudo chown -R www-data:www-data /var/www/joomla/ sudo chmod -R 755 /var/www/joomla/`

#### Step 3: Configure Apache

Create `/etc/apache2/sites-available/joomla.conf`:



`<VirtualHost *:80>     ServerName joomla.tanishq.com     DocumentRoot /var/www/joomla     <Directory /var/www/joomla>         Options Indexes FollowSymLinks         AllowOverride All         Require all granted     </Directory>     ErrorLog ${APACHE_LOG_DIR}/joomla_error.log     CustomLog ${APACHE_LOG_DIR}/joomla_access.log combined </VirtualHost>`

Enable site:



`sudo a2ensite joomla.conf sudo a2enmod rewrite sudo systemctl restart apache2`

#### Step 4: Setup MySQL

sql



`sudo mysql -u root -p  CREATE DATABASE joomla_db; CREATE USER 'joomla_user'@'localhost' IDENTIFIED BY 'strong_password'; GRANT ALL PRIVILEGES ON joomla_db.* TO 'joomla_user'@'localhost'; FLUSH PRIVILEGES; EXIT;`

#### Step 5: Finish web setup

- Open browser: `http://joomla.tanishq.com`
    
- Complete Joomla setup using the created database credentials.
    

#### Step 6: LDAP Plugin Configuration

- Enable plugin: System → Manage → Plugins → Authentication - LDAP
    
- Configure:
    

yaml


`Host: 192.168.29.38 Port: 389 LDAP V3: Yes Base DN: dc=tanishq,dc=com Users DN: ou=people,dc=tanishq,dc=com Search String: uid=[search] Authorization Method: Bind Directly as User`

#### Step 7: Test LDAP login in Joomla

text

CopyEdit

`Username: tanishq Password: [LDAP password]  Username: jay Password: [LDAP password]`

---

## **4. Configure FTP with LDAP Authentication**

#### Step 1: Install vsftpd

bash

CopyEdit

`sudo apt install vsftpd libpam-ldapd`

#### Step 2: Configure PAM for FTP

Edit `/etc/pam.d/vsftpd`:

text

CopyEdit

`auth    required        pam_ldap.so account required        pam_ldap.so session required        pam_ldap.so`

#### Step 3: Configure vsftpd

Edit `/etc/vsftpd.conf`:

text

CopyEdit

`listen=YES anonymous_enable=NO local_enable=YES write_enable=YES pam_service_name=vsftpd chroot_local_user=YES allow_writeable_chroot=YES pasv_enable=YES pasv_min_port=10000 pasv_max_port=10100 xferlog_enable=YES log_ftp_protocol=YES`

#### Step 4: Configure `/etc/nslcd.conf`

conf

CopyEdit

`uri ldap://192.168.29.38/ base dc=tanishq,dc=com binddn cn=admin,dc=tanishq,dc=com bindpw your_admin_password  base passwd ou=people,dc=tanishq,dc=com base group ou=groups,dc=tanishq,dc=com  map passwd uid              uid map passwd homeDirectory    homeDirectory map passwd loginShell       loginShell`

#### Step 5: Create user home directories

bash

CopyEdit

`sudo mkdir -p /home/tanishq sudo chown tanishq:users /home/tanishq`

#### Step 6: Open firewall ports

bash

CopyEdit

`sudo ufw allow 21/tcp sudo ufw allow 10000:10100/tcp`

#### Step 7: Restart services

bash

CopyEdit

`sudo systemctl restart nslcd sudo systemctl restart vsftpd sudo systemctl enable vsftpd`

#### Step 8: Test FTP login

bash

CopyEdit

`ftp 192.168.29.38 Username: tanishq Password: [LDAP password]`

---

## **Troubleshooting**

- **LDAP test:**
    

bash

CopyEdit

`ldapsearch -x -H ldap://192.168.29.38 -b "dc=tanishq,dc=com"`

- **Logs:**
    

bash

CopyEdit

`sudo tail -f /var/log/auth.log sudo journalctl -u nscd -f`

- **Test PostgreSQL connection:**
    

bash

CopyEdit

`psql -U tanishq -h 192.168.29.38 -d testdb -v ON_ERROR_STOP=1`

- **PHP LDAP module:**
    

bash

CopyEdit

`php -m | grep ldap`

---

## **Security Recommendations**

1. Use TLS/SSL for LDAP in production.
    
2. Apply firewall rules.
    
3. Use strong passwords.
    
4. Regular LDAP backups.
    
5. Monitor logs.
    
6. Use account lockout policies.