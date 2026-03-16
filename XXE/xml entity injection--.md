# 🛡️ XML eXternal Entity (XXE) Injection

> _A deep dive into one of the most critical web application vulnerabilities_

---

## 📖 Overview

**XML (eXtensible Markup Language)** is a markup language used for storing and transporting data in a structured, machine-readable format. Unlike HTML — which defines the _visual layout_ of documents — XML defines the **data structure and semantics** using tags that describe and organize data for easy interpretation and exchange between systems.

**XML External Entity (XXE) Injection** is a security vulnerability that occurs when an XML parser improperly processes external entities referenced in XML documents. An external entity is a type of entity defined _outside_ the XML document, typically pointing to external files or URLs.

> ⚠️ **Attackers exploit XXE** by injecting malicious external entities that an XML parser processes, which can lead to:
> 
> - 🔓 Unauthorized reading of local or remote files
> - 🌐 Server-Side Request Forgery (SSRF)
> - 💥 Denial of Service (DoS)
> - 💻 Remote Code Execution

---

## 🔖 Entities in XML

|Type|Description|Syntax|
|---|---|---|
|**Internal Entity**|Defined and referenced within the same XML document|`<!ENTITY name "value">`|
|**External Entity**|Defined outside the XML document, referenced within using a system identifier|`<!ENTITY name SYSTEM "URI">`|

---

## 📄 Document Type Definition (DTD)

> DTD is a **schema language** that defines the structure and the allowable content of XML documents.

It supports defining entities that can be reused in XML documents. These entities can be:

- 🏠 **Internal** — defined inside the document
- 🌍 **External** — referencing external resources

> 🔴 **Attacker Note:** Attackers use DTDs to inject malicious external entities during XXE attacks.

---

## 💣 Example of an XXE Attack Payload

```xml
<!DOCTYPE doc [
    <!ENTITY phonebook SYSTEM "file:///path/to/phonebook.xml">
]>
<doc>
  <header>
      &phonebook;
  </header>
</doc>
```

> 📌 _This example tries to include an external file from the server's file system into the XML response, potentially disclosing sensitive information._



```
```<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck>
<productId>&xxe;</productId>
<storeId>1</storeId>
</stockCheck>
---

## 🗂️ Types of XXE Attacks


```````
┌─────────────────────────────────────────┐
│           XXE Attack Vectors            │
├────────────────┬────────────────────────┤
│  Classic XXE   │  File system access    │
│  Blind XXE     │  Out-of-band data ex   │
│  Error-based   │  Error message leaks   │
│  SSRF via XXE  │  Internal port scans   │
└────────────────┴────────────────────────┘
```

---

## 🔐 Prevention & Mitigation

- ✅ **Disable external entity processing** in your XML parser
- ✅ **Use less complex formats** like JSON where possible
- ✅ **Patch and update** XML processing libraries
- ✅ **Input validation** — whitelist acceptable content
- ✅ **WAF rules** to detect and block XXE patterns

---

## 📚 References

- [OWASP XXE Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html)
- [PortSwigger XXE Guide](https://portswigger.net/web-security/xxe)
- [CWE-611: Improper Restriction of XML External Entity Reference](https://cwe.mitre.org/data/definitions/611.html)

---

<div align="center">

_🔒 Security is not a product, but a process._

</div>