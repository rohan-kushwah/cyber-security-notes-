# 🛡️ XML eXternal Entity (XXE) Injection

---

> **XML** _(eXtensible Markup Language)_ is a markup language used to **store and transport data** in a structured, organized, and machine-readable format. Unlike HTML, XML defines the **structure and meaning** of data rather than its visual layout.

---

## 🔍 What is XXE Injection?

**XML External Entity (XXE) Injection** is a security vulnerability in XML parsers that occurs when they process **external entities** — data pulled from outside sources. Attackers inject malicious external entities into XML documents, causing the parser to execute arbitrary code or access unauthorized files.

---

## ⚠️ Attack Types

|Attack|Description|
|---|---|
|📂 **Local File Inclusion**|Read sensitive files from the server|
|🌐 **Server-Side Request Forgery**|Make requests from the server to internal systems|
|🕵️ **Blind Attacks (Data Exfiltration)**|Extract data without direct response|
|💥 **Denial of Service**|Crash or overload the XML parser|
|💻 **Remote Command Execution**|Execute arbitrary commands on the server|

---

## 🔤 Character Entities

|Entity|Character|
|---|---|
|`&quot;`|`"`|
|`&amp;`|`&&`|
|`&apos;`|`''`|
|`&lt;`|`<<`|

---

## 🏗️ Internal vs External Entities

**Entities** in XML represent character sequences or data blocks that can be referenced multiple times within a document.

### 🔒 Internal Entities

Defined **within** the XML document itself and referenced only inside it.

```xml
<!DOCTYPE foo [
  <!ENTITY name "value">
]>
```

### 🌍 External Entities

Defined **outside** the XML document. They can reference **external files or URLs**, making them the core vector for XXE attacks.

```xml
<!DOCTYPE foo [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<root>&xxe;</root>
```

---

## 🛡️ Prevention

- ✅ Disable external entity processing in XML parsers
- ✅ Use less complex data formats (JSON) where possible
- ✅ Validate and sanitize all XML input
- ✅ Update/patch XML processing libraries regularly
- ✅ Use allowlists to restrict XML content

---

_Web Application Penetration Testing — XXE Injection Module_