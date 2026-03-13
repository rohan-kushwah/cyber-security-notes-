# 🕵️ TCP SYN Stealth Scan (`-sS`)

> _"The ghost of network scanning — it knocks, but never enters."_

---

## ⚡ What Is It?

The **TCP SYN Stealth Scan** is one of the most widely used and effective scanning techniques in penetration testing and network reconnaissance.

It is called a **half-open scan** because it **never completes** the full TCP three-way handshake.

---

## 🔄 How the `-sS` Scan Works

### 🟢 Open Port

```
Client  SYN    ──▶  Server
Client  ◀── SYN/ACK  Server
Client  RST    ──▶  Server
```

|Step|Who|Packet|Direction|
|---|---|---|---|
|1|Client|`SYN`|→ Server|
|2|Server|`SYN/ACK`|→ Client|
|3|Client|`RST`|→ Server|

> Instead of completing with `ACK`, the client sends `RST` to kill the connection immediately.

---

### 🔴 Closed Port

```
Client  SYN      ──▶  Server
Client  ◀── RST/ACK   Server
```

The server responds with `RST/ACK` — no service is listening.

---

### 🟡 Filtered Port

```
Client  SYN  ──▶  Server
         (no response)
```

No reply — a **firewall or security device** is blocking the request.

---

## ✅ Advantages

|Advantage|Description|
|---|---|
|🔕 **No Full Connection**|Handshake never completes → often **not logged** in application logs|
|👻 **Stealthier**|Many IDS/firewalls don't log half-open connections as thoroughly|
|🚀 **Fast & Efficient**|Can scan **thousands of ports** quickly|

---

## ❌ Disadvantages

|Disadvantage|Description|
|---|---|
|🔐 **Requires Root/Admin**|Raw packets need **elevated privileges**|
|📡 **RST Packets Detectable**|Frequent resets may be caught by **sophisticated monitors**|
|🚨 **Not Fully Undetectable**|Modern IDS/firewalls can log SYN patterns or **rate-limit** SYN packets|

---

## 🎯 Use Cases

- 🕵️ **Quiet reconnaissance** where stealth is critical
- 🔓 **Identifying open ports** without triggering firewall alerts or extensive logs
- 🗺️ **Mapping active services** before further penetration testing
- 🛡️ **Hardened network scanning** where full connect scans would be flagged

---

## 💻 Example Nmap Commands

```bash
# Basic verbose SYN scan (default for root)
nmap -v 192.168.1.46

# Scan port 23 with verbose output using SYN scan
nmap -v -sS -p 23 192.168.1.46

# Scan port 80 verbosely using SYN scan
nmap -v -sS -p 80 192.168.1.46
```

---

## 🧠 Summary

```
┌─────────────────────────────────────────────────────┐
│  Speed ████████████████████░░  Fast                 │
│  Stealth ███████████████████░░  High                │
│  Accuracy ████████████████████  Excellent           │
│  Privilege Required: ✔ Root/Admin                   │
└─────────────────────────────────────────────────────┘
```

> _This technique balances **speed**, **stealth**, and **accuracy** — making it the go-to scanning method for many security professionals._

---

_📁 Path: `Network-Reconnaissance-Scanning / Nmap / Nmap-Scan-Techniques / TCP-SYN-Stealth-Scan(-sS)`_

---

---

# 🔗 TCP Connect Scan (`-sT`)

> _"The polite knock — it shakes hands, says hello, then hangs up."_

---

## ⚡ What Is It?

The **TCP Connect Scan** is the **default scanning method** used by Nmap when the stealthier SYN scan (`-sS`) is unavailable — such as when running **without root or administrator privileges**.

Unlike `-sS`, this scan **completes the full TCP three-way handshake** before terminating the connection.

---

## 🔄 How the `-sT` Scan Works

### 🟢 Open Port

```
Client  SYN    ──▶  Server
Client  ◀── SYN/ACK  Server
Client  ACK    ──▶  Server
Client  RST    ──▶  Server
```

|Step|Who|Packet|Direction|
|---|---|---|---|
|1|Client|`SYN`|→ Server|
|2|Server|`SYN/ACK`|→ Client|
|3|Client|`ACK`|→ Server|
|4|Client|`RST`|→ Server|

> The full handshake is completed before the client resets — the port is confirmed **open**.

---

### 🔴 Closed Port

```
Client  SYN      ──▶  Server
Client  ◀── RST/ACK   Server
```

Server responds with `RST/ACK` — no service listening on that port.

---

### 🟡 Filtered Port

```
Client  SYN  ──▶  Server
         (no response)
```

No response received — a **firewall is blocking** the packets.

---

## ✅ Advantages

|Advantage|Description|
|---|---|
|🔓 **No Special Privileges**|Uses standard OS system calls — **no root/raw socket access** needed|
|✔️ **Reliable**|Completes full connection, so multiple services may respond differently|

---

## ❌ Disadvantages

|Disadvantage|Description|
|---|---|
|📋 **Easily Logged**|Firewalls, IDS, and server logs **detect and record** the full connection|
|🐢 **Slower than SYN**|Completing the full handshake adds **overhead** compared to half-open scans|

---

## 🎯 Use Cases

- 🖥️ Running scans where **elevated privileges are unavailable**
- 📢 When **stealth is not a priority** and logs are not a concern
- 🔬 Situations where **service reactions to full vs. half-open connections** is valuable

---

## 💻 Example Commands

