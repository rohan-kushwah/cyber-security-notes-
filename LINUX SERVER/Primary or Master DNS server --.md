
A **Primary DNS Server** is the main authoritative server for a specific domain. It stores the original zone file and is responsible for answering DNS queries with authoritative responses. It allows administrators to manage domain records (such as A, MX, and CNAME records) and updates changes directly. Secondary DNS servers retrieve copies from the primary server for redundancy and load balancing.



### **Authoritative vs. Non-Authoritative DNS Server**

|Feature|**Authoritative DNS Server**|**Non-Authoritative DNS Server**|
|---|---|---|
|**Definition**|Stores and manages DNS records for a domain.|Retrieves DNS records from other servers and caches them.|
|**Data Source**|Directly from zone files (original source).|Cached data from authoritative servers.|
|**Response Type**|Provides official, authoritative responses.|Provides non-authoritative, cached responses.|
|**Example Servers**|Primary and Secondary DNS Servers.|Recursive resolvers (e.g., ISP DNS, Google DNS 8.8.8.8).|
|**Use Case**|Manages domain name records.|Helps users resolve domain names faster.|

### **Example:**

- **Authoritative DNS:** If you ask "What is the IP of example.com?" the authoritative DNS server for example.com gives the official answer.
    
- **Non-Authoritative DNS:** If you ask your ISP's DNS server, it checks its cache. If it doesn’t have the record, it queries an authoritative server for you.
    

4o

##### You said:

types of authorative and non authorative dns serve

###### ChatGPT said:

### **Types of Authoritative and Non-Authoritative DNS Servers**

#### **1. Authoritative DNS Servers**

These servers store official domain records and provide authoritative responses.

✅ **Types:**

1. **Primary (Master) DNS Server**
    
    - Holds the original zone file and is responsible for updates.
        
    - Example: `ns1.example.com` (manages example.com).
        
2. **Secondary (Slave) DNS Server**
    
    - Copies zone data from the primary server for redundancy.
        
    - Cannot modify records; only provides authoritative responses.
        
3. **Root DNS Servers**
    
    - Top-level DNS servers that direct queries to the correct TLD (e.g., `.com`, `.org`).
        
    - Example: 13 global root servers (`a.root-servers.net`, etc.).
        
4. **TLD (Top-Level Domain) DNS Servers**
    
    - Handle specific domains like `.com`, `.net`, `.org`.
        
    - Example: Verisign manages `.com` and `.net` TLDs.
        
5. **Authoritative Name Servers for a Domain (Hosting Provider DNS)**
    
    - Maintained by domain registrars or hosting providers.
        
    - Example: GoDaddy, Cloudflare, Google Cloud DNS.
        

---

#### **2. Non-Authoritative DNS Servers**

These servers do not store original records but resolve queries by caching or forwarding them.

✅ **Types:**

1. **Recursive DNS Resolver**
    
    - Queries multiple DNS servers to resolve domain names for users.
        
    - Example: ISP DNS (Comcast, AT&Tj), Public DNS (Google 8.8.8.8, Cloudflare 1.1.1.1).
        
2. **Caching DNS Server**
    
    - Temporarily stores (caches) DNS responses to speed up future queries.
        
    - Example: Your local router’s DNS cache.
        
3. **Forwarding DNS Server**
    
    - Forwards queries to another DNS server instead of resolving them itself.
        
    - Often used by businesses or ISPs to control DNS traffic.
        

# practical portion---


## 🚀 **Forward Zone Configuration**

### ✅ **Set Hostname**

Set the hostname for the DNS server:

bash

CopyEdit

`vim /etc/hostname`

**Example content:**

lua

CopyEdit

`ns1.armour.local`

---

### ✅ **Edit Hosts File**

Add the hostname mapping in **/etc/hosts**:

bash

CopyEdit

`vim /etc/hosts`

**Example content:**

lua

CopyEdit

`127.0.0.1   localhost ns1 ns1.armour.local  
::1         localhost localhost6 ns1 ns1.armour.local   
192.168.1.31 ns1 ns1.armour.local`
add only last line
# edit in configuration file- 
 ```

 vim /etc/named.conf or 
 vim "/etc/named.rfc1912.zones"
```



forwarder ho toh hata dena
kyunki hume khud answer dena hai

VIM /ETC/NAMED.CONF--

// Forward Zone for example.com
zone "example.com" IN {
    type master;
    file "/var/named/example.com.zone";
    allow-update { none; };
};

// Reverse Zone for 192.168.1.0/24
zone "1.168.192.in-addr.arpa" IN {
    type master;
    file "/var/named/1.168.192.zone";
    allow-update { none; };
};

 vim "/etc/named.rfc1912.zones"--

 ![[Pasted image 20250324003507.png]]
 



