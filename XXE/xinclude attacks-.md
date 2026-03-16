## 🧩 XInclude — How It Works (Technical Terms) ⚙️

### 📌 Definition

**XInclude is an XML standard that allows one XML document to include data from another resource during parsing.**

It is processed by the XML parser, not by the application logic.

---

## ⚙️ How XInclude Works Internally

When an XML parser supports XInclude:

1. It scans the XML for XInclude tags
    
2. It fetches the referenced resource (file/URL)
    
3. It inserts that content into the XML document
    
4. Parsing continues with the combined data
    

---

## 🧪 XInclude Syntax (Technical)

<xi:include href="URI" parse="text|xml"/>

### Parameters:

- **xi:include** → XInclude element
    
- **href** → Path or URL of resource
    
- **parse="xml"** → Treat content as XML (default)
    
- **parse="text"** → Treat content as plain text
    

Namespace must be defined:

xmlns:xi="http://www.w3.org/2001/XInclude"

---

## 💀 How XInclude Attack Works Technically

If user input is embedded into XML and processed:

### Malicious Payload

<root xmlns:xi="http://www.w3.org/2001/XInclude">  
  <xi:include href="file:///etc/passwd" parse="text"/>  
</root>

---

## 🔥 What Happens in the Parser

1️⃣ Parser encounters `<xi:include>`  
2️⃣ Resolves URI: `file:///etc/passwd`  
3️⃣ Opens local file on server filesystem  
4️⃣ Reads file contents  
5️⃣ Inserts contents into XML tree  
6️⃣ Application processes or returns result

➡️ This results in **Local File Disclosure**

---

## 🌐 Supported Resource Types

XInclude can load from:

- 📂 Local files (`file://`)
    
- 🌍 Remote URLs (`http://`)
    
- 📄 Other XML documents
    
- 📁 Relative paths