```bash
# Verbose TCP Connect scan on port 80
nmap -v -sT -p 80 192.168.1.46

# Verbose TCP Connect scan on port 23
nmap -v -sT -p 23 192.168.1.46
```

---

## 🧠 Summary

```
┌─────────────────────────────────────────────────────┐
│  Speed   ██████████████░░░░░░  Moderate             │
│  Stealth ████████░░░░░░░░░░░░  Low                  │
│  Accuracy ████████████████████  Excellent           │
│  Privilege Required: ✘ None (uses OS syscalls)      │
└─────────────────────────────────────────────────────┘
```

> _Prioritizes **accessibility and simplicity** over stealth and speed — a reliable fallback when SYN scans are restricted._

---

_📁 Path: `Network-Reconnaissance-Scanning / Nmap / Nmap-Scan-Techniques / TCP-Connect-Scan(-sT)`_

---

---

# 🛡️ TCP ACK Scan (`-sA`)

> _"The interrogator — it asks if the firewall is watching, not whether the door is open."_

---

## ⚡ What Is It?

The **TCP ACK Scan** is primarily used to **probe firewall rules** and **map network filtering**. Unlike SYN or Connect scans, it does **not attempt to establish or complete a TCP connection** — it sends an ACK packet to determine whether ports are **filtered or unfiltered**.

---

## 🔄 How the `-sA` Scan Works

### 🟢 Unfiltered Port (Open or Closed)

```
Client  ACK  ──▶  Server
Client  ◀── RST   Server
```

|Step|Who|Packet|Meaning|
|---|---|---|---|
|1|Client|`ACK`|→ Server|
|2|Server|`RST`|Port is **not filtered** by firewall|

> ⚠️ This response is the **same whether the port is open or closed** — this scan only indicates **filtering status**, not port state.

---

### 🔴 Filtered Port

```
Client  ACK  ──▶  Server
         (no response)
```

No reply — the packet was **dropped or filtered** by a firewall or similar mechanism.

---

## ✅ Advantages

|Advantage|Description|
|---|---|
|🔥 **Firewall Detection**|Efficiently identifies which ports are **filtered (blocked) or unfiltered** by firewalls|
|👻 **Stealthier**|Does not attempt to open connections, reducing alerts by IDS/IPS|
|🔓 **Bypasses Some Stateful Firewalls**|Some stateful firewalls block SYN but allow ACK packets, revealing **hidden filtering rules**|

---

## ❌ Disadvantages

|Disadvantage|Description|
|---|---|
|❓ **Cannot Detect Port State**|Does not determine if a port is **open or closed**, only filtered/unfiltered|
|🚫 **May Be Blocked**|Some firewalls and IPS drop unexpected ACK packets, **limiting effectiveness**|

---

## 🎯 Use Cases

- 🔍 Penetration testing focused on **firewall rule discovery**
- 🗺️ Network mapping and understanding **filtering policies**
- 🔗 Often combined with other scans (e.g., SYN `-sS`, Connect `-sT`, Null `-sN`) for comprehensive analysis

---

## 💻 Example Command

```bash
nmap -v -sA -p 23 192.168.1.46
```

---

## 🧠 Summary

```
┌─────────────────────────────────────────────────────┐
│  Purpose: Firewall/Filter Detection ONLY            │
│  Stealth  ██████████████████░░  High                │
│  Port State Detection: ✘ Cannot determine open/closed│
│  Privilege Required: ✔ Root/Admin                   │
└─────────────────────────────────────────────────────┘
```

> _Valuable for **stealthy reconnaissance** to understand network perimeter defenses without triggering full connection attempts._

---

_📁 Path: `Network-Reconnaissance-Scanning / Nmap / Nmap-Scan-Techniques / TCP-ACK-Scan(-sA)`_

---

---

# 🪟 TCP Window Scan (`-sW`)

> _"The window inspector — reads the size of the gap to know if the door is truly open."_

---

## ⚡ What Is It?

The **TCP Window Scan** is an **advanced variation of the ACK scan** that attempts to **differentiate between open and closed ports** by analyzing the **TCP window size value** in the RST packets sent by the target. This enables better accuracy in detecting port states while maintaining stealth.

---

## 🔄 How the `-sW` Scan Works

### 🟢 Open Port

```
Client  ACK  ──▶  Server
Client  ◀── RST (window size ≠ 0)  Server
```

If the **TCP window size** in the RST packet is **nonzero** → port is considered **open**.

---

### 🔴 Closed Port

```
Client  ACK  ──▶  Server
Client  ◀── RST (window size = 0)  Server
```

If the **TCP window size** is **zero** → port is considered **closed**.

---

### 🟡 Filtered Port

```
Client  ACK  ──▶  Server
         (no response)
```

No response received — packet was **dropped by a firewall**.

---

## ✅ Advantages

|Advantage|Description|
|---|---|
|👻 **Stealthier Scan**|Avoids connection initiation, reducing logging and IDS/IPS triggers|
|🔥 **Firewall Detection**|Can discover actively filtered ports, similar to ACK scan|
|🎯 **Better Differentiation**|More reliable than simple ACK scans by using **TCP window size** to distinguish open/closed|

---

## ❌ Disadvantages

|Disadvantage|Description|
|---|---|
|⚠️ **Not Always Effective**|Some systems return RST packets with fixed or **non-informative window sizes**|
|💻 **OS-Dependent**|Works best on targets with **predictable TCP stack behavior**|
|🚫 **Firewall Interference**|Like ACK scans, can be blocked by firewalls **dropping ACK packets**|

---

## 🎯 Use Cases

