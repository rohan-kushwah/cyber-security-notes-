### **What is a DNS Server?**

A **DNS (Domain Name System) server** is a specialized server that translates human-friendly domain names (e.g., `example.com`) into IP addresses (e.g., `192.168.1.1`) so that computers can communicate over networks, including the internet.

# DNS Server Types

#### **Non-Authoritative (Recursive) Nameserver**

Non-authoritative nameservers are responsible for resolving DNS queries by contacting other nameservers to find the answer. They do not store original DNS records but rely on other sources.

#### **Caching Nameserver**

Caching nameservers temporarily store DNS query results to improve resolution time and reduce the load on authoritative servers.

**Example command to test DNS resolution using a caching nameserver:**

sh

CopyEdit
```
`dig example.com @8.8.8.8`
```



#### **Forwarders Nameserver**

Forwarding nameservers receive DNS queries and pass them to another nameserver (usually a caching or authoritative server) for resolution.

**Example command to configure a forwarding nameserver:**

sh

CopyEdit

```
```echo "nameserver 8.8.8.8" >> /etc/resolv.conf`
```



# AUTHORATIVE-
### **Authoritative Nameserver**

Authoritative nameservers store and provide responses for DNS queries about domains they are responsible for. They hold the actual DNS records.

#### **Primary Nameserver**

The primary nameserver is the main source for DNS zone data and is responsible for updates and changes to DNS records. Example of a `named.conf` configuration for a primary nameserver:

sh

CopyEdit
```

`zone "example.com" {     type master;     file "/etc/bind/db.example.com"; };`
```



#### **Secondary Nameserver**

The secondary nameserver obtains its zone data from the primary nameserver and acts as a backup in case the primary is unavailable. Example of a `named.conf` configuration for a secondary nameserver:

sh

CopyEdit
```

`zone "example.com" {     type slave;     file "/etc/bind/db.example.com";     masters { 192.168.1.1; }; };`
```


### **DNS Client Tools Commands**

Below are the common commands used to manage and query DNS client tools:

#### **Check Installed BIND Package**

Use the following command to list installed BIND packages:

sh

CopyEdit

`rpm -qa | grep bind`

#### **Check Installed BIND Utilities**

Use this command to list installed `bind-utils` packages:

sh

CopyEdit

`rpm -qa | grep bind-utils`

#### **Install BIND Utilities**

To install BIND utilities using `yum`:

sh

CopyEdit

`yum install bind-utils`

#### **List Files Installed by bind-utils**

To list all files installed by the `bind-utils` package:

sh

CopyEdit

`rpm -ql bind-utils`

#### **List Configuration Files for bind-utils**

To list configuration files for the `bind-utils` package:

sh

CopyEdit

`rpm -qc bind-utils`

### **List Documentation Files for bind-utils**

To list documentation files for the `bind-utils` package:

sh

CopyEdit

`rpm -qd bind-utils`

### **Common DNS Client Tools Installed with bind-utils**

These are the commonly used DNS client tools included with `bind-utils`:

- `/usr/bin/delv` – DNS lookup and validation utility
- `/usr/bin/dig` – Query DNS servers for information about domains
- `/usr/bin/host` – Simple utility for performing DNS lookups
- `/usr/bin/mdig` – Multiple query version of `dig`
- `/usr/bin/nslookup` – Legacy tool to query DNS servers
- `/usr/bin/nsupdate` – Dynamic DNS update utility
# DNS RECORDS-
### **DNS Records**

DNS records define how domain names are mapped to IP addresses and other resources.

#### **A (Address Mapping) Records**

- Specifies an IPv4 address for a given host.
- **Example:**
    
    CopyEdit
    
    `example.com → 192.168.1.1`
    

#### **AAAA (IPv6 Address) Records**

- Specifies an IPv6 address for a given host.
- **Example:**
    
    cpp
    
    CopyEdit
    
    `example.com → 2001:0db8::ff00:0042:8329`
    

#### **CNAME (Canonical Name) Records**

- Specifies an alias for another domain name.
- **Example:**
    
    CopyEdit
    
    `wp.armourinfosec.com → armourinfosec.wordpress.com → 20.20.20.20`
    

#### **NS (Name Server) Records**

- Specifies an authoritative name server for a given domain.
- **Example:**
    
    CopyEdit
    
    `example.com → ns1.example.com`
### **PTR (Pointer) Records**

- Used for reverse DNS lookups (mapping IP addresses to hostnames).
- **Example:**
    
    CopyEdit
    
    `192.168.1.1 → example.com`
    

### **SOA (Start of Authority) Records**

