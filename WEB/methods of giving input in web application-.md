# Methods of Giving Input in Web Applications

Web applications accept user input through multiple channels, often exploited during security testing. Below are the common input vectors:

***

### 1. URL Input Parameters — GET Method
Parameters passed directly in the URL as *query strings*.
```http
GET /page.php?name=<h1>hi</h1> HTTP/1.1
```
**Example:**
```
http://192.168.1.200/mutillidae/index.php?page=browser-info.php&n=<h1>hi</h1>
```
***
### 2. HTML Form Inputs — GET / POST Methods
Inputs submitted through HTML forms using either GET or POST.
```html
<form action="login.php" method="POST">
  <input name="username" value="<h1>hi</h1>" />
</form>
```
*   **GET**: Appends data to URL
*   **POST**: Sends data in request body
***
### 3. HTTP Headers — Request URI / Referrer / URL Injection ( GET )
Manipulation via headers or URLs.
```http
GET /index.php?page=info.php&n=<h1>hi</h1> HTTP/1.1
Referer: http://example.com/page.php?n=<script>alert(1)</script>
```
***
### 4. User-Agent Header — GET / POST
Injected payload in the `User-Agent` string.
```http
User-Agent: Mozilla/5.0 <h1>hi</h1> Chrome/90.0
```
***
### 5. Cookie Header — GET / POST
Cookies can carry custom name/value input pairs.
```http
Cookie: PHPSESSID=abc123; n=<h1>HI</h1>
```
**Example:**
```http
Cookie: showhints=1; n=<h1>HI</h1>; acgroupswithpersist=nada
```
***
### 6. X-Forwarded Headers — GET / POST
Spoofing client IP or injecting payloads via custom proxy headers.
```http
POST /php/process-server.php HTTP/1.1
Host: 192.168.1.45
X-Forwarded-For: <h1>hi</h1>
X-ProxyUser-IP: <h1>Hi</h1>
```
**Other headers:**
*   X-Forwarded-For
*   X-Forwarded-Host
*   X-Client-IP
*   X-Remote-IP
*   X-Remote-Addr
*   X-Host
***
### 7. File Name Fields — GET / POST
Injecting data via file names during upload or reference.
**Example:**
```
filename="<h1>test.php</h1>"
```

---
### 8. API-Specific Body Payloads — POST / PUT / PATCH

Modern applications heavily rely on APIs that consume structured data formats, each presenting a unique attack surface beyond simple form fields.

*   **JSON (JavaScript Object Notation):** The standard for modern REST APIs.
    ```json
    {
        "user_id": "123",
        "is_admin": "true",
        "preferences": {
            "theme": "dark",
            "notify_by_email": "<h1>INJECTED</h1>"
        }
    }
    ```
*   **XML (eXtensible Markup Language):** Common in older enterprise systems and SOAP APIs. A primary vector for XXE (XML External Entity) attacks.
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <user>
        <id>123</id>
        <name><h1>INJECTED</h1></name>
    </user>
    ```
*   **GraphQL:** A query language for APIs. The structure of the query itself is the input.
    ```graphql
    query {
      user(id: "123") {
        # The fields requested are an input
        name
        bio
      }
    }
    ```

***

### 9. URL Path Segments — GET / POST / etc.

In RESTful APIs, parts of the URL path itself are treated as input parameters, often for identifying resources.

**Example:**
```
https://api.example.com/v1/users/INJECTED-VALUE/orders
```
In the URL above, `INJECTED-VALUE` is often mapped directly to a variable in the application's code and is a prime spot for Insecure Direct Object Reference (IDOR) or SQL injection.

***

### 10. WebSocket Messages — (Persistent Connection)

WebSockets provide a persistent, two-way communication channel. Any message sent from the client to the server after the initial handshake is a direct input.

**Example (JSON payload over a WebSocket):**
```json
{
  "action": "post_message",
  "channel": "general",
  "message": "<script>alert('XSS over WebSockets')</script>"
}
```

***

### 11. File Upload Content — POST

While your list includes the *filename*, the **content** of the file is a massive input vector. The server will parse or process this content.

**Example Vectors:**
*   **Malicious Macros:** In `.docx` or `.xlsx` files.
*   **XML Bombs / XXE:** In `.xml`, `.svg`, or even `.docx` files.
*   **Polyglot Files:** A file that is a valid image (e.g., JPEG) but also contains a valid PHP web shell.
*   **Serialized Objects:** In language-specific formats (e.g., Java, PHP, Python) leading to deserialization vulnerabilities.

***

### 12. Custom & Uncommon HTTP Headers — GET / POST

Beyond the standard headers, applications often rely on custom (`X-*`) or less common headers for logic, which can be manipulated.

```http
GET /api/v2/data HTTP/1.1
Host: api.example.com
X-API-Version: <h1>2</h1>
X-Tenant-ID: <script>alert(1)</script>
Accept-Language: en-US,en;q=0.5' OR '1'='1
```

***

### 13. Out-of-Band Vectors — (Indirect)

This is an advanced category where the input doesn't come directly from your request but from another source that your application trusts or interacts with.

**Examples:**
*   **Webhook Data:** Data sent to your application from a third-party service (e.g., Stripe, Twilio, GitHub). An attacker might be able to poison the data *before* it gets sent to your application.
*   **Password Reset Emails:** The tokens and links sent via email can be considered an input vector if they can be manipulated.
*   **Data from a Cache Server:** An attacker could perform cache poisoning to feed malicious data to other users.

---
---