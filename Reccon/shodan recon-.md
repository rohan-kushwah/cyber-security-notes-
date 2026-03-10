# Shodan Recon
***
Shodan is a search engine for internet-connected devices. It's widely used in cybersecurity for reconnaissance, identifying exposed systems, services, and potential vulnerabilities.

*   **Website:** [https://www.shodan.io/](https://www.shodan.io/)
*   **Filter Reference:** [https://www.shodan.io/search/filters](https://www.shodan.io/search/filters)

## How Shodan Works
***
### 1. Global Internet Scanning

Shodan uses a large network of scanners to connect to every public IPv4 address and tries to interact with open ports. It behaves like a **banner grabber**.

For example:
*   It connects to `x.x.x.x:80` and records the HTTP response header.
*   It connects to `x.x.x.x:21` and records the FTP banner.

### 2. Banner Collection

When Shodan connects to a device or service, it retrieves the **service banner** or metadata that the service exposes. This may include:

*   Software name and version (e.g., Apache 2.4.7)
*   OS type and version
*   SSL certificate details
*   Web server headers
*   Authentication methods
*   Error messages
*   Custom titles or messages

### 3. Indexing and Tagging

Collected data is:
*   Indexed in a search database
*   Tagged by:
    *   Port (`port:80`)
    *   Protocol (`product:"Apache"`)
    *   Organization (`org:"Google"`)
    *   Location (`country:US`)
    *   Vulnerabilities (using banner signature + known CVEs)
    *   SSL certificate fields
    *   Web technologies (`http.component:"WordPress"`)

### 4. Vulnerability Matching

Shodan cross-references collected banners with known CVEs from:
*   [National Vulnerability Database (NVD)](https://nvd.nist.gov/)
*   Internal vulnerability signature sets

If a match is found, the system flags the service with the corresponding `vuln:CVE-XXXX-XXXX` tag.

### 5. User Interface + API

You can access all this information via:

*   [Web UI](https://www.shodan.io/)
*   Shodan CLI
*   [Shodan API](https://developer.shodan.io/)
    *   Allows automation and integrations
    *   Common in red team tools and threat intelligence

### Example of Data for an IP on Port 80

```json
 {
    "ip_str": "192.0.2.123",
    "port": 80,
    "org": "Example ISP",
    "location": {
        "country_name": "India",
        "city": "Delhi"
    },
    "http": {
        "title": "Apache2 Ubuntu Default Page",
        "server": "Apache/2.4.18 (Ubuntu)"
    },
    "os": "Linux",
    "vulns": ["CVE-2017-5638"]
 }
```

| Stage | Description |
| :--- | :--- |
| Scanning | Shodan probes IPs and ports like a port scanner |
| Banner Grab | It collects response data from open services |
| Parsing | Banners are parsed for product, version, os, etc. |
| Indexing | Data is indexed and tagged with location, org, technology |
| CVE Mapping | Services are matched against known vulnerabilities |
| Access | Users can search via UI, CLI, or API |
***
## Account Setup
***
1.  Register on [Shodan](https://www.shodan.io/).
2.  Visit your profile to obtain your **API Key**.

## Basic Search Usage
***
```
apache
```

```
ftp
```

```
wordpress
```

```
nginx
```

```
cisco ios
```

Use filters to narrow results:

```
apache port:80 country:US
```
***
## Filters Reference
***
**port:** - Search by Open Port
```
port:80
```

```
port:443
```

```
port:22
```

```
port:21
```

Examples:
```
port:80 Apache
```

```
device:webcam port:554
```

**product:** - Search by Product

```
product:"apache"
```

```
product:"apache tomcat"
```

```
product:"Remote Desktop Protocol"
```

```
product:"LiteSpeed httpd"
```

Combined:
```
port:8080 product:"Apache Tomcat"
```

**http.component:** - Web Technologies
```
http.component:"wordpress"
```

```
http.component:"PHP"
```

```
http.component:"MySQL"
```

```
http.component:"Yoast SEO"
```

With version or status:
```
http.component:"wordpress" http.status:200
```


```
http.component:"wordpress" version:"6.4.2"
```

**asn:** - Filter by ASN
```
asn:AS24560
```

Useful sites:
*   [https://asnlookup.com](https://asnlookup.com)
*   [https://hackertarget.com/as-ip-lookup](https://hackertarget.com/as-ip-lookup)

**net:** - IP Range/CIDR
```
net:8.8.8.0/24
```

```
net:103.86.99.0/24
```

**hostname:** - Filter by Hostname
```
hostname:tesla
```

```
hostname:*.gov
```

**os:** - Search by Operating System
```
os:"Windows Server 2012"
```

```
os:"Ubuntu" port:22
```

**country: / city:** - Geo Filter
```
country:IN
```

```
country:IN city:Mumbai
```

**geo:** - Latitude/Longitude
```
geo:"32.800000, -117.000000"
```
[View on Map](https://www.shodan.io/search/facet?query=geo%3A%2232.800000%2C+-117.000000%22&facet=geo)

**org:** - Organization Filter
```
org:google
```

```
org:"HATHWAY CABLE AND DATACOM LIMITED"
```

**title:** - Match HTML Title Tag
```
title:"Apache Tomcat/9.0.97"
```

```
title:armour
```

**has_screenshot:** - Visual RDP/Web Screenshot
```
port:3389 has_screenshot:true
```

```
product:"Remote Desktop Protocol"
```

**ssl:** - SSL/TLS Info
```
ssl:"Yum Brands Inc."
```

```
ssl.cert.subject.cn:"*.pizzahut.com"
```

**vuln:** - Search by CVE
```
vuln:CVE-2019-19781 country:DE
```

```
vuln:CVE-2024-52318 hostname:*.tesla.cn
```

**before: / after:** - Time of Index
```
after:"2024-01-01"
```

```
before:"2025-01-01"
```

**device:** - Search Device Types
```
device:webcam
```

**tag:** - IoT Classifications
```
tag:ics
```

```
tag:scada
```

```
tag:plc
```

**hash:** - Match Banner Hash
```
hash:-1761231231
```

---
## API Usage Sample (Python)
***
```python
import shodan

API_KEY = 'YOUR_API_KEY'
api = shodan.Shodan(API_KEY)

results = api.search('apache country:IN')

for result in results['matches']:
    print(f"IP: {result['ip_str']} | Org: {result.get('org')}")
```

## Tips for Pentesters
***
*   Combine filters: `product:apache port:80 country:IN`
*   Use `has_screenshot:true` for visual RDP/Web UI
*   Look for known CVEs using `vuln:`
*   Use CIDR or ASN to explore specific networks
*   Extract domains with `hostname:.example.com`

---
---