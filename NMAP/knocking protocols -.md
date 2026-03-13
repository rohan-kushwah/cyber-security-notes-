# Knocking protocols

"Knocking protocols" generally refer to **Port Knocking** or **Single Packet Authorization (SPA)**. These are network security mechanisms used to **hide open ports** on a server until a specific sequence of packets (or a single packet in the case of SPA) is received. They act as a **stealth firewall control mechanism** to reduce the attack surface of a system.

---

## 1. What is Port Knocking

- **Definition:**  
    Port knocking is a method of externally opening ports on a firewall by generating a connection attempt on a sequence of closed ports.
    
- **Purpose:**  
    It allows administrators to **hide sensitive services** (like SSH or RDP) behind closed ports until an authenticated user performs the correct "knock sequence."
    
- **Mechanism:**  
    The server runs a **knock daemon** that listens passively to connection attempts (or packets) on predefined closed ports. When it detects the correct sequence, it dynamically modifies the firewall to open the hidden port for the requesting IP address.    

---
## 2. What is Single Packet Authorization (SPA)

- **Definition:**  
    SPA is a more modern, secure replacement for port knocking that uses a **single encrypted packet** to authenticate and authorize access.
    
- **Purpose:**  
    SPA eliminates the risk of replay attacks and packet sniffing by using encryption and cryptographic authentication.
    
- **Mechanism:**  
    The client sends a specially crafted, encrypted packet to the server. The server verifies it, and if valid, it temporarily opens the requested port.

---
## 3. Why Knocking Protocols Exist

- **Reduce attack surface:**  
    They keep ports closed by default, making it harder for attackers to detect running services.
    
- **Prevent brute-force attacks:**  
    By hiding ports (e.g., SSH port 22), attackers cannot attempt password guessing unless they first authenticate via knocking.
    
- **Provide "Security by Concealment":**  
    Even if it is not a complete replacement for authentication, it works as an additional protective layer.

---
## 4. When They Are Used

- On **internet-facing servers** running sensitive services (e.g., SSH, RDP).    
- In **environments without VPNs**, where administrators still want controlled access.
- For **temporary or emergency access** to a system.
- In **hardened systems** where reducing port exposure is critical.

---

## 5. How Knocking Protocols Work Internally

### 5.1 Port Knocking Process

1. **Client sends knock sequence:**
    - Example: Connect to port `1000`, then `2000`, then `3000`.
    - These ports are closed and will not respond.
        
2. **Daemon monitors packets:**    
    - A knock daemon (e.g., `knockd`) watches the firewall logs for matching sequences.
        
3. **Firewall updates rules:**
    - If the sequence is correct, it adds a temporary firewall rule to open the hidden service port (e.g., port 22 for SSH) for the client's IP.
        
4. **Client connects to service:**
    - After the firewall opens the port, the client connects normally.
        
5. **Port closes automatically:**
    - After a timeout, the firewall removes the rule and closes the port.

---
### 5.2 SPA Process

1. **Client crafts encrypted packet:**    
    - The packet contains authentication data, a timestamp, and the requested action.

2. **Server validates packet:**    
    - The SPA daemon verifies it using a shared key or public-key cryptography.
    
3. **Firewall opens port:**
    - If valid, the firewall opens the port for the client's IP.
        
4. **Access is temporary:**
    - After a set time, the port closes again.

---

## 6. Advantages and Disadvantages

|Feature|Port Knocking|Single Packet Authorization|
|---|---|---|
|Security Level|Moderate (can be sniffed/replayed)|High (encrypted)|
|Complexity|Low|Moderate|
|Speed|Slower (multiple packets)|Fast (single packet)|
|Replay Protection|No|Yes|
|Ease of Deployment|Simple|Slightly more complex|

---

## 7. Common Tools

- **Port Knocking:**
    - `knockd` (Linux)
    - `fwknop` (supports SPA as well)
        
- **SPA Tools:**
    - `fwknop` (Firewall Knock Operator)
    - Custom iptables/NFTables scripts

---
## 8. Real-World Example (Port Knocking with knockd)

