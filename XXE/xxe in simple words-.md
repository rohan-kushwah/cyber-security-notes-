# 💀 XXE (XML External Entity) — Simple Notes  
  
## 🧾 What is XML?  
  
**XML = eXtensible Markup Language**  
  
It is a format used to store and transfer data (similar to JSON).  
  
### 📌 Example XML  
  
```xml  
<user>  
  <name>Rohan</name>  
  <age>20</age>  
</user>

👉 Used in APIs, configuration files, SOAP services, file imports, etc.

---

## 🧩 What is an Entity in XML?

An **entity = a shortcut or variable** inside XML.

Instead of repeating text, you define it once and reuse it.

### 📌 Example

<!ENTITY name "Rohan">

Usage:

<user>&name;</user>

👉 Output: **Rohan**

---

## 🔒 Internal Entity

An entity defined **inside the XML file itself**

### 📌 Example

<!DOCTYPE user [  
  <!ENTITY myname "Rohan">  
]>  
  
<user>&myname;</user>

✅ Data comes from inside XML  
✅ Usually safe

---

## 🌐 External Entity

An entity whose data comes from **outside the XML file**

It can access:

- 📂 Files on the server
    
- 🌍 URLs
    
- ⚙️ System resources
    

### 📌 Example

<!DOCTYPE user [  
  <!ENTITY file SYSTEM "file:///etc/passwd">  
]>  
  
<user>&file;</user>

👉 Server reads `/etc/passwd` and inserts its content 😱

---

## 💀 What is XXE?

**XXE = XML External Entity Attack**

A vulnerability where attackers exploit external entities to read sensitive data or attack the server.

### 🧠 Simple Meaning

> XXE = Malicious XML that forces the server to read secret data

---

## 🧨 Example XXE Payload

<?xml version="1.0"?>  
<!DOCTYPE data [  
  <!ENTITY xxe SYSTEM "file:///etc/passwd">  
]>  
<data>&xxe;</data>

If vulnerable:

👉 Server returns system file content

---

## 🔥 What Can XXE Do?

Attackers can:

- 📖 Read server files (`/etc/passwd`, config files)
    
- ☁️ Access cloud credentials
    
- 🌐 Perform SSRF (server-side requests)
    
- 🕵️ Access internal services
    
- 💻 Sometimes lead to Remote Code Execution
    

---

## 🔍 Where to Find XXE Vulnerabilities?

Look for places where XML is processed 👇

### 1️⃣ File Upload Features

If application allows XML uploads:

- Data import
    
- Configuration upload
    
- Document import
    

---

### 2️⃣ APIs Accepting XML

Check request headers:

Content-Type: application/xml

or

Content-Type: text/xml

---

### 3️⃣ SOAP Web Services

SOAP uses XML heavily 🧼

---

### 4️⃣ SSO / Authentication Systems

Some identity systems use XML (e.g., SAML)

---

### 5️⃣ Legacy / Old Applications

Older Java, PHP, and .NET apps are often vulnerable

---

## 🕵️ How Pentesters Detect XXE

Send a test payload:

<!DOCTYPE test [  
  <!ENTITY xxe SYSTEM "file:///etc/passwd">  
]>  
<test>&xxe;</test>

If response shows system file content → XXE confirmed ✅

---

## 🧯 Why XXE Happens?

Because XML parser is configured insecurely:

❌ External entities are enabled  
✅ Secure apps disable them

---

## 🧠 Quick Memory Trick

- 🔒 Internal Entity → Inside XML
    
- 🌐 External Entity → Outside data source
    
- 💀 XXE → Abuse of External Entity
    

---

## 💻 Real-World Scenario

Website feature:

📂 “Import users from XML file”

Attacker uploads malicious XML → server reads sensitive files

---