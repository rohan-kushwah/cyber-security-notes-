# Nmap Host Discovery

Host discovery is the first essential step in network reconnaissance with Nmap. It helps identify live targets before performing more intensive port scans, optimizing scan time and resources. Below is an expanded guide with common and advanced Nmap host discovery techniques, commands, and tips, formatted in Markdown.

---
### 1. List Scan – List Targets Only

Lists targets without sending packets, useful for validation or inventory purposes.

```bash
nmap -v -sL 192.168.1.1/24
```

- No ping, no port scan, no packets to the target (except DNS lookup).
- **Expands the target range**  
    (e.g., 192.168.1.0/24 → full list of 256 IPs)
- **Performs DNS reverse lookups**  
    If DNS is enabled, it will try:
    - PTR lookup (reverse DNS)
    - Resolve hostname if possible
- **Does NOT send any host discovery or port scan packets.**  
    No TCP SYN, no ICMP ping, no UDP — target is not touched.

---
### 2. Ping Scan – Check Which Hosts Are Up (No Port Scan)

Use this to detect live hosts without probing ports.
- (**"-sn"** called **“No port scan”** or **“-sP”** in old versions.)
- Nmap **checks which hosts are up**, but **does not scan any ports**.

Nmap tries one or more of these host-discovery methods:

#  . ICMP Echo (ping)
# ICMP Timestamp request
1. ARP request ( If scanning the same LAN, ARP is used automatically and is very reliable.)
2. TCP probes:
	If ICMP is blocked, it may use:
 TCP SYN to port 443 or 80
	- TCP ACK    
	- Others depending on OS and permissions

```bash
nmap -v -sn 192.168.1.1/24
```

```bash
nmap -sn 192.168.1.1/24
```

Filter output to show only live hosts:

```bash
nmap -sn 192.168.1.1/24 | grep "scan report" | cut -f 5 -d " " > live_hosts.txt
```
```bash
nmap -n -sn 192.168.1.1/24 | grep "scan report" | cut -f 5 -d " " > live_hosts.txt
```

Save output in multiple formats:

```bash
nmap -v -sn 192.168.1.1/24 -oA /tmp/mynetwork
```

```bash
cat /tmp/mynetwork.gnmap | grep -i Up | cut -f 2 -d " " > up_hosts.txt
```

```bash
cat /tmp/mynetwork.gnmap | grep -i down | cut -f 2 -d " " > down_hosts.txt
```

---
### 3. Scan Multiple Subnets or Networks

Perform ping scans on different CIDR ranges:

```bash
nmap -sn 8.8.8.8/24
```
```bash
nmap -sn 27.7.251.0/24
```
```bash
nmap -sn 43.255.154.0/24
```

---
### 4. Skip DNS Resolution to Speed Up Scan

Avoid looking up DNS names for faster scanning of large subnets:

```bash
nmap -sn -n 192.168.1.1/24
```

---
### 5. Scan Targets from Input File

Prepare a file (e.g., `ip.txt`) with a list of IP addresses or hostnames:

```
192.168.1.1
192.168.1.10
192.168.1.31
```

Scan hosts from the file:

```bash
nmap -v -iL ip.txt
```

---
### 6. Treat All Hosts as Online (Skip Host Discovery)

- With `-Pn`, Nmap **Skip Host Discovery** and **assumes every target is up**, then directly starts port scanning.
- Useful when firewall or IDS blocks ping probes:

```bash
nmap -v -Pn 43.255.154.9
```
```bash
nmap -v -P0 192.168.1.1 # Deprecated synonym for -Pn
```

---
### 7. Use Custom Ping Probes on Specific Ports

Send TCP SYN probes (`-PS`) to specified ports:
```bash
nmap -v -PS80 armourinfosec.com
```

```bash
nmap -v -PS21,22,23,25,80,110,443 43.255.154.9 -sn
```

Send TCP ACK probes (`-PA`):
```bash
nmap -v -PA80 armourinfosec.com
```

Send UDP probes (`-PU`):
```bash
nmap -v -PU53,67,68,69 -iL /tmp/iplist
```

Send SCTP INIT probes (`-PY`):
```bash
nmap -v -PY80 armourinfosec.com
```

Send ICMP Echo request probes (`-PE`), Timestamp request (`-PP`), or Netmask request (`-PM`):
```bash
nmap -v -PE 43.255.154.9
```
```bash
nmap -v -PP 43.255.154.9
```
```bash
nmap -v -PM 43.255.154.9
```

---
### 8. Excluding Hosts and Ranges from Scan

Exclude IPs or subnets to avoid scanning certain hosts:

```bash
nmap -v 192.168.1.1/24 --exclude 192.168.1.1-25
```

```bash
nmap -v 192.168.1.1/24 --excludefile excluded.txt
```

```bash
nmap -v 192.168.1.1/24 --exclude 192.168.1.52-100,250
```

---
### 9. Advanced Host Discovery Tips

Extract Top Ports for TCP, UDP, and SCTP from Nmap services list to perform targeted ping scans:

```bash
awk '$2~/tcp$/' /usr/share/nmap/nmap-services | sort -r -k3 | head -n 20 | awk -F '/tcp' '{print $1}' | awk '{print $2}' | tr '\n' ','
```

```bash
awk '$2~/udp$/' /usr/share/nmap/nmap-services | sort -r -k3 | head -n 20 | awk -F '/udp' '{print $1}' | awk '{print $2}' | tr '\n' ','
```

```bash
awk '$2~/sctp$/' /usr/share/nmap/nmap-services | sort -r -k3 | head -n 100 | awk -F '/sctp' '{print $1}' | awk '{print $2}' | tr '\n' ','
```

Combine aggressive ping scans to maximize host discovery under difficult conditions:
```bash
nmap -v -sn -PS30,23,443,21,22,25,389,110,445,139,143,53,135,3306,8080 82.196.42.195
```

---
## Summary
Nmap host discovery offers a rich suite of options and probes to adapt to any network environment or security context. Whether using simple ICMP pings or sophisticated TCP/UDP/SCTP probes, excluding certain hosts, or working with input files, these techniques improve scan accuracy and efficiency.

---
---