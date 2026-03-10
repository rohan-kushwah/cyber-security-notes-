## Shodan CLI
***
The **Shodan CLI** is a command-line tool that allows interaction with the Shodan search engine, including searching, downloading, and analyzing results.

### Installation
```bash
pip install shodan
```

---
### CLI Commands
#### Show Help Menu
```bash
shodan -h
```
#### Initialize with Your API Key
```bash
shodan init YOUR_API_KEY
```
Example:
```bash
shodan init VwikqRj9RsQ1b1J8qmCKkaRqKHW9QPp
```

```
shodan init niomYce15NOwSN7uppGB0EDD7iZD2bZW
```

This stores your API key locally for future use.

#### Search for a Term
```bash
shodan search QUERY
```
Example:
```bash
shodan search microsoft iis 6.0
```

```bash
shodan search 'title:"Apache Tomcat/9.0.97"'
```

#### Download Results to a File
```bash
shodan download FILENAME QUERY
```
Example:
```bash
shodan download iis6 microsoft iis 6.0
```
This stores raw data in `iis6.json.gz`.

```bash
shodan download Apache-Tomcat 'title:"Apache Tomcat/9.0.97"' --limit 10
```
This stores raw data in `Apache-Tomcat.json.gz`.

#### Convert Downloaded Results to JSON
```bash
shodan parse --fields ip_str,port,org,hostnames iis6.json.gz
```

```bash
shodan parse --fields ip_str,port,org,hostnames Apache-Tomcat.json.gz
```

Optional fields:
*   `ip_str`
*   `port`
*   `org`
*   `hostnames`
*   `location.city`
*   `ssl.cert.subject.cn`
*   `vulns`

## Additional Useful Commands
***
#### **Check Your Account Info**
```bash
shodan info
```
#### **Host Lookup**
```bash
shodan host <IP>
```
Example:
```bash
shodan host 8.8.8.8
```
#### **Scan IP/Range (Paid API Plan Required)**
```bash
shodan scan submit 192.168.1.1
```
#### **Search by Filters**
```bash
shodan search "apache country:IN port:80"
```

---
## Tips

- Use `shodan download` to archive searches for offline parsing.
- Combine with Linux tools like `jq`, `awk`, `grep` for advanced analysis.
- Automate recon pipelines using Shodan + Python or Bash scripts.
- Combine filters: `product:apache port:80 country:IN`
- Use `has_screenshot:true` for visual RDP/Web UI.
- Look for known CVEs using `vuln:`
- Use CIDR or ASN to explore specific networks.
- Extract domains with `hostname:*example.com`
---
---