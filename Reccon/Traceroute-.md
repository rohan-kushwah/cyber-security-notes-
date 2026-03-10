# IP Traceroute Tool
---

Traceroute is a diagnostic tool used to track the path that a packet takes from a source host to a destination IP or domain. It helps identify delays, routing loops, and points of failure in the network.

## How Traceroute Works
---

Traceroute sends packets with incrementally increasing Time-To-Live (TTL) values. Each router that handles the packet decreases the TTL by 1. When TTL reaches 0, the router returns a “Time Exceeded” message, revealing its IP.

## Common Traceroute Tools
---

| Tool              | Description                             | Platform       |
| :---------------- | :-------------------------------------- | :------------- |
| traceroute        | Default traceroute tool on Linux        | Linux          |
| tracert           | Traceroute command on Windows           | Windows        |
| mtr               | Combines traceroute and ping            | Linux          |
| nmap --traceroute | Traceroute integrated into network scan | Cross-platform |
| tcptraceroute     | Uses TCP instead of ICMP/UDP            | Linux          |
| tracepath         | Lightweight alternative to traceroute   | Linux          |

## Example Commands
---
### Linux (traceroute)
```
traceroute google.com
```
### Windows (tracert)
```
tracert google.com
```
### MTR (Live route and latency statistics)
```
mtr google.com
```
### TCP Traceroute (for filtered ICMP)
```
sudo tcptraceroute google.com 80
```
### Tracepath
```
tracepath google.com
```

## Online Traceroute Tools
---

| Tool               | URL                                                        |
| :----------------- | :--------------------------------------------------------- |
| GRC ShieldsUP!     | https://www.grc.com/shieldsup                             |
| Ping.eu            | https://ping.eu/traceroute/                               |
| DNS Checker        | https://dnschecker.org/traceroute                         |
| T1 Shopper         | https://www.site24x7.com/tools/traceroute.html (alternative) |
| YouGetSignal       | https://www.yougetsignal.com/tools/visual-tracert/        |

## Sample Output (Linux)
---
```
$ traceroute google.com
 1  192.168.1.1 (192.168.1.1)     1.123 ms   0.982 ms   1.074 ms
 2  100.72.96.1 (100.72.96.1)     9.241 ms   9.238 ms   9.230 ms
 3  172.217.163.110 (172.217.163.110) 25.647 ms  25.648 ms  25.649 ms
```

## Notes
---
*   ICMP/UDP filtering by firewalls can block standard traceroute responses.  
*   TCP-based traceroute is useful for bypassing ICMP filtering (e.g., via port 80).  
*   MTR is more dynamic and shows live latency/loss per hop.  
---
---