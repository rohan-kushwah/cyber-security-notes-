# Scans

Burp Suite provides **three types of scans**, each with distinct behavior.

## Types of Scans In Burp Suite

Burp Suite offers several scanning modes tailored to different phases of web application security testing.

### 1. Crawl and Audit

- Combines both crawling and active auditing in a single operation.
- Burp will:
    - Automatically **discover** endpoints and links
    - Perform **vulnerability checks** (e.g., XSS, SQLi) on discovered content
- Useful for automated end-to-end testing.

### 2. Crawl Only

- Focuses purely on **discovering content** without active vulnerability analysis.
- Recursively follows all links and parameters.
- Good for **mapping attack surface** before manual or selective auditing.

### 3. API-Only Scan

- Designed specifically for **REST APIs, OpenAPI, GraphQL**, etc.
- Allows you to:
    - Import API definitions (e.g., Swagger, Postman collections)
    - Test endpoints without requiring UI interaction
- Ideal for **headless apps** or **mobile backends**.

## 4. Live Tasks

Live tasks operate in the background and continuously analyze traffic **as you browse**.

### Live Audit

- Actively audits requests/responses while you explore the application manually.
- Ideal for **real-time vulnerability discovery**.

### Live Passive Crawl

- Only passively observes and maps new endpoints.
- No active probing or payloads—**stealthier**.

## Event Log

- Shows real-time events and logs within Burp Suite.
- Includes:
    - Errors
    - Informational messages
    - Output from custom extensions
- **Tip:** Check here if you're having issues intercepting or decoding traffic.

---
---