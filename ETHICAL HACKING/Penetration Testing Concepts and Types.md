# 🔐 Penetration Testing Concepts and Types

  

---

  

## 🧠 Internal Network Penetration Test

  

**What it is:**  

A simulation of an attack from within the organization’s network (e.g., a malicious insider or attacker who gained VPN access).

  

**Goal:**  

To identify what an attacker could do once inside the network — lateral movement, privilege escalation, data access, etc.

  

**Typical Scenarios:**

- Malicious employee or contractor

- Attacker plugged into internal network

- Compromised workstation

- Leaked VPN credentials

  

**Key Test Areas:**

- Network enumeration and segmentation

- Password cracking

- Privilege escalation

- Lateral movement (pivoting)

- Data access and exfiltration

- Exploiting internal services (SMB, RDP, AD)

- Domain controller attacks (e.g., Kerberoasting, Pass-the-Hash)

  

**Common Tools:**

- Nmap, BloodHound, CrackMapExec

- Mimikatz, Responder, PowerSploit

- Metasploit, Cobalt Strike

  

---

  

## 🌐 External Network Penetration Test

  

**What it is:**  

A simulation of an attack from the internet — like a black-hat hacker attempting unauthorized access.

  

**Goal:**  

Identify and exploit vulnerabilities in public-facing systems.

  

**Typical Scenarios:**

- Internet-wide scanning by attackers

- Targeted attack on company domains

- Testing cloud or SaaS services

  

**Key Test Areas:**

- Recon and OSINT

- Port scanning and banner grabbing

- Web app attacks (XSS, SQLi)

- Exploiting misconfigurations

- Brute-force/credential stuffing

- SSL/TLS configuration checks

  

**Common Tools:**

- Nmap, Nikto, Gobuster

- Burp Suite, OWASP ZAP

- Metasploit, Hydra

- Shodan, Censys

  

---

  

## 🔄 Comparison Table

  

| Feature              | Internal Pentest                     | External Pentest                        |

|----------------------|--------------------------------------|------------------------------------------|

| **Attacker's Location** | Inside the network                  | Outside (Internet)                      |

| **Threat Simulation**   | Insider threat, compromised host   | Black-hat hacker                        |

| **Access Level**        | Partial/local access               | No prior access                         |

| **Focus Areas**         | Lateral movement, privilege escalation | Perimeter defenses, open ports     |

| **Impact Tested**       | AD, file shares, internal apps     | Web servers, VPNs, APIs, DMZ            |

  

---

  

## 🛡️ What is a Jumpbox?

  

A **Jumpbox** (aka Jump Server, Bastion Host) is a hardened server that acts as a secure entry point to internal systems.

  

**Purpose:**

- Secure gateway to internal servers

- Reduces exposure of sensitive systems

- Central point for access auditing

  

**How It Works:**

1. User connects to the Jumpbox via SSH or RDP.

2. From there, connects to internal servers.

3. Internal servers block all external access except from the Jumpbox.

  

**Security Features:**

- Placed in DMZ or public subnet

- Enforces SSH key/MFA

- Logs all sessions

- Runs minimal services

- No direct internet access

  

**Example Scenario:**

In AWS or a private datacenter:

- Internal servers only accept traffic from the Jumpbox

- Developers access production via the Jumpbox only

  

**Benefits:**

  

| Benefit              | Explanation |

|----------------------|-------------|

| **Centralized Access** | One gateway to all systems |

| **Improved Security** | Limits public exposure |

| **Auditing**          | Logs access from one point |

| **Reduced Attack Surface** | Only Jumpbox is internet-facing |

  

---

  

## 📋 Types of Penetration Testing (Summary)

  

| **Type**               | **Brief Description** |

|------------------------|------------------------|

| **Network**            | Tests internal and external infrastructure (firewalls, routers, servers). |

| **Web Application**    | Looks for OWASP Top 10 vulnerabilities (e.g., XSS, SQLi). |

| **API**                | Evaluates REST/SOAP APIs for auth issues, data leaks, etc. |

| **Mobile Application** | Analyzes Android/iOS apps for poor security practices. |

| **Desktop Applications** | Tests local endpoint software for flaws. |

| **Wireless**           | Tests Wi-Fi networks for weak encryption and rogue devices. |

| **IoT**                | Examines smart devices for firmware flaws, open ports. |

| **Red Teaming**        | Full-scale simulated attack including social engineering. |

| **Cloud**              | Evaluates AWS, Azure, GCP for misconfigurations. |

| **Social Engineering** | Tests humans via phishing, impersonation, baiting. |

| **Physical Security**  | Attempts to breach physical access controls. |

  

---