- Specifies authoritative information about a DNS zone, including the primary nameserver and zone serial number.
- **Example:**
    
    pgsql
    
    CopyEdit
    
    `example.com. IN SOA ns1.example.com. admin.example.com. (       2025031101  ; serial       3600        ; refresh       1800        ; retry       1209600     ; expire       86400       ; minimum TTL   )`
    

### **HINFO (Host Information) Records**

- Provides information about a host’s hardware and operating system.
- **Example:**
    
    arduino
    
    CopyEdit
    
    `example.com. IN HINFO "Intel i7" "Linux"`
    

---

### **MX (Mail Exchanger) Records**

- Specifies a mail exchange server for a domain.
- **Example:**
    
    scss
    
    CopyEdit
    
    `example.com → mail.example.com (priority 10)`
    

### **TXT (Text) Records**

- Holds arbitrary text data for a domain.
- Often used for SPF (Sender Policy Framework) or DKIM (DomainKeys Identified Mail) records.
- **Example:**
    
    arduino
    
    CopyEdit
    
    `example.com. IN TXT "v=spf1 include:_spf.google.com ~all"`



# dig--
The `dig` (Domain Information Groper) command is a powerful DNS lookup utility used for querying DNS name servers. Below are the commonly used `dig` commands and their explanations:

---

### **Basic Queries**

1. **Simple DNS Lookup (A Record)**
    
    bash
    
    CopyEdit
    
    `dig example.com`
    
    - Retrieves the IPv4 address (A record) of `example.com`.
2. **Query a Specific Record Type**
    
    bash
    
    CopyEdit
    
    `dig example.com MX 
     #Mail Exchange record dig example.com AAAA 
      #IPv6 address record dig example.com TXT 
       #Text records dig example.com CNAME 
         #Canonical name record dig example.com NS  
         #Nameserver record`
    
3. **Reverse DNS Lookup (PTR Record)**
    
    bash
    
    CopyEdit
    
    `dig -x 192.168.1.1`
    
    - Finds the domain associated with an IP address.

---

### **Advanced Queries**

4. **Query a Specific DNS Server**
    
    bash
    
    CopyEdit
    
    `dig @8.8.8.8 example.com`
    
    - Queries Google’s public DNS (8.8.8.8) for `example.com`.
5. **Perform a Detailed Query**
    
    bash
    
    CopyEdit
    
    `dig +noall +answer example.com`
    
    - Shows only the answer section.
6. **Trace the DNS Resolution Path**
    
    bash
    
    CopyEdit
    
    `dig +trace example.com`
    
    - Displays each step of DNS resolution, from root servers down.
7. **Query ALL DNS Records**
    
    bash
    
    CopyEdit
    
    `dig example.com ANY`
    
    - Retrieves all available DNS records for the domain.
8. **Check SOA (Start of Authority) Record**
    
    bash
    
    CopyEdit
    
    `dig example.com SOA`
    
    - Displays the authoritative DNS details.

---

### **Customization & Debugging**

9. **Enable Short Output Mode**
    
    bash
    
    CopyEdit
    
    `dig +short example.com`
    
    - Returns only the IP address.
10. **Enable Detailed Debugging**
    
    bash
    
    CopyEdit
    
    `dig +nocmd example.com +noall +answer +stats`
    
    - Provides detailed statistics.
11. **Query with TTL Information**
    
    bash
    
    CopyEdit
    
    `dig example.com +ttlunits`
    
    - Displays TTL (Time To Live) in human-readable format.
12. **Query DNSSEC Information**
    
    bash
    
    CopyEdit
    
    `dig example.com DNSKEY`
    
    - Fetches DNS Security Extensions (DNSSEC) keys.

---

### **Batch Queries (Using a File)**

13. **Query Multiple Domains**
    
    bash
    
    CopyEdit
    
    `dig -f domains.txt`
    
    - Queries a list of domains from `domains.txt`.

---

### **Network Troubleshooting**

14. **Find the Name Server for a Domain**
    
    bash
    
    CopyEdit
    
    `dig example.com NS`
    
    - Lists authoritative name servers.
15. **Check if a DNS Resolver is Working**
    
    bash
    
    CopyEdit
    
    `dig @8.8.8.8 google.com`
    
    - Checks Google’s public DNS response.

`16`. for PTR record-
### **Shortened Reverse Lookup Output**

bash

CopyEdit

`dig -x 8.8.8.8 +short`

---

### **Reverse Lookup Using Bulk IPs**

#### **Create a File with IP Addresses**

Create a file named `ip_add_list.txt`:

bash

CopyEdit

`vim ip_add_list.txt`

Example content:

CopyEdit

`8.8.8.8 64.6.64.6 8.8.4.4 1.1.1.1 20.20.20.20`

---

### **Generate `-x` Format Using `awk`**

Convert IPs to `-x` format using `awk`:

bash

CopyEdit

`awk '$0="-x " $0' ip_add_list.txt > ip_add_list2.txt`

