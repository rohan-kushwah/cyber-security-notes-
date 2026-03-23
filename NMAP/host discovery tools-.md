# 🔍 Network Reconnaissance & Scanning

> _A field guide to network discovery tools for security professionals_

---

## 📡 nbtscan — NetBIOS Network Scanner

`nbtscan` is a command-line tool used to scan a network and collect information about NetBIOS-enabled devices, such as **hostnames**, **MAC addresses**, and **logged-in users**.

It works by sending **NetBIOS Name Service queries** (UDP port 137) across a subnet or IP range.

---

### 🔌 Core NetBIOS Ports

|Port|Service|Description|
|---|---|---|
|**UDP 137**|NetBIOS Name Service (NBNS)|Resolves NetBIOS names to IP addresses|
|**UDP 138**|NetBIOS Datagram Service|Handles broadcasts for messages and browsing|
|**TCP 139**|NetBIOS Session Service|Direct communication, often used by SMB for **file & printer sharing**|

---

### ⚙️ Common `nbtscan` Usage

```bash
# Scan the entire subnet
nbtscan 192.168.1.1/24
# 👉 Queries all IPs from 192.168.1.1 to 192.168.1.254

# Scan a specific IP range
nbtscan 192.168.1.1-100
nbtscan 192.168.1.200-254

# Recursive scan (rescans automatically after first pass)
nbtscan -r 192.168.1.1/24

# Custom output formatting (uses : as separator instead of whitespace)
nbtscan 192.168.1.1/24 -s :
```

---

### 🆚 Difference Between `nbtstat` and `nbtscan`

|Feature/Aspect|`nbtstat` _(Windows built-in)_|`nbtscan` _(3rd-party, cross-platform)_|
|---|---|---|
|**Purpose**|Query NetBIOS info on local or specific remote host|Sweep entire networks / IP ranges for NetBIOS info|
|**Scope**|Single machine focus|Network-wide reconnaissance|
|**Platform**|Windows only|Linux, macOS, Windows|
|**Examples**|`nbtstat -n` → local names<br>`nbtstat -a <IP>` → remote host info|`nbtscan 192.168.1.1/24` → scans whole subnet|
|**Output**|NetBIOS name tables, cache, current sessions|Hostnames, MAC addresses, usernames (bulk output)|
|**Use Case**|Troubleshooting or small-scale checks|Security auditing, network mapping, penetration testing|

---

## 🖥️ `nbtstat` — NetBIOS Statistics Tool _(Windows)_

`nbtstat` is a **Windows built-in command** used to display NetBIOS over TCP/IP (NetBT) statistics, NetBIOS name tables, and name cache details. It is commonly used for **diagnostics and troubleshooting NetBIOS name resolution**.

---

### 🛠️ Common `nbtstat` Commands

```bash
# Display basic NetBIOS statistics and help options
nbtstat

# Show the NetBIOS names registered on the local machine
nbtstat -n

# Display the NetBIOS name cache (recently resolved names)
nbtstat -c

# Purge and reload the NetBIOS name cache from the LMHOSTS file
nbtstat -R

# Display the NetBIOS names table for a remote system using its hostname
nbtstat -a <hostname>
nbtstat -a 192.168.1.115

# Display the NetBIOS names table for a remote system using its IP address
nbtstat -A <IP>
nbtstat -A 192.168.1.115
```

> 💡 **Tip:**
> 
> - Use `-a` when you know the **hostname**
> - Use `-A` when you only have the **IP address**

---

## 🌐 `netdiscover` — ARP-based Network Discovery Tool

`netdiscover` is a command-line network scanner that identifies live hosts on a local network by sending or listening for **ARP (Address Resolution Protocol)** requests. It is widely used in **penetration testing**, **network reconnaissance**, and **administration** because it is fast, lightweight, and effective on LANs.

---

### 🔄 Modes of Operation

|Mode|Description|Advantage|Limitation|
|---|---|---|---|
|**🟡 Passive**|Listens for ARP packets without sending requests|Stealthy (harder to detect by IDS/IPS)|Only sees hosts that are already communicating|
|**🟢 Active**|Sends ARP requests to a given range or subnet|Quickly discovers all live hosts|Easier to detect on monitored networks|

---

### ⚙️ Common Usage Examples

