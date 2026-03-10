# Access Controls in Squid  


## [🔗](https://wiki.squid-cache.org/SquidFaq/SquidAcl#the-basics-how-the-parts-fit-together) The Basics: How the parts fit together

Squid’s access control scheme is relatively comprehensive and difficult for some people to understand. There are two different components: _ACL elements_, and _access lists_. An access list consists of an _allow_ or _deny_ action followed by a number of ACL elements.

When loading the configuration file Squid processes all the [acl](http://www.squid-cache.org/Doc/config/acl) lines (directives) into memory as tests which can be performed against any request transaction. Types of tests are outlined in the next section _ACL Elements_. By themselves these tests do nothing. For example; the word “Sunday” matches a day of the week, but does not indicate which day of the week you are reading this.

To process a transaction another type of line is used. As each processing action needs to take place a check in run to test what action or limitations are to occur for the transaction. The types of checks are outlined in the next section _Access Lists_ followed by details of how the checks operate.

## [🔗](https://wiki.squid-cache.org/SquidFaq/SquidAcl#acl-elements) ACL elements

> ![:information_source:](https://github.githubassets.com/images/icons/emoji/unicode/2139.png ":information_source:") The information here is current for version 3.1. See [acl](http://www.squid-cache.org/Doc/config/acl) for the latest configuration guide list of available types. |

Squid knows about the following types of ACL elements:

### [🔗](https://wiki.squid-cache.org/SquidFaq/SquidAcl#available-acl-types) Available ACL types

- **src**: source (client) IP addresses
- **dst**: destination (server) IP addresses
- **myip**: the local IP address of a client’s connection
- **arp**: Ethernet (MAC) address matching
- **srcdomain**: source (client) domain name
- **dstdomain**: destination (server) domain name
- **srcdom_regex**: source (client) regular expression pattern matching
- **dstdom_regex**: destination (server) regular expression pattern matching
- **src_as**: source (client) Autonomous System number
- **dst_as**: destination (server) Autonomous System number
- **peername**: name tag assigned to the cache_peer where request is expected to be sent.
- **time**: time of day, and day of week
- **url_regex**: URL regular expression pattern matching
- **urlpath_regex**: URL-path regular expression pattern matching, leaves out the protocol and hostname
- **port**: destination (server) port number
- **myport**: local port number that client connected to
- **myportname**: name tag assigned to the squid listening port that client connected to
- **proto**: transfer protocol (http, ftp, etc)
- **method**: HTTP request method (get, post, etc)
- **http_status**: HTTP response status (200 302 404 etc.)
- **browser**: regular expression pattern matching on the request user-agent header
- **referer_regex**: regular expression pattern matching on the request http-referer header
- **ident**: string matching on the user’s name
- **ident_regex**: regular expression pattern matching on the user’s name
- **proxy_auth**: user authentication via external processes
- **proxy_auth_regex**: regular expression pattern matching on user authentication via external processes
- **snmp_community**: SNMP community string matching
- **maxconn**: a limit on the maximum number of connections from a single client IP address
- **max_user_ip**: a limit on the maximum number of IP addresses one user can login from
- **req_mime_type**: regular expression pattern matching on the request content-type header
- **req_header**: regular expression pattern matching on a request header content
- **rep_mime_type**: regular expression pattern matching on the reply (downloaded content) content-type header. This is only usable in the http_reply_access directive, not http_access.
- **rep_header**: regular expression pattern matching on a reply header content. This is only usable in the http_reply_access directive, not http_access.
- **external**: lookup via external acl helper defined by external_acl_type
- **user_cert**: match against attributes in a user SSL certificate
- **ca_cert**: match against attributes a users issuing CA SSL certificate
- **ext_user**: match on user= field returned by external acl helper defined by external_acl_type
- **ext_user_regex**: regular expression pattern matching on user= field returned by external acl helper defined by external_acl_type

---

## 🧱 **1. Location of Squid Config File**

Main config file (default path):


`/etc/squid/squid.conf`

Edit it with a text editor:


`sudo nano /etc/squid/squid.conf`

---

## 🔐 **2. ACL Basic Syntax**


`acl <name> <type> <value> http_access allow|deny <name>`

---

## ✅ **3. Common ACL Examples**

### Allow specific IP:


`acl allowed_ip src 192.168.1.100 http_access allow allowed_ip`

### Allow a subnet:


`acl lan_users src 192.168.1.0/24 http_access allow lan_users`

### Block a website (domain):

squid


`acl blocked_sites dstdomain .facebook.com .youtube.com http_access deny blocked_sites`

### Allow only HTTP port 80:

squid

`acl safe_ports port 80 http_access allow safe_ports`

### Deny all other traffic (MUST be last):

squid


`http_access deny all`

---

## 📂 **4. Block Websites from a File**

Create a file with domains (one per line):

bash


`sudo nano /etc/squid/blocked_sites.txt`

Add entries like:

`facebook.com instagram.com`

In `squid.conf`:

squid


`acl bad_domains dstdomain "/etc/squid/blocked_sites.txt" http_access deny bad_domains`

---

## 🛠️ **5. Reload or Restart Squid**

After changing the config:

bash


`sudo squid -k reconfigure       # Apply changes without stopping Squid sudo systemctl restart squid    # Restart the Squid service`

---

## 📊 **6. Test Configuration for Errors**

bash


`sudo squid -k parse`

---

## 🔍 **7. Check Logs**

bash


`tail -f /var/log/squid/access.log tail -f /var/log/squid/cache.log`

---

Would you like a real-world scenario or lab exercise using Squid ACLs?


---

# 🧱 Squid Proxy ACL: Full Configuration Guide (Linux)

---

## 📁 **1. Location & Editing**

Squid config file:


`/etc/squid/squid.conf`

Edit with:


`sudo nano /etc/squid/squid.conf`

---

## 🛠️ **2. Basic Structure**


`# ACL Definition acl <name> <type> <values>  # Access Control http_access allow|deny <name>`

Order **matters** — first matching rule wins.

---

## ✅ **3. IP Address Control**

### Allow single IP

`acl local_machine src 192.168.1.100 http_access allow local_machine`

### Allow a subnet


`acl local_network src 192.168.1.0/24 http_access allow local_network`

### Block an IP


`acl blocked_ip src 192.168.1.200 http_access deny blocked_ip`

---

## 🌐 **4. Domain & URL Filtering**

### Block specific websites


`acl bad_sites dstdomain .facebook.com .instagram.com http_access deny bad_sites`

### Block domains using a file


`# File: /etc/squid/blocked_domains.txt .facebook.com .youtube.com`


`acl blacklist dstdomain "/etc/squid/blocked_domains.txt" http_access deny blacklist`

---

## ⏰ **5. Time-Based Access**


`acl work_hours time MTWHF 09:00-17:00 http_access allow local_network work_hours http_access deny all`

> Allow only during Monday–Friday, 9 AM to 5 PM.

---

## 🔐 **6. User Authentication (Basic Auth)**

### Install required package:


`sudo apt install apache2-utils  # Debian/Ubuntu sudo yum install httpd-tools    # RHEL/CentOS`

### Create password file:


`sudo htpasswd -c /etc/squid/passwd user1`

### Enable basic auth in squid.conf:


`auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd auth_param basic realm Squid Proxy acl authenticated_users proxy_auth REQUIRED http_access allow authenticated_users`

---

## 📂 **7. File Extension Filtering**

### Block download of .exe, .mp4, .zip:


`acl bad_files url_regex -i \.exe$ \.mp4$ \.zip$ http_access deny bad_files`

---

## 🔌 **8. Port Control**

### Only allow HTTP (80) and HTTPS (443):


`acl safe_ports port 80 acl safe_ports port 443 http_access deny !safe_ports`

---

## 🌍 **9. Allow/Block Methods (GET/POST/CONNECT)**


`acl CONNECT method CONNECT http_access deny CONNECT !SSL_ports`

---

## 🗂️ **10. Block by MIME Type**

Block multimedia:


`acl multimedia rep_mime_type -i video/x-msvideo video/mpeg audio/mpeg http_access deny multimedia`

---

## 🚫 **11. Deny All by Default**

Put at the **bottom** of your config:


`http_access deny all`

---

## 🔄 **12. Apply Changes**

### Reload without restart:


`sudo squid -k reconfigure`

### Or full restart:

d
`sudo systemctl restart squid`

---

## ✅ **13. Test Configuration**


`sudo squid -k parse`

---

## 📊 **14. Logs for Debugging**

bash

CopyEdit

`tail -f /var/log/squid/access.log tail -f /var/log/squid/cache.log`

---

## 🔒 Summary: Best Practices for Security

|Best Practice|Why it matters|
|---|---|
|Least privilege (specific ACLs)|Reduces attack surface|
|Use `http_access deny all` last|Prevents unintended access|
|Time-based rules|Control user behavior|
|Authentication|Tie access to user identity|
|Log and monitor|Detect anomalies|
|Filter downloads|Prevent malware exfiltration or leakage|

---

Would you like a ready-to-use **full `squid.conf` template** with all of these ACLs?