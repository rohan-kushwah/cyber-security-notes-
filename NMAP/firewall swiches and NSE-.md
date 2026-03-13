# 🛡️ Nmap — Firewall, IDS/IPS Evasion & NSE Cheat Sheet

> _A comprehensive reference for stealth scanning techniques and scripting in Nmap_

---

## 📦 Packet Fragmentation (`-f` / `--mtu`)

> Splits IP packets into smaller pieces to evade packet inspection by less sophisticated firewalls or IDS.

|Flag|Behavior|
|---|---|
|`-f`|Fragments packets into chunks of **8 bytes** or less (after IP header)|
|`-f -f`|Double fragment (even smaller chunks)|
|`--mtu <size>`|Custom MTU (must be a multiple of 8)|

### 🔧 Example Commands

```bash
# Basic fragmented scan
nmap -v -Pn -f 192.168.1.151

# Double fragmented, skip ping, port 80
nmap -v -Pn -f -f -p 80 192.168.1.151

# Custom MTU (16 bytes)
nmap --mtu 16 192.168.1.151

# Verbose no-ping fragmented scan, all ports
nmap -v -Pn --mtu 8 -p- 192.168.1.151

# MTU 16, port 80
nmap -v -Pn --mtu 16 -p 80 192.168.1.40
```

> ⚠️ **Limitations:** Modern firewalls can handle fragmented packets. Overuse may raise suspicion or cause packet loss.

---

## 🎭 Decoy Scanning (`-D`)

> Sends scans from multiple **fake IP addresses** alongside the real scan to confuse logs and hide the real attacker's IP.

```bash
nmap -D 192.168.1.10,192.168.1.20,192.168.1.30 192.168.1.151
nmap -D 192.168.1.10,192.168.1.20,192.168.1.30,192.168.1.7 192.168.1.1
```

---

## 🎭 Spoofing Source IP (`-S`)

> Fakes the source IP address to bypass IP-based filtering.

```bash
nmap -S 192.168.1.100 192.168.1.151
nmap -v -Pn -e eth0 -S 192.168.1.100 -p- 192.168.1.151
```

> 📝 **Note:** Requires direct network access and proper routing.

---

## 🔌 Spoofing MAC Address (`--spoof-mac`)

> Changes MAC address to evade detection or impersonate trusted devices.

```bash
nmap --spoof-mac 00:11:22:33:44:55 192.168.1.151
nmap -v -Pn 192.168.1.40 --spoof-mac cisco
```

---

## 🔀 Randomizing Source Port (`-g` / `--source-port`)

> Sets source port to commonly allowed ports (e.g., **53 for DNS**) to evade basic filtering.

```bash
nmap -g 53 192.168.1.1
```

---

## 🌐 Scanning Through Proxies (`--proxies`)

> Routes scan traffic via HTTP/SOCKS proxies to hide the scan origin.

```bash
nmap --proxies http://proxy.example.com:8080 192.168.1.151
```

---

## 🧟 Idle Scan (`-sI`)

> Uses a **"zombie" host** to relay scan packets, keeping the real scanner completely hidden.

```bash
nmap -sI 192.168.1.50 192.168.1.1
```

---

## 🔀 Randomizing Scan Order (`--randomize-hosts`)

> Scans hosts in a randomized order instead of sequentially to make pattern detection harder.

```bash
nmap --randomize-hosts 192.168.1.0/24
nmap --randomize-hosts -p 80 192.168.1.0/24
```

---

## 🎭 Cloaking Nmap Signature (`--data-length`)

> Appends random data payloads to each packet to confuse signature-based IDS.

```bash
nmap --data-length 50 192.168.1.1
nmap -v -Pn --data-length 128 192.168.1.151 -p 80
nmap -v -Pn 192.168.1.151 -p 80 --data 41414141414141
nmap -v -Pn 192.168.1.40 -p 80 --data 4142434445
nmap -v -Pn 192.168.1.151 -p 80 --data-string armour
```

---

## 🔕 Avoiding Ping (`-Pn`)

> Skips host discovery (ping) — useful when **ICMP is blocked** but TCP/UDP traffic is allowed.

```bash
nmap -Pn 192.168.1.1
```

---

## ⚙️ Custom IP Header Options (`--ip-options`)

> Sets custom IP header options on scan packets, useful for evading or testing firewall processing.

```bash
nmap --ip-options "\x83\x03\x10\x10\x00" 192.168.1.100
nmap -v -Pn --ttl 50 192.168.1.151 -p 80
```

---

---

# 🚀 Nmap Scripting Engine (NSE)

> NSE enhances Nmap's capabilities by automating tasks such as discovery, version detection, vulnerability scanning, brute-forcing, and more. Scripts are written in **Lua**.

---

## 🧩 Basic Usage & Syntax

```bash
# Run default scripts
nmap -sC <target>

# Run specific script(s)
nmap --script <script-name> <target>
nmap --script <script1,script2,...> <target>

# Run scripts by category
nmap --script <category> <target>

# Use wildcards or expressions
nmap --script "http-*" <target>
nmap --script "*smb*" <target>
nmap --script "default and not intrusive" <target>

# Run custom script file
nmap --script /path/to/custom-script.nse <target>

# Full scan example
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

# Explore scripts by keyword
grep smb /usr/share/nmap/scripts/script.db
ls /usr/share/nmap/scripts/ | grep ftp
ls /usr/share/nmap/scripts/ | grep http
```

---

## 🏷️ NSE Script Categories

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

## 🛠️ Common & Useful Scripts

|Script Name|Purpose|Example Command|
|---|---|---|
|`ftp-anon`|Checks for anonymous FTP login|`nmap -v -p 21 --script=ftp-anon.nse 192.168.1.35`|
|`smb-brute`|SMB brute force login|`nmap -v -p 445 --script=smb-brute 192.168.1.50`|
|`ssh-brute`|SSH password brute force|`nmap -p 22 --script ssh-brute --script-args userdb=usr,passdb=pass 192.168.1.40`|
|`http-methods`|Enumerates HTTP methods|`nmap -v -p 80 --script=http-methods 192.168.1.35`|
|`http-robots.txt`|Checks and parses robots.txt file|`nmap -v -Pn -sT -sV --script=http-robots.txt.nse -p 443 example.com`|
|`smb-vuln-ms17-010`|Checks SMB vulnerability (EternalBlue)|`nmap -v -p 445 --script=smb-vuln-ms17-010.nse 192.168.1.15`|
|`vuln`|Runs multiple vulnerability detection scripts|`nmap -v -sT --script=vuln 192.168.1.15`|
|`modbus-discover`|Enumerates Modbus SCADA devices|`nmap -v -Pn -sT -sV -A -O -p- --script=scada 192.168.1.208`|

---

## 🔗 Running Multiple / Complex Script Commands

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

> 💡 _These techniques help evade detection and filtering while scanning, improving stealth and effectiveness depending on the target environment._

---

_Reference: Armour Infosec — Network Reconnaissance & Scanning / Nmap_