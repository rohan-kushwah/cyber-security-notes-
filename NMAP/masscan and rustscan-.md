# 🛰️ Network Reconnaissance Cheatsheet

> _High-speed scanning tools for modern security assessments_

---

## ⚡ MASSCAN — Internet-Scale TCP Port Scanner

> Masscan can transmit up to **10 million packets/sec** — scans the entire internet in under 5 minutes.

### 📦 Installation

```bash
# Debian / Ubuntu
apt-get update
apt-get install -y git make gcc
git clone https://github.com/robertdavidgraham/masscan
cd masscan && make && make install

# Quick via package manager
apt install masscan
```

---

### 🔧 Basic Usage

|Task|Command|
|---|---|
|Help|`masscan -h` or `masscan --help`|
|nmap-compatible options|`masscan --nmap`|
|Scan ports on subnet + IPv6|`masscan -p80,8000-8100 10.0.0.0/8 2603:3001:2d00:da00::/112`|
|All ports on single IP|`masscan -p0-65535 192.168.1.1 --max-rate 2000`|
|All ports on subnet|`masscan -p0-65535 192.168.1.1/24 --max-rate 2000`|
|Banner grabbing|`masscan -p- --max-rate 2000 --banners 192.168.1.51`|
|Top 20 common ports on subnet|`masscan 192.168.1.0/24 -p21,22,23,25,53,80,110,135,139,143,443,445,993,995,1723,3306,3389,5900,8080 --rate 100000 -oL results.txt`|
|Multi-target with custom user-agent|`masscan 10.0.0.1 10.0.0.2 -p80,443 --banners --http-user-agent "Mozilla/5.0 (Windows NT 10.0; Win64; x64)" --rate 100000 --source-ip 192.168.100.200 -oJ banners.json`|

---

### ⚙️ Important Options

```
-p / --ports <ports>        Specify ports or ranges  (e.g., -p80,443,8000-8100)
--max-rate <rate>           Max packets/sec          (default: 100)
--banners                   Enable banner grabbing
--source-ip <ip>            Use different source IP  (avoid OS RST conflicts)
--source-port <port>        Use specific source port
--exclude <ip/range>        Exclude IPs from scanning
--excludefile <file>        File with IPs/ranges to exclude
-oX <file>                  Output as XML
-oJ <file>                  Output as JSON
-oG <file>                  Output in grepable format
-oL <file>                  Output as simple list
```

---

### 📡 Banner Grabbing — Supported Protocols

```
FTP  •  HTTP  •  IMAP4  •  Memcached  •  POP3  •  SMTP
SSH  •  SSL   •  SMBv1  •  SMBv2      •  Telnet •  RDP  •  VNC
```

> ⚠️ **Note:** Banner grabbing requires a separate source IP or firewalled source ports to avoid OS TCP/IP stack interference.

---

### 💾 Config File Workflow

```bash
# Generate config from current options
masscan -p80,8000-8100 10.0.0.0/8 --echo > config.conf

# Run from config
masscan -c config.conf --rate 1000
```

---

---

## 🦀 RUSTSCAN — The Modern Port Scanner

> RustScan scans all **65,535 ports in seconds**, with a scripting engine supporting **Python, Lua, and Shell**, and auto-pipes into Nmap for deep analysis.

### 📦 Installation

```bash
# Via Cargo (Rust package manager)
cargo install rustscan
cp ~/.cargo/bin/rustscan /usr/local/bin

# macOS
brew install rustscan

# Arch Linux
yay -S rustscan
```

> Install Rust first: `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`

---

### 🔧 Basic Commands

|Task|Command|
|---|---|
|Help|`rustscan -h`|
|Scan single IP|`rustscan -a 192.168.1.51`|
|All ports on IP|`rustscan -a 192.168.1.51 -r 0-65535`|
|Specific ports|`rustscan -a 192.168.1.51 -p 80,443,8080`|
|Pipe into Nmap `-A`|`rustscan -a 192.168.1.45 -r 1-65535 -- -A`|
|Multiple hosts (grepable)|`rustscan -a 192.168.1.51,192.168.1.217 --accessible > /tmp/scan.gnmap`|
|Subnet with port filter|`rustscan -a 192.168.1.1/24 -p 80,443 -- -oG /tmp/subnet_scan.gnmap`|

---

### 💾 Output & Saving

```bash
# Grepable format
rustscan -a 82.196.42.194 -r 1-65535 --greppable > /tmp/1.gnmap

# Save to file (Nmap grepable)
rustscan -a 192.168.1.51 -r 1-65535 -- -oG /tmp/2.gnmap

# Multiple hosts to file
rustscan -a 192.168.1.51,192.168.1.217 -g > /tmp/3.gnmap

# Scan subnet, save output
rustscan -a 192.168.1.1/24 -p 81,300,591,593 -- -oG /tmp/192.168.1.1

# Accessible hosts only
rustscan -a 192.168.1.51,192.168.1.217 --accessible > /tmp/4.nmap
```

---

### 🔗 htop Integration

```bash
apt install htop
htop -F nmap
```

---

## 🔬 Nmap Service Version Detection Scripts

> Post-scan automation — parse RustScan/Nmap grepable output and run deep service detection.

### 📄 Extract IP and Port List

```bash
vim Extract-IP-and-Port-List.sh
```

```bash
#!/bin/bash
nmap_output_file="/tmp/2.gnmap"
cat $nmap_output_file | while read -r line
do
    ip=$(echo $line | cut -f 1 -d " ")
    echo $line | sed -e 's/\[//g' -e 's/\]//g' | cut -f 3 -d " " | sed -e 's/,/\n/g' | sed -e "s/^/$ip:/g" >> /tmp/ip-port-list.txt
done
```

---

### 🧪 Service and Version Detection

```bash
#!/bin/bash
nmap_output_file="/tmp/4.gnmap"
cat $nmap_output_file | while read -r line
do
    ip=$(echo $line | cut -f 1 -d " ")
    ports=$(echo $line | sed -e 's/\[//g' -e 's/\]//g' | cut -f 3 -d " ")
    nmap -v -Pn -sT -sV -sC -A -O $ip -p $ports
done
```

---

### 🔗 Resources

- GitHub: [https://github.com/RustScan/RustScan](https://github.com/RustScan/RustScan)
- Releases: [https://github.com/RustScan/RustScan/releases](https://github.com/RustScan/RustScan/releases)
- Rust Installation: [https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install)
- RustScan Guide: [https://github.com/RustScan/RustScan/wiki](https://github.com/RustScan/RustScan/wiki)

---

## 🆚 Quick Comparison

|Feature|Masscan|RustScan|
|---|---|---|
|Speed|~10M pkts/sec|65K ports in seconds|
|Nmap Integration|❌|✅ Auto-pipe|
|Banner Grabbing|✅|Via Nmap (`--`)|
|Scripting Engine|❌|✅ Python/Lua/Shell|
|IPv6 Support|✅|Limited|
|Output Formats|XML, JSON, grepable, list|grepable, Nmap formats|
|Best For|Internet-scale recon|Fast local/subnet scans|

---

> ⚠️ **Legal Reminder:** Only scan networks and systems you own or have explicit written permission to test. Unauthorized scanning is illegal.

---

_Cheatsheet compiled from Network Reconnaissance Scanning notes — Sep 2025_