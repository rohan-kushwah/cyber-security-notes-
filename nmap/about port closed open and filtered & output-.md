# 🔍 Nmap Port Scanning & Output

> _A complete reference for network reconnaissance, penetration testing & recon_

---

## 🚦 Port States (TCP SYN Scan)

### ✅ Open Port

```
Client (SYN)    ──▶  Server
Client          ◀──  (SYN/ACK)  Server
Client (RST)    ──▶  Server
```

- **Meaning:** Application **listens** on port.
- 💡 _Tip: RST avoids completing the connection → used in half-open scans._

---

### ❌ Closed Port

```
Client (SYN)    ──▶  Server
Client          ◀──  (RST/ACK)  Server
```

- **Meaning:** No service running, but **host is reachable**.

---

### 🔒 Filtered Port

```
Client (SYN)    ──▶  Server
                     (no response)
```

- **Meaning:** Cannot conclusively classify port as open or closed due to lack of feedback.
- 💡 _Tip: Sometimes filtered ports generate ICMP unreachable messages or other error responses, which Nmap can also interpret as filters. Distinguishing between "filtered" and "no response" can provide clues about network security controls in place._

---

## 🛠️ Common Nmap Commands

### 🔎 Basic Scans

|Description|Command|
|---|---|
|Simple scan|`nmap 192.168.1.51`|
|Verbose output|`nmap -v 192.168.1.51`|
|Max verbosity|`nmap -vvv 192.168.1.51`|
|Debug mode (internal details)|`nmap -d 192.168.1.36`|
|Extra debug|`nmap -ddd 192.168.1.235`|

---

### 🎯 Port Selection

| Description                       | Command                                      |
| --------------------------------- | -------------------------------------------- |
| Scan top 50 common ports          | `nmap -v 192.168.1.51 --top-ports 50`        |
| Show only open ports              | `nmap -v 192.168.1.51 --top-ports 50 --open` |
| Scan FTP port 21 with explanation | `nmap -p 21 192.168.1.35 --reason`           |
| View packet flow in detail        | `nmap -v 192.168.1.36 -p 80 --packet-trace`  |

---

### 📤 Output Control

| Description                                     | Command                                                |
| ----------------------------------------------- | ------------------------------------------------------ |
| Normal text output                              | `nmap -v 192.168.1.36 -oN windowsserver.nmap`          |
| XML output                                      | `nmap -v 192.168.1.36 -oX windowsserver.xml`           |
| Scripting output                                | `nmap -v 192.168.1.36 -oS windowsserver.scri`          |
| Grep-friendly                                   | `nmap -v 192.168.1.36 -oG windowsserver.grepable`      |
| All formats at once (`.nmap`, `.xml`, `.gnmap`) | `nmap -v 192.168.1.36 -oA winserver`                   |
| Append instead of overwrite                     | `nmap 192.168.1.36 -oN winserver.nmap --append-output` |

---

### 🔄 Batch & Resume Scans

```bash
nmap 157.191.64.0/18 -oN n1   # Scan large subnet
nmap --resume n1               # Resume interrupted scan
```

---

### 📊 Custom Reports

**Convert XML output into HTML:**

```bash
xsltproc 1.xml -o 1.html
```

**Apply bootstrap stylesheet for rich HTML:**

```bash
nmap -v -sV -A -sC 192.168.1.1/24 \
  --stylesheet https://raw.githubusercontent.com/honze-net/nmap-bootstrap-xsl/master/nmap-bootstrap.xsl \
  -oX mynetwork.xml
```

---

### 🧹 Parsing with Grep & Sed

Extract open ports from `.gnmap` results:

```bash
egrep -v "^#|Status: Up" nmap-out.gnmap \
  | cut -d' ' -f4- \
  | sed -n -e 's/Ignored.*//p' \
  | tr ',' '\n' \
  | egrep -v "closed|filtered" \
  | sed -e 's/^[ \t]*//' \
  | sort -n | uniq -c | sort -k 1 -r
```

---

## 💡 Practical Tips

- Use `--reason` → See **why** Nmap marked port as open/closed/filtered.
- Add `--packet-trace` → View every packet Nmap sends/receives.
- Use `--top-ports` for **quick scans**, `-p-` for **full scans** (all 65,535 ports).
- Combine with `-sV` (service/version detection) & `-A` (aggressive: OS + traceroute + script scan).

---

## 📋 Quick Reference Table

|Flag|Purpose|
|---|---|
|`-v` / `-vvv`|Verbose / Max verbosity|
|`-d` / `-ddd`|Debug / Extra debug|
|`-p <port>`|Specific port scan|
|`--top-ports N`|Top N common ports|
|`--open`|Show only open ports|
|`--reason`|Explain port state|
|`--packet-trace`|Show packet flow|
|`-oN`|Normal text output|
|`-oX`|XML output|
|`-oG`|Grepable output|
|`-oA`|All formats|
|`--append-output`|Append to existing file|
|`--resume <file>`|Resume interrupted scan|
|`-sV`|Service/version detection|
|`-A`|Aggressive scan|

---

> 🛡️ _Use responsibly. Always obtain proper authorization before scanning any network._