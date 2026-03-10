# Nmap (Network Mapper)
---
Nmap (Network Mapper) is a powerful open-source tool for network discovery and security auditing. Here are its primary uses:

*   **Host Discovery:** Identifies active devices and systems on a network.
*   **Port Scanning:** Detects open ports and associated network services.
*   **Service & Version Detection:** Determines the software and versions running on open ports, aiding in inventory and vulnerability checks.
*   **OS Detection:** Identifies the operating system and version used by target devices.
*   **Vulnerability Scanning:** Finds known security vulnerabilities by probing network services and analyzing responses.
*   **Firewall & IDS Evasion:** Includes techniques to bypass security controls like firewalls and Intrusion Detection Systems, such as using decoys, fragmenting packets, or manipulating packet headers.

These features make Nmap invaluable for administrators, security professionals, and auditors seeking to secure and understand their networks.

---
## What is Nmap?

*   **Purpose:** Nmap is an open-source utility for network discovery and security auditing. It maps networks by discovering hosts and services.
*   **How it Works:** Sends specially crafted packets to target hosts and analyzes responses to identify hosts, services, operating systems, and firewall types.
*   **Compatibility:** Runs on Linux, Windows, macOS, BSD, Solaris, etc., with command-line and GUI (Zenmap) interfaces.

---
## Key Features
*   Supports advanced techniques including TCP, UDP, ICMP and IP protocol scans.
*   Handles large network scans and single hosts efficiently.
*   Includes Nmap Scripting Engine (NSE) for automated vulnerability detection.
*   Widely used, awarded, and featured in films and publications.

![[Pasted image 20250910140804.png]]

## Nmap Interaction with OSI Model Layers
---
### 1. Network Layer in Nmap (Layer 3)

The Network Layer (Layer 3) of the OSI model manages the movement and routing of packets across networks. Nmap leverages this layer for advanced network discovery and scanning. Here's how Nmap utilizes the network layer:

**IP Packet Handling**
-   **Raw IP Packets:** Nmap sends raw IP packets to target hosts for scanning and to identify live systems.
-   **ICMP Echo Requests:** Uses ping sweeps (ICMP) to discover active hosts.
-   **Fragmented IP Packets:** Can send fragmented packets to help evade detection by firewalls and intrusion detection systems.

**Routing & Reachability**
-   **Route Selection:** Uses the network layer to determine optimal paths to targets.
-   **Custom Gateways:** Allows specifying a gateway or source IP with options like `--source-ip` or `--route-dst`.

**Custom IP Packet Manipulation**
-   **Raw Packet Crafting:** Enables crafting custom IP packets for different scan types (`-sS`, `-sU`, `-sA`).
-   **TTL Control:** Adjusts packet time-to-live (`--ttl`) to manage hop limits or for advanced probing.
-   **Packet Fragmentation:** Uses the `-f` option to fragment packets, aiding in security evasion.

**Firewall & IDS Evasion Techniques**
-   **Decoy Scanning (`-D`):** Sends packets from spoofed addresses to conceal the real source.
-   **Custom MTU (`--mtu`):** Sets a custom maximum transmission unit to further complicate detection.

**IP Protocol Scan (`-sO`)**
-   **Protocol Discovery:** Lists all supported network-layer protocols available on a target by sending different types of protocol packets (useful for discovering lesser-known services and VPNs).

**Summary:**
Nmap's network layer capabilities allow it to deftly probe ( chaturai se janch ), enumerate, and bypass network security measures, giving detailed visibility into both topology and host availability while providing multiple methods for stealth and evasion.

**Commands:**
*   `nmap -f 192.168.1.1` (fragmented packets)
*   `nmap -sO 192.168.1.1` (protocol scan)
*   `nmap -S 192.168.1.100 192.168.1.1` (IP spoofing)

---
### 2. Transport Layer (Layer 4)

The Transport Layer (Layer 4) is critical for end-to-end communication, reliability, and flow control between devices. Nmap leverages this layer extensively for scanning and analyzing network services and security. Here's how:

### How Nmap Uses the Transport Layer

*   **Port Scanning (TCP & UDP):**
    *   Nmap's core function at this layer is to check for open ports on a target system over both TCP and UDP.

*   **TCP Scans:**
    *   **SYN Scan (`-sS`):** Known as a "half-open" scan; it sends SYN packets and waits for responses without completing the handshake, making it stealthy.
    *   **Connect Scan (`-sT`):** Completes the TCP three-way handshake, appearing as a standard connection request.
    *   **Null Scan (`-sN`):** Sends TCP packets with no flags set to test responses, often used for firewall evasion.

*   **UDP Scans (`-sU`):**
    *   Probes targets for open UDP ports such as DNS, SNMP, and TFTP.
    *   Because UDP doesn't guarantee delivery, these scans can be slower and may require retransmissions.

*   **Service Version Detection (`-sV`):**
    *   After identifying open ports, Nmap interrogates them to determine the exact software and version in use.

*   **TCP Flags & Firewall Evasion:**
    *   **FIN Scan (`-sF`):** Sends TCP packets with the FIN flag set to probe firewalls and closed ports.
    *   Different flag combinations (FIN, Null, Xmas, ACK scans) help bypass basic firewall rules and evade detection.

*   **Determining Firewall Rules:**
    *   Options like `--scan-delay` and `--max-retries` adjust scan speed and packet transmission intervals to avoid triggering IDS/IPS devices or firewall thresholds.

**Summary:**
Nmap's transport layer functionality enables detailed port and service scanning, OS fingerprinting, and stealthy probing, making it invaluable for network security assessments and audits. It combines multiple scanning methods and customizations to provide flexible, reliable, and effective network enumeration.

---
### Transport Layer Scan Types with Examples

| Scan Type | Protocol | Description | Command Example |
| :--- | :--- | :--- | :--- |
| **TCP SYN Scan** | TCP (Half-Open) | Stealth scan, doesn't complete handshake | `nmap -sS 192.168.1.1` |
| **TCP Connect Scan** | TCP (Full Connection) | Completes full handshake | `nmap -sT 192.168.1.1` |
| **UDP Scan** | UDP | Checks UDP ports like DNS, SNMP | `nmap -sU -p 53 192.168.1.1` |
| **Service Version Detection** | TCP/UDP | Identifies software versions running | `nmap -sV 192.168.1.1` |
| **FIN Scan** | TCP | Sends FIN flag to probe ports | `nmap -sF 192.168.1.1` |
| **Xmas Scan** | TCP | Sends FIN, PSH, URG flags to probe | `nmap -sX 192.168.1.1` |

This summary provides you with frequently used Nmap commands for scanning different protocol ports and detecting services with examples you can try on your own network or the authorized test server [scanme.nmap.org].

---
---

## TCP Three-Way Handshake in Nmap Scans

*   **SYN:** Client initiates connection, sends initial sequence number.
*   **SYN-ACK:** Server acknowledges and sends its sequence number.
*   **ACK:** Client acknowledges server's sequence, connection established.
*   **Importance:** Ensures reliable communication, sequence tracking for data integrity and filtering evasion.

---
---