- 🔍 Detecting **firewall rules** and which ports are filtered
- 🕵️ Stealthy reconnaissance when **evading logs and alerts** is important
- 🔬 Advanced port state distinction when **other scans yield inconclusive results**

---

## 💻 Example Command

```bash
nmap -v -sW -p 80 192.168.1.46
```

---

## 🧠 Summary

```
┌─────────────────────────────────────────────────────┐
│  Stealth  ██████████████████░░  High                │
│  Accuracy ████████████████░░░░  Moderate-High       │
│  OS Dependency: ⚠️ Varies by target TCP stack       │
│  Privilege Required: ✔ Root/Admin                   │
└─────────────────────────────────────────────────────┘
```

> _A useful tool for security professionals needing a **stealthier and slightly more reliable** way to analyze port states behind firewalls and detection systems._

---

_📁 Path: `Network-Reconnaissance-Scanning / Nmap / Nmap-Scan-Techniques / TCP-Window-Scan(-sW)`_

---

---

# 👻 TCP Maimon Scan (`-sM`)

> _"The phantom flag — exploits what BSD stacks weren't supposed to admit."_

---

## ⚡ What Is It?

The **TCP Maimon Scan** is a **lesser-known stealth scan technique** discovered by **Uriel Maimon**. It targets a quirk in certain **TCP/IP stack implementations**, mainly in **BSD-based systems**, by sending `FIN/ACK` packets to the target port.

---

## 🔄 How the `-sM` Scan Works

### 🟢 Open Port

```
Client  FIN/ACK  ──▶  Server
         (no response)
```

The server **ignores** the packet. This lack of response indicates the port is either **open or filtered**.

---

### 🔴 Closed Port

```
Client  FIN/ACK  ──▶  Server
Client  ◀── RST         Server
```

The server responds with `RST` — port is **closed**.

---

### 🟡 Filtered Port

```
Client  FIN/ACK  ──▶  Server
         (no response)
```

No response received — packet was **blocked by a firewall or filtering device**.

---

## ✅ Advantages

|Advantage|Description|
|---|---|
|🔇 **Stealthy**|Avoids SYN packets, reducing chance of **IDS/IPS detection**|
|🔓 **Firewall Evasion**|Can bypass firewalls and filters that **block common SYN scans**|
|🎯 **Targets Specific TCP Stack Behavior**|Exploits a quirk in how **BSD-based stacks** handle FIN/ACK packets|

---

## ❌ Disadvantages

|Disadvantage|Description|
|---|---|
|🎯 **Limited Scope**|Only effective against systems with **specific TCP/IP stack behaviors** (mostly BSD)|
|🚫 **Potential Firewall Blocking**|Modern stateful firewalls can **drop FIN/ACK packets**, reducing effectiveness|
|⚠️ **Inconsistency**|Some OS treat FIN/ACK packets similarly to FIN packets, causing **varying reliability**|

---

## 🎯 Use Cases

- 🕵️ Stealth scanning environments known to use **vulnerable TCP stack implementations**
- 🔓 Evading firewalls that block SYN scans but **allow unusual TCP flag combinations**
- 🔗 Complementary scan to **enhance detection coverage** in penetration tests

---

## 💻 Example Command

```bash
nmap -v -sM -p 21 192.168.1.46
```

---

## 🧠 Summary

```
┌─────────────────────────────────────────────────────┐
│  Stealth  ██████████████████░░  High                │
│  Scope    ████████░░░░░░░░░░░░  Limited (BSD only)  │
│  Reliability: ⚠️ Varies by target OS                │
│  Privilege Required: ✔ Root/Admin                   │
└─────────────────────────────────────────────────────┘
```

> _A specialized technique for niche environments where **stealth and evasion are critical**, but its effectiveness depends heavily on the target's TCP/IP stack behavior._

---

_📁 Path: `Network-Reconnaissance-Scanning / Nmap / Nmap-Scan-Techniques / TCP-Maimon-Scan(-sM)`_

---

---

# 📡 UDP Scan (`-sU`)

> _"The silent messenger — sends into the void, reads what echoes back."_

---

## ⚡ What Is It?

The **UDP Scan** is used to **detect open UDP ports** on a target system. Unlike TCP's connection-oriented protocol, **UDP is connectionless** — no handshakes. The scan relies on **responses or lack thereof** to infer the port status.

---

## 🔄 How the `-sU` Scan Works

### 🟢 Open Port

```
Client  UDP PORT 53   ──▶  Server
Client  ◀── UDP PORT 53 Reply  Server
```

Server replies with a **valid UDP response** → port is **open**.

---

### 🔴 Closed Port

```
Client  UDP PORT 53  ──▶  Server
Client  ◀── ICMP Port Unreachable  Server
```

Server responds with **ICMP Port Unreachable** (Type 3, Code 3) → port is **closed**.

---

### 🟡 Open or Filtered Port

```
Client  UDP PORT 53  ──▶  Server
         (no response)
```

No response — the port could be **open but silent**, or **filtered by a firewall** dropping packets.

---

## ✅ Advantages

|Advantage|Description|
|---|---|
|⚡ **Lower Overhead**|UDP is connectionless — requires **no handshake**, making it lightweight|
|♾️ **No Connection Limits**|Unlike TCP, it's **not constrained by connection limits**|
|🪟 **Useful on Windows Networks**|Many important Windows services run on UDP: **DNS (53), NetBIOS (137), SNMP (161)**|

---

## ❌ Disadvantages