```bash
# Passive scan (default) — listens on the default interface
netdiscover

# Active scan on a subnet
netdiscover -r 192.168.1.1/24
# 👉 Scans all hosts in the subnet 192.168.1.1 – 192.168.1.254

# Specify a network interface
netdiscover -i eth0 -r 192.168.1.1/24
# 👉 Runs an active scan via interface eth0

# Printable/clean output (for documentation or exporting)
netdiscover -p

# Fast scan mode (sends multiple ARP requests concurrently)
netdiscover -f -r 192.168.1.1/24
# 👉 Useful for large or congested networks
```

---

### 📊 Output Information

- 🖧 IP address of discovered hosts
- 🔑 MAC address of each device
- 🏭 Vendor/manufacturer (from MAC prefix / OUI lookup)
- 🏷️ Hostnames if resolvable

---

### 🎯 Use Cases

- Quickly map devices on a local subnet
- Detect rogue or unauthorized hosts
- Build a network inventory for audits
- Troubleshoot connectivity issues
- Stealthy reconnaissance during penetration tests

---

### 📋 Example Active Scan Output

```
Currently scanning: 192.168.1.0/24 | Screen View: Unique Hosts

 IP              At MAC Address      Count  Len   MAC Vendor
 192.168.1.5     00:1A:2B:3C:4D:5E   5      60    Intel Corporate
 192.168.1.10    00:1E:68:9A:BC:DE   3      60    Cisco Systems
```

> ⚠️ **Note:**
> 
> - `netdiscover` requires **root privileges** to send/receive ARP packets
> - Active scans are **faster** but **more detectable**
> - Passive scans are **stealthier** but depend on existing ARP traffic

---

## 🛤️ Traceroute-related Tools

These tools trace the path that packets take to reach a destination by sending **probe packets** (ICMP, UDP, or TCP) and measuring responses from each hop (router). They are commonly used for **network troubleshooting**, **latency analysis**, and **connectivity diagnostics**.

---

### `traceroute` _(Linux/macOS)_

- Default on Linux/macOS
- Sends **UDP packets** (Linux/macOS) or **ICMP** (on some systems) to a target
- Displays each router (hop) with round-trip response times
- Helps identify where packets are delayed or dropped
- Requires root privileges for certain options (raw sockets)
- Supports protocol customization: UDP, ICMP, TCP

```bash
# Basic usage
traceroute 64.6.64.6

# ICMP mode
traceroute -I 64.6.64.6

# TCP SYN on port 443
traceroute -T -p 443 64.6.64.6
```

---

### `tcptraceroute`

- Works like `traceroute` but sends **TCP SYN packets**
- Useful for **bypassing firewalls** that block ICMP/UDP but allow TCP (e.g., port 80/443)
- Common in **penetration testing** and **firewall debugging**

```bash
tcptraceroute 64.6.64.6 80
```

---

### `tracepath` _(Linux)_

- Linux alternative to `traceroute`
- Sends **UDP packets**, but does **not require root privileges**
- Automatically detects **MTU (Maximum Transmission Unit)** along the path
- Useful for routing and MTU troubleshooting
- Fewer options than `traceroute`, but simpler to use

```bash
tracepath 64.6.64.6
```

---

### `tracert` _(Windows)_

- **Windows built-in** equivalent of traceroute
- Uses **ICMP Echo Requests** only
- Displays hops and round-trip times
- Does **not** support UDP or TCP probes
- Cannot bypass firewalls that block ICMP

```bash
tracert 64.6.64.6
```

---

### 🆚 Key Differences: `traceroute` vs `tracert`

|Feature|`tracert` _(Windows)_|`traceroute` _(Linux/macOS)_|
|---|---|---|
|**Default Protocol**|ICMP Echo Requests|UDP (Linux/macOS) or ICMP (some)|
|**Platform**|Windows only|Linux, macOS|
|**Firewall Bypass**|❌ No|✅ Yes, with TCP/ICMP options|
|**MTU Detection**|❌ No|✅ Yes, with `tracepath`|
|**Requires Root**|❌ No|✅ Yes, for many options|
|**Protocol Options**|ICMP only|UDP, ICMP, TCP|

---

### 📝 Summary

|Tool|Best Used For|
|---|---|
|🗺️ `traceroute` / `tracert`|General path mapping and latency diagnosis|
|🔥 `tcptraceroute`|Bypass ICMP/UDP blocks, test TCP connectivity through firewalls|
|🛣️ `tracepath`|Linux non-root alternative with MTU detection|

---

_📌 These notes cover Network Reconnaissance & Scanning techniques including NetBIOS scanning, ARP-based discovery, and traceroute-related path analysis tools._