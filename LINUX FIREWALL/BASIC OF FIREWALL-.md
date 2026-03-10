# Firewall

AFirewalls in Linux - Detailed Explanation 

1. Introduction to Firewalls 

A firewall is a security system that monitors and controls incoming and outgoing network traffic based on predefined security rules. It acts as a barrier between a trusted network (such as an internal system) and an untrusted network (such as the internet). The primary function of a firewall is to prevent unauthorized access, detect malicious activity, and enforce security policies. 

Firewalls are categorized based on their deployment: 

1. Host-Based Firewalls – These are installed on individual computers (workstations or servers) to protect them from network threats. 
    
2. Network-Based Firewalls – These are implemented between networks to filter traffic, such as corporate firewalls controlling access to the internet. 
    

A firewall evaluates network packets and determines whether to allow, deny, or drop them based on administrator-defined rules. These rules can filter packets based on: 

- Source IP Address – Determines which systems can send traffic. 
    
- Destination IP Address – Restricts access to certain services or sites. 
    
- Protocol – Filters based on protocols like TCP, UDP, and ICMP. 
    
- Ports – Controls access to services like SSH (port 22) and HTTP (port 80). 
    

Firewall rules are processed sequentially, meaning the first matching rule determines the packet’s fate. If no matching rule exists, the default policy applies, which is usually set to either allow or deny all traffic. 

1.1 Firewalls in Linux 

Linux firewalls operate using packet filtering frameworks integrated into the kernel. These frameworks allow system administrators to define traffic rules and enforce security policies efficiently. 

  

1. Key Functions of a Firewall 

2.1 Traffic Filtering 

- Firewalls examine packets and apply security rules based on IP addresses, ports, and protocols. 
    
- Example: A rule may allow HTTP traffic (port 80) but block FTP traffic (port 21) to prevent unauthorized file transfers. 
    

2.2 Access Control 

- Firewalls define who can access what within a network. 
    
- Example: An organization may allow SSH access (port 22) only from internal IPs while blocking external connections. 
    

2.3 Intrusion Prevention 

- Firewalls can detect and block suspicious activities, such as port scanning or excessive data transfers. 
    
- Example: A firewall may block IPs that attempt repeated failed login attempts to prevent brute-force attacks. 
    

2.4 Monitoring & Logging 

- Firewalls maintain logs of traffic activity for security auditing and forensic analysis. 
    
- Example: Administrators can review logs to detect unauthorized access attempts. 
    

  

1. Types of Firewalls 

Firewalls are classified based on how they inspect traffic and where they operate within a network. 

3.1 Packet-Filtering Firewalls (Stateless Firewalls) 

Packet-filtering firewalls operate at the network layer (Layer 3) of the OSI Model. They inspect packets based on: 

- Source and Destination IP Addresses 
    
- Source and Destination Ports 
    
- Protocol Type (TCP, UDP, ICMP, etc.) 
    

These firewalls do not track connection states, meaning each packet is treated independently without any knowledge of previous packets. 

Example Rule: 

- Allow TCP traffic from 192.168.1.100 to 10.0.0.5 on port 80. 
    

Pros: 

- Fast and efficient due to simple filtering logic. 
    
- Requires minimal system resources. 
    

Cons: 

- Less secure because it cannot differentiate between legitimate and spoofed packets. 
    
- Vulnerable to attacks like IP spoofing and packet fragmentation. 
    

  

3.2 Stateful Firewalls 

Stateful firewalls operate at both the network (Layer 3) and transport (Layer 4) layers of the OSI model. Unlike packet-filtering firewalls, stateful firewalls track active connections and ensure that only legitimate response packets are allowed through. 

When a connection is established (e.g., a TCP handshake), the firewall remembers its state. It then allows only valid response packets for that connection while blocking unsolicited packets. 

Example: 

1. A client sends a TCP SYN request to a web server. 
    
2. The firewall remembers this request and allows only legitimate SYN-ACK responses. 
    
3. If an unsolicited SYN-ACK arrives without an initial SYN request, the firewall blocks it. 
    

Pros: 

- More secure because it prevents unauthorized packets. 
    
- Protects against session hijacking and unauthorized access. 
    

Cons: 

- Requires more processing power as it maintains a state table. 
    
- May become overwhelmed in high-traffic environments if not optimized properly. 
    

  