```bash
# knockd.conf example
[options]
 logfile = /var/log/knockd.log

[openSSH]
 sequence    = 1000,2000,3000
 seq_timeout = 15
 command     = /sbin/iptables -A INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
 tcpflags    = syn

[closeSSH]
 sequence    = 3000,2000,1000
 seq_timeout = 15
 command     = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
 tcpflags    = syn
```

---
---
### Bypassing and Detecting Knocking Protocols (OSCP-Level)

Port knocking and SPA are **obfuscation-based security controls**. They can be bypassed or detected if an attacker has proper reconnaissance capabilities or is positioned in the right part of the network.

---
## 1. Reconnaissance Against Knocking Protocols

### 1.1 Detecting Knock Daemons

- **Behavioral detection:**  
    • Normally, closed ports should respond with **ICMP Port Unreachable** or no response at all.  
    • If a knock daemon is running, you may see **strange packet behavior** such as immediate temporary access after a specific sequence.
    
- **Banner fingerprinting:**  
    • Tools like `nmap` can be run repeatedly. If a port suddenly becomes open after a specific sequence of connections, it indicates port knocking.
    

---
### 1.2 Passive Sniffing

 **Man-in-the-middle (MITM):**  
 
 If an attacker is on the same LAN or has access to a compromised host in the path, they can use `tcpdump` or `Wireshark` to sniff knock packets.  

Example:

```bash
tcpdump -i eth0 port 1000 or port 2000 or port 3000
```

Once the sequence is captured, it can be **replayed** to unlock the port.    

---
### 1.3 Replay Attacks (Port Knocking)

**Why possible:**  
 Port knocking uses a static sequence (e.g., 1000 → 2000 → 3000).  
 If an attacker sees this once, they can reuse it because it lacks cryptographic authentication.

**How it's done:**
```bash
knock 192.168.1.10 1000 2000 3000
```

The firewall opens the port for the attacker’s IP as if they were the legitimate user.    

---
## 2. Bypassing Port Knocking

### 2.1 Brute Force Enumeration

If the knock sequence is short (3–4 ports) and the timeout is long, attackers can attempt brute forcing:
 
```bash
for p1 in {1000..1100}; do
  for p2 in {2000..2100}; do
	for p3 in {3000..3100}; do
	  knock target $p1 $p2 $p3
	  nmap -p22 target
	done
  done
done
```

This is noisy but sometimes works against poorly configured systems.    

---
### 2.2 Detecting Firewall Behavior

Use **Nmap timing options**:

```bash
nmap -p 1-65535 --max-retries 0 target
```

Repeated scans can show which ports become temporarily open, revealing the knock effect.

---
## 3. Bypassing SPA

SPA is harder to bypass because it uses **encryption and authentication**, but there are still potential weaknesses:

---
### 3.1 Credential Theft

- If an attacker compromises the client machine and steals SPA keys (e.g., from `fwknop.conf`), they can craft their own SPA packets.
---
### 3.2 Replay with Weak Implementation

- Poorly implemented SPA that lacks **nonce (random token)** or **timestamp verification** can be replayed.
- Example: If the packet is always the same, an attacker can capture and resend it.    

---
### 3.3 MITM Injection

- If the attacker controls the network (e.g., a malicious router), they can inject or modify packets if the SPA is not integrity-protected.    

---
## 4. Advanced Attack Vectors

- **Timing Analysis:**  
    • If a service suddenly opens after a series of packets, attackers may infer a knock sequence.
    
- **Side-Channel Leaks:**  
    • Sometimes firewall logs or misconfigured daemons leak sequence information via error messages.
    
- **Combination with Exploits:**  
    • If an attacker finds a way to modify firewall rules (via privilege escalation or command injection), knocking becomes irrelevant.    

---
## 5. Countermeasures Against Bypass

|Threat|Countermeasure|
|---|---|
|Replay attacks|Use SPA with encryption and timestamp validation|
|Sniffing|Use encrypted SPA (fwknop)|
|Brute-force knocking|Increase sequence length and enforce short timeout|
|MITM manipulation|Use HMAC or public-key SPA|
|Log monitoring|Monitor daemon logs and detect unauthorized knocking attempts|

---
---