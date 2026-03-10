# Web Security Concepts

## 1. Content Security Policy (CSP)

**Content Security Policy (CSP)** is a **web security standard** used to **prevent attacks such as Cross-Site Scripting (XSS), clickjacking, and data injection attacks**.

### In brief:
CSP allows website owners to **define which sources of content are trusted** and allowed to load on a web page (such as scripts, styles, images, fonts, etc.).

### Why CSP is important:
- Prevents execution of **malicious scripts**
- Reduces the impact of **XSS attacks**
- Improves overall **web application security**

### Example CSP header:
```http
Content-Security-Policy: default-src 'self';
This means the browser will only load content from the website’s own domain.

2. Client-Side XSS Protection
Client-side XSS protection refers to security mechanisms implemented in the browser or frontend code to detect and prevent Cross-Site Scripting (XSS) attacks before malicious scripts execute.

In brief:
It protects users by blocking or neutralizing harmful JavaScript code running in the browser.

Common client-side XSS protection methods:
Browser-based XSS filters

Input sanitization on the frontend

Output encoding of special characters

Using Content Security Policy (CSP)

Blocking inline JavaScript execution

Example:
Injected code:

html
Copy code
<script>alert('Hacked')</script>
Client-side protection prevents this script from running in the browser.

Importance:
Protects user data

Prevents session hijacking

Enhances browser-side security

Exam-ready summary:
CSP controls what content can run on a webpage.

Client-side XSS protection stops malicious scripts from executing in the browser.

markdown
Copy code
