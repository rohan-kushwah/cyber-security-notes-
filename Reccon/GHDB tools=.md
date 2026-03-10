## Google Hacking Database Tools
---
These tools automate **Google Dorking** and enhance reconnaissance using search engines like Google, Bing, Yahoo, and others. They are widely used in **OSINT**, **bug bounty**, and **penetration testing**.

### [pagodo (Passive Google Dork)](https://github.com/opsdisk/pagodo)
**Description:**
Automates the process of querying the [Google Hacking Database (GHDB)](https://www.exploit-db.com/google-hacking-database) using Google search. Useful for identifying sensitive files, directories, or configuration leaks.

**Example Usage:**
```
python3 pagodo.py -g dorks.txt -s armourinfosec.com
```
*   `-g dorks.txt`: File containing Google dorks
*   `-s armourinfosec.com`: Target domain

### [theHarvester](https://github.com/laramies/theHarvester)
---
**Description:**
A powerful tool for gathering emails, subdomains, hosts, and employee names using public sources like Google, Bing, Baidu, and more.

**Example: Scan a domain with Google**
```
theHarvester -d armourinfosec.com -l 500 -b google
```

**Example: Scan Facebook**
```
theHarvester -d facebook.com -l 500 -b google
```
*   `-d`: Domain to search
*   `-l`: Number of results to fetch
*   `-b`: Search engine backend (`google`, `bing`, `duckduckgo`, etc.)

### [metagoofil](https://github.com/laramies/metagoofil)
---
**Description:**
Metadata harvester to extract usernames, software versions, and more from publicly available documents (PDF, DOCX, PPTX, etc.).

**Example Usage:**
```
metagoofil -d armourinfosec.org -t pdf -n 20 -f report.html -o doc
```
*   `-d`: Domain to search
*   `-l`: Number of Google results to process
*   `-t`: File type (e.g., pdf, docx, ppt)
*   `-n`: Number of files to download
*   `-f`: Output HTML report file
*   `-o`: Output directory for downloaded documents

### Summary
---

| Tool           | Purpose                                         | Notes                             |
| :------------- | :---------------------------------------------- | :-------------------------------- |
| `pagodo`       | Run GHDB dorks against domains                  | Passive enumeration, customizable |
| `theHarvester` | Email & subdomain harvesting via search engines | Supports multiple engines         |
| `metagoofil`   | Metadata extraction from public documents       | Finds usernames, OS info, tools   |

---
--- 

# MAIN TOOL= EXPLOIT-DB GOOGLE DORKS


