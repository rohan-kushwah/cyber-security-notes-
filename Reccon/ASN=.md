# Autonomous System (AS / ASN / IP) Lookup
---
Autonomous Systems (AS) are large networks or groups of IP prefixes under the control of a single organization that presents a common routing policy to the internet.

## What Can You Look Up?
---
*   **ASN (Autonomous System Number):** A unique number (e.g., AS15169) assigned to each AS.
*   **AS Name/Organization:** The organization that owns the ASN.
*   **IP to ASN:** Identify the ASN responsible for a given IP address.
*   **ASN to IP Range:** View all IP ranges (CIDR blocks) announced by an ASN.

## Online Tools for AS/ASN/IP Lookup
---

| Tool                     | Description                            | URL                                      |
| :----------------------- | :------------------------------------- | :--------------------------------------- |
| bgp.he.net               | Detailed ASN info, prefixes, peers     | https://bgp.he.net/                      |
| IPinfo                   | IP → ASN, org, country, location       | https://ipinfo.io/                       |
| CIDR Report              | ASN → prefix list, aggregation         | https://www.cidr-report.org/             |
| RIPEstat                 | RIPE NCC IP/ASN lookup, graph, history | https://stat.ripe.net/                   |
| ViewDNS                  | ASN → organization, IP range           | https://viewdns.info/asnlookup/          |
| UltraTools               | ASN / IP Whois, owner lookup           | https://www.ultratools.com/tools/asninfo |
| Hurricane Electric Whois | Whois, prefixes, peerings              | https://bgp.he.net/                      |
| Whois (Linux command)    | Local ASN / IP lookup                  | `whois <IP or ASN>`                      |
| Team Cymru               | Command-line ASN lookup                | See below                                |

## Command Line ASN/IP Lookup
---
### 1. Using whois

Displays ASN, organization, and contact details.

```
whois 8.8.8.8
```

### 2. Using Team Cymru for IP -> ASN

Install `whois` if not available:
```
sudo apt install whois
```

**Single IP lookup:**
```
whois -h whois.cymru.com " -v 8.8.8.8"
```

**Bulk lookup (replace with your IP list):**
```
whois -h whois.cymru.com " -v <your_ip_list>"
```

**WHOis lookup ANS**
```bash
whois AS15169
```
## Sample Output for 8.8.8.8
---
```
 AS   | IP        | BGP Prefix   | CC | Registry | Allocated  | AS Name
15169 | 8.8.8.8   | 8.8.8.0/24   | US | arin     | 1992-12-01 | GOOGLE - Google LLC, US
```

---
---