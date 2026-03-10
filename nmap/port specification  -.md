# 🌐 Nmap Port Specification and Scan Order

> A quick guide to **understanding Nmap port data**, extracting **top ports**, and using them efficiently in **network reconnaissance**.

---

# 📚 Understanding Port Specification in Nmap

Nmap reads port and service data from the file:

/usr/share/nmap/nmap-services

Each line in this file contains:

|Field|Description|
|---|---|
|Service Name|Example: `http`, `ftp`, `ssh`|
|Port/Protocol|Example: `80/tcp`, `53/udp`|
|Open Frequency|Decimal value representing how often the port is found open|
|Comments|Optional description|

💡 **Important:**  
The **open-frequency value** helps Nmap determine which ports should be scanned first.

---

# 🔎 Viewing Most Common Ports

### Top TCP ports by frequency

sort -r -k3 /usr/share/nmap/nmap-services | grep tcp | head -n 10

This command:

1. Sorts ports by frequency
    
2. Filters TCP ports
    
3. Shows top 10 common ports
    

---

# ⚙️ Extract Top TCP Ports for Scanning

Generate a **comma-separated list of popular ports**.

awk '$2~/tcp$/' /usr/share/nmap/nmap-services | sort -r -k3 | head -n 100 | awk -F '/tcp' '{print $1}' | awk '{print $2}' | tr '\n' ','

Example output:

80,443,22,21,25,53,110,139,...

You can directly use this in Nmap:

nmap -p 80,443,22 target

---

# 📡 Host Discovery Workflow

Example **multi-step host discovery process**.

### Step 1 — Basic Ping Scan

nmap -v -n -sn 192.168.1.0/24 -oA my-network

Options:

|Option|Meaning|
|---|---|
|`-v`|Verbose|
|`-n`|Disable DNS resolution|
|`-sn`|Ping scan only|
|`-oA`|Output in all formats|

---

### Step 2 — TCP SYN Discovery

nmap -v -n -sn -PS21,22,23,25,53,80,110,111,135,139,143,443,445,993,995,1723,3306,3389,5900,8080 -iL DOWN_HOSTS_FILE_PATH -oA my-network2

`-PS` → Sends **TCP SYN packets** to specified ports.

---

### Step 3 — TCP ACK Discovery

nmap -v -n -sn -PA21,22,23,25,53,80,110,111,135,139,143,443,445,993,995,1723,3306,3389,5900,8080 -iL DOWN_HOSTS_FILE_PATH -oA my-network3

`-PA` → Sends **TCP ACK packets**.

---

### Step 4 — UDP Discovery

nmap -v -n -sn -PU631,161,137,123,138,1434,445,135,67,53 -iL DOWN_HOSTS_FILE_PATH -oA LIVE_HOSTS_TRY_4

`-PU` → Sends **UDP probes**.

---

# 🎯 Specifying Ports When Scanning

## Single or Multiple Ports

nmap -p 21,22,25,80,443 192.168.1.35

---

## Port Ranges

nmap -p 1-200 192.168.1.35

Multiple ranges:

nmap -p 1-200,443,5000-5050 192.168.1.35

---

## Scan All Ports

nmap -p 0-65535 192.168.1.235

or

nmap -p- 192.168.1.31

---

## Separate TCP and UDP Ports

nmap -p T:80,443,U:53,161 192.168.1.51

---

## Scan Top Most Common Ports

nmap --top-ports 20 192.168.1.31

---

# 🚫 Excluding Ports

Skip specific ports during scanning.

nmap -p- --exclude-ports 9100 192.168.1.105

Example excluding multiple ports:

nmap -p- --exclude-ports 80,83,85 192.168.1.51




# 🔍 Nmap — Port Specification & Scan Order

> _Precision targeting for network reconnaissance_

---

## 📡 Scan Both TCP & UDP

```bash
nmap -sU -sT -p T:80,443,U:53,161   192.168.1.235
nmap -sZ -p S:80,443                 192.168.1.235
```

