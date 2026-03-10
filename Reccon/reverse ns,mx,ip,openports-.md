# Reverse IP Domain Lookup
---

>THIS IS NOT JUST IP TO DOMAIN LOOKUP, BUT IT IS GETTING ALL DOMAINS RUNNING FROM A SPECIFIC IP ADDRESS.

Reverse IP lookup reveals which **domains** are hosted on a specific **IP address**. This is useful in OSINT, pentesting, and infrastructure mapping.

## Use Cases
*   Discover other domains sharing the same web server
*   Identify shared hosting or misconfigured virtual hosts
*   Uncover staging, dev, or internal sites
*   Pivot to related targets in recon or bug bounty

## Online Tools
---
### [YouGetSignal – Reverse IP Lookup](https://www.yougetsignal.com/tools/web-sites-on-web-server/)
*   Free and simple tool
*   Lists other domains hosted on the same IP
*   Useful for shared/VPS hosting scenarios

### [SecurityTrails](https://securitytrails.com/)
*   Premium-level passive DNS and historical data
*   Shows domain-IP associations over time
*   Offers subdomains, WHOIS, and DNS history

## Example Use
---
1.  **Find target IP:**
```bash
dig +short example.com
```

2.  **Lookup IP on** [YouGetSignal](https://www.yougetsignal.com/tools/web-sites-on-web-server/) or [SecurityTrails](https://securitytrails.com/).

---
## Notes
---
*   Shared hosting can return **hundreds of domains** per IP
*   Dedicated IPs may show only one domain
*   Cloud/CDN providers (like Cloudflare) will show many unrelated domains

## Defensive Tip
---
*   Use dedicated IPs or properly configure virtual hosts
*   Consider CDN/WAF services to mask real IP

---
---

# Reverse Mail Server Lookup
---

Reverse Mail Server Lookup is used to identify **domains** that share the same **mail (MX) server**. This can be useful in infrastructure mapping, OSINT, and discovering related domains.

## What It Does

Given a domain's **MX record**, reverse lookup tools reveal other domains using the **same mail server** (e.g., same IP or hostname).

## Use Cases
*   Discover organizations hosted on the same email infrastructure
*   Detect multi-tenant or shared email hosting providers
*   Identify email server misconfigurations or exposure
*   Expand target surface in reconnaissance

## How to Find MX Records
---
### Using `dig`:
```bash
dig armourinfosec.com mx
```

### Using `nslookup`:
```bash
nslookup -type=mx armourinfosec.com
```

## Online Tool
---
*   **[ViewDNS - Reverse MX Lookup](https://viewdns.info/reversemx/)**
    *   Enter an MX hostname or IP
    *   Returns all domains using that MX server
    *   Useful for shared email servers (e.g., `mail.l.google.com`, `mail.protection.outlook.com`)

## Example
---
If `armourinfosec.com` uses:
```
mx1.hostingprovider.com (IP: 192.0.2.10)
```

A reverse MX lookup might reveal:
*   company1.com
*   marketingagency.org
*   dev-clientsite.net

## Defense Tips
---
*   Use private or dedicated mail infrastructure
*   Mask internal MX records using email relays or gateways
*   Restrict unnecessary domain exposure via DNS


---```\


# Reverse Name Server (NS) Lookup

## What Is It?

Reverse NS Lookup helps identify all domains using a specific Name Server (NS). It's a power move in:

• Reconnaissance  
• OSINT investigations  
• Infrastructure mapping  
• Bug bounty pivoting

## What It Can Reveal

• Other domains hosted with the same provider  
• Hidden sub-brands, dev/staging environments  
• Related assets tied to the same DNS infrastructure  
• Pivot paths in an organization's digital footprint

## How to Find NS Records

### Using `dig`

```
dig armourinfosec.com ns
```

### Using `nslookup`

```
nslookup -type=ns armourinfosec.com
```

### Example Output

```
ns1.hostingprovider.com
ns2.hostingprovider.com
```

## Online Reverse NS Lookup Tools

### [ViewDNS Reverse NS Lookup](https://viewdns.info/reversens/)

• Input a name server hostname (e.g., `ns1.hostingprovider.com`)  
• Get a list of all domains using that NS  
• Great for shared hosting recon or mapping digital infrastructure

### [SecurityTrails](https://securitytrails.com/)

• Powerful passive DNS database  
• Shows historical domain – NS relationships  
• Also includes subdomains, WHOIS, DNS, and tech stack info

### [DNSDumpster](https://dnsdumpster.com/)

• Free passive DNS tool for domain mapping  
• Can indirectly reveal NS and A/CNAME relationships

## Example Use Case

If `armourinfosec.com` uses:

```
NS: ns1.examplehost.com
```

A reverse NS lookup may reveal domains like:

• `site1.com`  
• `companybeta.org`  
• `dev-clientproject.net`

You've just uncovered sibling infrastructure or hidden environments!

## Blue Team Defense Tips

• Use private or dedicated name servers for sensitive domains

• Avoid generic/shared DNS for critical infrastructure

• Use DNS providers that support obfuscation (e.g., [Cloudflare](https://www.cloudflare.com/))

• Monitor reverse DNS lookups and NS leaks via tools like:
  - [SecurityTrails Monitor](https://securitytrails.com/)
  - [Spyse](https://spyse.com/)
  - [GreyNoise](https://www.greynoise.io/)

----
----


# Open Ports Check
---

An **open port check** allows you to test if specific **TCP ports** on a target IP or domain are accessible from the internet. This is useful for reconnaissance, troubleshooting, and verifying firewall rules.

## Use Cases
*   Identify publicly accessible services (SSH, RDP, FTP, HTTP, etc.)
*   Check firewall or NAT configurations
*   Discover potentially vulnerable or misconfigured ports
*   Validate port forwarding and external access

## Online Port Scanning Tools
---
*   **[ViewDNS Port Scan](https://viewdns.info/portscan/)**
    *   Basic TCP port scan from an external location
    *   Useful for testing exposure of services

*   **[YouGetSignal Open Ports Tool](https://www.yougetsignal.com/tools/open-ports/)**
    *   Fast and simple port scanner
    *   Supports checking a single port (e.g., 22, 80, 443)
    *   Good for quick exposure tests

*   **[DNSChecker Port Scanner](https://dnschecker.org/port-scanner.php)**
    *   Scans multiple TCP ports across multiple servers
    *   Useful for verifying port accessibility globally

*   **[PortChecker.co](https://portchecker.co/)**
    *   Check if a port is open on your IP (client-side)
    *   Good for testing if a home or office router forwards a port properly

## Common Ports to Test
---

| Port | Service |
| :--- | :--- |
| 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 3306 | MySQL |
| 3389 | RDP (Windows) |

## Defense Tips
---
*   Close all unnecessary ports at firewall level
*   Use **port-knocking** or **VPN** for sensitive remote services
*   Regularly scan your public IPs for accidental exposure
*   Monitor logs for unauthorized access attempts

## CLI Alternative
---
```bash
nmap -Pn -p 1-1000 example.com
```

---
---