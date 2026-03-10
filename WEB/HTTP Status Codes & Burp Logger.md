# Table of contents

- Overview
    
- HTTP Status Code List (common and useful)
    
    - 1xx — Informational
        
    - 2xx — Success
        
    - 3xx — Redirection
        
    - 4xx — Client errors
        
    - 5xx — Server errors
        
- What is the Logger option in Burp?
    
    - Purpose / Why use it
        
    - Typical features
        
    - How to use (practical steps)
        
    - Use Logger++ (optional)
        
    - Example use-cases
        
- Quick examples (raw HTTP snippets)
    
- Quick reference — common filters
    
- Notes & tips
    
- Security & privacy
    

---

# Overview

This document is a compact reference combining a commonly used HTTP status code list and a practical guide to the **Logger** functionality in Burp Suite (including the popular Logger++ extension). Use it as a README or quick reference inside a GitHub repo.

---

# HTTP Status Code List (common and useful)

HTTP status codes are grouped by class:

- **1xx** — informational
    
- **2xx** — success
    
- **3xx** — redirection
    
- **4xx** — client error
    
- **5xx** — server error
    

Below are common and notable codes with short meanings.

## 1xx — Informational

- `100 Continue` — Client should continue the request (e.g., after sending headers).
    
- `101 Switching Protocols` — Server is switching protocols (e.g., HTTP → WebSocket).
    
- `102 Processing` (WebDAV) — Server is processing but no final response yet.
    

## 2xx — Success

- `200 OK` — Request succeeded; response contains requested resource.
    
- `201 Created` — Resource created (commonly from POST).
    
- `202 Accepted` — Request accepted for processing, not completed.
    
- `203 Non-Authoritative Information` — Meta-information from a third-party.
    
- `204 No Content` — Success, but no body returned.
    
- `205 Reset Content` — Request succeeded; client should reset view/form.
    
- `206 Partial Content` — Partial resource served (Range requests).
    
- `207 Multi-Status` (WebDAV) — Multiple status for different resources.
    
- `208 Already Reported` (WebDAV) — Members of binding already reported.
    
- `226 IM Used` — Response is a delta encoding of the resource.
    

## 3xx — Redirection

- `300 Multiple Choices` — Multiple options for the resource.
    
- `301 Moved Permanently` — Resource permanently moved (update links).
    
- `302 Found` — Temporary redirect (semantics vary historically).
    
- `303 See Other` — See other URI (often after POST; use GET).
    
- `304 Not Modified` — Cached copy is still valid (conditional GET).
    
- `305 Use Proxy` — Resource must be accessed through proxy (deprecated).
    
- `307 Temporary Redirect` — Temporary redirect; method must not change.
    
- `308 Permanent Redirect` — Permanent redirect; method preserved.
    

## 4xx — Client errors

- `400 Bad Request` — Malformed request or invalid syntax.
    
- `401 Unauthorized` — Authentication required / failed.
    
- `402 Payment Required` — Reserved for future use.
    
- `403 Forbidden` — Server refuses to fulfill the request.
    
- `404 Not Found` — Resource not found.
    
- `405 Method Not Allowed` — HTTP method not allowed for resource.
    
- `406 Not Acceptable` — Cannot send acceptable response per Accept headers.
    
- `407 Proxy Authentication Required` — Proxy auth required.
    
- `408 Request Timeout` — Client took too long to send request.
    
- `409 Conflict` — Request conflicts with current resource state.
    
- `410 Gone` — Resource permanently gone.
    
- `411 Length Required` — `Content-Length` header required.
    
- `412 Precondition Failed` — A condition given in request headers failed.
    
- `413 Payload Too Large` — Request entity too large.
    
- `414 URI Too Long` — Request-URI too long.
    
- `415 Unsupported Media Type` — Unsupported `Content-Type`.
    
- `416 Range Not Satisfiable` — Range header asks beyond resource bounds.
    
- `417 Expectation Failed` — `Expect` header could not be met.
    
- `418 I'm a teapot` — RFC joke (occasionally used in testing).
    
- `421 Misdirected Request` — Request routed to incorrect server.
    
- `422 Unprocessable Entity` (WebDAV) — Semantic errors in request.
    
- `423 Locked` (WebDAV) — Resource is locked.
    
- `424 Failed Dependency` (WebDAV) — Consequent failure due to dependency.
    
- `425 Too Early` — Server unwilling to process due to possible replay.
    
- `426 Upgrade Required` — Client should switch protocols.
    
- `428 Precondition Required` — Server requires preconditions.
    
- `429 Too Many Requests` — Rate limiting.
    
- `431 Request Header Fields Too Large` — Headers too large.
    