3.3 Next-Generation Firewalls (NGFWs) 

Next-Generation Firewalls (NGFWs) operate at the application layer (Layer 7) and provide deep packet inspection (DPI), allowing them to analyze the actual contents of a packet rather than just its headers. 

NGFWs combine traditional stateful firewall capabilities with advanced security features, including: 

- Intrusion Prevention Systems (IPS) – Detects and blocks network threats. 
    
- Malware Filtering – Scans traffic for known malware signatures. 
    
- Application Awareness – Identifies and controls applications like Skype, BitTorrent, and social media traffic. 
    
- VPN Support – Secure remote access for enterprise networks. 
    

Example: 

- An NGFW can detect and block malicious HTTP requests, such as: 
    
    - SQL Injection attempts. 
        
    - Cross-Site Scripting (XSS) attacks. 
        
    - Unauthorized API requests. 
        

Pros: 

- Offers superior security by inspecting packet payloads. 
    
- Provides detailed traffic control, preventing unauthorized applications from running. 
    

Cons: 

- Resource-intensive, requiring powerful hardware to process DPI. 
    
- More complex to configure compared to traditional firewalls. 
    

  

1. Firewalls in Linux 

Linux supports multiple firewall solutions, each offering unique features for different use cases. 

4.1 Commonly Used Linux Firewall Solutions 

|   |   |   |   |   |
|---|---|---|---|---|
|Component|Type|Role|Status|Used In|
|Netfilter|Kernel Framework|Core packet filtering in Linux kernel|✅ Active|All Linux Distributions|
|iptables|Firewall Framework|Legacy firewall tool using Netfilter|❌ Deprecated|Older Linux Versions|
|nftables|Firewall Framework|Modern replacement for iptables|✅ Latest|All New Linux Versions|
|firewalld|Firewall Manager|High-level firewall with zone-based security|✅ Active|RHEL-based (RHEL, CentOS, Fedora)|
|UFW|Firewall Manager|Simplified firewall interface|✅ Active|Ubuntu, Debian, Kali|

  

- ![](data:image/png;base64,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)
    

Firewalls in Linux  

Chapter 1: Introduction to Firewalls in Linux 

A firewall is a network security system that monitors and controls incoming and outgoing traffic based on predetermined security rules. It acts as a barrier between a trusted network (such as an internal system) and an untrusted network (such as the internet). Firewalls prevent unauthorized access and help in securing systems from attacks. 

In Linux, firewalls work by filtering network packets, which are small chunks of data transmitted over a network. These packets contain information such as the source IP address, destination IP address, port numbers, and protocols. 

To control the flow of these packets, Linux provides several firewall solutions: 

1. iptables – A legacy firewall system that uses the Netfilter framework. 
    
2. nftables – A modern replacement for iptables with improved efficiency. 
    
3. firewalld – A dynamic firewall manager used in RHEL-based distributions. 
    
4. ufw (Uncomplicated Firewall) – A simplified firewall interface for beginners. 
    

Each of these firewalls provides a way to define rules that determine how network traffic is handled. These rules can allow or deny packets based on criteria such as IP addresses, ports, and protocols. 

  

Chapter 2: Understanding How Linux Firewalls Work 

To understand how a Linux firewall works, we need to first understand the packet filtering framework in the Linux kernel. 

2.1 The Role of Netfilter 

All Linux firewalls rely on a kernel-level framework called Netfilter to process network packets. Netfilter is a built-in system in the Linux kernel that inspects packets at different stages of their journey through the network stack. 

When a packet enters a Linux machine, Netfilter processes it by following a set of rules that determine whether the packet should be accepted, dropped, rejected, or modified. 

2.2 The Path of a Network Packet in Linux 

When a network packet arrives at a Linux system, it goes through different stages of processing: 

1. PREROUTING: The packet first arrives at the system. This is the stage where Network Address Translation (NAT) can modify the packet before it is routed. 
    
2. Routing Decision: The system decides whether the packet is for the local machine or if it needs to be forwarded to another destination. 
    
3. INPUT Chain: If the packet is for the local machine, it moves through the INPUT chain, where firewall rules decide whether it is allowed or blocked. 
    
4. FORWARD Chain: If the packet is not for the local machine and needs to be forwarded to another system, it is processed by the FORWARD chain. 
    
