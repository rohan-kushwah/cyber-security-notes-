# Shodan 
---
## 1. What is Shodan?

- **Shodan** is a specialized search engine, just like Google or Bing, but instead of searching for websites and webpages, it searches for **internet-connected devices and services**.
    
- Its name comes from the video game character “SHODAN” (Sentient Hyper-Optimized Data Access Network).
    
- Shodan’s purpose is to **index devices that are directly connected to the internet**, such as servers, routers, webcams, industrial control systems (ICS), IoT devices, smart TVs, SCADA systems, and more.
---
## 2. What does Shodan do?

- Shodan scans the internet continuously and **collects information from devices listening on public IP addresses**.
- It gathers details from **service banners** (the metadata that servers return when you connect to them).
- For example, if you connect to a web server, it might tell you the server type: “Apache 2.4.41 (Ubuntu).”
- Shodan collects and organizes such banners from millions of IPs worldwide.
- This allows users to **search for devices by their service, software, version, country, organization, vulnerability, etc.**    

**Examples of what you can find on Shodan:**

- All webcams with no password in a specific country.
- All routers using outdated firmware.
- All databases (like MongoDB or Elasticsearch) exposed to the internet without authentication.
- Power plants, traffic lights, and hospital devices connected without security.    

---
## 3. How does Shodan work?

Shodan works in three main steps:

### (a) Scanning

- Shodan runs its own distributed scanning infrastructure.
- It scans the entire IPv4 address space (and some IPv6), just like nmap, but on a global scale.
- It sends probes to various ports (HTTP:80, HTTPS:443, FTP:21, SSH:22, RDP:3389, etc.) to see what service responds.

### (b) Collecting banners

- When a device responds, it usually gives a **banner**, which contains information like:
    - Software name and version.
    - Supported features.
    - Hostname.
    - Sometimes even configuration details.
- Shodan stores these banners in its database.

### (c) Indexing and Searching

- Just like Google indexes web pages, Shodan indexes banners.
- Users can then query the database with keywords, filters, or complex queries.
- Example search:
    - `port:21 country:"IN"` → All FTP servers in India.
    - `apache country:"US"` → All Apache servers in the US.
    - `has_screenshot:true` → Devices with web interfaces.

---
## 4. Who uses Shodan?

- **Cybersecurity researchers:** To discover vulnerable systems on the internet.
- **Penetration testers / Ethical hackers:** To gather intelligence about a target before testing.
- **Hackers / Criminals:** To find unsecured devices to exploit (illegal use).
- **Government agencies:** To monitor critical infrastructure.
- **Companies:** To see what of their own infrastructure is exposed.    

---
## 5. Why is Shodan important?

- Unlike Google, which shows only what is _intentionally published_, Shodan reveals **what is accidentally exposed**.
- Many organizations unknowingly leave sensitive systems open to the internet (databases, cameras, ICS).
- Attackers can use this to hack, but defenders can use it to **identify and fix exposures before attackers exploit them**.
- It acts as a **global security monitoring tool**.    

---
## 6. Where does Shodan apply?

- In **penetration testing** (reconnaissance phase).
- In **red team operations** (finding attack surfaces).
- In **blue team monitoring** (checking what assets are exposed).
- In **OSINT (Open Source Intelligence)** investigations.
- In **critical infrastructure security** (ICS, SCADA).

---
## 7. When is Shodan used?

- At the **start of a pentest**, to identify targets.
- During **security audits**, to verify company exposure.
- In **incident response**, to check if a compromised system is publicly indexed.
- By attackers, _before launching scans themselves_, to avoid detection.

---
## 8. Which features does Shodan provide?

- **Basic search** (by IP, port, service).
- **Filters**: country, organization, operating system, product, version, etc.
- **Shodan Maps**: Visualization of exposed systems worldwide.
- **Exploit integrations**: Maps banners to known CVEs (via ExploitDB / CVE).
- **Monitoring**: For organizations to keep track of their internet assets.
- **API**: For developers to integrate Shodan data into security tools.

---
## 9. Example case studies

- In 2015, researchers used Shodan to discover **tens of thousands of MongoDB databases** exposed without passwords.
- In 2017, **Hikvision CCTV cameras** vulnerable to remote code execution were found worldwide.
- In 2020, Shodan revealed thousands of **medical devices** exposed to the internet without encryption.

---
## 10. Limitations of Shodan

- It only indexes what is visible on **open ports**.
- It cannot break through authentication or encryption. 
- It does not scan every single port (it chooses popular ones).
- Some results may be outdated, as IPs change ownership.

---

✅ **Summary:**  
Shodan is the world’s first and most powerful **search engine for internet-connected devices**.  
It scans the global internet, collects service banners, indexes them, and allows security professionals (and attackers) to find exposed devices.  
It is a double-edged sword: a tool for defenders to protect systems, but also useful for attackers if misused.

---
---