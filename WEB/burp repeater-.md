# What is Burp **Repeater**?

**Repeater** is a Burp Suite tool for **manual, iterative testing** of individual HTTP(S) requests.  
It lets you take a captured request, modify it, resend it repeatedly, and inspect responses — making it ideal for verifying payloads, testing logic, checking input validation, and reproducing bugs.

Repeater is _not_ an automated fuzzer — it’s a manual workbench for focused, repeatable testing.

---

# Key features

- Edit any part of the request (start-line, headers, body).
    
- Send the edited request and view the full response (status, headers, body, timing, size).
    
- Multiple Repeater tabs/workspaces to test different variants in parallel.
    
- Preserves request history per tab so you can compare attempts.
    
- Request and Response views: **Raw**, **Headers**, **Params**, **Hex**, **Rendered** (when applicable).
    
- Copy / export requests and responses for reporting or reproduction.
    

---

# When to use Repeater

- Validate and refine an exploit or payload (SQLi, XSS, logic flaws).
    
- Reproduce bugs seen during scanning or proxying.
    
- Test authentication/session handling by adjusting cookies/headers.
    
- Check how small changes to inputs affect server responses.
    
- Manually confirm server-side validation or error messages.
    

---

# How to use — step-by-step

1. **Capture or obtain a request**
    
    - From **Proxy → HTTP history**, **Target → Site map**, or another tool, locate the request you want to test.
        
2. **Send to Repeater**
    
    - Right-click the request → **Send to Repeater**.
        
    - Or select the request and use the toolbar/context-menu option **Send to Repeater**.
        
3. **Open the Repeater tab**
    
    - Switch to the **Repeater** tab. The request opens in a new Repeater tab (named like `Tab 1`, `Tab 2`, or the request URL).
        
4. **Edit the request**
    
    - Modify the request start-line, headers, parameters, or body in the request pane. You can:
        
        - Edit path, query parameters, and request method.
            
        - Add/modify headers (e.g., `Authorization`, `Cookie`, `X-Forwarded-For`).
            
        - Change POST/JSON/XML form bodies.
            
5. **Send the request**
    
    - Click the **Go** (or **Send**) button. Repeater sends the exact request and displays the response.
        
6. **Inspect the response**
    
    - Check status code, response time, response length, and body. Use:
        
        - **Raw** — exact HTTP response (status-line, headers, body).
            
        - **Headers** — parsed header view.
            
        - **Params** — parsed parameters (where applicable).
            
        - **Hex** — binary / charset debugging.
            
        - **Rendered** — browser-like rendering for HTML responses.
            
7. **Iterate**
    
    - Make changes and click **Go** again. Repeater keeps a history for that tab so you can compare attempts.
        
8. **Save / export**
    
    - Copy the raw request/response or export for reporting (right-click options or use the built-in copy/save actions).
        

---

# Practical tips & best practices

- Use multiple Repeater tabs to test different vectors or payload stages concurrently.
    
- Keep an eye on **response time** and **status codes** — these often indicate side effects or server behavior changes.
    
- When testing authenticated endpoints, ensure cookies/Authorization headers are current. Use **Request → Use cookie jar** if available and needed.
    
- Use **Raw** to ensure exact bytes are sent (useful for binary content or odd encodings).
    
- For repeatable sequences (login → action), consider using **Sequencer/Intruder** or macros — Repeater is manual and single-request oriented.
    
- Redact sensitive data before saving or sharing Repeater history (tokens, passwords, PII).
    
- Combine with **Logger/Proxy history** to correlate Repeater requests with other traffic.
    

---

# Example — simple workflow

### Original captured GET request

`GET /api/user?id=123 HTTP/1.1 Host: vulnerable.example Accept: application/json Cookie: session=abcd1234`

### In Repeater — modify `id` to test for IDOR or enumeration

`GET /api/user?id=124 HTTP/1.1 Host: vulnerable.example Accept: application/json Cookie: session=abcd1234`

- Click **Go** → inspect response.
    
- If `/api/user?id=124` returns another user's data, you have an IDOR.
    

### Example POST modification

`POST /api/update HTTP/1.1 Host: vulnerable.example Content-Type: application/json Content-Length: 72 Cookie: session=abcd1234  {"id":123,"email":"attacker@example.com","isAdmin":true}`

- Send → watch response and status codes.
    
- Iterate by toggling `isAdmin` or other fields to test server-side validation.
    

---

# Common workflows vs other Burp tools

- **Proxy** — capture and forward traffic; use when browsing to collect requests.
    
- **Repeater** — manual editing + resend single requests; fine-grained verification.
    
- **Intruder** — automated/fuzzing and parameterized payloads at scale.
    
- **Scanner** — automated vulnerability discovery.  
    Use Repeater when you want full manual control over a single request/response cycle.
    

---

# Security & privacy reminder

Requests in Repeater may contain authentication tokens, cookies, or PII. Treat Repeater history and exported data as sensitive. Follow your organization’s handling and disclosure rules.

---

# Quick cheatsheet (commands & actions)

- **Send to Repeater** — right-click a request → _Send to Repeater_.
    
- **Edit** — directly edit the request pane.
    
- **Go/Send** — send the modified request.
    
- **View** — use Raw / Headers / Params / Hex / Rendered.
    
- **Copy/Export** — right-click or use menu to copy the raw request/response.