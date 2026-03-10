# **DOM-Based Cross-Site Scripting**

## **Understanding DOM-Based Cross-Site Scripting (DOM XSS)**

### **What are Sinks in DOM XSS?**

**Sinks** are specific JavaScript functions, properties, or methods that handle data in a way that causes execution or rendering of attacker-controlled input.  
When untrusted data flows into these sinks without proper sanitization or validation, it may result in **malicious script execution** in the victim’s browser.

---
## DEFIANTIO=
SINKS= CREATE DYNAMIC ELEMENTS
AB SINKS ME JO HUM DATA DALENGE WO SANITIZE NA HUA TOH SINKS KA JO ELEMENT RAHEGA WO DOM XSS SE VULNERABLE RAHEGA 

# oR
sinks javascript ka function hota hai jo dyanmicv element creatre karta hai us elemet ka work bhi pehele se define rehta hai but hum jo isme input dalte hai sink me agr wo sanitie nhi hua toh wo sink ka dyanmic element vulnerable hota hai


![[Pasted image 20251119183541.png]]

# **Common DOM XSS Sinks**

|**Sink Type**|**Sinks / Functions & Properties**|**Description**|
|---|---|---|
|**DOM Manipulation**|`document.write()`, `document.writeln()`, `element.innerHTML`, `element.outerHTML`, `element.insertAdjacentHTML()`, `element.onevent` (e.g., `onclick`)|Write or insert raw HTML or set event handlers dynamically|
|**Location & Navigation**|`location`, `location.href`, `location.host`, `location.hostname`, `location.pathname`, `location.search`, `location.protocol`, `location.assign()`, `location.replace()`|Control page URL, navigation, or inject code via URL manipulation|
|**Window & DOM APIs**|`open()`, `document.domain`|Open new windows or manipulate domain for same-origin policy|
|**AJAX and XMLHttpRequest**|`XMLHttpRequest.open()`, `XMLHttpRequest.send()`, `jQuery.ajax()`, `$.ajax()`|Send/load data dynamically, possibly injecting or loading unsafe scripts|
|**JavaScript Code Execution**|`eval()`, `Function()`, `setTimeout()`, `setInterval()`, `setImmediate()`, `execScript()`, `execCommand()`, `range.createContextualFragment()`|Execute strings as code or manipulate DOM fragments from HTML strings|
|**jQuery Sinks**|`add()`, `after()`, `attr()`, `append()`, `animate()`, `insertAfter()`, `insertBefore()`, `before()`, `html()`|DOM manipulation and attribute setting functions that can inject code|

---

# **Explanation**

- These sinks act as execution points for potentially malicious data.
    
- An attacker uses a **controlled source** (such as `window.location` or `document.referrer`) to inject data into these sinks.
    
- If the sink interprets the data as code or raw HTML without sanitization or encoding, malicious scripts run in the **context of the victim's browser**, enabling XSS attacks.
    
- For example, passing attacker data directly into `element.innerHTML` or into `eval()` will run scripts, leading to **DOM XSS**.


# What is DOM XSS?

DOM-based Cross-Site Scripting (DOM XSS) is a client-side vulnerability where malicious scripts run because JavaScript insecurely manipulates the Document Object Model (DOM). Unlike traditional XSS, DOM XSS does not rely on server responses but on unsafe client-side code that processes untrusted data.

---

# How DOM XSS Works

- When a webpage loads, the browser creates the DOM (a tree-like structure of page elements).
- JavaScript interacts with and changes the DOM dynamically.
- If JavaScript reads attacker-controlled data (URL parameters, hashes, referrer, cookies) and inserts it into the DOM insecurely, malicious scripts can execute.
- This usually happens when untrusted data flows into dangerous JavaScript functions called *sinks*.

---

# Common Sources of Untrusted Data

- window.location (href, search, hash, pathname)
- document.referrer
- document.URL
- Cookie values
- window.name

---

# Dangerous Sinks in JavaScript Leading to DOM XSS

## DOM Manipulation Sinks

| Sink | Description |
|------|-------------|
| document.write(), document.writeln() | Writes HTML directly into the page |
| element.innerHTML, element.outerHTML | Inserts HTML content into an element |
| element.insertAdjacentHTML() | Inserts HTML next to an element |
| element.onevent (e.g., onclick) | Dynamically assigns event handlers |
| document.domain | Changes domain for same-origin policy |
| location.*, assign(), replace() | Manipulates or redirects URLs |
| open() | Opens new browser tabs/windows |
| XMLHttpRequest.open(), XMLHttpRequest.send() | Handles AJAX requests |
| $.ajax(), jQuery.ajax() | jQuery AJAX methods |

---

# Execution-Related Sinks

| Function | Description |
|----------|-------------|
| eval() | Executes a string as JavaScript code |
| Function() | Creates a function from a string |

# code -
<!DOCTYPE html>
<html>
<head>
    <title>DOM XSS Lab Example</title>
</head>
<body>
    <h2>Welcome to the DOM XSS Lab</h2>
    <p id="output"></p>

    <script>
        // Unsafe: Extract 'name' parameter from the URL query string
        // Example: http://localhost/lab/dom-xss.php?name=John
        // BAD SINK: innerHTML

        const urlParams = new URLSearchParams(window.location.search);
        const name = urlParams.get('name') || '';

        document.getElementById('output').innerHTML = "Hello, " + name + "!"; // Vulnerable
    </script>

    <p>Try visiting: <code>?name=John</code> or 
       <code>?name=&lt;img src=x onerror=alert(1)&gt;</code>
    </p>
</body>
</html>
