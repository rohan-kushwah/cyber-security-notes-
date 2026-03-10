## 🗺️ Reconnaissance Checklist (Footprinting Phase)
***
### 1. 🌐 Website Reconnaissance

#### 🔍 Visit the Target Website
Manually browse the target's public website to extract visible information.
*   **Employee Details** – Names, positions, contact info, org chart clues.
*   **Location Details** – Office addresses, facility names, maps.
*   **Addresses** – Headquarters, branches, or PO boxes.
*   **Phone Numbers** – Helpdesk, sales, support, direct extensions.

#### 📋 Check Public Files
*   **sitemap.html** – Usually used for human users; can reveal hidden pages.
*   **sitemap.xml** – Used by search engines; shows all indexed site links.
*   **robots.txt** – Indicates directories disallowed for search engines (often valuable content).
*   Example:
    ```bash
    curl https://example.com/robots.txt
    ```

#### 🕷️ Spidering and Crawling
Use automated tools to crawl and map the structure of the website.
*   **Tools:** Burp Suite, OWASP ZAP, HTTrack, Scrapy, wget
*   **Online:** [https://xml-sitemaps.com](https://xml-sitemaps.com) 

#### 💾 Local Mirroring of the Website
*   Download complete site:
    ```bash
    wget --mirror --convert-links --adjust-extension --page-requisites --no-parent https://example.com
    ```
*   Analyze source code for comments, hardcoded credentials, and links to dev/test environments.

#### 📅 Wayback Machine (Archive.org)
*   View historical content and deleted pages:
    *   [https://web.archive.org/web/*/example.com](https://web.archive.org/web/*/example.com) 

***
### 2. 🧱 What the Website is Built With
***
*   Identify backend technologies, CMS, frameworks, JavaScript libraries.
*   Tools:
    *   [https://builtwith.com](https://builtwith.com) 
    *   [https://wappalyzer.com](https://wappalyzer.com) 
*   Browser extensions for Wappalyzer or BuiltWith

***
### 3. 🔎 WHOIS Lookup
***
#### ✅ Basic WHOIS Lookup
Identify domain registrant, creation/expiry dates, registrar, nameservers.
```bash
whois example.com
```

#### 🔁 Reverse WHOIS
Search for all domains registered with same email/organization.
*   **Tool:** [https://viewdns.info/reversewhois](https://viewdns.info/reversewhois) 

#### 🕒 WHOIS History
Reveal past ownership or registrar changes.
*   **Tool:** [https://whois-history.whoisxmlapi.com](https://whois-history.whoisxmlapi.com) 

***
### 4. 🌐 Domain to IP Resolution
***
Resolve a domain name to its corresponding IP address.
```bash
nslookup example.com
dig example.com
```

***
### 5. 🗺️ IP Geolocation
***
Map the IP address to a physical location.
*   **Tools:**
    *   [https://ipinfo.io](https://ipinfo.io) 
    *   [https://iplocation.net](https://iplocation.net) 

***
### 6. 🔀 IP Traceroute
***
Track the route packets take to the target.
```bash
traceroute example.com    # Linux/macOS
tracert example.com       # Windows
```

***
### 7. 📜 IP Address History
***
View historical IPs used by the domain.
*   **Tools:**
    *   [https://securitytrails.com](https://securitytrails.com) 
    *   [https://viewdns.info](https://viewdns.info) 

***
### 8. 🔁 Reverse IP Lookup
***
Find all domains hosted on the same server/IP address.
*   [https://viewdns.info/reverseip/](https://viewdns.info/reverseip/) 
*   [https://hackertarget.com/reverse-ip-lookup/](https://hackertarget.com/reverse-ip-lookup/) 

***
### 9. 📧 Reverse Mail Server Lookup
***
Identify domains using the same mail server.
*   [https://mxtoolbox.com/](https://mxtoolbox.com/) 

***
### 10. 🔁 Reverse Name Server Lookup
***
List all domains using the same name server.
*   **Tool:** [https://viewdns.info/reversens/](https://viewdns.info/reversens/) 

***
### 11. 🚪 Open Port Scanning
***
Check for open ports and running services.
```bash
nmap -Pn -sV -T4 example.com
```
*   **Online:** [https://pentest-tools.com](https://pentest-tools.com) 

***
### 12. 🔎 Google Dorking (Google Hacking)
***
Search sensitive data using Google operators.
*   **Basic:** `site:`, `inurl:`, `intitle:`, `filetype:`
*   **Advanced:** `cache:`, `link:`, `related:`, `inanchor:`
*   **DB:** [https://www.exploit-db.com/google-hacking-database](https://www.exploit-db.com/google-hacking-database) 
*   Example Dork:
    ```
    site:example.com filetype:pdf
    ```

***
### 13. 🌐 Shodan Reconnaissance
***
Search for publicly exposed devices and services.
*   [https://shodan.io](https://shodan.io) 
*   Example:
    ```
    shodan search org:"Target Organization" port:21
    ```

***
### 14. 💻 GitHub Recon
***
Find exposed code, credentials, API keys.
*   [https://github.com/search](https://github.com/search) 
*   **Tools:** `gitrob`, `truffleHog`, `Gitleaks`, `GitDorker`

***
### 15. 🤖 GPT-Powered Recon
***
*   Use LLMs to parse, analyze, and summarize bulk recon data.
*   Examples:
    *   Log analysis
    *   Dork generation
    *   Email pattern prediction

***
### 16. 🧩 OSINT Framework
***
Comprehensive list of OSINT tools.
*   [https://osintframework.com](https://osintframework.com) 

***
### 17. 👥 People Search Engines
***
*   [https://pipl.com](https://pipl.com) 
*   [https://peekyou.com](https://peekyou.com) 
*   [https://zabasearch.com](https://zabasearch.com) 

***
### 18. 📈 IP Logger
***
Log IP of users by sending a custom tracking URL.
*   [https://grabify.link](https://grabify.link) 
*   [https://iplogger.org](https://iplogger.org) 

***
### 19. ✉️ Email Tracking and Spoofing
***
#### 🔍 Email Header Analysis
Trace sender, IP, location.

#### 📮 DMARC, SPF, DKIM Lookup
*   **Tools:**
    *   [https://easydmarc.com/tools/dmarc-lookup](https://easydmarc.com/tools/dmarc-lookup) 
    *   ```bash
        dig TXT _dmarc.example.com
        ```

#### 🕵️ Spoofing Emails
*   **Tools:**
    *   [https://emkei.cz/](https://emkei.cz/) 
    *   [https://anonymailer.net/](https://anonymailer.net/) 

***
### 20. 📞 Call Spoofing
***
Make calls with fake caller ID (for social engineering).
*   **Tools:**
    *   Mobile apps like TextMe, SpoofCard
    *   **Web:** [http://www.crazycall.net](http://www.crazycall.net) 

***
### 21. 📚 Newsgroup Searches
***
Look for legacy discussions, leaks, or company messages.
*   [https://groups.google.com](https://groups.google.com) 
*   Yahoo Groups (archived)

***
### 22. 💼 Online Job Listings
***
Scrape info on tech stack, tools, locations, departments.
*   LinkedIn Jobs
*   Indeed, Glassdoor
*   Careers page of target

***
### 23. 📄 Resumes
***
Look for tech usage, internal tools, emails, and infrastructure clues.
*   **Search:**
    ```
    site:linkedin.com/in "company name"
    filetype:pdf resume "company name"
    ```

***
### 24. 🛠️ Tech Support Forums
***
Look for employees leaking details in questions or discussions.
*   [https://stackoverflow.com](https://stackoverflow.com) 
*   [https://quora.com](https://quora.com) 
*   [https://glassdoor.com](https://glassdoor.com) 

***
### 25. 📝 Blogs and Community Forums
***
Find articles and discussions authored by employees.
*   Medium
*   Reddit (subreddits like r/netsec, r/sysadmin)
*   Blogger, WordPress

***
### 26. 📱 Social Media Profiling
***
Analyze company and employee activity across platforms.
*   [https://tweetdeck.twitter.com](https://tweetdeck.twitter.com) 
*   [https://hootsuite.com](https://hootsuite.com) 
*   [https://www.social-searcher.com](https://www.social-searcher.com) 

***
### 27. 🔔 Google Alerts
***
Monitor mentions of target in real time.
*   [https://www.google.com/alerts](https://www.google.com/alerts) 

---
---