5. OUTPUT Chain: If the local machine generates an outgoing packet, it moves through the OUTPUT chain before being sent. 
    
6. POSTROUTING: Before the packet leaves the system, NAT rules in the POSTROUTING chain may modify its source address. 
    

This process ensures that every packet passing through a Linux system is filtered and processed according to firewall rules. 

Chapter 3: Working of iptables (Legacy Firewall System) 

3.1 What is iptables? 

iptables is an older firewall system in Linux that works with Netfilter to filter packets. It allows administrators to define rules that control network traffic at various stages. 

iptables uses tables and chains to process packets. 

3.2 iptables Architecture 

The iptables system is built on two main concepts: 

1. Chains – These are rule sets that packets pass through. 
    
2. Tables – These contain different types of chains, each serving a specific function. 
    

3.3 Understanding Chains in iptables 

A chain in iptables is a sequence of rules that determine what happens to a packet. These rules can either accept, drop, reject, or modify packets based on their attributes. 

There are three main chains in the filter table: 

1. INPUT Chain: Handles packets that are destined for the local system. 
    
2. OUTPUT Chain: Handles packets originating from the local system. 
    
3. FORWARD Chain: Handles packets that pass through the system (e.g., if the system is acting as a router). 
    

Additionally, there are two chains used for Network Address Translation (NAT): 

1. PREROUTING Chain: Modifies incoming packets before routing. 

2. POSTROUTING Chain: Modifies outgoing packets before they leave the system. 

For example, if an external machine sends an SSH request to a Linux server, the request first enters the PREROUTING chain, then passes through the INPUT chain, where firewall rules determine whether the request is allowed. 

  

3.4 Understanding Tables in iptables
A table is a collection of chains that serve different purposes. The main tables in iptables are: 

1. filter table – This is the default table used for packet filtering. It contains the INPUT, OUTPUT, and FORWARD chains. 
    
2. nat table – This table handles Network Address Translation (NAT). It modifies source or destination addresses of packets. 
    
3. mangle table – This table is used for modifying packet headers, such as changing the TTL (Time To Live) field. 
    
4. raw table – This table is used for packets that should bypass connection tracking. 
    

Each of these tables has specific functions in controlling packet behavior. 

  

3.5 How iptables Processes a Packet 

When a packet enters the system, iptables follows these steps: 

1. The packet is checked against the rules in the PREROUTING chain. 
    
2. If the packet is destined for the local system, it is checked against the INPUT chain. 
    
3. If the packet is to be forwarded to another system, it passes through the FORWARD chain. 
    
4. If the local system generates an outgoing packet, it goes through the OUTPUT chain. 
    
5. Before leaving the system, the packet is processed in the POSTROUTING chain. 
    

Each of these steps follows the first matching rule, meaning that once a rule matches a packet, the action is applied immediately, and the packet is no longer checked against other rules. 

Chapter 4: Working of nftables (Modern Firewall in Linux) 

4.1 Introduction to nftables 

nftables is a modern firewall framework introduced as a replacement for iptables. It provides a more efficient and flexible way to manage firewall rules in Linux. Unlike iptables, which relies on multiple tables and chains, nftables uses a single ruleset that makes rule processing faster and more efficient. 

It is built on the same Netfilter framework as iptables, meaning it works at the kernel level to inspect and control network traffic. However, it provides a simpler syntax and supports better performance optimizations. 

4.2 Why was nftables Introduced? 

While iptables was widely used for a long time, it had several limitations: 

1. Complex Rule Management: Rules were stored in multiple tables (filter, nat, mangle, raw) and multiple chains, making rule management difficult. 
    
2. Performance Issues: iptables processed rules linearly, which slowed down performance in systems with large rulesets. 
    
3. No Atomic Rule Updates: Any change in iptables required flushing and reloading all rules, which could lead to downtime or errors. 
    
4. Limited Data Types: iptables only supported numeric values (like ports and IP addresses), making rule definitions restrictive. 
    

To overcome these issues, nftables was developed, offering: 

- Unified rule management with a single ruleset. 
    
- Faster packet processing through optimized rule matching. 
    
- Atomic rule updates, meaning new rules can be added or modified without affecting active connections. 
    
- Support for maps and sets, which allows defining multiple IPs and ports in a single rule. 
    

4.3 Architecture of nftables 

nftables operates using three main components: 

1. Tables – These store firewall rules and contain chains. 
    
