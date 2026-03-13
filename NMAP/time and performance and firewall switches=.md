# 🛡️ Nmap Advanced Options

### _Timing, Performance & Firewall Evasion Guide_

---

## ⏱️ Timing Templates (`-T<0-5>`)

> Predefined timing templates balance **speed** versus **stealth**.

|Option|Mode|Description|
|:-:|:-:|:--|
|`-T0`|🐢 **Paranoid**|Serial scan with long delays — _most stealthy_|
|`-T1`|🦊 **Sneaky**|Very slow scan with long delays|
|`-T2`|🐾 **Polite**|Slower scan to reduce bandwidth usage|
|`-T3`|⚖️ **Normal**|Default timing balance|
|`-T4`|🚀 **Aggressive**|Faster scan, may trigger IDS/IPS|
|`-T5`|⚡ **Insane**|Fastest scan, likely causes packet loss|

### 📋 Examples

```bash
nmap -v -sT -T4 -p- 192.168.1.50   # Aggressive TCP scan
nmap -T4 -sS 192.168.1.1            # SYN scan with aggressive timing
nmap -v -sS -T1 -p- 192.168.1.151  # Sneaky SYN scan
nmap -v -T0 -p- 192.168.1.151      # Paranoid stealth scan
nmap -v -T5 -p- 192.168.1.151      # Insane speed scan
```

---

## 🔀 Parallel Scanning Options

> Control the number of hosts or probes scanned **concurrently**.

|Option|Description|
|:--|:--|
|`--min-hostgroup`|Minimum hosts scanned in parallel|
|`--max-hostgroup`|Maximum hosts scanned in parallel|
|`--min-parallelism`|Minimum probes sent in parallel|
|`--max-parallelism`|Maximum probes sent in parallel|

### 📋 Example

```bash
# Scans subnet with up to 20 simultaneous probes
nmap --min-hostgroup 10 --max-parallelism 20 192.168.1.1/24
```

---

## 🕐 Round Trip Time (RTT) Timeout Options

> Tune how long Nmap **waits for responses** before retrying or timing out.

|Option|Description|
|:--|:--|
|`--min-rtt-timeout`|Minimum wait time for response|
|`--max-rtt-timeout`|Maximum wait time for response|
|`--initial-rtt-timeout`|Initial RTT timeout estimate|

### 📋 Example

```bash
# Limits RTT timeout to 500 milliseconds
nmap --max-rtt-timeout 500ms 192.168.1.1
```

---

## 🔄 Retries and Scan Timeouts

> Configure retries and host scanning durations to manage **slow or unresponsive hosts**.

|Option|Description|
|:--|:--|
|`--max-retries`|Maximum retries for failed probes|
|`--host-timeout`|Abort scanning a host if it exceeds time limit|
|`--scan-delay`|Minimum delay between sending probes|
|`--max-scan-delay`|Maximum delay between probes|

### 📋 Example

```bash
# Limits retries to 2 and aborts hosts after 5 minutes
nmap --max-retries 2 --host-timeout 5m 192.168.1.1
```

---

## 📡 Packet Rate Control

> Control the **speed packets are sent**.

|Option|Description|
|:--|:--|
|`--min-rate`|Minimum packets per second|
|`--max-rate`|Maximum packets per second|

### 📋 Examples

```bash
nmap -v -Pn -sT -p- --max-rate 10 192.168.1.50
nmap --min-rate 1000 --max-rate 5000 192.168.1.1
# ↑ Send packets between 1,000 and 5,000 packets per second
```

---

---

# 🔥 Firewall, IDS & IPS Evasion · Spoofing in Nmap

> Nmap provides multiple techniques to **bypass firewalls**, Intrusion Detection Systems (IDS), and Intrusion Prevention Systems (IPS), aiding stealthy reconnaissance and penetration testing.

---

## 🧱 Stateless vs Stateful Firewalls

### 🔵 Stateless Firewall

> Examines each packet **individually** based solely on predefined rules — no memory of past packets or connection state.

|||
|:--|:--|
|✅ **Advantages**|High speed, low resource consumption; simple config for basic filtering; good for high-throughput environments|
|❌ **Limitations**|Cannot track TCP sessions; susceptible to spoofing & fragmented packet evasion; no context-aware filtering|

### 🟢 Stateful Firewall

> Tracks and records the **state of active connections** (TCP streams, UDP flows) in a state table.

|||
|:--|:--|
|✅ **Advantages**|Stronger security via connection tracking; detects spoofing & session hijacking; simplifies rule management|
|❌ **Limitations**|More resource-intensive (CPU/memory); slightly slower under heavy load; higher configuration complexity|

### 📊 Summary of Differences

|Aspect|🔵 Stateless|🟢 Stateful|
|:--|:--|:--|
|Packet Inspection|Each packet independently|Tracks state & inspects in context|
|Connection Awareness|❌ None|✅ Maintains state tables|
|Security Level|Basic; vulnerable to spoofing|Higher; detects spoofing & hijacking|
|Performance|Faster, less resource-demanding|Slightly slower, more resource-heavy|
|Configuration|Manual, extensive ACLs needed|Simpler with automatic connection tracking|
|Use Cases|Basic filtering, routers, high-throughput|Enterprise security, VPN gateways|

---

## 🕵️ Common Nmap Evasion Techniques

### 🧩 Fragmented Packets (`-f`)

> Splits scan packets into **small IP fragments** making detection harder for simple packet inspectors.

**Why it works:**

- Some firewalls and IDS have trouble reassembling fragmented packets
- Can evade signature-based detection and content inspection
- Bypasses filters that only examine initial fragments

```bash
nmap -f 192.168.1.208                     # Basic fragmented scan
nmap -v -Pn -f 192.168.1.208             # Verbose, skip ping discovery
nmap --mtu 16 192.168.1.1                # Custom MTU (16 bytes)
nmap -v -Pn --mtu 8 -p- 192.168.1.243   # Verbose no-ping, all ports
```

---

### 🎭 Decoy Scanning (`-D`)

> Sends scans from **multiple fake IP addresses** alongside the real scan to confuse logs and hide the real attacker's IP.

```bash
nmap -D 192.168.1.10,192.168.1.20,192.168.1.30 192.168.1.208
nmap -D 192.168.1.10,192.168.1.20,192.168.1.30,192.168.1.7 192.168.1.1
```

---

### 🎯 Spoofing Source IP (`-S`)

> **Fakes the source IP address** to bypass IP-based filtering.

```bash
nmap -S 192.168.1.100 192.168.1.1
nmap -v -Pn -p- 192.168.1.1 -e eth0 -S 192.168.1.200
```

> ⚠️ _Note: Requires direct network access and proper routing._

---

### 🔀 Spoofing MAC Address (`--spoof-mac`)

> **Changes MAC address** to evade detection or impersonate trusted devices.

```bash
nmap --spoof-mac 00:11:22:33:44:55 192.168.1.1
nmap -v -Pn 192.168.1.40 --spoof-mac cisco
```

---

_Generated from Nmap Timing, Performance & Firewall Evasion study notes._