---

## 🏆 Top N Most Common Ports

> Scan only the most frequently exposed ports — fast and effective.

|Command|Description|
|---|---|
|`nmap --top-ports 20 192.168.1.31`|Top 20 ports (any protocol)|
|`nmap -sT --top-ports 200 192.168.1.235`|Top 200 TCP ports|
|`nmap -sU --top-ports 20 192.168.1.235`|Top 20 UDP ports|

---

## 🚫 Exclude Ports

> Avoid scanning specific ports — useful when targeting sensitive or noisy services.

```bash
# Exclude a single noisy port
nmap -p- --exclude-ports 9100  192.168.1.105

# Exclude multiple ports from a range
nmap -p 79-99 --exclude-ports 80,83,85  192.168.1.51
```

---

## 🏷️ Port Name Patterns

> Specify ports by **service name** or with **wildcards** — no need to remember numbers.

```bash
nmap -p http,ftp,smb   192.168.1.1     # Named services
nmap -p '*http'        192.168.1.31    # Wildcard prefix
nmap -p 'ftp*'         192.168.1.31    # Wildcard suffix
nmap -p 'ftp*'         192.168.1.235
nmap -p 'proxy*'       192.168.1.235
nmap -p '*proxy'       192.168.1.235
```

---

## 📊 Filtering Ports by Open Frequency

> The `--port-ratio` option filters ports based on their **open-frequency**.

```bash
# Scan only ports with open-frequency ≥ 6%
nmap --port-ratio 0.06   192.168.1.207
```

> 💡 **Tip:** Set ratio to `0` or omit to disable filtering and scan **all available ports**.

---

## 🖨️ Handling Special Ports (e.g., Printers)

> ⚠️ Port **9100** (printer services) is known to "print" any payload, causing noise in version detection.

- Nmap **excludes** such ports by default during version detection
- Override with `--allports` to force-include them

```bash
nmap -sV --allports   192.168.1.105
```

---

## 🔀 Combining TCP and UDP Scans on Specific Ports

> Scan both TCP and UDP common service ports simultaneously:

```bash
nmap -v -sU -sT -p T:80,23,443,21,22,25,U:53,161,137,123   192.168.1.51
```

---

## 📋 Top Ports Reference Lists

### 🔵 Top 20 TCP Ports

```
80, 443, 23, 21, 22, 25, 3389, 110, 445, 139, ...
```

### 🟠 Top 20 UDP Ports

```
53, 161, 137, 123, 138, 1434, 445, 135, 67, 139, ...
```

---

## 🗂️ Quick Reference Summary

|Feature|Syntax|Example|
|---|---|---|
|Range|`-p 1-1024`|`nmap -p 1-1024 host`|
|Specific|`-p 22,80,443`|`nmap -p 22,80 host`|
|All ports|`-p-`|`nmap -p- host`|
|By name|`-p http,ftp`|`nmap -p http host`|
|Wildcard|`-p 'ftp*'`|`nmap -p 'ftp*' host`|
|Top N|`--top-ports N`|`nmap --top-ports 100 host`|
|Exclude|`--exclude-ports`|`nmap -p- --exclude-ports 9100 host`|
|Ratio filter|`--port-ratio 0.x`|`nmap --port-ratio 0.1 host`|

---

_📁 Part of: `Network-Reconnaissance-Scanning / Nmap / Port-Specification-and-Scan-Order`_
---

# 💡 Pro Tips

✔ Use **top ports** when speed matters  
✔ Use **full scan (`-p-`)** when doing deep recon  
✔ Combine **TCP + UDP scans** for better coverage  
✔ Always run **host discovery before full scans**

---

# 📁 Recommended Recon Workflow

1️⃣ Host Discovery  
   nmap -sn network  
  
2️⃣ Identify live hosts  
  
3️⃣ Fast scan (top ports)  
   nmap --top-ports 100  
  
4️⃣ Deep scan  
   nmap -p-  
  
5️⃣ Service detection  
   nmap -sV