2. Chains – These define where in the packet processing path a rule applies. 
    
3. Rules – These specify actions to take on packets (allow, drop, modify, redirect, etc.). 
    

4.4 Understanding Tables, Chains, and Rules in nftables 

4.4.1 Tables in nftables 

A table is a container that holds firewall rules and chains. It defines the type of rules it will process. Tables can work in different network families: 

- inet (for both IPv4 and IPv6 traffic) 
    
- ip (for IPv4-only traffic) 
    
- ip6 (for IPv6-only traffic) 
    
- arp (for Address Resolution Protocol packets) 
    

4.4.2 Chains in nftables 

A chain is a collection of rules that apply to packets at a specific stage in their journey through the network stack. Chains are categorized based on their hook (where they apply): 

1. input – Handles packets destined for the local machine. 
    
2. output – Handles packets originating from the local machine. 
    
3. forward – Handles packets that are being forwarded through the system. 
    
4. prerouting – Modifies packets before routing decisions are made (used for NAT). 
    
5. postrouting – Modifies packets after routing decisions are made (used for NAT). 
    

4.4.3 Rules in nftables 

A rule defines how packets should be handled. It consists of match conditions (such as protocol, IP, or port) and an action (such as accept, drop, or modify). 

4.5 How nftables Processes a Packet 

When a network packet arrives at the system, nftables follows these steps: 

1. The packet enters the system and is checked against the PREROUTING chain (if it exists). 
    
2. If the packet is for the local machine, it moves to the INPUT chain. 
    
3. If the packet is meant to be forwarded, it moves to the FORWARD chain. 
    
4. If the system is generating an outgoing packet, it moves to the OUTPUT chain. 
    
5. Before leaving the system, the packet is processed in the POSTROUTING chain. 
    

At each step, nftables checks the packet against the rules in the corresponding chain and applies the first matching rule. 

Chapter 5: Working of firewalld (Dynamic Firewall Management in Linux) 

5.1 Introduction to firewalld 

firewalld is a dynamic firewall management tool that provides an easier way to manage firewall rules in Linux. Unlike iptables and nftables, which require manually defining rules and chains, firewalld simplifies firewall management using zones and services. 

It acts as a high-level interface to control network traffic without requiring deep knowledge of iptables or nftables. firewalld is the default firewall management system in Red Hat-based distributions like RHEL, CentOS, and Fedora. 

  

5.2 Why was firewalld Introduced? 

Before firewalld, administrators had to manually configure iptables or nftables, which involved writing and maintaining complex firewall rules. firewalld was introduced to: 

1. Provide a Zone-Based Firewall – Instead of configuring rules for every interface, firewalld allows defining zones (such as public, home, or work) and assigning interfaces to them. 
    
2. Enable Dynamic Rule Management – Changes take effect immediately without restarting the firewall, unlike iptables, which requires rule flushing. 
    
3. Simplify Configuration – Users can enable or disable services (like SSH, HTTP) without defining port numbers manually. 
    
4. Support Both nftables and iptables – firewalld works as a wrapper around either framework, allowing compatibility with both. 
    

  

5.3 Architecture of firewalld 

firewalld operates using: 

- Zones – Define different security levels and apply rules accordingly. 
    
- Services – Predefined rules for common applications (SSH, HTTP, FTP, etc.). 
    
- Rich Rules – Advanced filtering rules with source and destination control. 
    
- Direct Rules – Custom rules that interact directly with iptables or nftables. 
    

5.4 Understanding firewalld Zones 

A zone is a security level that determines how network traffic is handled. Instead of configuring individual rules for every interface, you assign an interface to a zone, and all rules within that zone apply to traffic on that interface. 

5.4.1 Default firewalld Zones 

|   |   |   |
|---|---|---|
|Zone|Security Level|Description|
|drop|Highest|Drops all incoming traffic. Only outgoing connections are allowed.|
|block|High|Similar to drop, but returns "ICMP destination unreachable" for rejected packets.|
|public|Moderate|Allows only specific services (like SSH, HTTP). Used for public networks.|
|external|Moderate|Used for routers/NAT with masquerading enabled.|
|dmz|Moderate|Used for Demilitarized Zones (DMZ), allowing specific inbound traffic.|
|work|Low|Allows more trusted services but restricts incoming traffic.|
|home|Lower|Allows most inbound services, assuming a trusted network.|
|trusted|No filtering|Allows all incoming and outgoing traffic.|

