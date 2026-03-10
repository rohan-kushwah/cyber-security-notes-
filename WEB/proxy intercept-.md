# 🌐 Proxy

***

The **Proxy** tab in Burp Suite enables you to **intercept**, **modify**, and **inspect** HTTP/S traffic in real time. It is one of the most heavily used features during manual testing, as it displays **live requests**, **responses**, and **WebSocket** communications (in the **Professional** version).

  

### 🔻 Intercept

***

This is the **default landing tab** of the Proxy section. It allows real-time interception and manipulation of HTTP requests.

  

#### Key Features:

  

*   **Intercept On/Off**

    *   When enabled, Burp intercepts **all HTTP requests**, even those **not in scope**.

    *   You can choose to **edit**, **forward**, or **drop** intercepted requests.

*   **Action**

    *   Perform specific actions on the intercepted request, such as:

        *   Send to Repeater

        *   Send to Intruder

        *   Change request method

        *   Match and replace rules

*   **Open Browser**

    *   Launches Burp's built-in **Chromium browser** with the proxy automatically configured.

    *   ▲ **Note:** This browser does **not remember extensions**, **logins**, or **stored passwords**—suitable for temporary testing only.

  

### 📜 HTTP History

***

This tab keeps a full record of all HTTP requests and responses passed through the proxy.


# Burp Suite Proxy Settings

Burp Suite Proxy allows you to intercept, inspect, and modify HTTP(S) traffic from browsers or applications. Below is a detailed breakdown of the Proxy settings.

---

## 1. Proxy → Options → Proxy Listeners
Configure the interface (IP and port) that Burp listens on for incoming traffic.

- **Bind to port**:  
  The port number Burp will listen on (default `8080`).  
  *Use a different port if 8080 is in use.*

- **Bind to address**:  
  - `127.0.0.1` (localhost): Only traffic from your machine.  
  - `All interfaces`: Allows external devices to send traffic through Burp.

- **Support invisible proxying**:  
  Intercepts traffic from clients that do not support explicit proxy configuration (e.g., some mobile apps).

- **Request handling**:  
  Choose whether Burp forwards requests automatically or waits for user action.

- **TLS settings**:  
  Handle HTTPS traffic using Burp’s certificate.

---

## 2. Proxy → Options → Intercept Client Requests
Control which requests coming from the browser/application Burp should stop.

- **Intercept requests based on rules**:  
  Intercept all requests or only those matching specific criteria (domain, method, etc.).

- **Do not intercept**:  
  Define requests Burp should ignore (e.g., static content, analytics calls).

---

## 3. Proxy → Options → Intercept Server Responses
Control which server responses Burp should intercept.

- **Response rules**:  
  Intercept responses based on URL, status code, content type, etc.

---

## 4. Proxy → Options → Match and Replace
Automatically modify requests or responses passing through Burp.

- Examples:  
  - Change `User-Agent` headers.  
  - Replace cookie values.  
  - Rewrite response content for testing.

---

## 5. Proxy → Options → SSL/TLS Settings
Configure HTTPS handling.

- **Client SSL certificates**:  
  Authenticate to the server if required.

- **TLS versions**:  
  Specify supported TLS versions for intercepted traffic.

- **CA Certificate**:  
  Burp generates a CA certificate to intercept HTTPS without browser errors.

---

## 6. Proxy → Options → Connections
Control connection handling between Burp, client, and server.

- **Timeouts**: Time Burp waits for responses.  
- **Follow redirections**: Automatically handle HTTP 3xx redirects.

---

## 7. Proxy → Options → HTTP/2 Settings
Configure how Burp handles HTTP/2 traffic for modern web applications.

---

## Summary

| Section | Purpose |
|---------|---------|
| Proxy Listeners | Where Burp listens for traffic |
| Intercept Client Requests/Server Responses | Control which traffic to stop/analyze |
| Match and Replace | Automatically modify requests/responses |
| SSL/TLS Settings | Handle HTTPS traffic securely |
| Connections & HTTP/2 | Fine-tune connection behavior for reliability |

---

