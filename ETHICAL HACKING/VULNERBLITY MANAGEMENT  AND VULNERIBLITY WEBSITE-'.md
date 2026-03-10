# Vulnerability Management

## Overview
**Vulnerability Management (VM)** is the **continuous process of identifying, evaluating, prioritizing, and mitigating security vulnerabilities** in systems, networks, and applications.  
It helps organizations reduce risk exposure by addressing weaknesses **before attackers can exploit them**.

---

## Objectives
- **Identify** security weaknesses across assets.  
- **Assess** the severity and impact of vulnerabilities.  
- **Remediate** or mitigate issues before exploitation.  
- **Report** on risk posture and progress over time.  
- **Continuously monitor** for new threats and exposures.

---

## Vulnerability Management Lifecycle

| Phase | Description | Example |
|--------|--------------|---------|
| **1. Discovery / Asset Identification** | Identify all devices, applications, and systems in the environment. | Scanning IP ranges to find active hosts. |
| **2. Vulnerability Scanning** | Use automated tools to detect known vulnerabilities. | Running Nessus or Qualys to scan servers. |
| **3. Assessment & Prioritization** | Evaluate risks based on severity (CVSS score), asset criticality, and exploitability. | Prioritizing CVSS 9.8 vulnerabilities on production servers. |
| **4. Remediation / Mitigation** | Apply patches, configuration fixes, or compensating controls. | Installing OS patches or disabling unused services. |
| **5. Verification & Reporting** | Re-scan to confirm remediation and document progress. | Generating compliance reports for ISO 27001 or SOC 2. |
| **6. Continuous Monitoring** | Schedule regular scans and track emerging vulnerabilities. | Weekly scans and real-time threat alerts. |

---

## Common Vulnerability Management Tools

### 1. **Nessus (by Tenable)**
- **Purpose:** Industry-standard vulnerability scanner.  
- **Features:** Detects misconfigurations, missing patches, and outdated software.  
- **Use Case:** Regular server and network scans to identify critical vulnerabilities.  
- **Website:** [https://www.tenable.com/products/nessus](https://www.tenable.com/products/nessus)

---

### 2. **Qualys Vulnerability Management**
- **Purpose:** Cloud-based platform for large-scale vulnerability scanning.  
- **Features:** Continuous monitoring, asset inventory, compliance checks.  
- **Use Case:** Enterprise-grade scanning across hybrid cloud and on-prem environments.  
- **Website:** [https://www.qualys.com/solutions/vulnerability-management/](https://www.qualys.com/solutions/vulnerability-management/)

---

### 3. **Rapid7 InsightVM (formerly Nexpose)**
- **Purpose:** Real-time vulnerability management and analytics.  
- **Features:** Risk scoring, dashboards, automated remediation tracking.  
- **Use Case:** Integrates with CI/CD pipelines for DevSecOps environments.  
- **Website:** [https://www.rapid7.com/products/insightvm/](https://www.rapid7.com/products/insightvm/)

---

### 4. **OpenVAS (Greenbone Vulnerability Manager)**
- **Purpose:** Open-source vulnerability scanning tool.  
- **Features:** Regular updates to detection databases, customizable scanning profiles.  
- **Use Case:** Cost-effective scanning for small to medium organizations.  
- **Website:** [https://www.greenbone.net/en/](https://www.greenbone.net/en/)

---

### 5. **Microsoft Defender Vulnerability Management**
- **Purpose:** Integrated solution for Windows and Microsoft 365 environments.  
- **Features:** Endpoint security, threat analytics, real-time risk assessments.  
- **Use Case:** Monitoring Windows systems for unpatched software and configuration issues.  
- **Website:** [https://www.microsoft.com/en-us/security/business/threat-protection/defender-vulnerability-management](https://www.microsoft.com/en-us/security/business/threat-protection/defender-vulnerability-management)

---

## Popular Websites for Tracking Daily Vulnerabilities

| Website | Description | URL |
|----------|--------------|-----|
| **NIST NVD** | National Vulnerability Database – official U.S. government repository for CVEs and CVSS scores. | [https://nvd.nist.gov/](https://nvd.nist.gov/) |
| **CVE Details** | Aggregated view of CVE records with vendor/product filters. | [https://www.cvedetails.com/](https://www.cvedetails.com/) |
| **MITRE CVE** | The official CVE (Common Vulnerabilities and Exposures) list. | [https://cve.mitre.org/](https://cve.mitre.org/) |
| **Exploit Database (Offensive Security)** | Repository of public exploits and proof-of-concept codes. | [https://www.exploit-db.com/](https://www.exploit-db.com/) |
| **SecurityFocus (Bugtraq)** | Historical vulnerability database and mailing list. | [https://www.securityfocus.com/vulnerabilities](https://www.securityfocus.com/vulnerabilities) |
| **CERT/CC (Carnegie Mellon)** | Alerts and advisories for software and network vulnerabilities. | [https://www.kb.cert.org/vuls/](https://www.kb.cert.org/vuls/) |
| **Rapid7 Vulnerability Database** | Up-to-date list of emerging threats and security flaws. | [https://www.rapid7.com/db/](https://www.rapid7.com/db/) |

---

## Example Workflow

1. **Scan** the environment using Nessus weekly.  
2. **Identify** CVEs and cross-check with the NVD database.  
3. **Prioritize** based on CVSS > 7 and asset importance.  
4. **Patch or mitigate** vulnerabilities using ITSM tickets.  
5. **Re-scan** to verify successful remediation.  
6. **Report** metrics to management for compliance tracking.

---

## Summary Diagram (Conceptual)

```mermaid
graph TD
A[Asset Discovery] --> B[Vulnerability Scanning]
B --> C[Risk Assessment]
C --> D[Remediation / Patching]
D --> E[Verification & Reporting]
E --> F[Continuous Monitoring]
