# HTML Injection - IFrame Vulnerable

HTML injection in an iframe context means user input is directly inserted into the `src` attribute of an `<iframe>`, allowing attackers to control what is loaded and rendered within the frame. If proper validation or sanitization is missing, this can lead to serious security risks such as clickjacking, phishing, and content spoofing.

## Vulnerable Code Review: Html Injection via IFrame src

```php
html-injection-iframe.php
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Html Injection IFrame</title>
</head>
<body>

<h1>Html Injection IFrame</h1>

<?php
$url = $_GET['url'];
?>

<iframe src="<?php echo $url; ?>" width="800px" height="800px"></iframe>

</body>
</html>
```

* It is **vulnerable to HTML Injection** (or web injection) because the `url` parameter is directly injected into the iframe `src` attribute without any validation or sanitization.

## Explanation of Vulnerability

• The value of `$_GET['url']` is inserted **directly** into the `src` attribute of the `<iframe>`.
• An attacker can supply a malicious URL or specially crafted string to:
  • Load an untrusted external page inside the iframe (phishing, clickjacking).
  • Exploit iframe context or browser vulnerabilities.
  • Break out of the attribute or tag context by injecting quotes or tags, leading to further HTML or JavaScript injection.

## Example Malicious URL:

```
http://example.com/html-injection-iframe.php?url=https://evil.com/
```

Or inject HTML by closing attributes and adding new ones:

```
http://example.com/html-injection-iframe.php?url=" onload="alert('XSS')
```

## Important:

• **Always validate or whitelist** URL parameters used in dynamic contexts like iframe `src`.
• **Never output untrusted user input directly** into HTML attributes or content without proper escaping or validation.

---
## Vulnerable Code Review: Html Injection IFrame 2

The iframe `src` is set from POSTed `website` value without validation, allowing an attacker to send any URL via crafted POST requests, leading to HTML injection risks.

```php
html-injection-iframe2.php
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>HTML Injection IFrame 2</title>
</head>
<body>

<h1>Html Injection IFrame 2</h1>

<form action="html-injection-iframe2.php" method="POST">
    <label for="cars">Choose a Website:</label>
    <select name="website" id="website">
        <option value="http://127.0.0.1/">localhost</option>
        <option value="https://www.armourinfosec.com/">Armour Infosec</option>
        <option value="https://www.google.com/">Google</option>
        <option value="https://www.facebook.com/">Facebook</option>
    </select>
    <br><br>
    <input type="submit" name="submit" value="Submit">
</form>

<?php
if (isset($_POST['submit'])) {
    // Directly assigning user input (POST 'website') to $url without validation
    $url = $_POST['website'];
}
?>

<iframe src="<?php echo $url; ?>" width="800px" height="800px"></iframe>

</body>
</html>
```

## Vulnerability Details

• The `<select>` options suggest users can only choose predefined URLs.
• However, an attacker can craft a POST request with any arbitrary `website` value.
• This allows the attacker to control the iframe source URL, potentially redirecting victims to malicious sites or causing unexpected behavior.

## Example Attack

Using a tool like curl or custom form submit, attacker sets `website` to:

```
http://malicious-site.com
```

and the iframe loads untrusted content.

## Notice

• This code does **not** validate or whitelist the `website` POST parameter.
• To avoid this vulnerability, **always validate input server-side** or map input to predefined, safe URLs.

---
## Vulnerable Code Review: Html Injection IFrame 3

This version limits the iframe source URL to predefined options based on the user's selection, which **reduces the risk of arbitrary HTML injection by disallowing freeform URLs**.

However, **the code still echoes the `$url` variable directly into the iframe `src` attribute without escaping, so some injection risk exists if the input is tampered with outside the provided options**.

```php
html-injection-iframe3.php
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>HTML Injection IFrame 3</title>
</head>
<body>

<h1>Html Injection IFrame</h1>

<form action="html-injection-iframe3.php" method="POST">
    <label for="cars">Choose a Website:</label>
    <select name="website" id="website">
        <option value="0">localhost</option>
        <option value="1">Armour Infosec</option>
        <option value="2">Google</option>
        <option value="3">Facebook</option>
    </select>
    <br><br>
    <input type="submit" name="submit" value="Submit">
</form>

<?php
if (isset($_POST['submit'])) {
    
    switch ($_POST['website']) {
        case '0':
            $url = "http://127.0.0.1/";
            break;
            
        case '1':
            $url = "https://www.armourinfosec.com/";
            break;
            
        case '2':
            $url = "https://www.google.com/";
            break;
            
        case '3':
            $url = "https://www.facebook.com/";
            break;
            
        default:
            $url = "http://192.168.1.45/";
            break;
    }
}
?>

<iframe src="<?php echo $url; ?>" width="800px" height="800px"></iframe>

</body>
</html>
```

## Security Notes

• Because URLs are strictly selected within a `switch` statement on a controlled set of values ( 0 - 3 ), users **cannot directly inject arbitrary strings into the iframe source via the form**.
• However, if an attacker manually crafts a POST request with an unexpected `website` value **not covered by the cases**, the default URL `http://192.168.1.45/` is loaded, which is safe.
• The `$url` value is echoed without escaping, but since it contains only fixed, trusted URLs, the risk of injection here is minimal.
• This approach is a **basic whitelist model** restricting input to known safe options.

**This code** remains vulnerable** if the whitelist block is changed or bypassed, but as-is it limits arbitrary injection vectors.

## Summary Table

| File                       | User Controls src? | Vulnerable to Arbitrary URL? | Notes                  |
| -------------------------- | ------------------ | ---------------------------- | ---------------------- |
| html-injection-iframe.php  | Yes (via GET)      | **Yes**                      | Most dangerous         |
| html-injection-iframe2.php | Yes (via POST)     | **Yes** (with crafted POST)  | Hidden risk            |
| html-injection-iframe3.php | Menu only          | **No** (predefined URLs)     | Safer due to whitelist |

---
---