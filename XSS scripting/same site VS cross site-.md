# Same-Site, Cross-Site, and XSS

---
## Same-Site

- **Definition**:  
    Two resources belong to the **same registrable domain (eTLD+1)**. Subdomains count as same-site.

	**eTLD:**
	- The term **eTLD+1** stands for *effective Top-Level Domain plus one label* and represents the **registrable domain** that an organization can actually own. The **eTLD** is determined using the Public Suffix List (for example, `.com`, `.org`, `.co.uk`, `.gov.in`), and then we add one label to the left of it to get the eTLD+1 (for example, `bank.com`, `amazon.co.uk`, `rishabhsoni.in`). All subdomains under the same eTLD+1, such as `app.example.com` and `blog.example.com`, are treated as **same-site**, because they belong to the same registrable domain, whereas domains with different eTLD+1 values, such as `example.com` and `example.org`, are treated as **cross-site**. This distinction is fundamental in browser security models like the Same Origin Policy, cookie handling, and SameSite rules.
	
- **Examples**:
    - `app.example.com` and `blog.example.com` → same-site (both under `example.com`).
    - `example.com` and `example.org` → different site.

- **Relevance**:    
    - Browser cookie rules (`SameSite=Strict`, `Lax`, `None`) depend on this.
    - Helps prevent **Cross-Site Request Forgery (CSRF)**.

---
## Cross-Site

- **Definition**:  
    Two resources belong to **different registrable domains**.

- **Examples**:
    - `example.com` loading an image/script from `evil.com` → cross-site request.

- **Risks**:
    - **CSRF** → attacker site forces a victim’s browser to send authenticated requests.
    - **Data leaks** → if server does not properly check `Origin`/`Referer`.
    - Potential abuse of cookies if they are not marked `SameSite`.

---
## Cross-Site Scripting (XSS)

- **Definition**:  
    Injection of malicious script into a trusted web application such that it runs in the victim’s browser **with the privileges of the vulnerable site’s origin**.
    
- **Why “Cross-Site”?**  
    Historically: attacker’s input from one “site” (untrusted) executes in another site’s trusted context.
    
- **Effects**:
    - Read cookies/session tokens.
    - Impersonate user actions (account takeover).        
    - Modify the DOM (defacements, phishing).

- **Scope**:
    - Limited to the vulnerable site’s **origin** by Same Origin Policy (SOP).
    - Does **not** directly give control of other sites or the whole browser (that would require a browser exploit).    

---
## Power Contexts

- **User power**: scripts run because the user chooses them (console, extensions).
- **Website power**: scripts that the browser trusts as part of the page’s origin.
- **XSS power**: attacker’s script injected into a trusted origin → executes as if it were written by the legitimate site.

---
## XSS is **not**

- Running your own JS in your console → that’s user power, not XSS.    
- Loading attacker.com inside an iframe sandbox → that’s just another origin, not XSS.

---
## Summary Table

| Term       | Meaning                                                   | Example                                          | Risk                                              |
| ---------- | --------------------------------------------------------- | ------------------------------------------------ | ------------------------------------------------- |
| Same-Site  | Resources share same eTLD+1                               | `blog.example.com` ↔ `app.example.com`           | Safe for cookies & SOP                            |
| Cross-Site | Resources belong to different eTLD+1                      | `example.com` ↔ `evil.com`                       | CSRF, data leaks                                  |
| XSS        | Attacker injects script that runs in victim site’s origin | `<script>alert(1)</script>` inside `example.com` | Cookies theft, account takeover, DOM manipulation |

---

✅ **One-line takeaway for memory**:  
**XSS = attacker’s code runs in the victim’s browser with the authority of the victim site’s origin, bypassing SOP and hijacking user trust.**

---
---