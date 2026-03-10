# Reconnaissance (Footprinting)

Reconnaissance is the initial phase of a penetration test or cyberattack, where the attacker gathers as much information as possible about the target system or organization. This phase is crucial as it helps to plan the subsequent stages of an attack or penetration test.

---
## Types of Reconnaissance

### Passive Reconnaissance

Collecting information without directly interacting with the target.

**Examples:**
- Searching public records
- Examining social media
- Using tools like WHOIS, DNS queries, or public websites (e.g., job boards, company websites)

### Active Reconnaissance

Involves direct interaction with the target to gather information.

**Examples:**
- Scanning ports and services using tools like Nmap
- Sending phishing emails to gather credentials
- Probing web applications for vulnerabilities

---

## Goals of Reconnaissance

- Identify potential entry points
- Understand the target's network structure
- Collect details about users, roles, and infrastructure
- Determine possible vulnerabilities

---

## Key Areas of Focus

### 1. Network Vulnerabilities

- **Open Ports**: Unnecessary or unsecured ports exposing services
- **Misconfigured Devices**: Routers, firewalls, and switches with weak/default configurations
- **Weak Network Protocols**: Use of outdated protocols (e.g., Telnet, FTP, SMBv1)
- **Unauthorized Access**: Systems that allow connections without proper authentication

### 2. Web Application Vulnerabilities

Common issues (OWASP Top 10):

- **Injection Attacks**: SQL, NoSQL, Command, and LDAP injection
- **Broken Authentication**: Weak or missing authentication mechanisms
- **Cross-Site Scripting (XSS)**: Malicious scripts in web pages
- **Cross-Site Request Forgery (CSRF)**: Forcing users to perform actions unknowingly
- **Insecure Deserialization**: Exploiting serialized data to execute arbitrary code

### 3. System and Server Vulnerabilities

- **Unpatched Software**: Missing security updates
- **Default Credentials**: Factory-set usernames and passwords
- **Privilege Escalation**: Gaining higher privileges via misconfigurations or vulnerabilities
- **Insecure File Permissions**: Sensitive files accessible to unauthorized users

### 4. Authentication and Authorization Issues

- **Weak Password Policies**: Easily guessable or reused passwords
- **Brute Force and Dictionary Attacks**: Attempting to crack credentials
- **Misconfigured MFA**: Weak or missing multi-factor authentication

### 5. Data Exposure

- **Sensitive Information in Code/Logs**: Credentials, API keys, or PII in source code, error messages, or logs
- **Data Leaks**: Exposed databases, cloud storage, or backups
- **Improper Data Encryption**: Use of outdated or weak encryption

### 6. Social Engineering Weaknesses

- **Phishing Susceptibility**: Employees falling for fake emails or websites
- **Pretexting**: Impersonation to gain sensitive information
- **Physical Security Gaps**: Unsecured physical access to systems or sensitive areas

### 7. Wireless Network Vulnerabilities

- **Insecure Wi-Fi Configurations**: Open or weakly encrypted wireless networks
- **Rogue Access Points**: Unauthorized devices mimicking legitimate networks
- **Man-in-the-Middle (MITM) Attacks**: Intercepting communication between devices

### 8. Cloud Security Issues

- **Misconfigured Storage Buckets**: Publicly accessible (e.g., S3 buckets)
- **Exposed APIs**: APIs lacking authentication or rate limiting
- **IAM Misconfigurations**: Overly permissive roles or exposed credentials

### 9. IoT and Embedded System Weaknesses

- **Hardcoded Credentials**: Fixed credentials in devices
- **Insecure Firmware**: Firmware vulnerabilities
- **Lack of Updates**: Devices not receiving regular security patches

---

## Organization Information

- **Organization Website**: Public-facing resource with general/technical information
- **Company Directory**: Lists employees, roles, and contact details
- **Employee Details**: Profiles from LinkedIn, social media, or public platforms
- **Location Details**: Physical addresses and office locations
- **Addresses/Phone Numbers**: Valuable for pretexting or phone-based phishing
- **Comments in HTML Source Code**: Developer notes or references
  - Example: `<!-- TODO: Remove this link before going live -->`
- **Security Policies Deployed**: Public policies or notices showing security posture
- **Web Server Links**: External APIs or third-party services linked to the website
- **Background of Organization**: Company's history and structure
- **News/Press Releases**: Recent changes, expansions, or acquisitions

## Network Information

- **Domain Names**: Identifies organization's internet presence  
  Example: `example.com`, `sub.example.com`
- **Internal Domains**: Insights into internal naming conventions  
  Example: `internal.local`, `intranet.example.com`
- **IP Addresses**: Public and private IP ranges
- **Unmonitored/Private Websites**: Internal applications not publicly indexed
- **TCP/UDP Services**: Services running on specific ports  
  Example: HTTP (port 80), SSH (port 22)
