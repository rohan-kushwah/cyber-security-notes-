# 🐞 What is a bug-bounty platform?

A **bug-bounty platform** is a marketplace where organizations publish programs that pay security researchers for finding valid vulnerabilities in their systems. Platforms manage rules of engagement, triage, communication, and payments between the company and hunters.

**Popular platforms (examples):**

- HackerOne
    
- Bugcrowd
    
- Synack (invite-only / vetted)
    
- Intigriti
    
- Open Bug Bounty (open disclosure)
    
- GitHub Security Lab / private programs run by companies
    

---

# 🧭 How bug-bounty works — step by step

1. **Find a program**
    
    - Choose a platform or company list. Read the program page carefully (scope, exclusions, rewards, disclosure policy).
        
2. **Understand scope & rules**
    
    - Allowed targets (domains, subdomains, apps), disallowed targets (employee accounts, physical attacks), test windows, and safe-testing rules (no DoS).
        
3. **Recon (enumeration)**
    
    - Subdomain discovery, directory brute, port scan (if allowed), third-party services, open S3 buckets, JavaScript analysis.
        
4. **Vulnerability discovery**
    
    - Manual testing + automated tools: scanner, Burp, ffuf, sqlmap when permitted, etc. Focus on high-impact classes: RCE, auth bypass, IDOR, SSRF, SQLi, broken access control.
        
5. **Proof of concept (PoC)**
    
    - Create a minimal reproducible PoC demonstrating impact. Include request/response, screenshots, curl commands, or exploit steps.
        
6. **Write the report**
    
    - Clear title, severity, affected host(s), steps to reproduce, PoC, impact, and recommended mitigation. Keep it concise and evidence-based.
        
7. **Submit via platform**
    
    - Use the platform’s submission form; answer triage questions. Respond quickly to requests for more info.
        
8. **Triage, fix, and reward**
    
    - Vendor triages; if valid, they assign severity and pay per program policy. You may be asked to verify fixes for bounty release.
        

---

# 🛠️ Example bounty workflow (quick)

- Program: `https://example.com` (web)
    
- Recon: found `payments.example.com` and `debug.example.com`
    
- Finding: `payments.example.com/api/transfer` has IDOR — you can transfer money between accounts by changing `account_id` parameter
    
- PoC: show two requests (victim and attacker) and responses proving unauthorized transfer
    
- Submit: title “IDOR in payments API allows funds transfer between accounts”; severity: High; include curl commands and remediation (authorize server-side check; validate owner of account_id)
    

---

# 📝 Example minimal report (copy/paste)

**Title:** IDOR in `/api/v1/transfer` allows transfer between accounts  
**Target:** `https://payments.example.com/api/v1/transfer`  
**Severity:** High (unauthorized funds transfer)  
**Description:** Changing `account_id` parameter in the POST body allows an authenticated attacker to move funds from arbitrary accounts to their own. No server-side ownership checks present.  
**Steps to reproduce (PoC):**

`# Attacker logged in (token: ATTACKER_TOKEN) curl -s -X POST "https://payments.example.com/api/v1/transfer" \   -H "Authorization: Bearer ATTACKER_TOKEN" \   -H "Content-Type: application/json" \   -d '{"from_account": "VICTIM_ACCOUNT_ID", "to_account": "ATTACKER_ACCOUNT_ID", "amount": 100}'`

**Response:**

`HTTP/1.1 200 OK {"status":"success","transaction_id":"tx12345"}`

**Impact:** Attacker can transfer funds from any account they know the ID of — funds loss, fraud.  
**Suggested fix:** Enforce server-side ownership check: ensure `from_account` belongs to the authenticated user and reject otherwise. Log and rate-limit transfers.  
**Attachments:** request/response screenshots, transaction id `tx12345`.

---

# 🧾 Common reward ranges (approximate)

Rewards vary hugely by program and company policy — small startups pay less, large tech firms pay more, and private programs pay the most. These are rough, common ranges in **USD**:

- **Critical (RCE, auth bypass, mass data leak):** $5,000 → $50,000+
    
- **High (account takeover, large data exposure, privilege escalation):** $1,000 → $10,000
    
- **Medium (SQLi limited, stored XSS with medium impact, IDOR limited):** $200 → $1,000
    
- **Low (reflected XSS, info disclosure non-sensitive):** $50 → $200
    
- **Minimal / Triaged (duplicate, out-of-scope, low impact):** $0 → $50 (or recognition only)
    
- **Hall of Fame / Recognition only:** no cash, but public kudos
    

Notes:

- Enterprise & financial companies often pay higher.
    
- Fixed-pay programs: some companies have fixed tiers (e.g., $500 for RCE).
    
- VDP (vulnerability disclosure programs) sometimes only offer recognition, not cash.
    

---

# ✅ Tips to increase your chances of paid bounties

- Read program rules and scope carefully — being out-of-scope kills bounties.
    
- Focus on high-impact classes (auth, access control, injection, SSRF).
    
- Produce clean, reproducible PoCs — triage loves clear evidence.
    
- Communicate respectfully and promptly with triage.
    
- Target new or poorly tested endpoints (APIs, admin panels, upload endpoints).
    
- Keep logs: curl commands, Burp history, screenshots, timestamps.
    
- Learn secure coding fixes so you can recommend realistic remediation.
    

---

# ⚖️ Legal & ethical notes (very important)

- Only test **in-scope** targets and follow the program’s rules.
    
- Never exfiltrate or publish sensitive user data; do not perform DoS unless explicitly allowed.
    
- If unsure, ask program contacts or keep testing to non-production / staging targets.
    
- Use VPNs/proxies as allowed; be mindful of local laws — unauthorized hacking is illegal.
    

---

# 📚 Tools commonly used

- Burp Suite (proxy + scanner)
    
- Nmap, ffuf, dirb/dirbuster, masscan
    
- sqlmap (only if allowed)
    
- HTTPie / curl for PoC requests
    
- ZAP, GHunt, Sublist3r, Amass for recon
    
- GitHub search for leaked keys