# 🛡️ Nmap Scripting Engine (NSE) — Complete Reference

> _Scripts are written in **Lua**, enabling flexibility and extensibility across all scan types._

---

## ⚡ Basic Usage & Syntax

|Task|Command|
|---|---|
|Run default scripts|`nmap -sC <target>`|
|Run specific script|`nmap --script <script-name> <target>`|
|Run multiple scripts|`nmap --script <script1,script2,...> <target>`|
|Run by category|`nmap --script <category> <target>`|
|Use HTTP wildcard|`nmap --script "http-*" <target>`|
|Use SMB wildcard|`nmap --script "*smb*" <target>`|
|Exclude intrusive|`nmap --script "default and not intrusive" <target>`|
|Custom script file|`nmap --script /path/to/custom-script.nse <target>`|

### 🔍 Full Scan Example

```bash
nmap -v -Pn -sT -sV -A -O -p- --script=default 192.168.1.208
```

---

## 📂 Managing & Finding NSE Scripts

```bash
# List all scripts
ls /usr/share/nmap/scripts/
ls /usr/share/nmap/scripts/ | wc -l

# Update script database
nmap --script-updatedb

# Search by keyword
grep smb /usr/share/nmap/scripts/script.db
ls /usr/share/nmap/scripts/ | grep ftp
ls /usr/share/nmap/scripts/ | grep http
```

---

## 🗂️ NSE Script Categories

|Category|Description|
|---|---|
|`auth`|Authentication bypass, brute-force attacks|
|`broadcast`|Network discovery via broadcast|
|`brute`|Brute-force login attempts|
|`discovery`|Host, service, and configuration discovery|
|`dos`|Denial-of-Service testing|
|`exploit`|Exploit known vulnerabilities|
|`external`|Leverage external services (e.g., WHOIS, Shodan)|
|`fuzzer`|Send unexpected data for robustness testing|
|`intrusive`|May affect targets or trigger alerts|
|`malware`|Detect malware-infected hosts|
|`safe`|No attack or disruption; safe scans|
|`version`|Enhance version detection|
|`vuln`|Vulnerability detection|
|`default`|A curated set of useful scripts (various categories)|
|`scada`|SCADA/ICS industrial control systems testing|

---

## 🌟 Common & Useful Scripts

|Script Name|Purpose|Example Command|
|---|---|---|
|`ftp-anon`|Checks for anonymous FTP login|`nmap -v -p 21 --script=ftp-anon.nse 192.168.1.35`|
|`smb-brute`|SMB brute force login|`nmap -v -p 445 --script=smb-brute 192.168.1.50`|
|`ssh-brute`|SSH password brute force|`nmap -p 22 --script ssh-brute --script-args userdb=usr,passdb=pass 192.168.1.40`|
|`http-methods`|Enumerates HTTP methods|`nmap -v -p 80 --script=http-methods 192.168.1.35`|
|`http-robots.txt`|Checks and parses robots.txt|`nmap -v -Pn -sT -sV --script=http-robots.txt.nse -p 443 example.com`|
|`smb-vuln-ms17-010`|Checks SMB vulnerability (EternalBlue)|`nmap -v -p 445 --script=smb-vuln-ms17-010.nse 192.168.1.15`|
|`vuln`|Runs multiple vulnerability detection scripts|`nmap -v -sT --script=vuln 192.168.1.15`|
|`modbus-discover`|Enumerates Modbus SCADA devices|`nmap -v -Pn -sT -sV -A -O -p- --script=scada 192.168.1.208`|

---

## 🔄 Running Multiple / Complex Script Commands

```bash
# Multiple specific scripts
nmap --script "ssh-brute,ftp-brute,smb-brute" 192.168.1.208

# Using wildcards
nmap --script "http-*" 192.168.1.208

# Exclude certain scripts
nmap --script "default and not intrusive" 192.168.1.208

# Based on regex patterns
nmap --script "^(smb|ftp)-.*" 192.168.1.208
```

---

## 🌐 HTTP NSE Scripts

> Nmap offers several powerful HTTP-related NSE scripts for web server enumeration, vulnerability detection, and information gathering.

