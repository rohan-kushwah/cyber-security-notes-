# 🧠 How to Find Code Injection Vulnerabilities

_(Educational & Ethical Purpose Only)_

---

## 🔍 1. Understand What Code Injection Is

**Code Injection happens when:**

- User input is executed as **code**
    
- The application trusts user input
    
- Dangerous functions are used
    

### Common Dangerous Functions

|Language|Risky Functions|
|---|---|
|PHP|`eval()`, `system()`, `exec()`, `shell_exec()`, `assert()`|
|Python|`eval()`, `exec()`|
|Java|`Runtime.exec()`|
|Node.js|`eval()`, `child_process.exec()`|

---

## 🧪 2. Where to Look (Very Important)

### 🔹 A. Input Fields

Look for:

- Forms (login, search, contact)
    
- URL parameters
    
- POST requests
    
- File upload names
    

Example:

`index.php?cmd=`

---

### 🔹 B. Backend Code (If You Have Access)

Search for:

`eval($_GET['x']); system($_POST['cmd']); exec($input);`

⚠️ These are **red flags**.

---

## 🔎 3. Signs of Code Injection Vulnerability

### 🚩 Red Flags in Code

- User input passed directly to a function
    
- No validation
    
- Dynamic function calls
    
- Base64 decoding user input
    
- String concatenation
    

Example:

`$cmd = $_POST['cmd']; system($cmd);`

---

## 🧠 4. How to Test Safely (Educational Way)

### ✔ Step 1: Observe Behavior

- Does the page react differently to inputs?
    
- Do errors appear?
    
- Does output change?
    

### ✔ Step 2: Use Benign Input

Use harmless test input like:

`test 123 hello`

📌 You are checking **behavior**, not exploitation.

---

## 🧪 5. Code Review Method (Best for Learning)

When reviewing PHP code, ask:

✅ Is user input used directly?  
✅ Is eval() used?  
✅ Is system() used?  
✅ Is there a whitelist?  
❌ Is there input validation?

If **NO validation + execution function = vulnerability**

---

## 🧩 6. Common Code Injection Patterns

### ❌ Vulnerable

`eval($_POST['data']);`

### ✅ Secure

`if ($_POST['action'] === 'info') {     phpinfo(); }`

---

## 🛡️ 7. How Professionals Detect It

### Manual:

- Read source code
    
- Test parameters
    
- Observe error messages
    

### Tools (Educational Use):

- Burp Suite
    
- OWASP ZAP
    
- Static code analyzers
    

⚠️ Tools **assist**, they don’t replace thinking.