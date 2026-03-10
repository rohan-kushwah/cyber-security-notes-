\# Nmap Scanning Workflow

Performing an effective Nmap scan involves a structured sequence of phases, moving from broad discovery to deep enumeration. Following this workflow ensures that scans are efficient, accurate, and yield the most valuable information.

## Phase 1: Host Discovery (Ping Sweep)

Before launching a detailed port scan, the first crucial step is to determine which target hosts are online. This prevents wasting time and resources scanning IP addresses that are not active.

**Nmap's Intelligent Discovery Process:**

Nmap is smart about how it discovers hosts based on the network location of the target:
-   **Local Network (LAN):** For targets on the same subnet, Nmap defaults to sending **ARP requests**. This is extremely fast and reliable, as hosts on a LAN cannot hide from ARP.
-   **Remote Network (WAN):** For remote targets, Nmap uses a combination of probes to bypass common firewall rules:
    -   ICMP Echo Request (standard ping)
    -   TCP SYN packet to port 443
    -   TCP ACK packet to port 80
    -   ICMP Timestamp Request

A response to any of these probes confirms the host is online.

---
**Key Flags & Concepts:**
-   If a target is configured to block these discovery probes (e.g., a firewall blocking ICMP), a default Nmap scan will report it as "down" and stop.
-   `nmap -Pn target.com`: This flag is essential for these situations. It tells Nmap to **skip the host discovery phase** and treat all specified targets as online. This forces Nmap to proceed directly to port scanning, which is necessary when you know a host is live but it isn't responding to pings.
---
## Phase 2: Reverse DNS Resolution

By default, Nmap performs a **reverse DNS (rDNS) lookup** for each host it discovers. This involves querying a DNS server for a **PTR (Pointer) record**, which maps the target's IP address back to a hostname.

This step is valuable for reconnaissance, as a hostname can reveal its function (e.g., `mail.company.com` or `dev-server.lan`).

---
**Key Flags & Concepts:**
-   If you scan a domain name, Nmap first resolves the domain to an IP, then performs this reverse lookup on the IP to see if it resolves back to a hostname.
-   `nmap -n target.com`: If you want to speed up your scan and are not interested in hostnames, this flag tells Nmap **not to perform any DNS resolution**.
---
## Phase 3: Port Scanning

This is the core of Nmap's functionality. The goal is to identify which ports are open, closed, or filtered on the target system to discover potentially running services. By default, Nmap performs a **TCP SYN Scan**.

-   **Default Behavior:** Without any port specification (like `-p`), Nmap scans the **1,000 most common TCP ports**. This list is based on extensive research of internet traffic and is far more effective than just scanning ports 1-1000.
-   **Port States:**
    -   **Open:** An application is actively accepting connections on this port.
    -   **Closed:** The port is accessible, but no application is listening on it.
    -   **Filtered:** A firewall or other network device is blocking access to the port, so Nmap cannot determine if it is open or closed.

---
## Phase 4: Deep Enumeration

Once open ports are identified, the next phase is to probe them deeply to understand exactly what is running.

#### **Service and Version Detection (`-sV`)**
This is one of Nmap's most powerful features. It sends a series of probes to each open port to determine the exact service (e.g., Apache, Microsoft IIS, OpenSSH) and its version number. This information is critical for vulnerability assessment.
```bash
nmap -sV target.com
```

#### **Operating System Detection (`-O`)**
Nmap can determine the operating system of the target by sending a series of specifically crafted packets and analyzing the subtle differences in the response from the target's TCP/IP network stack. This provides key insights into the target's architecture and potential vulnerabilities.
```bash
nmap -O target.com
```

---
## Phase 5: Nmap Scripting Engine (NSE)

As a final, advanced step, Nmap can use its powerful scripting engine to run scripts against the discovered services. These scripts can automate tasks like advanced vulnerability detection, malware discovery, and deeper enumeration.

```bash
# Run the default set of safe scripts
nmap -sC target.com

# Run all scripts in the 'vuln' category
nmap --script=vuln target.com
```

---
## Phase 6: Saving the Output

It is crucial to save scan results for analysis, reporting, and future reference. Nmap supports several highly useful output formats.

**Common Output Commands:**

-   **Normal Output (`-oN`):** Human-readable text format, perfect for quick review.
```bash
nmap -oN output.txt target.com
```
-   **XML Output (`-oX`):** A structured format that is ideal for importing into other security tools (like Metasploit) or for parsing with custom scripts.
```bash
nmap -oX output.xml target.com
```
-   **Grepable Output (`-oG`):** A simplified format designed for easy parsing with command-line tools like `grep`, `awk`, and `cut`.
```bash
nmap -oG output.gnmap target.com
```
-   **All Formats (`-oA`):** The recommended best practice. This saves the output in all three formats simultaneously.
```bash
nmap -oA output_prefix target.com
```

---
---