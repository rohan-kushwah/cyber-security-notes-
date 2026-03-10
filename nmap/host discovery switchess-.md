# 🛰️ Nmap Host Discovery Switches Guide

Host discovery determines **which hosts are online before scanning ports**.  
These switches control **how Nmap discovers active machines on a network**.

---

# 📡 Host Discovery Options

---

## 🔹 `-sL` — List Scan

### 📖 Meaning

`-sL` performs a **List Scan**, which simply lists the target IP addresses **without sending any packets to them**.

✔ Useful for:

- Verifying target ranges
    
- Passive reconnaissance
    

### 💻 Command Example

nmap -sL 192.168.1.0/24

### 🔎 What Happens

- No host is scanned
    
- Nmap only lists IP addresses and reverse DNS names.
    

---

## 🔹 `-sn` — Ping Scan (No Port Scan)

### 📖 Meaning

`-sn` disables port scanning and performs **host discovery only** to determine which hosts are online.

✔ Useful for:

- Network mapping
    
- Finding active devices
    

### 💻 Command Example

nmap -sn 192.168.1.0/24

### 🔎 What Happens

Nmap sends discovery probes such as:

- ICMP echo request
    
- TCP SYN to port 443
    
- TCP ACK to port 80
    

Then it reports **alive hosts only**.

---

## 🔹 `-Pn` — Skip Host Discovery

### 📖 Meaning

`-Pn` tells Nmap to **skip host discovery** and treat all hosts as **online**.

✔ Useful when:

- Firewalls block ping
    
- Host discovery fails
    

### 💻 Command Example

nmap -Pn 192.168.1.10

### 🔎 What Happens

Nmap:

1. Assumes host is alive
    
2. Directly starts **port scanning**
    

---

## 🔹 `-PS` — TCP SYN Ping

### 📖 Meaning

Sends a **TCP SYN packet** to specified ports to determine if a host is online.

✔ Useful when ICMP is blocked.

### 💻 Command Example

nmap -PS80,443 192.168.1.0/24

### 🔎 What Happens

If the host replies with:

- SYN/ACK → Host is **alive**
    
- RST → Host is **alive**
    

---

## 🔹 `-PA` — TCP ACK Ping

### 📖 Meaning

Sends **TCP ACK packets** to discover hosts through firewalls.

### 💻 Command Example

nmap -PA80 192.168.1.0/24

### 🔎 What Happens

Host responds with:

- RST → Host is **alive**
    

---

## 🔹 `-PU` — UDP Ping

### 📖 Meaning

Sends **UDP packets** to specified ports to check if a host responds.

### 💻 Command Example

nmap -PU53 192.168.1.0/24

### 🔎 What Happens

Possible responses:

- ICMP Port Unreachable
    
- UDP response
    

Both indicate the host is **alive**.

---

## 🔹 `-PY` — SCTP Ping

### 📖 Meaning

Uses **SCTP INIT packets** for host discovery.

SCTP is used in telecom networks.

### 💻 Command Example

nmap -PY80 192.168.1.0/24

---

# 🌐 ICMP Discovery Probes

---

## 🔹 `-PE` — ICMP Echo Request

### 📖 Meaning

Sends a standard **ping request**.

### 💻 Command Example

nmap -PE 192.168.1.0/24

Equivalent to normal:

ping

---

## 🔹 `-PP` — ICMP Timestamp Request

### 📖 Meaning

Requests **system timestamp** from the host.

### 💻 Command Example

nmap -PP 192.168.1.0/24

---

## 🔹 `-PM` — ICMP Netmask Request

### 📖 Meaning

Requests the **network mask** of the target system.

### 💻 Command Example

nmap -PM 192.168.1.0/24

---

# 🔌 `-PO` — IP Protocol Ping

### 📖 Meaning

Sends **IP packets with different protocol numbers** to determine if a host is online.

### 💻 Command Example

nmap -PO1,2,4 192.168.1.0/24

### 🔎 Example Protocol Numbers

|Number|Protocol|
|---|---|
|1|ICMP|
|6|TCP|
|17|UDP|

---

# 🌍 DNS Resolution Options

---

## 🔹 `-n` — No DNS Resolution

### 📖 Meaning

Prevents Nmap from resolving hostnames.

✔ Makes scanning **faster**.

### 💻 Command Example

nmap -n 192.168.1.0/24

---

## 🔹 `-R` — Always Resolve DNS

### 📖 Meaning

Forces **reverse DNS resolution** for every target.

### 💻 Command Example

nmap -R 192.168.1.0/24

---

## 🔹 `--dns-servers`

### 📖 Meaning

Specify **custom DNS servers**.

### 💻 Command Example

nmap --dns-servers 8.8.8.8,1.1.1.1 scanme.nmap.org

---

## 🔹 `--system-dns`

### 📖 Meaning

Use the **operating system DNS resolver** instead of Nmap's internal DNS system.

### 💻 Command Example

nmap --system-dns scanme.nmap.org

---

# 🛣️ `--traceroute` — Network Path Discovery

### 📖 Meaning

Performs a **traceroute** to determine the path packets take to the target host.

### 💻 Command Example

nmap --traceroute scanme.nmap.org

### 🔎 Output Shows

- Routers between you and the target
    
- Network hops
    
- Latency
    

---

# ⚡ Example Combined Command

nmap -sn -PE -PS80,443 -PA80 --traceroute 192.168.1.0/24

### ✔ This Command Will

- Perform host discovery
    
- Use ICMP echo
    
- Send TCP SYN to ports 80 and 443
    
- Send TCP ACK to port 80
    
- Show traceroute to each host
    

---

# 🧠 Quick Summary

|Switch|Purpose|
|---|---|
|`-sL`|List targets only|
|`-sn`|Ping scan (no ports)|
|`-Pn`|Skip host discovery|
|`-PS`|TCP SYN ping|
|`-PA`|TCP ACK ping|
|`-PU`|UDP ping|
|`-PY`|SCTP ping|
|`-PE`|ICMP echo ping|
|`-PP`|ICMP timestamp|
|`-PM`|ICMP netmask|
|`-PO`|IP protocol ping|
|`-n`|Disable DNS resolution|
|`-R`|Always resolve DNS|
|`--dns-servers`|Use custom DNS|
|`--system-dns`|Use OS DNS|
|`--traceroute`|Show network path|

---

✅ If you want, I can also make a **full 150+ Nmap switches cybersecurity cheatsheet (.md)** used in **penetration testing, bug bounty, and CEH**.



# host discovery workflow=
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