- **VPN/IDS/IPS/Access Controls**: Secure access and network protection mechanisms
- **VPN Information**: Configuration details or vulnerabilities in VPN solutions
- **Phone Numbers/VoIP**: Communication systems for potential social engineering

---

## Operating System Information

- **User & Group Names/Info**: Usernames and group memberships
- **Banner Grabbing**: Captures banners to identify software/versions
- **Routing Tables**: Network routes and interfaces
- **SNMP (Simple Network Management Protocol)**: Device and network details
- **System Architecture**: 32-bit, 64-bit, or other platform details
- **Remote Systems**: Connected or shared systems
- **System Names**: Naming conventions indicating roles  
  Example: `DC01` (Domain Controller), `SQLServer`
- **Passwords**: Credentials from exposed services, brute force, or social engineering

---

## External Reconnaissance

- **Network**: Public-facing IPs and domains
- **Phone**: Public or leaked phone numbers
- **Website**: Analyze forms, scripts, and structure
- **Source Code**: Public repositories (e.g., GitHub) containing sensitive information
- **Website Mirroring**: Create offline copies using tools like `wget` or `HTTrack`
- **Archive Sites**: Historical snapshots (e.g., Archive.org, CachedView)
- **GitHub Recon**: Tools like Gitrob, truffleHog to search repositories and commits
- **Whois**: Domain registration details and admin contacts
- **Web Server Content**: Backup files, configuration files, or sensitive data
- **Email Header**: Metadata showing IPs, server paths, and infrastructure
- **Google/Search Engines**: Google Dorking for sensitive data or internal links
- **Google Hacking**: Advanced queries (`filetype:`, `site:`, `inurl:`)
- **People Sites**: People search engines (e.g., Pipl, BeenVerified)
- **Social Networks**: Data from LinkedIn, Twitter, Facebook, etc.
- **Job Sites**: Job postings revealing tech stacks or internal tools
- **Alert Website**: Monitoring changes (e.g., Google Alerts)

---

## Internal Reconnaissance

- **IP Addresses**: Internal network mapping and critical systems
- **Internal DNS**: Internal domain records and zone transfers
- **Private Websites**: Intranet and other internal web apps
- **Dumpster Diving**: Searching for sensitive information in discarded items  
  Examples: Printed documents, sticky notes, old hardware
- **Shoulder Surfing**: Observing user activity for credentials or data

---
----
## Attack Surface Mapping
----
Attack surface mapping is the process of identifying and analyzing all possible points of interaction with a target system or organization. This helps security professionals understand the scope of potential vulnerabilities that attackers could exploit.

### Key Components of Attack Surface Mapping

### 1. External Attack Surface

Focuses on everything exposed to the public internet.

- **Public-Facing Applications**: Web apps, APIs, email servers, VPN gateways
- **Public IP Addresses**: IP ranges associated with the organization
- **DNS Records**: Subdomains, MX records, and other DNS entries
- **Cloud Services**: Public cloud resources, storage buckets, and exposed APIs
- **Third-Party Services**: External dependencies or integrations

### 2. Internal Attack Surface

Involves assets that may not be directly exposed but could be exploited once an attacker gains internal access.

- **Internal Web Applications**: Intranet portals, admin consoles
- **Internal Networks**: Network segments, shared drives, internal APIs
- **User Accounts**: Usernames, group memberships, privilege levels
- **Internal Services**: Databases, file servers, and messaging services

### 3. Physical Attack Surface

Considers physical locations and devices.

- **Office Buildings**: Physical access points and security controls
- **Employee Devices**: Laptops, mobile devices, USB drives
- **IoT Devices**: Cameras, sensors, and smart devices
- **Dumpster Diving Risks**: Documents or hardware in the trash

---

## Process of Attack Surface Mapping

1. **Discovery**: Identify all assets (external, internal, physical)
2. **Enumeration**: Gather detailed information about each asset (e.g., versions, configurations)
3. **Classification**: Determine the sensitivity and criticality of each asset
4. **Risk Assessment**: Evaluate potential vulnerabilities and the impact of an attack
5. **Visualization**: Map out the attack surface using diagrams or tables
6. **Prioritization**: Focus on the most critical and vulnerable points

---

## Tools and Techniques

- **Network Scanning**: Nmap, Masscan
- **Web Crawling**: OWASP ZAP, Burp Suite
- **OSINT**: Recon-ng, Maltego, Shodan
- **Vulnerability Scanning**: Nessus, OpenVAS
- **Cloud Tools**: ScoutSuite, CloudMapper
- **Physical Recon**: Physical walkthroughs and social engineering assessments

---

## Benefits of Attack Surface Mapping

- Provides a clear view of exposure points
- Enables proactive risk management
- Informs security testing and vulnerability assessments
- Enhances incident response and monitoring efforts

---
---