```bash
# List HTTP-related scripts
grep http /usr/share/nmap/scripts/script.db

# Scan HTTP methods allowed on port 80
nmap -p 80 --script http-methods <target>
nmap -v -p 80 --script=http-methods 192.168.1.35

# Fetch HTTP server headers
nmap -v -Pn -sT -sV --script=http-server-header.nse -p 80 192.168.1.205

# Generate sitemap by crawling HTTP(S) on multiple ports
nmap -v -sT -sV -p 80,443,8080,8443 --script=http-sitemap-generator.nse 192.168.1.51

# Test all HTTP methods (including non-standard)
nmap -p 80 --script http-methods --script-args http-methods.test-all=true <target>

# Check HTTP security headers on multiple ports
nmap -v -sT -sV --script=http-security-headers.nse -p 80,443,8080,8443,9443 192.168.1.51

# Detect PHP versions and vulnerabilities
nmap -v -sT -sV --script=http-php-version.nse -p 80,443,8080,8443,9443 192.168.1.51
nmap -v -sT -sV --script=http-phpmyadmin-dir-traversal.nse -p 80,443,8080,8443,9443 192.168.1.51

# Run all HTTP-related scripts on common web ports
nmap -v -sT -sV -p 80,443,8080,8443 --script=http* 192.168.1.51

# Enumerate directories and files on HTTP servers
nmap -v -sT --script=http-enum.nse -p 80 192.168.1.44

# Check for robots.txt file
nmap -v -Pn -sT -sV --script=http-robots.txt.nse -p 443 www.armourinfosec.com

# Run all HTTP scripts on port 80
nmap -v -sT --script=http* -p 80 192.168.1.44
```

---

## 🖧 SMB NSE Scripts

> Enumerate SMB shares, users, groups, vulnerabilities, and system information on Windows and Samba hosts.

```bash
# Discover SMB OS details
nmap -v -p 445 --script=smb-os-discovery.nse 192.168.1.50
nmap -v -sT -sV -p 445 --script=smb-os-discovery.nse 192.168.1.243

# Enumerate SMB shares
nmap -v -p 445 --script=smb-enum-shares.nse 192.168.1.40

# Enumerate SMB services (default scripts)
nmap -v -p 445 -sC smb-enum-services.nse 192.168.1.50

# Enumerate SMB users, groups, domains, processes, sessions, and system info
nmap -v -sT -sV -p 445 --script=smb-enum-users.nse 192.168.1.243
nmap -v -sT -sV -p 445 --script=smb-enum-groups.nse 192.168.1.243
nmap -v -sT -sV -p 445 --script=smb-enum-domains.nse 192.168.1.243
nmap -v -sT -sV -p 445 --script=smb-enum-processes.nse 192.168.1.243
nmap -v -sT -sV -p 445 --script=smb-enum-sessions.nse 192.168.1.243
nmap -v -sT -sV -p 445 --script=smb-system-info.nse 192.168.1.243

# Gather SMB server statistics
nmap -v -sT -sV -p 445 --script=smb-server-stats.nse 192.168.1.243

# Check for SMB vulnerabilities (MS08-067, MS17-010)
nmap -v -sT -sV -p 445 --script=smb-vuln-ms08-067.nse 192.168.1.151
nmap -v -sT -sV -p 445 --script=smb-vuln-ms17-010.nse 192.168.1.151
nmap -v -sT -sV -p 445 --script=smb-vuln-ms* 192.168.1.151
nmap -v -sT -sV -p 445 --script=smb-vuln* 192.168.1.205,151

# Combine TCP and UDP SMB-related scans on multiple ports
nmap -v -sT -sU -p T:445,139,U:137 --script=smb* 192.168.1.47

# List all SMB shares
nmap -v -sT -sV -p 445 --script=smb-ls.nse 192.168.1.151

# Enumerate SMB shares on multiple target hosts
nmap -v -p 445 -sT -sV --script=smb-enum-shares.nse 192.168.1.41 192.168.1.220 192.168.1.222

# Run multiple SMB enumeration scripts together
nmap -v -p 445 -sT -sV --script=smb-enum-shares.nse,smb-ls.nse 192.168.1.41 192.168.1.220 192.168.1.222
```

---

## 📁 FTP NSE Scripts

> NSE scripts for FTP service enumeration, vulnerability detection, and security checks.

```bash
# Detect anonymous FTP login
nmap -v -p 21 --script=ftp-anon.nse 192.168.1.35

# Check for vulnerable or backdoored FTP software
nmap -v -sT --script=ftp-proftpd-backdoor.nse -p 21 192.168.1.44
nmap -v -sT --script=ftp-vsftpd-backdoor.nse -p 21 192.168.1.44

# Enumerate system info and directory listings via FTP
nmap -v -sV --script=ftp-syst.nse -p 21 192.168.1.35

# Perform brute-force login attempts (requires wordlists)
nmap --script=ftp-brute -p 21 192.168.1.151
```

