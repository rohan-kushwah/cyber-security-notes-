# Unvalidated-Redirects-and-Forwards

## Unvalidated Redirects and Forwards (Open Redirection Vulnerability)

Unvalidated Redirects and Forwards (Open Redirection) occur when a web application redirects or forwards users to a new URL based on user-controlled input without proper validation.

---

### Why Redirects Are Necessary

- Redirects help route visitors to different internal or external pages.
- They support user flows such as login, registration, and landing pages.
- Redirects can point to both internal and external links.

---

### The Vulnerability

- Attackers exploit the "trust" users have in links provided by a legitimate domain.
- When redirects are unvalidated, attackers can manipulate these to send users to malicious external sites.
- This can be used for phishing attacks that rely on the perceived trustworthiness of the original domain.

---

### Security Impact

- Users are deceived into visiting harmful websites.
- Leads to theft of credentials, malware installation, and other phishing tactics.
- User trust in the affected domain is compromised.

---

### Mitigation Strategies

- Do **not** use user input directly for redirection URLs.
- Validate redirect targets strictly against an allow-list of trusted destinations.
- Use server-side mappings for redirect tokens instead of full URLs.
- Regularly audit and test applications for open redirect flaws.

---

## unvalidated-redirects.php

```
vim unvalidated-redirects.php
```

```php
<!DOCTYPE html>
<html>
<head>
<title>Unvalidated Redirects and Forwards</title>
</head>
<body>

<?php
// Output an HTML paragraph containing a hyperlink to redirect.php with a URL parameter pointing to Armour Infosec website
echo '<p> Please Visit <a href="redirect.php?url=https://www.armourinfosec.com"> Armour Infosec </a> </p>';
?>

</body>
</html>
```

```
vim redirect.php
```

```php
<?php
// Redirect the user to the URL specified in the "url" query parameter without validation (vulnerable to open redirect attacks)
header('Location: '.$_GET["url"]);
// Stop executing the script after sending the redirect header
die();
?>
```

---

## unvalidated-redirects-2.php

```
vim unvalidated-redirects-2.php
```

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

```
vim redirect-1.php
```

```php
<?php
// Perform an HTTP redirect to the URL specified in the "url" query parameter after decoding it from URL encoding
// NOTE: This is still vulnerable to open redirect attacks because there is no validation of the URL
header('Location: '.urldecode($_GET["url"]));
// Terminate script execution after redirect
die();
?>
```

---

## unvalidated-redirects-3.php

```
vim unvalidated-redirects-3.php
```

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

```
vim redirect-3.php
```

```php
<?php
// Redirect the user to the URL after first URL-decoding and then base64-decoding the "url" query parameter
// This is still vulnerable to open redirect attacks because there is no validation of the decoded URL
header('Location: '.base64_decode(urldecode($_GET["url"])));
// Terminate the script after redirecting
die();
?>
```

---

## unvalidated-redirects-4.php

```
vim unvalidated-redirects-4.php
```

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

```
vim redirect-4.php
```

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
```

---

## unvalidated-redirects-5.php

```
echo -n 'https://www.google.com' | md5sum
```

```
vim unvalidated-redirects-5.php
```

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
echo '<p> Please Visit <a href="redirect-5.php?url=https://www.armourinfosec.com&hash=c1c09956e5eae1df25b93cd3d6857d1"> Armour Infosec </a> </p>';
?>

</body>
</html>
```

```
vim redirect-5.php
```

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

### Key Points:

- The code improves security slightly by adding a salt ("qazwsxedc") before hashing, which makes it harder for attackers to forge valid hashes without knowing the salt.
- This prevents trivial hash forgery compared to the earlier version.
- Still, this does **not fully prevent open redirect vulnerabilities** because:
    - The URL itself is not validated against a whitelist or trusted domains.
    - If the salt leaks or can be guessed, an attacker can again create valid redirect hashes.
- Best practice remains to validate redirect URLs by strict allow-listing rather than relying solely on hash checks.

---

## unvalidated-redirects-6.php

```
vim unvalidated-redirects-6.php
```

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

```
vim redirect-6.php
```

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