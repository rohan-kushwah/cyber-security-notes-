# Website Reconnaissance

Website reconnaissance is the process of gathering publicly available information about a website, its infrastructure, and its online presence. It's typically one of the first steps in penetration testing or ethical hacking, but it's also used in SEO, research, and competitive analysis.

## What is Website Reconnaissance?

Website reconnaissance involves collecting information about a target website or web application to understand its structure, technologies, and potential vulnerabilities. The gathered data can help identify weaknesses, or simply provide a detailed picture of the website's digital footprint.

## Why Perform Website Reconnaissance?

- **Security Assessment:** To identify potential entry points or vulnerabilities.
- **Competitive Analysis:** To understand a competitor's digital presence.
- **SEO/Marketing:** To analyze site structure, backlinks, and indexed pages.
- **Compliance and Monitoring:** To ensure compliance with data protection regulations and discover exposed sensitive data.

## Typical Activities

- **Gathering Basic Details:** Such as IP addresses, domain ownership, and SSL certificates.
- **Collecting Metadata:** Site title, technologies (e.g., frameworks, CMS), and server information.
- **Finding Exposed Information:** Emails, phone numbers, or hidden directories.
- **Spidering and Crawling:** Mapping out links and site structure.
- **Historical Analysis:** Using the Wayback Machine to see how the site looked in the past.

## Key Tools Used

- **Online Recon Tools:** Shodan, Censys, BuiltWith.
- **Crawlers:** Screaming Frog, XML Sitemaps generators.
- **Command Line Tools:** wget, HTTrack for mirroring sites.
- **Archive Tools:** Archive.org, Archive.ph.

---

## 1. Visit Website

- Employee details
- Location details
- Addresses
- Phone numbers
- Emails
- Usernames

---

## 2. Find sitemap.html, sitemap.xml, and robots.txt

### `sitemap.html` — User-facing index
This is a human-readable page that typically lists the main sections or important links on a website. It's like a "table of contents" for visitors, helping them navigate the site.

### `sitemap.xml` — Bot index
This is an XML file meant for search engine crawlers (like Googlebot). It lists URLs the site owner wants to have indexed by search engines. It helps search engines find and understand the site's structure, especially for new or updated content.

### `robots.txt` — Restrictions for search engines
This is a text file that tells search engine crawlers which parts of the site they should not access or index. It's a way to control crawler behavior for privacy or security reasons.

---

## Example Sitemap URLs

- **India TV sitemap (user-facing HTML)**
  - https://www.indiatv.in/cms/sitemap.html

- **MCA India sitemap (user-facing HTML)**
  - https://www.mca.gov.in/MinistryV2/sitemap.html

- **Telangana Transport sitemap (user-facing HTML)**
  - https://www.transport.telangana.gov.in/html/sitemap.html

- **Online Sitemap Generator**
  - https://www.xml-sitemaps.com/

---

## 3. Spidering and Crawling

Good question! Let's clarify the difference:

### Spidering

Spidering (or **web spidering**) involves automated scripts or tools that **traverse and map out the links and pages** on a website. It's like sending a robot to click every link it finds, noting what's there and what's linked from it. It's typically used to:

- Identify **internal and external links**
- Discover **hidden directories** or **unlinked pages**
- Map out the **site's structure**

### Crawling

Crawling is a broader term that includes spidering. Crawling means **visiting a web page, downloading content, and following links to find new pages**. Search engines (like Googlebot) use crawling to index sites. Reconnaissance tools (like **Screaming Frog** or **Katana**) also crawl to:

- **Download page data**
- **Analyze metadata** (like page titles, headers, and scripts)
- **Check for vulnerabilities**

In a recon context, "spidering" and "crawling" are often used interchangeably because both involve automated tools scanning through a site's structure. The difference is mostly in nuance — **spidering** emphasizes mapping links, while **crawling** includes downloading and analyzing content.

### Online Tools

- **XML Sitemaps**
- **ConvertCSV URL Extractor**
- **Screaming Frog SEO Spider**

### Web Crawling Tools

#### Using Katana

```bash
# Katana example
katana -u https://www.armourinfosec.com
```

#### Using Dirhunt

Install dirhunt:
```bash
pip3 install dirhunt
```

Show help and usage info:
```bash
dirhunt --help
```

Basic usage:
```bash
dirhunt https://www.armourinfosec.com
```

Save output to file:
```bash
dirhunt https://www.armourinfosec.com --to-file /tmp/1.json
```

---
## 4. Locally Mirroring the Website — Read Source Code

### Using wget

```bash
wget -m https://www.armourinfosec.com
```

### Using HTTrack
*Note: This is an older tool*

Install HTTrack
```bash
apt install httrack
```

Run HTTrack interactive mirror tool
```
httrack
```

---
## 5. Wayback Machine — View Historical Versions

- **[Archive.org](https://archive.org/)** 🔗
- **[Web Archive](https://web.archive.org/)** 🔗
- **[Archive.ph](https://archive.ph/)** 🔗
- **Orkut Archive** (historical, if applicable)

---
---