|Disadvantage|Description|
|---|---|
|🔐 **Requires Privileged Access**|Root or administrator privileges needed to **send raw packets**|
|🐢 **Slow Scans**|Many UDP services do not respond, **significantly slowing scan times**|
|🚧 **ICMP Rate Limiting**|Firewalls may limit or block ICMP Unreachable messages, causing **false positives**|

---

## 🎯 Use Cases

- 🌐 Discovering **UDP-based services** like DNS, DHCP, SNMP, NTP, TFTP
- 🕵️ Scanning for **hidden services** in networks that primarily filter TCP
- 🪟 Penetration testing **Windows environments** to find services such as NetBIOS and MSRPC

---

## 💻 Example Commands

```bash
# Verbose UDP scan on all common ports
nmap -v -sU 192.168.1.51

# Verbose UDP scan on port 53 (DNS)
nmap -v -sU -p 53 192.168.1.51
```

---

## 🌐 Common UDP Ports to Scan

|Port|Service|
|---|---|
|53|DNS|
|67|DHCP|
|69|TFTP|
|123|NTP|
|135|MSRPC|
|137|NetBIOS|
|138|NetBIOS Datagram|
|161|SNMP|
|445|SMB|
|631|IPP|
|1434|MS SQL Server|

---

## 🧠 Summary

```
┌─────────────────────────────────────────────────────┐
│  Speed    ████████░░░░░░░░░░░░  Slow                │
│  Stealth  ████████████░░░░░░░░  Moderate            │
│  Protocol: UDP (connectionless)                     │
│  Privilege Required: ✔ Root/Admin                   │
└─────────────────────────────────────────────────────┘
```

> _UDP scanning is **essential for comprehensive network reconnaissance**, especially to find services invisible to TCP scans._

---

_📁 Path: `Network-Reconnaissance-Scanning / Nmap / Nmap-Scan-Techniques / UDP-Scan(-sU)`_

---

---

# 🚫 TCP Null Scan (`-sN`)

> _"The blank stare — sends nothing, reads everything from the silence."_

---

## ⚡ What Is It?

The **TCP Null Scan** is a stealthy scanning technique that sends **TCP packets with no flags set** (Null packets) to determine whether ports are open, closed, or filtered — based on the target's responses or lack thereof.

---

## 🔄 How the `-sN` Scan Works

### 🟢 Open Port

```
Client  None (no flags)  ──▶  Server
         (no response)
```

The server does **not respond**, compliant with RFC 793. This indicates the port is **open or filtered**.

---

### 🔴 Closed Port

```
Client  None (no flags)  ──▶  Server
Client  ◀── RST/ACK              Server
```

The server replies with `RST/ACK` → port is **closed**.

---

### 🟡 Filtered Port

```
Client  None (no flags)  ──▶  Server
         (no response)
```

No response — a **firewall drops the packet**.

> ⚠️ Note: Open and Filtered ports both produce no response — they cannot be distinguished with this scan alone.

---

## ✅ Advantages

|Advantage|Description|
|---|---|
|👻 **Stealthy**|Unlike `-sT` or `-sS`, Null scans do **not trigger many firewall or IDS logs**|
|🔓 **May Bypass Firewalls**|If a firewall only blocks SYN packets, Null scans **might bypass it**|
|🐧 **Effective on Non-Windows Systems**|Most Unix/Linux TCP stacks follow **RFC 793**, making this scan effective there|

---

## ❌ Disadvantages

|Disadvantage|Description|
|---|---|
|🪟 **Ineffective on Windows**|Windows treats closed ports differently, marking them as **filtered** — Null scans unreliable|
|🚫 **Blocked by Modern Firewalls**|Many contemporary firewalls and IDS **detect and block** Null scans|
|❓ **Ambiguous for Open Ports**|Lack of response can mean either **open or filtered**, making it hard to distinguish conclusively|

---

## 🎯 Use Cases

- 🕵️ Evasion of IDS and firewall logging due to the **absence of SYN packets**
- 🐧 Scanning **Unix/Linux/BSD systems** where RFC 793 compliance is expected
- 🔬 Testing **firewall rules for SYN packet filtering** while other packets might pass

---

## 💻 Example Command

```bash
nmap -sN -p 23 192.168.1.46
```

---

## 🧠 Summary

```
┌─────────────────────────────────────────────────────┐
│  Stealth  ████████████████████  Very High           │
│  Flags Sent: ∅ None                                 │
│  Open vs Filtered: ⚠️ Indistinguishable             │
│  Works on Windows: ✘ No                             │
│  Privilege Required: ✔ Root/Admin                   │
└─────────────────────────────────────────────────────┘
```

> _A specialized stealth technique best suited for network reconnaissance where **minimal detectability is desired**, primarily against non-Windows targets._

---

_📁 Path: `Network-Reconnaissance-Scanning / Nmap / Nmap-Scan-Techniques / TCP-Null-Scan(-sN)`_

---

---

# 🏁 TCP FIN Scan (`-sF`)

> _"The final whisper — sends a farewell packet to a connection that never existed."_

---

## ⚡ What Is It?

The **TCP FIN Scan** is a stealthy scanning technique that sends TCP packets with only the **FIN flag set**. It is used to identify open, closed, or filtered ports based on the target's response or lack thereof.

---

## 🔄 How the `-sF` Scan Works

### 🟢 Open Port

```
Client  FIN  ──▶  Server
         (no response)
```

The server does **not respond**, following RFC 793 standard behavior — port is **open or filtered**.

---

### 🔴 Closed Port

```
Client  FIN    ──▶  Server
Client  ◀── RST/ACK  Server
```

