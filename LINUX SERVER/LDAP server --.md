### 📘 `ldap-server-with-dns-centos9.md`

markdown
# 📘 Full LDAP + DNS (BIND) Server Setup on CentOS 9

This guide walks through setting up a fully functional **LDAP server** on CentOS 9, including **DNS configuration using BIND** so clients can resolve the LDAP server by name (e.g., `ldap.example.com`).

---

## 🧰 Prerequisites

- CentOS Stream 9 or RHEL 9
- Root privileges
- Hostname configured (e.g., `ldap.example.com`)
- SELinux/firewalld awareness

---

## 🌐 Step 1: Configure Hostname

```bash
sudo hostnamectl set-hostname ldap.example.com


Add to `/etc/hosts` if needed:

bash

CopyEdit

`127.0.0.1   ldap.example.com ldap localhost`

---

## 🧭 Step 2: Install and Configure BIND (DNS Server)

### Install BIND and tools:

bash

CopyEdit

`sudo dnf install -y bind bind-utils`

### Configure `/etc/named.conf`

Uncomment and update the `listen-on` and `allow-query`:

conf

CopyEdit

`listen-on port 53 { any; }; allow-query     { any; };`

Allow zone transfers if needed (optional).

### Add DNS Zone Files

Create forward zone for `example.com`:

bash

CopyEdit

`sudo tee /var/named/example.com.zone > /dev/null <<EOF \$TTL 86400 @   IN  SOA ldap.example.com. root.example.com. (         2025051201 ; Serial         3600       ; Refresh         1800       ; Retry         604800     ; Expire         86400 )    ; Minimum  @       IN  NS    ldap.example.com. ldap    IN  A     192.168.1.10 EOF`

Update `/etc/named.conf` to include:

conf

CopyEdit

`zone "example.com" IN {     type master;     file "example.com.zone"; };`

Set permissions:

bash

CopyEdit

`sudo chown root:named /var/named/example.com.zone`

### Start and enable BIND:

bash

CopyEdit

`sudo systemctl enable --now named`

### Test DNS:

bash

CopyEdit

`dig @localhost ldap.example.com`

---

## 🔐 Step 3: Install OpenLDAP Server

bash

CopyEdit

`sudo dnf install -y openldap-servers openldap-clients sudo systemctl enable --now slapd`

---

## 🗝 Step 4: Set LDAP Admin Password

bash

CopyEdit

`slappasswd`

Save the `{SSHA}...` output.

Create `chrootpw.ldif`:

ldif

CopyEdit

`dn: olcDatabase={2}mdb,cn=config changetype: modify replace: olcRootPW olcRootPW: {SSHA}YOUR_HASH_HERE`

Apply it:

bash

CopyEdit

`sudo ldapmodify -Y EXTERNAL -H ldapi:/// -f chrootpw.ldif`

---

## 🏢 Step 5: Configure LDAP Domain

Replace `example.com` with your domain.

Create `basedomain.ldif`:

ldif

CopyEdit

`dn: dc=example,dc=com objectClass: top objectClass: dcObject objectclass: organization o: Example Org dc: example  dn: cn=Manager,dc=example,dc=com objectClass: organizationalRole cn: Manager description: Directory Manager`

Add it:

bash

CopyEdit

`ldapadd -x -D cn=Manager,dc=example,dc=com -W -f basedomain.ldif`

---

## 🧬 Step 6: Add Base OUs

ldif

CopyEdit

`dn: ou=People,dc=example,dc=com objectClass: organizationalUnit ou: People  dn: ou=Groups,dc=example,dc=com objectClass: organizationalUnit ou: Groups`

Apply:

bash

CopyEdit

`ldapadd -x -D cn=Manager,dc=example,dc=com -W -f baseou.ldif`

---

## 👤 Step 7: Add LDAP Users

ldif

CopyEdit

`dn: uid=alice,ou=People,dc=example,dc=com objectClass: inetOrgPerson uid: alice sn: Liddell givenName: Alice cn: Alice Liddell displayName: Alice userPassword: alicepass mail: alice@example.com`

bash

CopyEdit

`ldapadd -x -D cn=Manager,dc=example,dc=com -W -f user.ldif`

---

## 🔥 Step 8: Firewall and SELinux

bash

CopyEdit

`sudo firewall-cmd --permanent --add-service=ldap sudo firewall-cmd --permanent --add-service=dns sudo firewall-cmd --reload sudo setsebool -P allow_ldap_connect 1`

---

## 🧪 Step 9: Test LDAP and DNS

### DNS

bash

CopyEdit

`dig @localhost ldap.example.com`

### LDAP

bash

CopyEdit

`ldapsearch -x -b dc=example,dc=com`

---

## ✅ Summary

|Service|Port|Protocol|
|---|---|---|
|DNS|53|TCP/UDP|
|LDAP|389|TCP|
|LDAPS|636|TCP (optional)|

---

## 📚 References

- https://www.openldap.org/doc/admin24/
    
- [https://bind9.readthedocs.io/](https://bind9.readthedocs.io/)
    

yaml

CopyEdit

``---  Would you like this in a downloadable `.md` file or expanded to include **LDAP client setup wi``