### ✅ Create Forward Zone File

Create the forward zone file **/var/named/example.com.zone** 


`vim /var/named/example.com.zone`
SOA CHAIYE FILE ME TOH
cp -v named.localhost ko jo file ka nam chaiye usme

#### Example content:

$TTL 86400
@   IN  SOA ns1.example.com. admin.example.com. 
                 (2025031101 ; Serial   tarikh de do
        3600       ; Refresh
        1800       ; Retry
        604800     ; Expire
        86400 )    ; Minimum TTL
@   IN  NS  ns1.example.com.
ns1 IN  A   192.168.1.50
www IN  A   192.168.1.100
ns1          IN      NS      ns1.armour.local.
ns1          IN      A       192.168.1.31
armour.local. IN      A       192.168.1.31
www          IN      CNAME   armour.local.

router       IN      A       192.168.1.1
emp1         IN      A       192.168.1.200
emp2         IN      A       192.168.1.201

; Mail exchange
mail         IN      MX 10   mail.armour.local.
mail         IN      A       192.168.1.50

; Text record for SPF
@            IN      TXT     "v=spf1 mx a -all"

; Additional Services
ftp          IN      CNAME   armour.local.
dev          IN      A       192.168.1.100
fileserver   IN      A       192.168.1.110
db           IN      A       192.168.1.120
test         IN      A       192.168.1.130


---
@ ==== fqdn
### ✅ Create Reverse Zone File

Create the reverse zone file **/var/named/1.168.192.zone**

copy mar do cp -v forward reverse me
ab minor changes karne rahenge 
unke ptr bana do
![[Pasted image 20250324005957.png]]

`vim /var/named/1.168.192.zone`

#### Example content:
$TTL 1D
@       IN SOA  ns1.hardia.local. root.hardia.local. (
                                        20250324     ; serial
                                        120000  ; refresh
                                        1222    ; retry
                                        1W      ; expire
                                        1H )    ; minimum
@      IN  NS   ns1.hardia.local.
ns1    IN   A     192.168.180.139
hardia.local   IN  A 192.168.180.139
www  IN CNAME   hardia.local.
router IN A 192.168.180.105
emp1 IN A 192.168.180.200
emp2 IN A 192.168.180.201

; Mail exchange
mail IN MX 10 mail.hardia.local.
mail IN A 192.168.180.100

; Text record for SPF
@ IN TXT "v=spf1 mx a -all"

; Additional Services
ftp IN CNAME hardia.local.
dev IN A 192.168.180.20
fileserver IN A 192.168.180.110
db IN A 192.168.180.123
test IN A 192.168.180.130


# host digit ki hi entry karni hai



![[Pasted image 20250324005505.png]]



---

This configuration defines:  
✅ **Forward Zone:** Maps `example.com` to IPs.  
✅ **Reverse Zone:** Maps IPs to domain names.


### ✅ Set File Permissions

Ensure that the `named` user owns the zone files:
```

chown named:named /var/named/example.com.zone
chown named:named /var/named/1.168.192.zone
chmod 640 /var/named/example.com.zone
chmod 640 /var/named/1.168.192.zone

---
```

### ✅ Start and Enable BIND Service

Start and enable the BIND service:

bash

CopyEdit

`systemctl start named systemctl enable named`

---

These commands:  
✅ **Set ownership** of zone files to the `named` user.  
✅ **Set permissions** to `640` (owner can read/write, group can read, others have no access).  
✅ **Start and enable** the BIND DNS service for automatic startup. 🚀

### ✅ Test Configuration

Use `dig` to test the DNS resolution:


`dig @192.168.1.50 example.com 
dig @192.168.1.50 www.example.com  
dig -x 192.168.1.100`

---

![[Pasted image 20250324004749.png]]
1 - main file ko  check karega


![[Pasted image 20250324004952.png]]
### ✅ Firewall Configuration

Allow DNS traffic through the firewall:

#### **UFW (Uncomplicated Firewall):**


`ufw allow 53/tcp ufw allow 53/udp`

#### **Firewalld:**

`firewall-cmd --add-port=53/tcp --permanent 
firewall-cmd --add-port=53/udp --permanent 
firewall-cmd --reload`

---

These commands:  
✅ **Test DNS resolution** using `dig`.  
✅ **Allow DNS traffic** on port **53 (TCP/UDP)** through the firewall.  
✅ **Reload firewall rules** to apply changes. 🚀

### ✅ **Explanation**

- **`zone "example.com" IN { type master; ... }`**
    
    - This sets up a **master DNS server** for the **example.com** zone.
        
    - The **`file`** parameter points to the zone file containing the DNS records.
        
- **`zone "1.168.192.in-addr.arpa" IN { type master; ... }`**
    
    - This sets up **reverse DNS resolution** for the IP range **192.168.1.0/24**.
        