Server replies with `RST/ACK` — port is **closed**.

---

### 🟡 Filtered Port

```
Client  FIN  ──▶  Server
         (no response)
```

No response — a **firewall is blocking** the packet. Port is considered **filtered**.

---

## 🧠 Summary

```
┌─────────────────────────────────────────────────────┐
│  Stealth  ████████████████████  Very High           │
│  Flags Sent: FIN only                               │
│  Open vs Filtered: ⚠️ Indistinguishable             │
│  Works on Windows: ✘ No                             │
│  Privilege Required: ✔ Root/Admin                   │
└─────────────────────────────────────────────────────┘
```

> _Sends a farewell to a session that never began — exploits RFC 793 behavior to slip past simple packet filters._

---

_📁 Path: `Network-Reconnaissance-Scanning / Nmap / Nmap-Scan-Techniques / TCP-FIN-Scan(-sF)`_

---

---

# 🎄 TCP Xmas Scan (`-sX`)

> _"The lit-up packet — every flag blazing like a Christmas tree, dazzling its way past defenses."_

---

## ⚡ What Is It?

The **TCP Xmas Scan** is a stealthy and advanced scanning technique named for the TCP packets it sends with **FIN, PUSH, and URG flags** set — which make the packet "light up" like a Christmas tree.

---

## 🔄 How the `-sX` Scan Works

### 🟢 Open Port

```
Client  FIN/PSH/URG  ──▶  Server
         (no response)
```

The server sends **no response**, following RFC-compliant behavior — port is **open or filtered**.

---

### 🔴 Closed Port

```
Client  FIN/PSH/URG  ──▶  Server
Client  ◀── RST/ACK         Server
```

Server responds with `RST/ACK` — port is **closed**.

---

### 🟡 Filtered Port

```
Client  FIN/PSH/URG  ──▶  Server
         (no response)
```

No response — likely due to a **firewall dropping the packet**. Port is **filtered**.

---

## ✅ Advantages

|Advantage|Description|
|---|---|
|👻 **High Stealth**|Avoids SYN packets, reducing IDS/IPS detection chance|
|🔓 **Firewall Evasion**|Can bypass firewalls that only filter SYN packets|
|🐧 **Effective on Unix/Linux**|Works on systems with RFC-compliant TCP stacks|

---

## ❌ Disadvantages

|Disadvantage|Description|
|---|---|
|🪟 **Doesn't Work on Windows**|Windows ignores these flags, treating all ports as filtered|
|🚫 **Blocked by Modern Firewalls**|Many contemporary firewalls and IDS detect and block Xmas scans|
|❓ **Ambiguous Open Ports**|No response can mean **open or filtered** — interpretation is difficult|

---

## 🎯 Use Cases

- 🕵️ Evading **IDS/IPS detection** by avoiding SYN packets
- 🐧 Scanning **Unix/Linux systems** consistent with RFC behavior
- 🔬 Testing if firewalls **filter SYN but allow other TCP flags**

---

## 💻 Example Command

```bash
nmap -sX -p 23 192.168.1.46
```

---

## 🧠 Summary

```
┌─────────────────────────────────────────────────────┐
│  Stealth  ████████████████████  Very High           │
│  Flags Sent: FIN + PSH + URG                        │
│  Open vs Filtered: ⚠️ Indistinguishable             │
│  Works on Windows: ✘ No                             │
│  Privilege Required: ✔ Root/Admin                   │
└─────────────────────────────────────────────────────┘
```

> _A powerful stealth scanning technique commonly used on Unix-like systems to evade detection, but **modern defenses frequently mitigate** its effectiveness._

---

_📁 Path: `Network-Reconnaissance-Scanning / Nmap / Nmap-Scan-Techniques / TCP-Xmas-Scan(-sX)`_

---

---

## 📊 Nmap Scan Techniques — Complete Comparison

|Scan|Flag|Flags Sent|Privilege|Stealth|Windows?|Open Port Response|Closed Port Response|Purpose|
|---|---|---|---|---|---|---|---|---|
|SYN Stealth|`-sS`|SYN|Root|🟢 High|✅ Yes|SYN/ACK|RST/ACK|Default stealth scan|
|TCP Connect|`-sT`|SYN|None|🔴 Low|✅ Yes|SYN/ACK→ACK|RST/ACK|No-root fallback|
|TCP ACK|`-sA`|ACK|Root|🟢 High|✅ Yes|RST (unfiltered)|RST (unfiltered)|Firewall mapping only|
|TCP Window|`-sW`|ACK|Root|🟢 High|✅ Yes|RST (window≠0)|RST (window=0)|Advanced ACK variant|
|TCP Maimon|`-sM`|FIN/ACK|Root|🟢 High|❌ No|No response|RST|BSD stack exploit|
|UDP|`-sU`|UDP|Root|🟡 Medium|✅ Yes|UDP reply|ICMP Unreachable|UDP service discovery|
|TCP Null|`-sN`|None (∅)|Root|🟢 Very High|❌ No|No response|RST/ACK|Stealth / SYN evasion|
|TCP FIN|`-sF`|FIN|Root|🟢 Very High|❌ No|No response|RST/ACK|Stealth / SYN evasion|
|TCP Xmas|`-sX`|FIN+PSH+URG|Root|🟢 Very High|❌ No|No response|RST/ACK|Stealth / SYN evasion|

---

### 🎄 FIN vs. Null vs. Xmas — Side-by-Side

