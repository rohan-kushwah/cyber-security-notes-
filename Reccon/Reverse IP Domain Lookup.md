
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