---

### **Generate `-x` Format Using `sed`**

Convert IPs to `-x` format using `sed`:

bash

CopyEdit

`cat ip_add_list.txt | sed -e 's/.*/-x &/' > ip_add_list3.txt`

### **Query Reverse DNS for Multiple IPs**

Perform reverse lookups using the modified file:

bash

CopyEdit

`dig -f ip_add_list2.txt +short`

---

### **Query Reverse DNS from an IP List File**

bash

CopyEdit

`dig -f ip.txt +short`

4o
# nslookup-
`nslookup` is a command-line network administration tool used to query Domain Name System (DNS) servers. Below are its commonly used commands:

### **1. Basic DNS Lookup**

bash

CopyEdit

`nslookup example.com`

_Queries the DNS for the IP address of `example.com`._

---

### **2. Reverse DNS Lookup (PTR Record)**

bash

CopyEdit

`nslookup 192.168.1.1`

_Finds the domain name associated with an IP address._

---

### **3. Query a Specific DNS Server**

bash

CopyEdit

`nslookup example.com 8.8.8.8`

_Queries `example.com` using Google's public DNS server (8.8.8.8)._

---

### **4. Interactive Mode**

Start interactive mode by simply typing:

bash

CopyEdit

`nslookup`

Then, enter specific commands like:

bash

CopyEdit

`> set type=A > example.com > set type=MX > example.com`

_Retrieves A (IPv4) or MX (Mail Exchanger) records for `example.com`._

---

### **5. Query Different Record Types**

bash

CopyEdit

`nslookup -type=A example.com    # IPv4 Address Record   nslookup -type=AAAA example.com # IPv6 Address Record   nslookup -type=MX example.com   # Mail Exchanger Record   nslookup -type=NS example.com   # Name Server Record   nslookup -type=CNAME example.com # Canonical Name Record   nslookup -type=PTR 192.168.1.1  # Reverse Lookup (PTR)   nslookup -type=TXT example.com  # Text Record`  

_Queries different DNS record types._

---

### **6. Specify a Custom Port**

bash

CopyEdit

`nslookup -port=53 example.com`

_Queries `example.com` on a custom port (default is 53)._

---

### **7. Query a Non-Recursive DNS Lookup**

bash

CopyEdit

`nslookup -norecurse example.com`

_Performs a non-recursive DNS lookup._

---

### **8. Debugging Mode**

bash

CopyEdit

`nslookup -debug example.com`

_Provides detailed output for debugging DNS queries._

---

### **9. Use a Specific Network Protocol**

bash

CopyEdit

`nslookup -vc example.com`

_Uses a virtual circuit (TCP instead of UDP) for the query._


# host -
The `host` command is a simple DNS lookup utility used to query DNS records. Below are the commonly used commands:

---

### **1. Basic DNS Lookup**

bash

CopyEdit

`host example.com`

_Queries the IP address of `example.com`._

---

### **2. Reverse DNS Lookup (PTR Record)**

bash

CopyEdit

`host 192.168.1.1`

_Finds the domain name associated with an IP address._

---

### **3. Query a Specific DNS Server**

bash

CopyEdit

`host example.com 8.8.8.8`

_Queries `example.com` using Google's public DNS server (8.8.8.8)._

---

### **4. Query Different Record Types**

bash

CopyEdit

`host -t A example.com      # IPv4 Address Record  host -t AAAA example.com   # IPv6 Address Record  host -t MX example.com     # Mail Exchanger Record  host -t NS example.com     # Name Server Record  host -t CNAME example.com  # Canonical Name Record  host -t TXT example.com    # Text Record  host -t PTR 192.168.1.1    # Reverse Lookup (PTR)  host -t SOA example.com    # Start of Authority Record`  

_Queries specific DNS record types._

---

### **5. Query All Available DNS Records**

bash

CopyEdit

`host -a example.com`

_Retrieves all available DNS records for `example.com`._

---

### **6. Use IPv6 for Queries**

bash

CopyEdit

`host -6 example.com`

_Forces the query to use IPv6 instead of IPv4._

---

### **7. Display Only the IP Address**

bash

CopyEdit

`host -t A example.com | awk '{ print $4 }'`

_Extracts and displays only the IP address from the query result._

---

### **8. Query a Non-Recursive DNS Lookup**

bash

CopyEdit

`host -r example.com`

_Performs a non-recursive query (asks only the specified DNS server, without following referrals)._

---

### **9. Verbose Mode for Debugging**

bash

CopyEdit

`host -v example.com`

_Provides detailed information about the DNS lookup._

---

### **10. Query Using TCP Instead of UDP**

bash

CopyEdit

`host -T example.com`

_Uses TCP instead of UDP for the query (useful for large DNS responses)._