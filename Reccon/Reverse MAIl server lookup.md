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

---
---