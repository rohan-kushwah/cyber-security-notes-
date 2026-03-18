# 🔭 Naabu — Fast & Simple Port Scanner

> _A lightweight, Go-powered port scanner built for attack surface discovery workflows_

---

## ✨ Features

|Feature|Details|
|---|---|
|🔌 Scan Types|Fast SYN / CONNECT / UDP port scanning|
|🌐 DNS Support|DNS port scan with automatic IP deduplication|
|📡 IP Versions|IPv4 and experimental IPv6 support|
|🕵️ Passive Enum|Passive port enumeration via Shodan InternetDB|
|🧭 Host Discovery|Experimental, with multiple probe options|
|🔗 Nmap Integration|Auto-pipe into Nmap for service discovery|
|📥 Input Methods|STDIN, hostnames, IPs, CIDR blocks, ASNs|
|📤 Output Formats|JSON, text, CSV, STDOUT|
|🛡️ CDN/WAF Exclusion|Limit scans to ports 80/443 on CDN IPs|
|📊 Metrics|Local scan metrics on configurable port|

---

## 📦 Installation

### Prerequisites — Install `libpcap`

```bash
# Linux (Debian/Ubuntu)
apt install libpcap-dev

# macOS
brew install libpcap
```

### Install via Go _(recommended)_

```bash
go install -v github.com/projectdiscovery/naabu/v2/cmd/naabu@latest
```

### Install via Binary Release

```bash
# Download latest release
wget https://github.com/projectdiscovery/naabu/releases/download/v2.3.5/naabu_2.3.5_linux_amd64.zip

# Extract
unzip naabu_2.3.5_linux_amd64.zip

# Move to PATH
cp -v naabu /usr/local/bin
```

---

## 🔧 Basic Usage

|Task|Command|
|---|---|
|Help|`naabu -h`|
|Update Naabu|`naabu -up`|
|Scan hostname|`naabu -host hackerone.com`|
|All TCP ports on host|`naabu -p - -host 192.168.1.51`|
|50 concurrent workers|`naabu -c 50 -p - -host armourinfosec.com`|
|Scan hosts from file|`naabu -list hosts.txt`|
|Scan ASN for ports 80/443|`echo AS14421 \| naabu -p 80,443`|
|Scan all ports on IP|`naabu -p - -host armourinfosec.com`|
|Faster with concurrency|`naabu -c 50 -p - -host armourinfosec.com`|

---

## 🔍 Host Discovery

### Host Discovery Only

```bash
# ARP/ping probes
naabu -sn -host 192.168.1.1/24

# TCP SYN ping on port 80
naabu -wn -ps 80 -host 192.168.1.1/24
```

### Scan Both IPv4 and IPv6

```bash
echo hackerone.com | naabu -iv 4,6 -sa -p 80 -silent
```

---

## 🌐 Perform Basic Network Scans

```bash
# Ping scan
naabu -sn -host 192.168.1.1/24

# Silent mode
naabu -sn -silent -host 192.168.1.1/24
```

### Scan Specific TCP Flags

```bash
# SYN Ping (-ps)
naabu -sn -ps -silent -host 192.168.1.1/24

# ACK Ping (-pa)
naabu -sn -pa -silent -host 192.168.1.1/24
```

---

## 📂 Scanning from Input Files

```bash
# Echo a single IP
echo 192.168.1.1 | naabu

# Scan a subnet
echo 192.168.1.1/24 | naabu

# Scan a single IP address
naabu -host 192.168.1.1
```

---

## ⚙️ Advanced Scanning Options

### Scan with Specific Flags and Types

```bash
# Skip host discovery (-Pn)
naabu -v -Pn -scan-type c -p - -host 192.168.1.51

# Increase concurrency, scan all ports
naabu -v -Pn -scan-type c -c 100 -p - -host 192.168.1.51

# Silent mode with maximum concurrency
naabu -silent -Pn -scan-type c -c 1000 -p - -host 192.168.1.51
```

### Scan from a File

```bash
# Pipe from file
cat ip2.txt | naabu -silent -Pn -scan-type c -c 1000 -p -

# Use -l flag
naabu -l ip.txt -p - -c 1000 -rate 1000
```

### Echo Input for Silent Mode

```bash
echo 192.168.1.51 | naabu -silent -Pn -scan-type c -c 1000
```

---

## 💾 Output Options

```bash
# Save to text file
naabu -host armourinfosec.com -o output.txt

# JSON format
naabu -host armourinfosec.com -json -o output.json

# CSV format
naabu -host armourinfosec.com -csv -o output.csv

# Verify ports and save
naabu -c 50 -p - -verify -host armourinfosec.com
```

---

## 🚀 Customizing Scan Rate & Concurrency

```bash
# Increase rate and concurrency
naabu -p - -host 192.168.1.51 -c 1000 -rate 1000
```

---

## 🔗 Nmap Integration

Run Nmap on discovered ports automatically:

```bash
echo hackerone.com | naabu -nmap-cli 'nmap -sV'
```

> ⚠️ Requires Nmap to be installed: `apt install nmap`

---

## 🛡️ Advanced Options

```bash
# Rate limit packets/sec
naabu -rate 1000 -p - -host 192.168.1.51

# Exclude ports from scan
naabu -p - -exclude-ports 80,443

# CDN/WAF exclusion (only scan ports 80 & 443 on CDN IPs)
naabu -exclude-cdn -host example.com
```

---

## 🧑‍💻 Using Naabu as a Go Library

```go
package main

import (
    "log"
    "context"
    "github.com/projectdiscovery/goflags"
    "github.com/projectdiscovery/naabu/v2/pkg/result"
    "github.com/projectdiscovery/naabu/v2/pkg/runner"
)

func main() {
    options := runner.Options{
        Host:     goflags.StringSlice{"scanme.sh"},
        ScanType: "s", // SYN scan
        // ... more options
    }
    // initialize and run scanner
}
```

---

## 🆚 Naabu vs Other Scanners

|Feature|Naabu|Masscan|RustScan|
|---|---|---|---|
|Language|Go|C|Rust|
|SYN/CONNECT/UDP|✅ All three|SYN only|TCP only|
|Nmap Integration|✅ `-nmap-cli`|❌|✅ `--` flag|
|CDN/WAF Exclusion|✅|❌|❌|
|Shodan Passive|✅|❌|❌|
|ASN Input|✅|❌|❌|
|Output Formats|JSON, CSV, text|XML, JSON, grepable|grepable, Nmap|
|Best For|Bug bounty / recon|Internet-scale|Fast local scans|

---

## 🔗 Resources

- GitHub: [https://github.com/projectdiscovery/naabu](https://github.com/projectdiscovery/naabu)
- ProjectDiscovery Docs: [https://docs.projectdiscovery.io/tools/naabu](https://docs.projectdiscovery.io/tools/naabu)
- Releases: [https://github.com/projectdiscovery/naabu/releases](https://github.com/projectdiscovery/naabu/releases)

---

> ⚠️ **Legal Reminder:** Only scan systems and networks you own or have explicit written permission to test. Unauthorized scanning is illegal.

---

_Cheatsheet compiled from Network Reconnaissance Scanning notes — Sep 2025_