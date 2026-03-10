# Domain Name to IP Address Lookup
---
When you want to find the IP address associated with a domain name, you can use various DNS query commands and online tools. Here's a summary of some commonly used commands and websites:

## Command-Line Tools
---
### 1. dig (Domain Information Groper)

Query the A (IPv4) record:
```
dig armourinfosec.com
```

Query the NS (Name Server) record:
```
dig armourinfosec.com ns
```

Query the MX (Mail Exchange) record:
```
dig armourinfosec.com mx
```

Query any record type from a specific DNS server (example for tesla.com):
```
dig tesla.com any @a1-12.akam.net
```

### 2. nslookup
 
 Basic DNS lookup for a domain:
```
nslookup armourinfosec.com
```

### 3. **Bash Command for All Records (Except ANY)**

```bash
domain="example.com"

for type in A AAAA NS SOA MX CNAME DNAME PTR SRV NAPTR TXT SPF HINFO RP DNSKEY RRSIG DS NSEC NSEC3 NSEC3PARAM CDS CDNSKEY CAA APL LOC URI HTTPS SVCB OPENPGPKEY SMIMEA SSHFP TKEY TSIG; do
    echo "=== $type ==="
    dig +short "$domain" $type
    echo
done
```

---
## Online DNS Lookup Tools
---
Here's a list of web-based tools for performing DNS lookups and checks:

| Tool & Description              | URL                                                             |
| :------------------------------ | :-------------------------------------------------------------- |
| Google Dig Tool                 | [Google Dig](https://toolbox.googleapps.com/apps/dig/)          |
| Google Public DNS Query Tool    | [Google DNS Query](https://dns.google/)                         |
| ViewDNS DNS Report              | [ViewDNS Report](https://viewdns.info/dnsreport/)               |
| ViewDNS DNS Records             | [ViewDNS Records](https://viewdns.info/dnsrecord/)              |
| UltraTools DNS Lookup           | [UltraTools Lookup](https://www.ultratools.com/tools/dnsLookup) |
| CentralOps.net DNS Lookup Tools | [CentralOps](https://centralops.net/co/)                        |
| DNS Tools (Swiss-based)         | [DNSTools.ch](https://en.dnstools.ch/dns-nameserver.html)       |

---
## Summary
---
-  `dig` and `nslookup` are great command-line tools to resolve domain names and query specific DNS records.
- Online tools like ViewDNS, Google DNS Query, UltraTools, and CentralOps offer easy access for DNS lookups if you're not on a terminal.

---
---