# 🛰️ Nmap — Service & OS Detection Cheatsheet

> _"Know your network. Own your security."_

---

## ⚡ Service Version Detection (`-sV`)

> The `-sV` flag allows Nmap to determine **detailed information** about software running on open ports — including application names, version numbers, device types, and specific protocol features.

---

### 🔍 How the `-sV` Scan Works

```
┌─────────────────────────────────────────────────┐
│  1. Initial Port Scan                           │
│     └─ SYN or TCP scans to find open ports      │
│                                                 │
│  2. Probing Open Ports                          │
│     ├─ Banner grabbing (greeting messages)      │
│     ├─ Protocol commands (SMTP HELO, HTTP GET/) │
│     └─ TLS/SSL handshakes for encryption info   │
│                                                 │
│  3. Analyzing Responses                         │
│     ├─ Compares against service/version DB      │
│     └─ Makes educated guesses via heuristics    │
└─────────────────────────────────────────────────┘
```

---

### 📊 Port Status Identification Examples

|Port|Service|Version|
|---|---|---|
|22/tcp|OpenSSH|OpenSSH 8.4p1 Ubuntu|
|80/tcp|Apache|Apache httpd 2.4.41|
|443/tcp|Nginx|Nginx 1.18.0|
|8080/tcp|unknown|Possibly a web server|

---

### 🧪 Example Commands

```bash
# Basic service version detection
nmap -sV 192.168.1.51

# Specific ports (22, 80, 443) on a domain
nmap -sV -p 22,80,443 example.com

# Verbose TCP connect scan on port 80
nmap -v -sT -p 80 192.168.1.51

# Verbose TCP connect scan on port 23
nmap -v -sT -p 23 192.168.1.51

# Light version detection
nmap -v -sT -sV --version-light -p 80 192.168.1.51

# Full version detection
nmap -v -sT -sV --version-all -p 80 192.168.1.51

# Moderate intensity (3) on port 80
nmap -v -sT -sV --version-intensity 3 -p 80 192.168.1.51

# Maximum intensity (9) on port 80
nmap -v -sT -sV --version-intensity 9 -p 80 192.168.1.51

# Max intensity on default ports
nmap -sV --version-intensity 9 192.168.1.51

# With banner script
nmap -sV --script=banner 192.168.1.51

# Verbose no-ping with banner script on port 80
nmap -v -Pn -sT -sV --script=banner -p 80 192.168.1.51

# All ports scans
nmap -v -p- 192.168.1.51
nmap -v -sT -p- 192.168.1.51
nmap -v -Pn -p- 192.168.1.51
nmap -sT -sV -p- 192.168.1.51
```

---

## 🖥️ Operating System Detection (`-O`)

> The `-O` flag identifies the **operating system** on a target machine by sending specially crafted packets and analyzing responses against a large database of OS fingerprints.

---

### 🔬 How the `-O` Scan Works

```
┌──────────────────────────────────────────────────────┐
│  ① Initial Port Scan                                │
│     └─ Requires at least ONE open + ONE closed port  │
│                                                      │
│  ② Sending Probes                                   │
│     └─ TCP, UDP, ICMP probes analyzing:              │
│        packet structure · TTL · window sizes         │
│        responses · OS-specific characteristics       │
│                                                      │
│  ③ Matching Fingerprints                            │
│     ├─ Compares to Nmap's OS fingerprint database    │
│     └─ Exact match OR best guess with confidence %   │
└──────────────────────────────────────────────────────┘
```

---

### 📋 Example Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2025-02-14 14:32 IST
Nmap scan report for 192.168.1.51
Host is up (0.0050s latency).
Not shown: 998 filtered ports
PORT    STATE  SERVICE
22/tcp  open   ssh
80/tcp  open   http

