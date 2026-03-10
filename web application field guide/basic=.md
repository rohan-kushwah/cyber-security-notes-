# 🕷️ Web Application Penetration Testing — Complete Field Guide

> _Methodology · Checklist · Attack Surface Coverage_

---

## 📚 Books for Web Application Penetration Testing

> _Here are some of the best and most recommended books for Web Application Penetration Testing in 2025:_

|#|Book|Key Focus|
|---|---|---|
|1|📖 **The Web Application Hacker's Handbook** — _Dafydd Stuttard & Marcus Pinto_|Classic comprehensive guide: info gathering → attacking web app vulnerabilities. **Must-have for pentest professionals.**|
|2|📖 **Penetration Testing: Execution and Analysis** — _Rafay Baloch_|Step-by-step walkthrough: preparation, info gathering, exploitation, and reporting.|
|3|📖 **The Penetration Tester's Guide to Web App Security** — _Ryan Burnham_|Core web app attacks: SQL injection, cross-site scripting, and CSRF.|
|4|📖 **Hacking: The Art of Exploitation** — _Jon Erickson_|Fundamentals of exploitation: buffer overflows and shellcode.|
|5|📖 **Penetration Testing with Python: A Practical Guide** — _Dinesh S. Shenai_|Python-driven practical pentest techniques applicable to web apps.|
|6|📖 **The Hacker Playbook 3**|Lab-based, real-world attack campaigns and methodologies for web app pentesters.|

> 💡 For hands-on learning, **PortSwigger Web Security Academy** is highly recommended as a free and up-to-date resource with practical exercises.

---

## ✅ Web Application Pentest Checklist

> _A structured security testing checklist covering all major attack surfaces._

---

### 🔐 Authentication

- Test for weak/default credentials
- Brute-force protection & account lockout
- Multi-factor authentication bypass
- Password reset flow vulnerabilities
- OAuth / SSO misconfigurations

---

### 🍪 Session Management

- Session token entropy and predictability
- Session fixation attacks
- Cookie attributes (`HttpOnly`, `Secure`, `SameSite`)
- Session timeout and invalidation on logout
- Concurrent session handling

---

### 🚪 Access Control & Authorization

- Horizontal & vertical privilege escalation
- Forced browsing / parameter tampering
- Role-based access control (RBAC) testing
- JWT authorization bypass

---

### 🤖 Anti-Automation & Attack Prevention

- CAPTCHA effectiveness
- Rate limiting on sensitive endpoints
- Bot detection bypass techniques
- Account enumeration via timing attacks

---

### 🔒 Cryptography & Secure Data Storage

- Weak cipher suites / deprecated TLS versions
- Hardcoded secrets or API keys in source
- Sensitive data in browser storage / logs
- Insecure cryptographic implementations

---

### 🛡️ Security Headers & HTTP Configurations

- `Content-Security-Policy` (CSP)
- `X-Frame-Options` / Clickjacking
- `Strict-Transport-Security` (HSTS)
- `X-Content-Type-Options`
- CORS misconfigurations

---

### 💉 Injection Prevention

|Vulnerability|Description|
|---|---|
|🟥 **HTML Injection**|Injecting HTML markup via user-controlled input|
|🟥 **XSS** (Cross-Site Scripting)|Reflected, Stored, DOM-based|
|🟥 **Code Injection**|Executing arbitrary code server-side|
|🟥 **OS Command Injection**|Running OS commands via app input|
|🟥 **File Inclusion**|LFI / RFI vulnerabilities|
|🟥 **SQL Injection**|Classic, Blind, Time-based, Out-of-band|
|🟥 **XXE Injection**|XML External Entity attacks|
|🟥 **Unvalidated Redirects & Forwards**|Open redirect abuse|
|🟥 **CSRF**|Cross-Site Request Forgery|
|🟥 **SSRF**|Server-Side Request Forgery|
|🟥 **HTTP Host Header Attacks**|Host header manipulation|
|🟥 **CRLF Injection**|Carriage Return Line Feed injection|

---

### 📝 Input Validation & Data Integrity

- Client-side vs server-side validation bypass
- Mass assignment / parameter pollution
- File upload validation (type, size, content)
- Serialization input tampering

---

### 🧠 Business Logic Security

- Multi-step process manipulation
- Price / quantity tampering
- Workflow bypass (skipping steps)
- Race conditions / TOCTOU issues

---

### 🖥️ Client-Side Security

- **Same Origin Policy (SOP)** enforcement
- **CORS (Cross-Origin Resource Sharing)** misconfigurations
- DOM-based vulnerabilities
- Sensitive data exposure in JavaScript source
- Clickjacking protections

---

### ☁️ Cloud & Infrastructure Security

- S3 bucket / blob storage exposure
- Metadata endpoint abuse (SSRF → cloud metadata)
- IAM misconfigurations
- Container / Kubernetes security

---

### 🪵 Error Handling & Logging

- Verbose error messages leaking stack traces
- Logging of sensitive user data
- Log injection attacks
- Missing security event logging

---

### ☠️ Malicious Code & Supply Chain Security

- Third-party library vulnerabilities
- Subresource integrity (SRI) checks
- Dependency confusion attacks
- CDN / script injection risks

---

### 🔍 Other Security Checks

- **Insecure Deserialization** — Object injection via serialized data
- **JSON Web Tokens (JWTs)** — `alg:none`, weak secret, kid injection
- **Insecure Direct Object References (IDOR)** — Accessing other users' resources
- **Session Management edge cases**
- **Logger++ Filter** testing for log manipulation

---

### 🌐 External Testing

- Subdomain enumeration & takeover
- DNS zone transfer
- Public-facing API endpoint discovery
- Certificate transparency log review
- Google dorking & OSINT

---

## 🗺️ Attack Surface Map

```
Web App
├── Authentication Layer
│   ├── Login / Logout
│   ├── Password Reset
│   └── OAuth / SSO
├── Session & State
│   ├── Cookies
│   └── Token Management
├── Input Vectors
│   ├── Forms / Parameters
│   ├── Headers / Cookies
│   ├── File Uploads
│   └── API Endpoints
├── Server-Side Logic
│   ├── Database Queries (SQLi)
│   ├── OS Commands (CMDi)
│   ├── File System (LFI/RFI)
│   └── External Requests (SSRF/XXE)
├── Client-Side
│   ├── DOM Manipulation (XSS)
│   ├── CORS / SOP
│   └── CSP / Headers
└── Infrastructure
    ├── Cloud Storage
    ├── Subdomains
    └── Third-party Dependencies
```

---

> 🎯 _These checks cover both foundational theory and practical exploitation techniques necessary for effective web application penetration testing._

---

_Reference: Web Application Penetration Testing — Methodology & Checklist_