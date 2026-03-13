# Unvalidated Redirects and Forwards (Open Redirection Vulnerability)

## Overview

Unvalidated Redirects and Forwards (Open Redirection) occur when a web application redirects or forwards users to a new URL based on user-controlled input without proper validation.

---

## Why Redirects Are Necessary

- Redirects help route visitors to different internal or external pages.
- They support user flows such as login, registration, and landing pages.
- Redirects can point to both internal and external links.

---

## The Vulnerability

- Attackers exploit the "trust" users have in links provided by a legitimate domain.
- When redirects are unvalidated, attackers can manipulate these to send users to malicious external sites.
- This can be used for phishing attacks that rely on the perceived trustworthiness of the original domain.

---

## Security Impact

- Users are deceived into visiting harmful websites.
- Leads to theft of credentials, malware installation, and other phishing tactics.
- User trust in the affected domain is compromised.

---

## Mitigation Strategies

- **Do not** use user input directly for redirection URLs.
- Validate redirect targets strictly against an allow-list of trusted destinations.
- Use server-side mappings for redirect tokens instead of full URLs.
- Regularly audit and test applications for open redirect flaws.

---

## Code Examples

### Vulnerable Example 1: Direct User Input (unvalidated-redirects.php)

```php
<?php
// Output an HTML paragraph containing a hyperlink to redirect.php with a URL parameter pointing to Armour Infosec website
echo '<p> Please Visit <a href="redirect.php?url=https://www.armourinfosec.com"> Armour Infosec </a></p>';
?>
```

**redirect.php (Vulnerable):**

```php
<?php
// Redirect the user to the URL specified in the "url" query parameter without validation (vulnerable to open redirect attacks)
header('Location: '.$_GET["url"]);
// Stop executing the script after sending the redirect header
die();
?>
```

**Vulnerability:** The redirect URL is taken directly from `$_GET["url"]` without any validation, allowing attackers to craft malicious links.

---

### Vulnerable Example 2: URL-Encoded Input (unvalidated-redirects-2.php)

**unvalidated-redirects-2.php:**

```php
<!DOCTYPE html>
<html>
<head>
<title>Unvalidated Redirects and Forwards</title>
</head>
<body>

<?php
// Output an HTML paragraph containing a hyperlink to redirect-1.php
// The URL parameter is URL-encoded for safe inclusion in the link
echo '<p> Please Visit <a href="redirect-2.php?url=https%3A%2F%2Fwww.armourinfosec.com%2F"> Armour Infosec </a> </p>';
?>

</body>
</html>
```

**redirect-1.php (Still Vulnerable):**

```php
<?php
// Perform an HTTP redirect to the URL specified in the "url" query parameter after decoding it from URL encoding
// NOTE: This is still vulnerable to open redirect attacks because there is no validation of the URL
header('Location: '.urldecode($_GET["url"]));
// Terminate script execution after redirect
die();
?>
```

**Vulnerability:** Even with URL decoding, there's no validation of the destination URL. Attackers can still manipulate the `url` parameter to redirect users to malicious sites.

---

### Vulnerable Example 3: Base64-Encoded Input (unvalidated-redirects-3.php)

**unvalidated-redirects-3.php:**

```php
<!DOCTYPE html>
<html>
<head>
<title>Unvalidated Redirects and Forwards</title>
</head>
<body>

<?php
// Output an HTML paragraph containing a hyperlink to redirect-3.php
// The URL parameter is base64-encoded ("aHR0cHM6Ly93d3cuYXJtb3VyaW5mb3NlYy5jb20=" is base64 for "https://www.armourinfosec.com")
// This encoding is used for obscuring the URL in the query string
echo '<p> Please Visit <a href="redirect-3.php?url=aHR0cHM6Ly93d3cuYXJtb3VyaW5mb3NlYy5jb20="> Armour Infosec </a> </p>';
?>

</body>
</html>
```

**redirect-3.php (Still Vulnerable):**