OS details: Linux 5.4 - 5.11 (Ubuntu), Windows Server 2019, or FreeBSD 12
Network Distance: 1 hop
```

---

### 🚦 Port Status and OS Detection

|Status|Description|
|---|---|
|✅ Exact Detection|Sufficient data found; OS displayed with confidence|
|🔶 Guessing|Partial matches yield a best guess with confidence %|
|❌ Failed Detection|Insufficient data (e.g. all ports filtered)|

---

### ✅ Advantages of `-O`

- 🔍 Provides detailed OS fingerprinting for reconnaissance
- 📦 Helps in asset management by tracking device OS versions
- 🎯 Assists vulnerability assessments targeting specific OS weaknesses

### ⚠️ Disadvantages of `-O`

- Requires at least one **open** and one **closed** port; may fail if all ports are filtered
- Potentially triggers **IDS/IPS** monitoring and alerts
- OS fingerprinting can be **inaccurate** if targets use custom or obscured network stacks

---

### 🌐 Common Protocols, Kernel, and OS Examples

|Protocol #|Service|Kernel|OS Examples|
|---|---|---|---|
|1|ICMP|Linux Kernel 5.x|Linux (Ubuntu, Debian, CentOS)|
|6|TCP|Windows NT Kernel|Windows 10, Windows Server 2019|
|17|UDP|FreeBSD Kernel|FreeBSD 12.x|
|47|GRE|Linux Kernel 4.x|VPN Tunnels, Cisco Routers|
|50|ESP|Linux Kernel 5.x|IPSec VPN on Linux|
|51|AH|OpenBSD Kernel|OpenBSD 6.x (Firewalls)|
|58|ICMPv6|Linux Kernel 6.x|IPv6 Networking (Ubuntu, RHEL)|
|89|OSPF|Cisco IOS Kernel|Cisco Routers and Switches|

---

### 🧪 Example Commands

```bash
# Basic OS detection
nmap -O 192.168.1.51

# OS detection with guess on domain
nmap -O --osscan-guess example.com

# OS scan with limit
nmap -O --osscan-limit 10.10.10.10

# Verbose OS scan
nmap -O -v 192.168.1.1

# Very verbose: version + TCP connect + OS on all ports
nmap -vv -sV -sT -O -p- 192.168.1.44

# Very verbose: version + TCP connect + OS on port 80
nmap -vv -sV -sT -O -p 80 192.168.1.44

# Verbose: TCP connect + version + OS + osscan-limit on port 445
nmap -v -sT -sV -O --osscan-limit -p 445 192.168.1.51

# Verbose: TCP connect + version + OS + osscan-guess on port 445
nmap -v -sT -sV -O -p 445 --osscan-guess 192.168.1.51

# Verbose: TCP connect + version + OS + osscan-guess on all ports
nmap -v -sT -sV -O -p- --osscan-guess 192.168.1.51

# Very verbose: version + TCP connect + OS + aggressive on all ports
nmap -vv -sV -sT -O -A -p- 192.168.1.44

# Verbose: TCP connect + version + OS + NSE scripts + aggressive
nmap -v -sT -sV -O -A -sC -p- 192.168.1.51

# Full combo: no-ping, TCP connect, version, NSE, aggressive, OS, multi-ports
nmap -v -Pn -sT -sV -sC -A -O \
  -p 21,23,40,53,135,139,443,445,3306,3389,5985,7878,8000,8080,33060,47001,\
49152,49153,49154,49155,49156,49157,49158,49159,49164,49165 \
  192.168.1.51
```

---

## 🗂️ Use Cases

|Scenario|Flag Combo|
|---|---|
|Recon running app versions|`-sV`|
|Penetration testing vuln detection|`-sV --version-all`|
|Check for outdated services|`-sV -p-`|
|Identify target OS|`-O`|
|Full recon profile|`-sV -sT -O -A -sC`|
|Firewall/IDS evasion testing|`-Pn -sT -sV -O -A`|

---

> 💡 _Combine `-sV` with `-O` and `-sC` for comprehensive reconnaissance profiles during security assessments._

---

_Notes compiled from: Day 66 — Nmap sV Service Version Detection & OS Detection_