| Scan Type    | Flags Sent      | Open Port Response | Closed Port Response | Works on Windows? | Stealth Level |
| ------------ | --------------- | ------------------ | -------------------- | ----------------- | ------------- |
| FIN (`-sF`)  | `FIN`           | No response        | RST/ACK              | ❌ No              | ✅ High        |
| Null (`-sN`) | No flags        | No response        | RST/ACK              | ❌ No              | ✅ High        |
| Xmas (`-sX`) | `FIN, PSH, URG` | No response        | RST/ACK              | ❌ No              | ✅ High        |

# ⚡ NMAP — SCAN TYPES REFERENCE

> _The Complete Field Manual for Network Reconnaissance_

---

```
███╗   ██╗███╗   ███╗ █████╗ ██████╗
████╗  ██║████╗ ████║██╔══██╗██╔══██╗
██╔██╗ ██║██╔████╔██║███████║██████╔╝
██║╚██╗██║██║╚██╔╝██║██╔══██║██╔═══╝
██║ ╚████║██║ ╚═╝ ██║██║  ██║██║
╚═╝  ╚═══╝╚═╝     ╚═╝╚═╝  ╚═╝╚═╝  SCAN TYPES
```

---

## 📡 TCP SCAN TYPES

---

### `-sS` — TCP SYN Scan _(Stealth Scan)_

> **The default and most popular scan**

```
Attacker          Target
   |── SYN ──────────►|
   |◄─ SYN/ACK ───────|   ← Port is OPEN
   |── RST ──────────►|   ← Never completes handshake
```

|Property|Detail|
|---|---|
|**Privilege**|Requires root/admin|
|**Speed**|Fast|
|**Stealth**|High — never completes TCP handshake|
|**Reliability**|High|

**How it works:** Sends a SYN packet. If a SYN/ACK is received, the port is open — but nmap immediately sends RST to tear down the connection before it fully establishes. Because the 3-way handshake is never completed, many systems won't log the connection.

**Use when:** You want fast, stealthy reconnaissance with minimal footprint on target logs.

---

### `-sT` — TCP Connect Scan

> **Full connection scan — no raw packets needed**

```
Attacker          Target
   |── SYN ──────────►|
   |◄─ SYN/ACK ───────|
   |── ACK ──────────►|   ← Full handshake COMPLETED
   |── RST/FIN ───────►|   ← Nmap closes connection
```

|Property|Detail|
|---|---|
|**Privilege**|Does NOT require root|
|**Speed**|Slower than SYN scan|
|**Stealth**|Low — full connection is logged|
|**Reliability**|High|

**How it works:** Uses the OS's `connect()` syscall to establish a full TCP connection. No raw sockets needed, making it usable by unprivileged users. The downside: the target system fully logs the connection.

**Use when:** You don't have raw socket privileges, or you're scanning through a proxy/NAT that requires full connections.

---

### `-sA` — TCP ACK Scan

> **Firewall mapping, not port discovery**

```
Attacker          Target
   |── ACK ──────────►|
   |◄─ RST ───────────|   ← UNFILTERED (firewall allows)
   |   (no response)  |   ← FILTERED (firewall blocks)
```

|Property|Detail|
|---|---|
|**Privilege**|Requires root/admin|
|**Purpose**|Firewall rule mapping|
|**Result**|Returns FILTERED or UNFILTERED (not open/closed)|

**How it works:** Sends ACK packets (which belong to an established connection). Stateful firewalls will drop them; stateless firewalls let them through and the target responds with RST. This tells you about firewall rules, **not** whether a port is open.

**Use when:** You want to map firewall rules and determine which ports are filtered vs. unfiltered.

---

### `-sW` — TCP Window Scan

> **ACK scan variant that exploits TCP window size quirks**

```
Attacker          Target
   |── ACK ──────────►|
   |◄─ RST (win > 0) ─|   ← Interpreted as OPEN
   |◄─ RST (win = 0) ─|   ← Interpreted as CLOSED
```

|Property|Detail|
|---|---|
|**Privilege**|Requires root/admin|
|**Reliability**|OS-dependent (works on some BSD/Linux)|
|**Purpose**|Port state detection via window size|

**How it works:** Similar to ACK scan but examines the TCP window size field in returned RST packets. On certain operating systems, open ports respond with a non-zero window value while closed ports respond with zero. Results are unreliable across different OS implementations.

**Use when:** Targeting specific systems known to exhibit window size differences in RST packets.

---

### `-sM` — TCP Maimon Scan

> **FIN/ACK probe — named after Uriel Maimon (1996)**

```
Attacker          Target
   |── FIN/ACK ───────►|
   |   (no response)   |   ← OPEN (BSD behavior)
   |◄─ RST ────────────|   ← CLOSED
```

|Property|Detail|
|---|---|
|**Privilege**|Requires root/admin|
|**Target**|BSD-derived systems|
|**Reliability**|Low on modern systems|

**How it works:** Sends FIN/ACK packets. On BSD systems, open ports drop the packet silently while closed ports reply with RST. Most modern systems (Linux, Windows) respond with RST regardless of port state, making this scan largely ineffective today.

**Use when:** Targeting older BSD-derived systems or testing firewall behavior against FIN/ACK probes.

---

## 🔮 STEALTH / EXOTIC TCP SCANS

---

### `-sN` — TCP Null Scan

> **No flags set at all**

```
Attacker          Target
   |── [NO FLAGS] ────►|
   |   (no response)   |   ← OPEN or FILTERED
   |◄─ RST/ACK ────────|   ← CLOSED
```