5.5 Understanding firewalld Services 

A service in firewalld is a predefined set of firewall rules that allows traffic for a specific application. Instead of manually specifying port numbers, you can enable a service like SSH, HTTP, or FTP. 

5.6 Understanding firewalld Ports and Protocols 

If a service is not predefined, you can manually allow or block specific ports. 

5.7 Advanced Configuration: firewalld Rich Rules 

For more granular control, firewalld supports rich rules, which allow filtering based on source addresses, logging, and custom actions. 

5.8 How firewalld Processes a Packet 

When a packet arrives at a Linux system with firewalld enabled, the following steps occur: 

1. The system identifies the network interface through which the packet arrived. 
    
2. The packet is associated with a zone based on the assigned interface. 
    
3. The rules defined in that zone are checked. 
    
4. If a matching rule is found (allow/block), the corresponding action is applied. 
    
5. If no specific rule matches, the default zone behavior determines whether the packet is allowed or dropped. 
    

This process allows firewalld to dynamically control traffic based on security zones. 

Chapter 6: Working of UFW (Uncomplicated Firewall in Linux) 

6.1 Introduction to UFW 

ufw (Uncomplicated Firewall) is a user-friendly firewall management tool for Linux that simplifies working with iptables. It is the default firewall in Ubuntu and other Debian-based distributions. 

Unlike iptables, which requires complex rule definitions, ufw provides simple, human-readable commands to allow or block network traffic. It is designed to make firewall management easy for beginners while still offering powerful features for advanced users. 

6.2 Why was UFW Introduced? 

Before ufw, managing firewalls in Linux was difficult because: 

1. iptables has a complex syntax, making it hard for beginners to use. 
    
2. Manually configuring rules was time-consuming. 
    
3. Persistent firewall rules required additional scripts or manual configurations. 
    

To solve these issues, ufw was created with: 

- A simplified command structure for easier rule management. 
    
- Predefined application profiles to allow services like SSH and HTTP without specifying ports. 
    
- Automatic persistence, ensuring that rules survive system reboots. 
    

6.3 How UFW Works 

UFW operates as a wrapper around iptables, meaning it interacts with iptables but provides a simpler command-line interface for managing firewall rules. 

It follows these steps to process network traffic: 

1. A packet arrives at the system. 
    
2. UFW checks its ruleset to determine if the packet should be allowed or blocked. 
    
3. If a matching rule is found, the packet is processed accordingly (allowed or dropped). 
    
4. If no matching rule is found, the packet follows the default policy (deny or allow). 
    

6.4 Default Policies in UFW 

By default, UFW denies all incoming traffic and allows all outgoing traffic. This ensures that the system is protected from unauthorized access while still being able to connect to external services. 

6.9 UFW Application Profiles 

UFW includes predefined application profiles for commonly used services like SSH, Apache, and OpenVPN. 

Example : 

  OpenSSH   

  Apache   

  Nginx Full   

  OpenVPN   

6.11 How UFW Processes a Packet 

1. A packet enters the system. 
    
2. UFW checks its rules for a matching entry. 
    
3. If a matching rule exists, the packet is allowed or blocked based on the rule. 
    
4. If no rule matches, the packet follows the default policy (deny or allow). 
    
5. The packet is either forwarded to the application or dropped. 
    

This ensures that only authorized traffic reaches the system while blocking unauthorized access. 

✅ Red Hat-Based OS (RHEL, CentOS, Fedora, Rocky, AlmaLinux) 

- firewalld is the default firewall (uses nftables or iptables as backend). 
    
- nftables is preferred for manual rule management. 
    
- iptables is deprecated, but still available for legacy compatibility. 
    

✅ Debian-Based OS (Debian, Ubuntu, Kali, Linux Mint) 

- UFW (Uncomplicated Firewall) is default in Ubuntu/Debian for ease of use. 
    
- nftables is used for advanced firewall configurations. 
    
- iptables is deprecated, but still available for older scripts. 
    

Inbound traffic refers to any data coming into a system or network from an external source. This includes requests from remote devices, users, or servers trying to communicate with services running on the local machine. 

Outbound traffic refers to any data leaving a system or network and going to an external destination. This includes requests made by local applications, servers, or users to remote systems or websites.

---
