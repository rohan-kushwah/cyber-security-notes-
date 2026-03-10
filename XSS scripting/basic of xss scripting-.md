# Cross-Site Scripting (XSS) Explained Simply

Cross-Site Scripting (XSS) is a web security vulnerability where an attacker injects malicious JavaScript into a website, and that script runs in the browser of another user.

Because the browser *trusts the website*, the injected script is also trusted — which makes it dangerous.

---

## 🎯 What XSS Can Do
- Steal cookies or session information  
- Redirect users to malicious pages  
- Display fake login forms  
- Change webpage content  
- Perform actions as the victim  

---

## 📝 Simple Example

An attacker submits this in a comment box:

```html
<script>
alert("I hacked you!");
</script>
If the site does not sanitize inputs, the script runs for any user who views the page.

```

           ┌──────────────────────────┐
           │        Attacker          │
           └───────────┬──────────────┘
                       │ (1) Injects malicious script
                       ▼
        ┌────────────────────────────────────┐
        │      Vulnerable Website            │
        │  - Stores or reflects input        │
        │  - Fails to sanitize JavaScript    │
        └───────────┬────────────────────────┘
                    │ (2) Sends page with script
                    ▼
           ┌──────────────────────────┐
           │       Victim User        │
           │  Browser runs the script │
           └──────────────────────────┘

## ✔️ Types of XSS

1. **Stored XSS** – malicious script is saved in the site’s database
    
2. **Reflected XSS** – script comes from a crafted URL
    
3. **DOM-based XSS** – insecure JavaScript in the page executes harmful input
    

---

## 🛡️ How to Prevent XSS

- Escape user input
    
- Sanitize HTML/JavaScript
    
- Validate all user inputs
    
- Use Content Security Policy (CSP)
    
- Do not trust user-submitted content
    

---