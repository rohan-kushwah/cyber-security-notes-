# Email Spoofing

***
### ❓ What is Email Spoofing?
Email spoofing is a technique used in cyber attacks to forge the **`From`** address of an email to make it appear as though it came from a **trusted or legitimate** source, even though it did not.

*   🎯 **Goal:** To trick the recipient into opening, trusting, or acting on a fraudulent email (e.g., clicking malicious links, downloading attachments, or disclosing sensitive information).

***
### 🛠️ How Email Spoofing Works
Spoofing typically modifies the **`SMTP "MAIL FROM"`** or **`"From" header`** in an email:

*   SMTP lacks built-in authentication, making it easy for attackers to forge the sender field.
*   Attackers often use **open mail relays** or **misconfigured MTAs** to send spoofed messages.
*   Advanced spoofing includes faking **`reply-to`**, **`display name`**, or entire email headers.

***
### 📄 Example of Spoofed Header
```
1 From: "Bank Support" <support@bank.com>
2 Reply-To: hacker@gmail.com
3 Received: from attacker-server.com (192.0.2.1)
```
The 'From' address looks legit, but the mail actually came from `attacker-server.com`.

***
### ▲ Risks of Email Spoofing
*   Phishing attacks
*   Business Email Compromise (BEC)
*   Credential theft
*   Malware distribution

***
### 🛡️ How to Detect Email Spoofing
***
*   Check **`email headers`** for suspicious **`Received:`** lines and IP addresses.
*   Verify **`SPF`**, **`DKIM`**, and **`DMARC`** records.

***
### ✅ Preventing Email Spoofing (Defensive Measures)
***

| Technology | Purpose |
| :--- | :--- |
| **SPF (Sender Policy Framework)** | Defines which mail servers are allowed to send on behalf of a domain |
| **DKIM (DomainKeys Identified Mail)** | Adds digital signature to verify the message content hasn't been altered |
| **DMARC (Domain-based Message Authentication, Reporting & Conformance)** | Combines SPF & DKIM to enforce policy and report spoofing attempts |

***
### 🧪 Tools to Analyze Spoofed Emails
***
*   [MXToolbox SPF/DKIM Checker](https://mxtoolbox.com/SuperTool.aspx)
*   [Google Admin Toolbox Messageheader Analyzer](https://toolbox.googleapps.com/apps/messageheader/)
*   `email-tracer` tools like:
    *   [Cyber Forensics](https://whatismyipaddress.com/trace-email)
    *   [IP2Location Email Tracer](https://www.ip2location.com/email-tracer)

### 🛠️ Online Email Spoofing Tools (For Testing & Education Only)
***

| Tool                                                                      | Description                                                |
| :------------------------------------------------------------------------ | :--------------------------------------------------------- |
| [Anonymailer.net](https://anonymailer.net/)                               | Send anonymous or spoofed emails without registration      |
| [Emkei.cz](https://emkei.cz/)                                             | Popular spoof email web interface with full header control |
| [SpoofBox Email Preview](https://www.spoofbox.com/en/preview/spoof-email) | Spoof email and SMS testing tools with optional login      |

### 🔎 DMARC Record Checker Tools
***
Use these tools to check a domain's DMARC, SPF, and DKIM records and verify email protection settings.

| Tool                                                              | Description                                               |
| :---------------------------------------------------------------- | :-------------------------------------------------------- |
| [EasyDMARC Lookup Tool](https://easydmarc.com/tools/dmarc-lookup) | Checks DMARC, SPF, DKIM for any domain                    |
| [MXToolbox](https://mxtoolbox.com/)                               | Multi-purpose DNS, SMTP, and email authentication checker |
| [DMARCian Inspector](https://dmarcian.com/dmarc-inspector/)       | Visualizes DMARC records and evaluates alignment          |

### 🧪 Command-Line: Check DMARC/SPF/DKIM TXT Records with `dig`
***

*   Check TXT records for `armourinfosec.com`
    ```bash
    dig -t txt armourinfosec.com
    ```
*   Check TXT records for `india.gov.in`
    ```bash
    dig -t txt india.gov.in
    ```

Look for results containing:

*   `v=DMARC1;` for DMARC
*   `v=spf1` for SPF
*   `k=rsa; p=...` for DKIM (in selector subdomain)

---
----