### **1️⃣ Host Header: HTTP vs HTTPS**

- **The Host header exists in both HTTP and HTTPS.**

- **In HTTP, it's a standard request header.**

- **In HTTPS, it's sent during the TLS handshake via SNI (Server Name Indication).**

---

### **2️⃣ HTTP Request/Response Headers (Plain HTTP - Port 80)**

If you open **`http://site1.local`**, the **actual HTTP headers** are exchanged as follows:

#### **🔹 Request (Client → Server)**

```

GET /index.html HTTP/1.1

Host: site1.local

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)

Accept: text/html,application/xhtml+xml,application/xml;q=0.9

Connection: keep-alive

```

**Explanation:**

- `GET /index.html` → Requesting `/index.html` from the server.

- `Host: site1.local` → This is the **Host Header**, indicating **which site** is being accessed.

- `User-Agent` → Name and version of the browser.

- `Accept` → Client states it wants HTML/XML content.

- `Connection: keep-alive` → Indicates the connection should be kept open.

#### **🔹 Response (Server → Client)**

```

HTTP/1.1 200 OK

Date: Wed, 07 Feb 2025 12:00:00 GMT

Server: Microsoft-IIS/10.0

Content-Type: text/html

Content-Length: 345

Connection: keep-alive

<html>

  <body>

    <h1>Welcome to Site1</h1>

  </body>

</html>

```

**Explanation:**

- `HTTP/1.1 200 OK` → Request was successful.

- `Server: Microsoft-IIS/10.0` → Server identifies itself as IIS.

- `Content-Type: text/html` → Sending HTML content.

- `Content-Length: 345` → Size of the response content.

---

### **3️⃣ HTTPS Request/Response Headers (Port 443)**

If you open **`https://site1.local`**, a **TLS handshake along with HTTP headers** is exchanged.

#### **🔹 Step 1: TLS Handshake (Client Hello)**

In the first request, the client performs a **TLS handshake** and sends **SNI**.

```

Client Hello

  - Protocol Version: TLS 1.2

  - Cipher Suites: TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256, ...

  - Extensions:

      - Server Name Indication (SNI): site1.local

```

**Explanation:**

- `Client Hello` → The client initiates the TLS handshake.

- `Server Name Indication (SNI): site1.local` → This is the HTTPS equivalent of the **Host Header**.

- `Cipher Suites` → Sends supported encryption methods for a secure connection.

#### **🔹 Step 2: TLS Server Hello**

The server accepts the handshake and sends an SSL certificate.

```

Server Hello

  - Protocol Version: TLS 1.2

  - Cipher Suite: TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256

  - Certificate: site1.local SSL Certificate

  - Session ID: 12345

```

**Explanation:**

- `Server Hello` → The server accepts the TLS handshake.

- `Certificate: site1.local` → The server sends the SSL certificate for `site1.local`.

- `Session ID` → A session is established for future requests.

---

### **4️⃣ HTTP Request Headers within HTTPS (After TLS Handshake)**

After the handshake is complete, an **encrypted HTTP request is sent**.

#### **🔹 Request (Client → Server)**

```

GET /index.html HTTP/1.1

Host: site1.local

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)

Accept: text/html,application/xhtml+xml,application/xml;q=0.9

Connection: keep-alive

```

(Same as HTTP, but **encrypted**.)

#### **🔹 Response (Server → Client)**

```

HTTP/1.1 200 OK

Date: Wed, 07 Feb 2025 12:00:00 GMT

Server: Microsoft-IIS/10.0

Content-Type: text/html

Content-Length: 345

Connection: keep-alive

```

(Same as HTTP, but **encrypted**.)

---

### **5️⃣ Key Differences Between HTTP and HTTPS**

| Feature          | HTTP (Port 80) | HTTPS (Port 443)       |

|------------------|----------------|------------------------|

| Encryption       | ❌ No           | ✅ Yes (TLS/SSL)        |

| Security         | 🔴 Not Secure   | 🟢 Secure               |

| SNI Support      | ❌ No           | ✅ Yes (TLS Handshake)  |

| Data Visibility  | ✅ Plaintext    | ❌ Encrypted            |

| Certificate      | ❌ Not Required | ✅ Required             |

---

### **6️⃣ Summary**

✔ **Host Header** is present in both HTTP and HTTPS requests. In HTTPS, its equivalent during the TLS handshake is **SNI (Server Name Indication)**.

✔ **HTTP headers are visible in plaintext**, whereas in HTTPS, **a TLS handshake occurs first, and then the headers are sent encrypted**.

✔ **In a client's HTTPS request, SNI is sent first**, which allows IIS to serve the correct website.

✔ **DNS only converts the domain to an IP address**; without proper IIS binding, the request may fail.