```php
<?php
// Redirect the user to the URL after first URL-decoding and then base64-decoding the "url" query parameter
// This is still vulnerable to open redirect attacks because there is no validation of the decoded URL
header('Location: '.base64_decode(urldecode($_GET["url"])));
// Terminate the script after redirecting
die();
?>
```

**Vulnerability:** Despite using base64 encoding to obscure the URL, there's still no validation. Attackers can base64-encode any malicious URL and exploit this redirect.

---

### Vulnerable Example 4: Hash-Based Validation (Weak) (unvalidated-redirects-4.php)

**unvalidated-redirects-4.php:**

```php
<!DOCTYPE html>
<html>
<head>
<title>Unvalidated Redirects and Forwards</title>
</head>
<body>

<?php
// Output an HTML paragraph with a link to redirect-4.php
// The "url" parameter contains the redirect destination (plain text, not encoded)
// The "hash" parameter contains the MD5 hash of the "url" parameter
echo '<p> Please Visit <a href="redirect-4.php?url=https://www.armourinfosec.com&hash=5089e40f00d4e2dbefeaf5eb2b8d6768"> Armour Infosec </a> </p>';
?>

</body>
</html>
```

**redirect-4.php (Still Vulnerable - Weak Validation):**

```php
<?php

// Retrieve and URL-decode the "url" and "hash" parameters from the query string
$url = urldecode($_GET["url"]);
$hash = urldecode($_GET["hash"]);

// Check if the provided hash matches the MD5 hash of the URL parameter
if ($hash == hash("md5", $url)) {

    // If valid, redirect to the URL provided in the "url" parameter (no further validation)
    header('Location: '.$_GET["url"]);
    die();

} else {

    // If the hash does not match, deny the redirect and output an error message
    echo "Invalid Redirect URL";
    die();

}

?>
```

**Vulnerability:** This implementation uses MD5 hash verification but:

- The URL itself is still not validated against any allow-list
- Attackers can craft their own URL with a valid MD5 hash
- This only ensures the hash matches the URL, not that the URL is safe
- Still vulnerable to open redirect attacks

---

### Vulnerable Example 5: Hash with Salt (Still Weak) (unvalidated-redirects-5.php)

**unvalidated-redirects-5.php:**

```php
<!DOCTYPE html>
<html>
<head>
<title>Unvalidated Redirects and Forwards</title>
</head>
<body>

<?php
// Output an HTML paragraph with a link to redirect-5.php
// The "url" parameter contains the redirect destination URL (not encoded)
// The "hash" parameter contains the MD5 hash of the URL concatenated with a salt string "qazwsxedc"
echo '<p> Please Visit <a href="redirect-5.php?url=https://www.armourinfosec.com&hash=c1c099656e5eae1df25b93cd3d6857d1"> Armour Infosec </a> </p>';
?>

</body>
</html>
```