|Property|Detail|
|---|---|
|**Privilege**|Requires root/admin|
|**Stealth**|High — unusual traffic pattern|
|**Windows Support**|❌ Does not work on Windows|

**How it works:** Sends a TCP packet with zero flags. Per RFC 793, receiving systems should respond with RST if the port is closed and drop the packet if open. Works only on RFC-compliant stacks — Windows ignores these probes.

---

### `-sF` — TCP FIN Scan

> **Only the FIN flag is set**

```
Attacker          Target
   |── FIN ───────────►|
   |   (no response)   |   ← OPEN or FILTERED
   |◄─ RST/ACK ────────|   ← CLOSED
```

|Property|Detail|
|---|---|
|**Privilege**|Requires root/admin|
|**Stealth**|High — mimics connection teardown|
|**Windows Support**|❌ Does not work on Windows|

**How it works:** Sends only a FIN flag — normally used to close established connections. Closed ports reply with RST; open ports silently drop it. Can bypass some non-stateful firewalls and packet filters.

---

### `-sX` — TCP Xmas Scan

> **FIN + PSH + URG flags — "lit up like a Christmas tree"**

```
Attacker          Target
   |── FIN+PSH+URG ───►|
   |   (no response)   |   ← OPEN or FILTERED
   |◄─ RST/ACK ────────|   ← CLOSED
```

|Property|Detail|
|---|---|
|**Privilege**|Requires root/admin|
|**Stealth**|High|
|**Windows Support**|❌ Does not work on Windows|

**How it works:** Sets FIN, PSH, and URG flags simultaneously — an illegal combination in normal TCP. Behavior follows the same RFC 793 rules as Null and FIN scans. Named "Xmas" because the packet looks "lit up" with multiple flags.

---

### `--scanflags <flags>` — Custom TCP Flag Scan

> **Design your own TCP probe**

```bash
# Examples:
nmap --scanflags URGACKPSHRSTSYNFIN target   # All flags
nmap --scanflags SYN target                   # Same as -sS
nmap --scanflags SYNFIN target                # Custom combo
nmap --scanflags 0x29 target                  # Hex notation
```

|Property|Detail|
|---|---|
|**Privilege**|Requires root/admin|
|**Flexibility**|Full control over TCP flags|
|**Flags Available**|`URG` `ACK` `PSH` `RST` `SYN` `FIN`|

**How it works:** Allows you to manually specify any combination of TCP flags using flag names or hex values. Can be combined with `-sS`, `-sF`, etc. to define the scan base type while overriding the flags. Powerful for custom IDS evasion or protocol research.

**Use when:** You need to craft unusual packets for evasion testing, firewall analysis, or researching specific IDS/IPS signatures.

---

## 🕵️ ADVANCED & SPECIAL SCANS

---

### `-sI <zombie host[:probeport]>` — Idle Scan


kisi aur kr behalf pe scan karna matlab jombie kisii ke wash me leke kam karta hai usi tarah

zombie host me apna ya kisis ka bhi host ka ip wo uske behal pe scan karega


> **The ultimate stealth scan — your IP never touches the target**

```
Attacker         Zombie           Target
   |─ probe IP ID ──►|
   |◄─ IP ID=X ──────|
   |                 |── SYN (spoofed as zombie) ──►|
   |                 |◄─ SYN/ACK (if open) ──────────|
   |                 |   IP ID increments to X+2
   |─ probe IP ID ──►|
   |◄─ IP ID=X+2 ────|   ← Port OPEN (ID jumped by 2)
   |◄─ IP ID=X+1 ────|   ← Port CLOSED (ID jumped by 1)
```

|Property|Detail|
|---|---|
|**Privilege**|Requires root/admin|
|**Stealth**|Maximum — attacker's IP never seen by target|
|**Requirement**|Zombie host with predictable IP ID sequence|
|**Complexity**|High|

**How it works:** Exploits "dumb" hosts with incrementing IPID sequences. By probing the zombie before and after spoofed SYN packets are sent to the target, you can infer whether the target port is open based on how much the zombie's IPID counter incremented. Completely hides your identity.

**Use when:** Maximum anonymity is required. Useful in penetration tests where attribution must be avoided or to bypass source-IP-based firewall rules.

```bash
nmap -sI zombie.host.com:80 target.host.com
```

---

### `-sY` — SCTP INIT Scan

> **SCTP equivalent of TCP SYN scan**

```
Attacker          Target
   |── INIT ──────────►|
   |◄─ INIT-ACK ───────|   ← Port OPEN
   |◄─ ABORT ──────────|   ← Port CLOSED
   |── ABORT ─────────►|   ← Never completes association
```

|Property|Detail|
|---|---|
|**Protocol**|SCTP (Stream Control Transmission Protocol)|
|**Privilege**|Requires root/admin|
|**Stealth**|High — association never completed|
|**Use Case**|Telecom / SS7 infrastructure scanning|

**How it works:** SCTP is used in telephony signaling (SS7/SIGTRAN). Sends an INIT chunk — the SCTP equivalent of a SYN. If INIT-ACK is returned, the port is open. Nmap aborts without completing the 4-way SCTP handshake, similar to how `-sS` works for TCP.

---

### `-sZ` — SCTP COOKIE-ECHO Scan

> **Stealthier SCTP scan using COOKIE-ECHO chunks**

```
Attacker          Target
   |── COOKIE-ECHO ───►|
   |   (no response)   |   ← OPEN (packet silently dropped)
   |◄─ ABORT ──────────|   ← CLOSED
```

