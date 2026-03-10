 BURP SUITE PROFESSIONAL – FULL DASHBOARD & TOOL GUIDE

  

---

  

## 0️⃣ BEFORE YOU BEGIN (Essential Setup)

  

> 🔧 **Concept:** Burp Suite acts as an **intercepting proxy**, sitting between your browser and the server. To work properly, the browser must send all traffic through Burp.

  

### ✅ Initial Setup

- Set browser proxy to: `127.0.0.1:8080`

- Install Burp's **CA Certificate** into browser (to avoid HTTPS warnings)

- Visit `http://burp` to download certificate

- Import into browser’s certificate manager as a trusted CA    

  

---

  

## 1️⃣ DASHBOARD – 📊 Real-Time Control Center

  

> 🔎 **Concept:** Dashboard gives an overview of all **active scans, tasks, and found vulnerabilities.**  

> This is your mission control – not where you do attacks, but where you **monitor** them.

  

### 🧩 Tasks Panel

  

- Shows background activities (scans, crawl, audit)    

- Each task has:

    - Status: (Running, Finished)

    - Progress bar

    - Hostname in scope

- ✅ You can pause/resume/delete from here

  

### 🛑 Event Log

  

- Logs major actions (e.g., scan started, proxy listening)

- Useful for **debugging errors** (e.g., scan not working)

  

### ⚠️ Issue Activity

  

- Shows issues found in real-time, with severity (High, Medium, Low)

    - Clicking each opens the **Advisory**

  

### 📚 Advisory

  

- Detailed explanation of selected issue

- Contains:

    - Vulnerability description

    - Severity

    - Affected URLs

    - Remediation advice

    - CWE and references

  

---

  

## 2️⃣ TARGET – 🎯 Scoping + Site Map

  

> 🔎 **Concept:**  

> Burp maps out the application’s structure as you browse it.  

> You can define what parts of a site are “in-scope” (allowed for testing).

  

### 🌳 Site Map

  

- Shows all URLs visited or discovered (via crawl or proxy)

- Structure:

    - Domain > Folder > Endpoints

- Color codes:

    - **Grey** = not scanned

    - **Green/Yellow/Red** = scanned and issue found

- Right-click any item → Send to Repeater, Intruder, Scanner

  

### 📌 Scope

  

- Lets you define **what is in-scope** for scanning or interception

- Add domains, paths, or regex filters

- Scans, crawls, and tools like Intruder will **ignore out-of-scope targets**

  

> ✅ Always configure **Scope first** before scanning to avoid hitting third-party content accidentally.

  

---

  

## 3️⃣ PROXY – 🔄 Live Traffic Interception

  

> 🔎 **Concept:**  

> The proxy captures all HTTP(S) requests and responses.  

> You can modify, block, forward, or replay them in real time.

  

### 🔥 Intercept (Tab)

  

- Shows live request when Intercept is ON

- Buttons:

    - **Forward** – send to server

    - **Drop** – block

    - **Intercept is on/off** – toggle live interception

  

> ✅ Use this to **modify login forms**, change headers, test CSRF tokens manually.

  

### 📜 HTTP History

  

- Shows full list of past requests (even when Intercept was OFF)

- Filter by:

    - Method (GET, POST)

    - MIME type

    - Status code

  

> ✅ Great for finding sensitive URLs, headers, or cookies

  

### 💬 WebSocket History

  

- View messages from WebSocket connections

- Supports JSON, raw text, binary decoding

  

### ⚙️ Options

  

- Set proxy port, rules for request interception, match & replace headers, etc.

  

---

  

## 4️⃣ REPEATER – 🔁 Manual Request Editor

  

> 🔎 **Concept:**  

> Repeater allows **manual testing** of a single request repeatedly, with modified parameters.  

> This is essential for testing **authentication, authorization, tokens, injections.**

  

### 💻 Request/Response Editor

  

- Modify method, headers, body

- Send → see new response immediately

- Auto color codes response sections

  

### 🗂 Tabs

  

- Each request opens in its own tab

- Tabs can be renamed, saved, or exported

  

> ✅ Use this to test payloads, token expirations, bypass attempts manually.

  

---

  

## 5️⃣ INTRUDER – 🔫 Automated Payload Testing

  

> 🔎 **Concepts:**

>

> - **Fuzzing**: Sending variations of input to discover unexpected behavior

>    

> - **Brute Force**: Trying many values (e.g., passwords)  

>     Intruder automates this process.

>    

  

### 🎯 Positions

  

- Define placeholders for payloads using `§` markers

