The `screen` command in Linux is a terminal multiplexer that allows you to create, manage, and detach multiple terminal sessions within a single shell. This is especially useful when working on remote servers via SSH, as it allows you to keep processes running even after disconnecting.

### Key Features of `screen`:

- Run multiple terminal sessions simultaneously.
- Keep processes running even if you disconnect from SSH.
- Reattach to running sessions later.

### Install `screen`

sh

CopyEdit
```
`yum install screen -y`
```


```
screen --- se naya session la deta ab umse commands de do session hatanee ke bad bhi chalegi
```
### Start a new screen session


```
```screen -S mysession`
```

### Run a command in a detached screen session


```
`screen -S mysession -dm "ping google.com"`
```
dm se direct command chala sakte hai
### List running screen sessions

```
`screen -ls`
```

### Reattach to a running session

restore karni hai wo command jo session hatane ke badd background me chali gai
```
`screen -r session indentit/namee`
```

### Check running processes

```
`ps aux`
```



# tcpdump=
`tcpdump` is a command-line packet analyzer used to capture and inspect network traffic. It helps in network troubleshooting, security analysis, and debugging by allowing users to filter and capture packets based on specific criteria.

---

### **Basic `tcpdump` Commands**

#### **1. Install `tcpdump`**

sh

CopyEdit

`# For RHEL/CentOS yum install tcpdump -y  # For Ubuntu/Debian apt install tcpdump -y`

#### **2. Capture Packets on a Specific Interface**

sh

CopyEdit

`tcpdump -i eth0`

- Captures packets on interface `eth0`.

#### **3. Capture Only N Number of Packets**

sh

CopyEdit

`tcpdump -c 10 -i eth0`

- Captures only 10 packets on `eth0`.

#### **4. Display Packets in Verbose Mode**

sh

CopyEdit

`tcpdump -v -i eth0`

- Provides more details about captured packets.

#### **5. Capture Packets and Save to a File**

sh

CopyEdit

`tcpdump -i eth0 -w capture.pcap`

- Saves captured packets to `capture.pcap` for later analysis with Wireshark.

#### **6. Read Captured Packets from a File**

sh

CopyEdit

`tcpdump -r capture.pcap`

- Reads and displays packets from a saved capture file.

#### **7. Filter Traffic by Host**

sh

CopyEdit

`tcpdump -i eth0 host 192.168.1.10`

- Captures packets to and from `192.168.1.10`.

#### **8. Filter Traffic by Port**

sh

CopyEdit

`tcpdump -i eth0 port 80`

- Captures HTTP traffic (port 80).

#### **9. Filter Traffic by Protocol**

sh

CopyEdit

`tcpdump -i eth0 tcp tcpdump -i eth0 udp tcpdump -i eth0 icmp`

- Captures only TCP, UDP, or ICMP packets.

#### **10. Capture Packets with Specific String in Payload**

sh

CopyEdit

`tcpdump -i eth0 -A | grep "password"`

- Searches for "password" in the packet data.

---

### **Use Case Example**

To capture and analyze SSH traffic on `eth0` for a specific IP (`192.168.1.100`):

sh

CopyEdit

`tcpdump -i eth0 port 22 and host 192.168.1.100 -vv`

---

### **Note:**

- Run `tcpdump` as root (`sudo tcpdump`).
- Use Wireshark to analyze `.pcap` files visually.
- Be cautious when using `tcpdump` on a public network due to security co




# advanced commands=
# basic tcpdump commands
---
tcpdump                     # Capture packets on the default interface
tcpdump -i eth0             # Capture packets on a specific interface (eth0)
tcpdump -D                  # List all available network interfaces
tcpdump -c 100 -i eth0      # Capture only 100 packets
tcpdump -w file.pcap        # Save captured packets to a file
tcpdump -r file.pcap        # Read packets from a saved file





# Capture Specific Traffic
tcpdump host 192.168.1.1                # Capture packets from/to a specific host
tcpdump src host 192.168.1.1            # Capture packets only from a specific source
tcpdump dst host 192.168.1.1            # Capture packets only to a specific destination
tcpdump net 192.168.1.0/24              # Capture traffic for an entire network
tcpdump port 443                        # Capture traffic on a specific port (HTTPS)
tcpdump src port 22                     # Capture packets from a specific source port
tcpdump dst port 53                     # Capture packets to a specific destination port
tcpdump portrange 20-25                 # Capture traffic within a port range




# Filter Traffic by Protocol
tcpdump arp                             # Capture ARP packets
tcpdump icmp                            # Capture ICMP (ping) packets
tcpdump tcp                             # Capture only TCP traffic
tcpdump udp                             # Capture only UDP traffic
tcpdump ip                              # Capture only IP traffic
tcpdump ether                            # Capture Ethernet packets



#  Advanced Filtering
tcpdump 'tcp and port 80'               # Capture only TCP traffic on port 80
tcpdump 'udp and port 53'               # Capture only DNS traffic (UDP port 53)
tcpdump 'src host 192.168.1.1 and tcp'  # Capture TCP traffic from a specific source
tcpdump 'dst net 10.0.0.0/8'            # Capture traffic destined for a specific network
tcpdump 'port 22 and not host 192.168.1.10' # Capture SSH traffic excluding a specific host




#  Verbose Output and Display Options
tcpdump -v                              # Verbose mode (detailed output)
tcpdump -vv                             # More verbose
tcpdump -vvv                            # Even more verbose
tcpdump -X                              # Show packet content in both HEX and ASCII
tcpdump -XX                             # Show full packet with Ethernet header
tcpdump -A                              # Show ASCII content of packets
tcpdump -n                              # Do not resolve hostnames
tcpdump -nn                             # Do not resolve hostnames or ports
tcpdump -e                              # Display Ethernet headers

# . Capture with Time Options**

sh

CopyEdit

`tcpdump -tttt                           # Human-readable timestamp tcpdump -ttt                            # Time difference between packets tcpdump -i eth0 -s 0 -w capture.pcap    # Capture full packet size (not truncated)`

---

### **7. Save and Read Captures**

sh

CopyEdit

`tcpdump -w capture.pcap                 # Save packets to a file
tcpdump -r capture.pcap                 # Read packets from a file
tcpdump -r capture.pcap -A | grep "password"  # Search for "password" in captured data`

---

### **8. Capture Specific Packet Length**

sh

CopyEdit

`tcpdump less 100                         # Capture packets smaller than 100 bytes 
tcpdump greater 1000                     # Capture packets larger than 1000 bytes`

---

### **9. Limit Packet Capture Size**

sh

CopyEdit

`tcpdump -s 0                       
#Capture full packets (default is truncated)
tcpdump -s 128                         
#Capture only the first 128 bytes of each packet`

---

### **10. Run `tcpdump` in Background**

sh

CopyEdit

`` tcpdump -i eth0 -w capture.pcap &        # Run in background disown                                   # Detach it from the terminal jobs                                     # Check running background jobs kill -9 <PID>                            # Stop `tcpdump` ``

---

### **Example Use Case**

Capture all SSH traffic (port 22) for a specific host and save to a file:

sh

CopyEdit

`tcpdump -i eth0 host 192.168.1.10 and port 22 -w ssh_traffic.pcap`

This should cover most of the **`tcpdump`** commands you might need. Let me know if you need more details! 🚀