|Property|Detail|
|---|---|
|**Protocol**|SCTP|
|**Privilege**|Requires root/admin|
|**Stealth**|Higher than INIT scan|
|**Drawback**|Cannot distinguish open from filtered|

**How it works:** Sends a COOKIE-ECHO chunk (used in the middle of the SCTP handshake). Open ports silently discard it; closed ports send ABORT. More stealthy than INIT scan but less informative since open/filtered look the same.

---

### `-sO` — IP Protocol Scan

> **Discovers supported IP protocols, not ports**


protocols ko check kasrrega ki work kar rhe hai ki nhi

```
Attacker          Target
   |── IP proto=1  ───►|   (ICMP)
   |── IP proto=6  ───►|   (TCP)
   |── IP proto=17 ───►|   (UDP)
   |── IP proto=50 ───►|   (ESP)
   |◄─ ICMP proto unreachable |   ← Protocol NOT supported
   |   (any response)  |   ← Protocol SUPPORTED
```

|Property|Detail|
|---|---|
|**Privilege**|Requires root/admin|
|**Scans**|IP protocols (0–255), not TCP/UDP ports|
|**Purpose**|Protocol support enumeration|

**How it works:** Iterates through IP protocol numbers (1–255), sending raw IP packets with various protocol headers. If the target responds at all (even with an ICMP error), that protocol is supported. ICMP "protocol unreachable" messages indicate unsupported protocols.

**Use when:** You want to know which IP-level protocols a host supports (ICMP, OSPF, GRE, ESP, etc.) rather than which ports are open.

---

### `-b <FTP relay host>` — FTP Bounce Scan


ksisi ftp server ke behalf pe scan karega likhe zombie jaise

> **Uses FTP proxy feature to scan third-party hosts**

```
Attacker     FTP Server      Target
   |─ PORT cmd ──►|
   |              |── connect to target:port ──►|
   |◄─ 200 OK ────|   ← Target port OPEN
   |◄─ 425 Err ───|   ← Target port CLOSED
```

|Property|Detail|
|---|---|
|**Privilege**|No root required (uses FTP protocol)|
|**Requirement**|Vulnerable FTP server that allows PORT abuse|
|**Stealth**|Hides attacker — FTP server IP hits the target|
|**Availability**|Rare — most modern FTP servers disable this|

**How it works:** Exploits the FTP `PORT` command, which was originally designed to allow FTP to connect to a data channel on a different host. By connecting to an FTP server and issuing `PORT` commands pointing at a third-party target, you can use the FTP server as a proxy to scan other systems. The target sees the FTP server's IP, not yours.

**Use when:** You discover a misconfigured FTP server and want to pivot it for scanning. Mostly a legacy technique — rare in modern environments.

```bash
nmap -b ftp.vulnerable-server.com target.host.com
```

---

## 🌐 NON-TCP SCAN TYPES

---

### `-sU` — UDP Scan

|Property|Detail|
|---|---|
|**Privilege**|Requires root/admin|
|**Speed**|Very slow (rate-limited by ICMP)|
|**Target**|DNS (53), SNMP (161), DHCP (67/68), NTP (123)|

**How it works:** Sends UDP packets to each port. Open ports may respond with data; closed ports typically send back ICMP port-unreachable messages. Rate limiting by the OS makes this scan slow. Often combined with `-sV` for service detection.

---

### `-sV` — Version Detection _(not a scan type, but commonly paired)_

Probes open ports to determine service/version information. Often used alongside any scan type:

```bash
nmap -sS -sV -p 22,80,443 target
```

---

## 🗂️ QUICK REFERENCE CHEAT SHEET

|Flag|Scan Type|Requires Root|Stealth|Protocol|
|---|---|:-:|:-:|---|
|`-sS`|SYN (Stealth)|✅|⭐⭐⭐|TCP|
|`-sT`|Connect|❌|⭐|TCP|
|`-sA`|ACK|✅|⭐⭐|TCP|
|`-sW`|Window|✅|⭐⭐|TCP|
|`-sM`|Maimon|✅|⭐⭐|TCP|
|`-sN`|Null|✅|⭐⭐⭐|TCP|
|`-sF`|FIN|✅|⭐⭐⭐|TCP|
|`-sX`|Xmas|✅|⭐⭐⭐|TCP|
|`--scanflags`|Custom Flags|✅|Varies|TCP|
|`-sI`|Idle|✅|⭐⭐⭐⭐⭐|TCP|
|`-sY`|SCTP INIT|✅|⭐⭐⭐|SCTP|
|`-sZ`|SCTP COOKIE-ECHO|✅|⭐⭐⭐⭐|SCTP|
|`-sO`|IP Protocol|✅|⭐⭐|IP|
|`-b`|FTP Bounce|❌|⭐⭐⭐|FTP/TCP|
|`-sU`|UDP|✅|⭐⭐|UDP|

---

## ⚠️ LEGAL DISCLAIMER

```
┌─────────────────────────────────────────────────────────┐
│  ⚠  USE ONLY ON SYSTEMS YOU OWN OR HAVE EXPLICIT        │
│     WRITTEN PERMISSION TO TEST.                         │
│                                                         │
│     Unauthorized scanning may violate:                  │
│     • Computer Fraud and Abuse Act (CFAA)               │
│     • Computer Misuse Act (UK)                          │
│     • Various local and international laws              │
└─────────────────────────────────────────────────────────┘
```

---

_Reference: [nmap.org](https://nmap.org/book/man-port-scanning-techniques.html) · Gordon "Fyodor" Lyon · Nmap Network Scanning_