## 🔹 Directory Traversal

**What it is:**  
A vulnerability that lets an attacker **move outside the intended folder** to access files.

**How it works:**  
Uses `../` to go back in directories.

**Example:**

`http://site.com/page.php?file=../../etc/passwd`

**What attacker gets:**  
📁 System files  
📁 Config files  
📁 Password files

✅ **Only reads files**  
❌ **Does NOT execute code**

---

## 🔹 LFI (Local File Inclusion)

**What it is:**  
A vulnerability where the application **includes and executes a local file**.

**How it works:**  
PHP includes a file using user input.

**Example:**

`include($_GET['page']);`

Attack:

`page=../../../../etc/passwd`

**What attacker gets:**  
📁 File contents  
⚠️ Can execute PHP code  
⚠️ Can lead to RCE (Remote Code Execution)

---

## 🔑 Main Difference (Easy Table)

|Feature|Directory Traversal|LFI|
|---|---|---|
|Purpose|Read files|Include & execute files|
|Code execution|❌ No|✅ Possible|
|Risk level|Medium|High|
|Common in|File download/view|PHP include()|

---

## 🧠 Easy way to remember

👉 **Traversal = Read files**  
👉 **LFI = Run files**

If you want, I can also explain:

- LFI → RCE chain
    
- Real-world payloads (OSCP style)
    
- Secure coding prevention
    

Just tell me 👍