---

## 🗄️ MySQL NSE Scripts

> Specialized NSE scripts to gather information about MySQL services, enumerate users and databases, and check for weak or empty passwords.

```bash
# Discover basic server info
nmap -v -sT --script=mysql-info.nse -p 3306 <target>

# Enumerate users (if possible)
nmap -v -sT --script=mysql-enum.nse -p 3306 <target>

# Check for accounts with empty passwords
nmap -v -sT --script=mysql-empty-password.nse -p 3306 <target>
```

---

## 📂 NFS NSE Scripts

> Built-in NSE scripts to enumerate and gather information from Network File System (NFS) services.

```bash
# List NFS exports
nmap -v -sT --script=nfs-showmount -p 2049 192.168.1.151

# List files and file attributes in exports
nmap -v -sT --script=nfs-ls -p 2049 192.168.1.151
```

---

## 💥 Brute-Force NSE Scripts

> Example commands for brute-forcing SSH, HTTP, FTP, Telnet, and MySQL services.

```bash
# Get help and info about the SSH brute-force script
nmap --script-help=ssh-brute.nse

# SSH brute-force with custom username and password files, timeout 4s
nmap -p 22 --script ssh-brute --script-args userdb=/opt/user.txt,passdb=/opt/password.txt,ssh-brute.timeout=4s 192.168.1.151

# Verbose TCP connect scan with SSH brute-force using argument file
nmap -v -sT -p 22 --script=ssh-brute.nse --script-args-file=/opt/nmap-user-password.db 192.168.1.151

# Verbose SSH brute-force scan with script tracing enabled
nmap -v -sT -p 22 --script=ssh-brute.nse --script-args-file=/opt/nmap-user-password.db --script-trace 192.168.1.151

# HTTP brute-force attack on port 80
nmap -v -sT -p 80 --script=http-brute.nse --script-args="userdb=/opt/user.txt,passdb=/opt/password.txt" 192.168.1.151

# FTP brute-force attack on port 21
nmap -v -sT -p 21 --script=ftp-brute.nse --script-args="userdb=/opt/user.txt,passdb=/opt/password.txt" 192.168.1.15

# Telnet brute-force on port 23 with timeout and credential lists
nmap -v -sT -p 23 --script=telnet-brute.nse --script-args="userdb=/opt/user.txt,passdb=/opt/password.txt,telnet-brute.timeout=8s" 192.168.1.15

# MySQL brute-force on port 3306 with credential lists and timeout
nmap -p 3306 --script=mysql-brute.nse --script-args="userdb=/opt/user.txt,passdb=/opt/password.txt,telnet-brute.timeout=8s" 192.168.1.151

# SSH brute-force using custom username and password lists with timeout
nmap -p 22 --script ssh-brute --script-args userdb=/opt/username.list,passdb=/opt/password.list,ssh-brute.timeout=4s 192.168.1.151
```

---

## 📋 Summary Table of Script Expression Usage

|Expression|Function|
|---|---|
|`--script "ftp-anon"`|Run single script|
|`--script "ssh-brute,ftp-brute"`|Run multiple scripts|
|`--script "http-*"`|Run all HTTP-related scripts|
|`--script "vuln"`|Run vulnerability detection scripts|
|`--script "*smb*"`|Run all SMB-related scripts|
|`--script "not safe"`|Exclude safe scripts|
|`--script "discovery and not intrusive"`|Run discovery scripts excluding intrusive ones|
|`--script "brute or vuln"`|Run brute-force and vulnerability scripts|
|`--script "http-title" -vv`|Run script with verbose output|

---

## 💡 Tips and Best Practices

- ✅ Always get **written permission** before scanning any target
- ✅ Use `-v` or `-vv` flags for **verbose output** to better understand results
- ✅ Use `--script-help=<script>` to learn about any script before running it
- ✅ Combine `--script-updatedb` regularly to keep the script database current
- ⚠️ The `intrusive`, `brute`, `dos`, and `exploit` categories **can disrupt services**
- ⚠️ Use `safe` category for non-disruptive scanning in production environments
- 🔍 Use `grep` on `script.db` to quickly find scripts by keyword

---

_Reference: Armour Infosec — Network Reconnaissance & Scanning > Nmap > NSE Script Scan_