- Types:

    - Sniper (one-at-a-time)

    - Battering Ram (same payload everywhere)

    - Pitchfork (parallel inputs)

    - Cluster Bomb (combinations)

  

### 📦 Payloads

  

- Choose wordlists or custom values

- Add encoding (URL, Base64)

- Enable insertion of payload markers

  

### ⏱ Resource Pool

  

- Control speed, threads, throttle

- Useful when attacking rate-limited targets

  

> ✅ Use this for testing:

>

> - Login brute force

>    

> - IDOR (ID fuzzing)

>    

> - XSS injection with payload sets

>    

  

---

  

## 6️⃣ SCANNER – 🚨 Auto Vulnerability Testing (Pro Only)

  

> 🔎 **Concepts:**

>

> - **Crawling**: Discovering links and forms

>    

> - **Scanning**: Sending payloads to detect vulnerabilities

>    

  

### 🚀 Scan Launcher

  

- Launch scan on:

    - A request

    - A folder

    - An entire domain

- Choose scan type: Crawl + Audit, Audit only, etc.

  

> ✅ Right-click any URL in Target tab → "Scan"

  

### ⚠️ Issues

  

- All discovered vulnerabilities listed

- Each entry has:

    - Severity

    - URL

    - Request/Response

    - Advisory link

  

> ✅ Scanner automatically tests for:

>

> - SQL Injection

>    

> - Cross-Site Scripting (XSS)

>    

> - CSRF

>    

> - Information Disclosure

>    

> - SSRF, etc.

>    

  

---

  

## 7️⃣ DECODER – 🔐 Encode/Decode Data

  

> 🔎 **Concept:**  

> Often data in requests (e.g., tokens, cookies) is **encoded** or **obfuscated**. Decoder helps translate it.

  

### 🔁 Operations

  

- Encode: Base64, URL, HTML, Hex, Unicode, JWT, etc.

- Decode: Same

- Hash: SHA1, SHA256, MD5

  

### 🧠 Smart Decode

  

- Auto attempts correct decoding on paste

  

> ✅ Use for:

>

> - Decoding cookies, tokens

>    

> - Preparing encoded payloads

>    

> - Reversing obfuscation

>    

  

---

  

## 8️⃣ COMPARER – 🔍 Diff Requests or Responses

  

> 🔎 **Concept:**  

> Sometimes you need to see **exact differences** between two requests (e.g., before and after login)

  

### 📄 Word/Byte Comparison

  

- Paste two requests/responses

- Shows diffs at word or byte level

  

> ✅ Use for:

>

> - Analyzing what changes when a user logs in

>    

> - Comparing error vs success responses

>    

> - Token analysis

>    

  

---

  

## 9️⃣ LOGGER – 📖 Raw Traffic Viewer

  

> 🔎 **Concept:**  

> Unlike HTTP History, Logger shows **all traffic from all tools** – not just proxy

  

### 📅 Logger

  

- Chronological view of all requests from Proxy, Intruder, Scanner, etc.

- Full filtering by:

    - Tool

    - Method

    - URL

    - Status

  

> ✅ Ideal for:

>

> - Tracking test activity

>    

> - Debugging request sequences

>    

> - Filtering payload traffic only

>    

  

---

  

## 🔟 EXTENDER – 🔌 Extend Burp with Add-ons

  

> 🔎 **Concept:**  

> Burp supports Java and Python-based extensions that add more tools and views.

  

### 🛍 BApp Store

  

- One-click install from curated list

- Useful add-ons:

    - Auth Analyzer

    - Logger++

    - Hackvertor

    - Turbo Intruder

  

### 🔄 Extensions

  

- Manage installed plugins

- Load local JAR/Python extensions

  

> ✅ Use Extender when:

>

> - You need advanced decoding

>    

> - Want to integrate custom tools

>    

> - Working with token automation

>    

  

---

  

## ✅ RECOMMENDED BEGINNER PATH

  

1. 🔄 **Proxy** → Capture login form request

2. 🔁 **Repeater** → Replay and test login

3. 🔫 **Intruder** → Bruteforce with payloads

4. 🎯 **Target** → Define scope and map endpoints

5. 🚨 **Scanner** → Scan site automatically

6. 🔍 **Comparer** → Compare before/after login

7. 🔐 **Decoder** → Decode any tokens or cookies

8. 📖 **Logger** → Trace all activity for auditing

9. 🔌 **Extender** → Add automation with BApps

10. 📊 **Dashboard** → Monitor all issues in real time

  

---