**redirect-5.php (Still Vulnerable - Salt Doesn't Fix Core Issue):**

```php
<?php

// URL-decode the "url" and "hash" parameters from the query string
$url = urldecode($_GET["url"]);
$hash = urldecode($_GET["hash"]);

// Append a secret salt string to the URL before hashing
$url1 = $url."qazwsxedc";

// Compare the provided hash with the MD5 hash of the salted URL string
if ($hash == hash("md5", $url1)) {
    // If hashes match, perform the redirect to the original URL
    header('Location: '.$_GET["url"]);
    die();

} else {
    // If hashes do not match, reject and output error message
    echo "Invalid Redirect URL";
    die();
}

?>
```

**Key Points:**

- The code improves security slightly by adding a salt ("qazwsxedc") before hashing, which makes it harder for attackers to forge valid hashes without knowing the salt.
- This prevents trivial hash forgery compared to the earlier version.
- **However**, if attackers discover or guess the salt, they can still compute valid hashes for arbitrary URLs and bypass this check.
- The actual redirect URL is **still not validated** against any trusted allow-list or domain restrictions.

---

### Vulnerable Example 6: Double MD5 Hashing (Still Insecure) (unvalidated-redirects-6.php)

**unvalidated-redirects-6.php:**

```php
<!DOCTYPE html>
<html>
<head>
<title>Unvalidated Redirects and Forwards</title>
</head>
<body>

<?php
// Output an HTML paragraph with a link to redirect-6.php
// The "url" parameter contains the redirect destination URL (not encoded)
// The "hash" parameter contains the MD5 hash of the MD5 hash of the URL (double MD5 hashing)
echo '<p> Please Visit <a href="redirect-6.php?url=https://www.armourinfosec.com&hash=ab8685b18d233b771e7c7167a3d2d329"> Armour Infosec </a> </p>';
?>

</body>
</html>
```

**redirect-6.php (Still Vulnerable - Double Hashing Doesn't Validate URLs):**

```php
<?php

// URL-decode the "url" and "hash" parameters from the query string
$url = urldecode($_GET["url"]);
$hash = urldecode($_GET["hash"]);

// Compare the provided hash with the MD5 hash of the MD5 hash of the URL (double hashing)
if ($hash == hash("md5", hash("md5", $url))) {
    // If hashes match, perform the redirect to the original URL
    header('Location: '.$_GET["url"]);
    die();

} else {
    // If hashes do not match, reject and output error message
    echo "Invalid Redirect URL";
    die();
}

?>
```

**Key Points:**

- This code uses double MD5 hashing for the URL parameter to verify the hash.
- Double hashing increases complexity slightly but does not fundamentally improve security against open redirect attacks.
- The actual redirect URL is **still not validated** against any trusted allow-list or domain restrictions.
- Attackers who know the hashing method can compute double MD5 hashes for arbitrary URLs and bypass this check.
- For true security, the application should validate redirect targets against a whitelist of allowed URLs or domains rather than relying on hash integrity checks alone.

---

---

### Improved Example 7: Regex Validation with Domain Restriction (unvalidated-redirects-7.php)

**unvalidated-redirects-7.php:**

```php
<!DOCTYPE html>
<html>
<head>
<title>Unvalidated Redirects and Forwards</title>
</head>
<body>

<?php
// Output a paragraph with a link to redirect-7.php
// The "url" parameter points to a URL on armourinfosec.com domain
echo '<p> Please Visit <a href="redirect-7.php?url=https://www.armourinfosec.com"> Armour Infosec </a> </p>';
?>

</body>
</html>
```

**redirect-7.php (Better but Still Vulnerable to Bypass):**

```php
<?php

// URL-decode the "url" parameter from query string
$url = urldecode($_GET["url"]);

// Check if the URL contains the string ".armourinfosec.com" using regex pattern
// This is an attempt to validate that the redirect is only to armourinfosec.com or its subdomains
if (preg_match('/.armourinfosec.com/', $url)) {
    // If matched, perform safe redirect to the provided URL
    header('Location: '.$_GET["url"]);
    die();

} else {
    // If not matched, reject the redirect and show error message
    echo "Invalid Redirect URL";
    die();
}

?>
```

**Key Points:**

- The validation relies on a regex check to ensure the URL contains the domain `armourinfosec.com`.
- This restricts redirection only to URLs within `armourinfosec.com` or any subdomain like `sub.armourinfosec.com`.
- This is a significant improvement over prior examples with no validation as it enforces domain allow-listing.
- **However, the regex `/.armourinfosec.com/` should be carefully chosen:**
    - It will match any string containing any character followed by `armourinfosec.com`.
    - **For better precision, consider anchoring the pattern and escaping the dot:**
        
        ```php
        '/^https:\/\/[a-z0-9-]+\.armourinfosec\.com\/i'
        ```
        

**Bypass Example:**

```
http://192.168.1.31/web-pentest/redirect-7.php?url=https://www.armourinfosec.com@google.com
```

This could potentially bypass the check if the regex isn't strict enough.

---

### Improved Example 8: Stricter Regex with Protocol Validation (unvalidated-redirects-8.php)

**unvalidated-redirects-8.php:**

```php
<!DOCTYPE html>
<html>
<head>
<title>Unvalidated Redirects and Forwards</title>
</head>
<body>

<?php
// Output a paragraph with a hyperlink to the redirect-7.php script
// The URL parameter points to an armourinfosec.com subdirectory
echo '<p> Please Visit <a href="redirect-8.php?url=https://www.armourinfosec.com/category/ethical-hacking/"> Armour Infosec </a> </p>';
?>

</body>
</html>
```

**redirect-8.php (Better Validation):**

```php
<?php

// Decode the URL parameter from the query string
$url = urldecode($_GET["url"]);

// Use a regex to check if the URL starts with "https://www.armourinfosec.com"
if (preg_match('/^https:\/\/www.armourinfosec.com/', $url)) {
    // If it matches, redirect to the given URL
    header('Location: '.$_GET["url"]);
    die();

} else {
    // If it doesn't match, output an error message
    echo "Invalid Redirect URL";
    die();
}

?>
```

**Key Points:**

- Uses regex pattern `/^https:\/\/www.armourinfosec.com/` to validate URLs
- The `^` anchor ensures the URL **starts with** the specified pattern
- This is more secure than example 7 as it checks the beginning of the URL
- **Still vulnerable to bypass attacks:**
    - `https://www.armourinfosec.com@google.com` - user-info domain trick
    - `https://www.armourinfosec.com.google.com` - subdomain trick

**Bypass Examples:**

```
http://192.168.1.31/web-pentest/redirect-8.php?url=https://www.armourinfosec.com@google.com
http://192.168.1.31/web-pentest/redirect-8.php?url=https://www.armourinfosec.com.google.com
```

---

### More Secure Example 9: Multiple Validation Checks (unvalidated-redirects-9.php)

**unvalidated-redirects-9.php:**

```php
<!DOCTYPE html>
<html>
<head>
<title>Unvalidated Redirects and Forwards</title>
</head>
<body>

<?php
// Output a paragraph with a hyperlink to the redirect-9.php script
// The URL parameter links to the ethical hacking category on Armour Infosec
echo '<p> Please Visit <a href="redirect-8.php?url=https://www.armourinfosec.com/category/ethical-hacking/"> Armour Infosec </a> </p>';
?>

</body>
</html>
```

**redirect-9.php (Improved with Multiple Checks):**

```php
<?php

// Decode the URL parameter from the query string
$url = urldecode($_GET["url"]);

// Use a regex to check if the URL starts with "https://www.armourinfosec.com"
// Also ensure there is no "@" symbol in the URL to prevent user-info domain tricks
if (preg_match('/^https:\/\/www.armourinfosec.com/', $url) && !preg_match('/\@/', $url)) {
    // If both conditions are true, perform the redirect
    header('Location: '.$_GET["url"]);
    die();

} else {
    // If validation fails, output an error message
    echo "Invalid Redirect URL";
    die();
}

?>
```

**Key Points:**

- Uses **two validation checks**:
    1. URL must start with `https://www.armourinfosec.com`
    2. URL must NOT contain `@` symbol (prevents user-info bypass)
- This prevents common bypass techniques like `https://www.armourinfosec.com@google.com`
- More secure than examples 7 and 8
- **However, still potentially vulnerable** to other bypass techniques

---

## Common Open Redirect Payloads

Open Redirect vulnerabilities can be exploited using various payload formats. Here's a comprehensive list from the **Open-Redirect-Payloads** repository:

```
//localdomain.pw/%2f..
//www.whitelisteddomain.tld@localdomain.pw/%2f..
///localdomain.pw/%2f..
///www.whitelisteddomain.tld@localdomain.pw/%2f..
////localdomain.pw/%2f..
////www.whitelisteddomain.tld@localdomain.pw/%5c..
https://localdomain.pw/%2f..
https://www.whitelisteddomain.tld@localdomain.pw/%2f..
/https://localdomain.pw/%2f..
/https://www.whitelisteddomain.tld@localdomain.pw/%2f..
//localdomain.pw/%2f%2e%2e
//www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
///localdomain.pw/%2f%2e%2e
///www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
////localdomain.pw/%2f%2e%2e
////www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
https://localdomain.pw/%2f%2e%2e
https://www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
/https://localdomain.pw/%2f%2e%2e
/https://www.whitelisteddomain.tld@localdomain.pw/%2f%2e%2e
//localdomain.pw/
//www.whitelisteddomain.tld@localdomain.pw/
///localdomain.pw/
///www.whitelisteddomain.tld@localdomain.pw/
////localdomain.pw/
////www.whitelisteddomain.tld@localdomain.pw/
https://localdomain.pw/
https://www.whitelisteddomain.tld@localdomain.pw/
```

**Explanation of Payload Techniques:**

1. **Protocol-relative URLs**: `//domain.com` - inherits the protocol from the current page
2. **User-info bypass**: `//trusted.com@malicious.com` - browser navigates to malicious.com
3. **Multiple slashes**: `///domain.com` - some parsers may misinterpret
4. **Path traversal encoding**: `/%2f..` - URL-encoded path traversal
5. **Mixed encoding**: `/%2f%2e%2e` - double URL encoding

---

## Attack Scenario

1. Attacker crafts a malicious URL using a trusted domain:
    
    ```
    https://trustedsite.com/redirect.php?url=https://malicious-site.com/phishing
    ```
    
2. User sees the trusted domain (`trustedsite.com`) and clicks the link.
    
3. The vulnerable redirect script forwards the user to `malicious-site.com/phishing`.
    
4. User believes they're on a legitimate site due to the trusted origin of the link.
    
5. Attacker steals credentials, installs malware, or performs other malicious actions.
    

---

## Secure Implementation Examples

### Using Allow-list Validation

```php
<?php
$allowed_domains = [
    'www.armourinfosec.com',
    'blog.armourinfosec.com',
    'shop.armourinfosec.com'
];

$redirect_url = $_GET["url"] ?? '';
$parsed_url = parse_url($redirect_url);

if (isset($parsed_url['host']) && in_array($parsed_url['host'], $allowed_domains)) {
    header('Location: ' . $redirect_url);
    die();
} else {
    // Redirect to safe default or show error
    header('Location: /error.php');
    die();
}
?>
```

### Using Token-Based Redirects

```php
<?php
$redirect_map = [
    'home' => '/index.php',
    'about' => '/about.php',
    'contact' => '/contact.php',
    'external_partner' => 'https://trusted-partner.com'
];

$token = $_GET["redirect"] ?? 'home';

if (isset($redirect_map[$token])) {
    header('Location: ' . $redirect_map[$token]);
    die();
} else {
    header('Location: /index.php');
    die();
}
?>
```

---

## Testing for Open Redirect Vulnerabilities

### Manual Testing

1. Identify redirect parameters in URLs (`?url=`, `?redirect=`, `?next=`, `?returnUrl=`)
2. Modify the parameter value to point to an external domain
3. Check if the application redirects to the external domain without validation

### Common Parameter Names to Test

- `url`
- `redirect`
- `redirect_uri`
- `return`
- `returnUrl`
- `next`
- `goto`
- `target`
- `destination`

### Test Payloads

```
?url=https://evil.com
?url=//evil.com
?url=/\/evil.com
?url=https://trusted.com@evil.com
?url=https://trusted.com.evil.com
```

---

## Prevention Checklist

- [ ] Never use user input directly in redirect destinations
- [ ] Implement strict allow-list validation for redirect targets
- [ ] Use indirect reference maps (tokens) instead of full URLs
- [ ] Validate and sanitize all redirect parameters
- [ ] Log redirect attempts for security monitoring
- [ ] Educate users about verifying URLs before clicking
- [ ] Conduct regular security audits and penetration testing
- [ ] Use security headers (Content-Security-Policy)
- [ ] Implement proper error handling for invalid redirects

---

## Additional Resources

- OWASP Top 10: A10 - Unvalidated Redirects and Forwards
- CWE-601: URL Redirection to Untrusted Site ('Open Redirect')
- Security testing tools: Burp Suite, OWASP ZAP

---

**Source:** Armour Infosec Security Training Materials