- `451 Unavailable For Legal Reasons` — Resource blocked for legal reasons.
    

## 5xx — Server errors

- `500 Internal Server Error` — Generic server error.
    
- `501 Not Implemented` — Server lacks capability to fulfill request.
    
- `502 Bad Gateway` — Invalid response from upstream server.
    
- `503 Service Unavailable` — Server currently unable to handle request (overload/maintenance).
    
- `504 Gateway Timeout` — Upstream server timed out.
    
- `505 HTTP Version Not Supported` — Protocol version not supported.
    
- `506 Variant Also Negotiates` — Transparent content negotiation error.
    
- `507 Insufficient Storage` (WebDAV) — Server lacks storage to complete request.
    
- `508 Loop Detected` (WebDAV) — Infinite loop detected in resource references.
    
- `510 Not Extended` — Further extensions required.
    
- `511 Network Authentication Required` — Network authentication required (e.g., captive portals).
    

---

# What is the Logger option in Burp?

The **Logger** in Burp Suite refers to the logging capability (built-in histories plus third-party extensions like **Logger++**) that records HTTP/S traffic and events so you can review, filter, search, and export captured activity during testing.

> Burp’s built-in places to view logged traffic: **Proxy → HTTP history**, **Target → Site map**, **Scanner / Intruder /Repeater** tool-specific histories. Logger++ is a community extension that enhances logging and export features.

## Purpose / Why use it

- Keep an auditable record of requests and responses that passed through Burp.
    
- Debug complex workflows by reviewing previous requests/responses, headers, and bodies.
    
- Spot anomalies (status codes, large responses, unexpected redirects, server errors).
    
- Trace sessions, cookies, tokens and correlate requests across tools.
    
- Export logs for reporting, incident analysis, or automated processing.
    

## Typical features

- **Capture**: raw request & response (start-line, headers, body).
    
- **Metadata**: timestamp, source tool (Proxy/Repeater/Scanner/Intruder), host, URL, method, response code, size.
    
- **Filters**: filter by host, status code class, HTTP method, tool, MIME type, time range.
    
- **Search**: text or regex inside requests/responses.
    
- **Export / Save**: save selected items or full history (raw HTTP, CSV, JSON).
    
- **Persistence**: optional persistent logging across project sessions (via extension).
    

## How to use (practical steps)

1. **Capture traffic**
    
    - Configure your browser to use Burp's proxy, or use Burp's embedded browser.
        
    - Ensure **Intercept** is off to allow traffic flow, or use Intercept on and forward requests manually.
        
2. **View history**
    
    - Open **Proxy → HTTP history** to see proxied requests/responses.
        
    - See other tool histories (Target, Scanner, Intruder, Repeater) for tool-specific logs.
        
3. **Inspect a message**
    
    - Select an entry; the right pane shows `Request` and `Response` tabs (Raw, Headers, Hex, Rendered).
        
    - The **Raw** view is the HTTP source code for that response (status line + headers + body).
        
4. **Filter & search**
    
    - Use the filter bar (host, method, MIME type, status) and the search box (text or regex).
        
5. **Export / save**
    
    - Select entries → right-click → **Save items** (save as raw HTTP or other supported formats).
        
    - Extensions like Logger++ provide additional export formats (CSV/JSON) and options.
        

## Use Logger++ (optional)

- **Logger++** is a popular BApp (Burp extension) that provides: advanced filtering, persistent project logs, scheduled exports, customizable columns, and richer export formats.
    
- Install from the **BApp Store** within Burp Suite (Project option / Extensions).
    

## Example use-cases

- While scanning, filter logs for `500` responses to inspect server errors and payloads.
    
- Search logs for `Set-Cookie` or authorization headers to debug authentication flows.
    
- Export relevant raw requests/responses to reproduce issues for developers.
    

---

# Quick examples (raw HTTP snippets)

### Example request

`GET /path/resource?id=10 HTTP/1.1 Host: example.com User-Agent: curl/7.88.1 Accept: */*`

### Example response (what you see in Burp → Response → Raw)

`HTTP/1.1 200 OK Date: Thu, 13 Nov 2025 12:00:00 GMT Server: nginx/1.24.0 Content-Type: application/json; charset=utf-8 Content-Length: 85  {"id":10,"name":"sample","status":"ok"}`

---

# Quick reference — common filters you’ll use in the Logger / HTTP history

- **Status codes**: `200`, `3xx`, `4xx`, `5xx`
    
- **Methods**: `GET`, `POST`, `PUT`, `DELETE`, `OPTIONS`
    
- **MIME types**: `text/html`, `application/json`, `image/*`
    
- **Host / path substring search**: `example.com/api`
    
- **Time range**: recent X minutes (UI/extension dependent)