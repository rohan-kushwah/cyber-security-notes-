# Binding-with-Domain-Name

## 🔵 Binding with Domain Names in Apache

Apache can bind to specific **domain names** using the `ServerName` and `ServerAlias` directives in Virtual Hosts. This allows you to host multiple websites on the same IP address, each accessible by a unique domain name.

### 1. Configure Hostnames (DNS or /etc/hosts)

To simulate DNS resolution, define the domain names in the `/etc/hosts` file:

#### Edit `/etc/hosts`:

sh

CopyEdit

`vim /etc/hosts`

Add the following entries:

sh

CopyEdit

`127.0.0.1   localhost ns1 ns1.armour.local   ::1         localhost localhost6 ns1 ns1.armour.local   192.168.1.34 ns1 ns1.armour.local www.armour.local ai.local www.ai.local infosec.local www.infosec.local`  

This will resolve the domain names to the local IP (`192.168.1.31`).


## 🌐 2. Test DNS Resolution

Use `dig` to confirm that the domain names resolve to the correct IP:

sh

CopyEdit

`dig www.armour.local +short`

### Example output:

sh

CopyEdit

`192.168.1.31`

### Repeat for other domains:

sh

CopyEdit

`dig www.infosec.local +short`

sh

CopyEdit

`dig www.ai.local +short`\

## 🌍 3. Create Apache Virtual Host Files

Define virtual hosts for each domain. Apache will route incoming requests based on the `ServerName` and `ServerAlias`.

### 3.1. Main Virtual Host

Create a default virtual host file:


`vim /etc/httpd/conf/httpd.conf`

#### Example:


`<VirtualHost 192.168.1.34:80>     DocumentRoot /var/www/html/armour/     DirectoryIndex index.html </VirtualHost>`

### 3.2. Virtual Host for `armour.local`

Create a virtual host file for `armour.local`:
### 3.1. Main Virtual Host

Create a default virtual host file:


`vim /etc/httpd/conf/httpd.conf`

#### Example:
<VirtualHost 192.168.1.34:80>
    DocumentRoot /var/www/html/armour/
    DirectoryIndex index.html
</VirtualHost>

### 3.2. Virtual Host for `armour.local`

Create a virtual host file for `armour.local`:


`vim /etc/httpd/conf.d/armour.conf`

<VirtualHost 192.168.1.34:80>
    ServerName armour.local
    ServerAlias www.armour.local
    DocumentRoot /var/www/html/site1/
</VirtualHost>


### 3.3. Virtual Host for `infosec.local`

Create a virtual host file for `infosec.local`:


`vim /etc/httpd/conf.d/infosec.conf`

#### Example:

<VirtualHost 192.168.1.34:80>
    ServerName infosec.local
    ServerAlias www.infosec.local
    DocumentRoot /var/www/html/site2/
    DirectoryIndex index.html
</VirtualHost>


### 3.4. Virtual Host for `ai.local`

Create a virtual host file for `ai.local`:

sh

CopyEdit

`vim /etc/httpd/conf.d/ai.conf`

#### Example:
<VirtualHost 192.168.1.34:80>
    ServerName ai.local
    ServerAlias www.ai.local
    DocumentRoot /var/www/html/site3/
    DirectoryIndex index.html
</VirtualHost>


## 4. Binding Domains with Different Ports

You can also bind the same domains to different ports using `Listen`.
same ip ko alg alg port pe bind kar do 
aur wo ports main configuration file me listen me dal do
the allow in firewaal
httpd -t for synatx
restart apache
netstat
then ab jis user me browse karo usme dns me server ka ip 
then browse
### 4.1. Bind `armour.local` on Port 8080

Edit Apache config:


`vim /etc/httpd/conf.d/armour-8080.conf`

#### Example:
Listen 8080

<VirtualHost 192.168.1.31:8080>
    ServerName armour.local
    ServerAlias www.armour.local
    DocumentRoot /var/www/html/site2/
    DirectoryIndex index.html
</VirtualHost>
 ''

main configuration file me bhi entry kar sakte hai
![[Pasted image 20250328182120.png]]
like that