- **`dig` commands** are used to verify that the **DNS server** is correctly resolving names and addresses.






# errors ---
![Uploaded image](https://files.oaiusercontent.com/file-KvyUavMuQqyBQsWwH1kdw9?se=2025-03-24T09%3A53%3A33Z&sp=r&sv=2024-08-04&sr=b&rscc=max-age%3D299%2C%20immutable%2C%20private&rscd=attachment%3B%20filename%3Dimage.png&sig=yhj4csD8Q%2B6F/4FceXy/GAbQ61zWSYTw3fSbMNAqhyk%3D)

me primary dns server ka forward zone bana rha hu toh is file me koi dikkat toh nhi hai

###### ChatGPT said:

Aapki provided configuration file BIND (Berkeley Internet Name Domain) DNS server ke liye lag rahi hai. Forward zone banane ke liye aapko ensure karna hoga ki:

1. **Forward Zone Properly Defined Hai:**
    
    - Aapki file mostly default localhost aur loopback zones ko define karti hai.
        
    - Agar aap primary DNS forward zone bana rahe hain, toh aapko apne domain ke liye ek zone add karna hoga, jaise:
        
    
    bash
    
    CopyEdit
    
    `zone "example.com" IN {     type master;     file "db.example.com";     allow-update { none; }; };`
    
2. **Zone File ka Path aur Format Sahi Hai:**
    
    - `file "named.loopspell";` jaise file references diye gaye hain. Ensure karein ki yeh files `/var/named/` (ya jo bhi aapka BIND ka zone file directory ho) mein properly configured ho.
        
3. **Reverse Lookup Zone Bhi Configure Hai:**
    
    - Reverse DNS lookup ke liye `1.0.0.127.in-addr.arpa` aur `0.in-addr.arpa` diye gaye hain, jo sahi lag rahe hain.
        
    - Agar aapke forward zone ka alag IP range hai, toh uska bhi reverse zone banana hoga.
        
4. **Syntax Errors Toh Nahi Hain:**
    
    - Agar sure hona hai ki koi error nahi hai, toh `named-checkconf` aur `named-checkzone` commands se configuration validate karein.
        
    
    bash
    
    CopyEdit
    
    `named-checkconf  # Configuration syntax check named-checkzone example.com /var/named/db.example.com  # Zone file validation`
    

Agar aap ek specific domain ka forward zone add kar rahe hain, toh uska `zone` block aur `zone file` mujhe share karein, taaki main aur validate kar sakun. 🚀

4o

##### You said:

![Uploaded image](https://files.oaiusercontent.com/file-4U2vv8k991DzajusQMGHh9?se=2025-03-24T09%3A56%3A03Z&sp=r&sv=2024-08-04&sr=b&rscc=max-age%3D299%2C%20immutable%2C%20private&rscd=attachment%3B%20filename%3Dimage.png&sig=JnRAdFzeR1OvcIU9yiOSbL5LRN45z9s0je5X/PBXCnc%3D)

upar wale aur is image ko sath me dekhe aur batai jab me dig command se query kar rha hu toh mujhe wo us particular domain ka ip kyu nhi de rha hai

###### ChatGPT said:

Aapki BIND DNS configuration aur zone file dekh kar kuch possible issues dikh rahe hain jo query resolve na hone ka reason ho sakte hain. Niche kuch troubleshooting steps diye gaye hain:

---

### **Possible Issues & Fixes:**

#### **1. named.loopspell Zone File ko Named.conf Me Include Kiya Hai Kya?**

- Aapke `named.conf` ya `named.conf.local` me yeh zone defined hona chahiye:
    
    bash
    
    CopyEdit
    
    `zone "loopspell" IN {     type master;     file "/var/named/named.loopspell";     allow-update { none; }; };`
    

🔹 **Check karein:** Yeh entry correctly added hai aur file ka path sahi hai.

---

#### **2. Serial Number Update Kiya Kya?**

- Jab bhi aap `named.loopspell` file me koi change karte hain, toh `serial` number **increment** karna zaroori hota hai:
    
    bash
    
    CopyEdit
    
    `20250324 ; serial`
    

🔹 **Fix:** Agar aapne recently changes kiye hain, toh `serial` number ko increment karke **named service restart karein**:

bash

CopyEdit

`systemctl restart named`

---

#### **3. Bind Service Running Hai Kya?**

- Yeh check karein ki named/BIND service sahi se run ho rahi hai:
    
    bash
    
    CopyEdit
    
    `systemctl status named`
    
    Agar error show ho raha hai, toh logs dekhein:
    
    bash
    
    CopyEdit
    
    `journalctl -xe | grep named`
    

---

#### **4. Query DNS Server Par Jaa Rahi Hai Kya?**

- Agar aap DNS query `dig` command se kar rahe hain, toh ensure karein ki aap **apne local DNS server** ko target kar rahe hain:
    
    bash
    
    CopyEdit
    
    `dig @192.168.180.139 hardia.local`
    
- Agar aapko `SERVFAIL` ya `NXDOMAIN` error mil raha hai, toh zone file me koi error ho sakta hai.
    

---

#### **5. Syntax Errors Toh Nahi Hain?**

BIND me syntax check karne ke liye yeh command chalayein:

bash

CopyEdit

`named-checkconf named-checkzone loopspell /var/named/named.loopspell`

Agar koi syntax issue hai, toh output me dikhega.

---

#### **6. AAAA Record Ki Formatting Theek Karo**

Aapke `AAAA` record ka format galat lag raha hai. Yeh sahi format me likhein:

bash

CopyEdit

`test IN AAAA ::1`

🔹 **Fix:** Yeh change karne ke baad `named` restart karein.

---

### **Conclusion**

Agar DNS response nahi de raha, toh yeh steps follow karein:

1. Ensure karein ki `loopspell` zone `named.conf` me properly defined ho.
    
2. `serial` number increment karke `systemctl restart named` karein.
    
3. `named-checkconf` aur `named-checkzone` se syntax errors check karein.
    
4. `dig @192.168.180.139 hardia.local` command se proper query karein.
    

Agar ab bhi problem ho toh `dig` ka output share karein, mai aur analyze kar sakta hoon. 🚀

4o

##### You said:

![Uploaded image](https://files.oaiusercontent.com/file-R8QtAWZaDq4fjDWV7ieN2z?se=2025-03-24T10%3A06%3A07Z&sp=r&sv=2024-08-04&sr=b&rscc=max-age%3D299%2C%20immutable%2C%20private&rscd=attachment%3B%20filename%3Dimage.png&sig=%2BTaebfFjHRr9CjhlKR8FDXbrGagAyW3eY81kYQv9KEg%3D)

###### ChatGPT said:

Aapke `dig` output se yeh dikh raha hai ki **NXDOMAIN** error aa raha hai, jo indicate karta hai ki `hardia.local` zone DNS server pe properly configure nahi hai. Chaliye isko troubleshoot karte hain. 🚀

---

### **Possible Issues & Fixes:**

### ✅ **1. Ensure Karein Ki Zone "hardia.local" BIND Me Define Hai**

Aapki zone file me `hardia.local` ka `A` record hai, lekin aapke `named.conf` me **zone declaration missing ho sakti hai**.

🔹 **Check karein:** `/etc/named.conf` ya `/etc/named/named.conf.local` file me yeh entry honi chahiye:

bash

CopyEdit

`zone "hardia.local" IN {     type master;     file "/var/named/named.loopspell";     allow-update { none; }; };`

👆 Yeh entry confirm karein, aur agar missing ho toh add karein.

---

### ✅ **2. `named` Service Restart Karein**

Agar aapne `named.conf` ya `zone` file update ki hai, toh **BIND restart karna zaroori hai**:

bash

CopyEdit

`systemctl restart named systemctl status named`

Agar koi error hai, toh yeh command se logs dekhein:

bash

CopyEdit

`journalctl -xe | grep named`

---

### ✅ **3. `named-checkconf` Aur `named-checkzone` Se Validation Karein**

Apni configuration aur zone file check karein:

bash

CopyEdit

`named-checkconf   # Named configuration syntax check named-checkzone hardia.local /var/named/named.loopspell  # Zone file validation`

Agar koi error aaye toh batayein.

---

### ✅ **4. Dig Query Ko Test Karein**

Agar sab kuch sahi hai, toh query phirse test karein:

bash

CopyEdit

`dig @192.168.180.139 hardia.local A`

Agar still `NXDOMAIN` aa raha hai, toh yeh bhi check karein:

bash

CopyEdit

`rndc reload rndc flush`

Yeh `named` cache ko clear karega aur naye changes ko reload karega.

---

### **Expected Output (Agar Sab Kuch Theek Hai)**

Agar sab kuch sahi se configure hai, toh `dig` output kuch aisa aayega:

bash

CopyEdit

`hardia.local.    86400    IN    A    192.168.180.139`

---

### **Conclusion**

Agar `dig` response `NXDOMAIN` show kar raha hai, toh:

1. **Ensure karein ki `hardia.local` zone `named.conf` me properly defined hai.**
    
2. **BIND service restart karein (`systemctl restart named`).**
    
3. **Syntax aur zone check karein (`named-checkconf`, `named-checkzone`).**
    
4. **DNS cache flush karein (`rndc reload && rndc flush`).**
    

In steps ko follow karein aur phir `dig` ka output share karein agar issue still persist kare. 🚀

4o