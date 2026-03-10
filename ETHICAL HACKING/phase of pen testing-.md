# Penetration Testing Summary Flow

## ✅ 1. Planning

**Objective:**  
Define the scope, goals, and legal boundaries of the penetration test.

**Activities Include:**

- Getting client approvals
    
- Defining target systems/IP ranges
    
- Setting up timelines and deliverables
    
- Identifying rules of engagement (ROE)
    

---

## 🕵️‍♂️ 2. Reconnaissance

**Objective:**  
Gather information about the target without direct interaction (passive) or with limited probing (active).

**Tools Used:**  
theHarvester, Maltego, Shodan, FOCA

**Installation (Kali Linux):**

- `sudo apt install theharvester`
    
- `sudo apt install maltego`
    
- `pip install shodan` (then `shodan init <API_KEY>`)
    
- FOCA is Windows-based; run via Wine or VM
    

**Activities Include:**

- Identifying domain names, emails, IPs
    
- Social media research
    
- WHOIS, DNS info collection
    

---

## 🔎 3. Vulnerability Analysis (Scanning & Analysis)

**Objective:**  
Identify potential vulnerabilities in the target system.

**Tools Used:**  
Nmap, Nessus, OpenVAS, Nikto

**Installation (Kali Linux):**

- `sudo apt install nmap`
    
- Download Nessus from tenable.com
    
- `sudo dpkg -i Nessus-*.deb && sudo systemctl start nessusd`
    
- `sudo gvm-setup && sudo gvm-start` (for OpenVAS)
    
- `sudo apt install nikto`
    

**Activities Include:**

- Port and service scanning
    
- OS and version detection
    
- Vulnerability scans and misconfiguration checks
    

---

## 💥 4. Exploitation

**Objective:**  
Use discovered vulnerabilities to gain unauthorized access.

**Tools Used:**  
Metasploit, SQLMap, Burp Suite, custom scripts

**Installation (Kali Linux):**

- `sudo apt install metasploit-framework`
    
- `sudo apt install sqlmap`
    
- `sudo apt install burpsuite`
    

**Activities Include:**

- Gaining access to systems or networks
    
- Escalating privileges
    
- Pivoting to other systems
    

---

## 🔐 5. Post-Exploitation

**Objective:**  
Assess the value of the compromised system and maintain access.

**Tools Used:**  
Mimikatz, BloodHound, PowerView

**Installation (Kali Linux):**

- BloodHound: `sudo apt install bloodhound`
    
- Neo4j DB (for BloodHound): `neo4j console`
    
- Mimikatz and PowerView are used on Windows targets (via payloads or remote shells)
    

**Activities Include:**

- Extracting credentials
    
- Mapping internal network (Active Directory)
    
- Installing persistence mechanisms
    

---

## 📝 6. Reporting

**Objective:**  
Document findings, risks, and recommendations in a formal report.

**Tools Used:**  
Dradis, Serpico, Markdown/PDF tools, Screenshot tools

**Installation (Kali Linux):**

- `sudo apt install dradis`
    
- Serpico:
    
    ```
    git clone https://github.com/SerpicoProject/Serpico.git
    cd Serpico
    ./scripts/kali_install.sh
    ./start_serpico.sh
    ```
    
- Markdown & PDF: `sudo apt install pandoc libreoffice`
    
- Screenshots: `sudo apt install shutter flameshot`
    

**Activities Include:**

- Compiling vulnerability data and evidence
    
- Assigning severity scores (e.g., CVSS)
    
- Creating an executive summary and technical details
    

---

## 🔁 7. Retesting (Optional)

**Objective:**  
Ensure that identified vulnerabilities have been resolved effectively.

**Activities Include:**

- Repeating exploitation tests on patched systems
    
- Verifying if mitigations were successful
    
- Confirming no new vulnerabilities were introduced