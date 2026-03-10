 Web Application Endpoints

  

In a web application, **endpoints** are specific URLs or routes that the application exposes to handle requests and responses between the client (e.g., browser, mobile app) and the server.

  

## Common Types of Endpoints

  

### 1. API Endpoints

  

These are used to handle data exchange, typically in JSON or XML format.

  

- GET /api/users → Fetch list of users

- POST /api/login → Authenticate a user

- PUT /api/profile/123 → Update a user profile

- DELETE /api/posts/456 → Delete a specific post

  

### 2. UI (Page) Endpoints

  

These return rendered HTML pages and are used by users via the browser.

  

- /home

- /login

- /dashboard

- /admin/settings

  

### 3. Static Resource Endpoints

  

Serve unchanging content such as stylesheets, scripts, and images.

  

- /css/style.css

- /js/app.js

- /images/logo.png

  

### 4. Authentication & Session Endpoints

  

- POST /auth/login → User login

- POST /auth/logout → User logout

- GET /auth/session → Retrieve session details

  

### 5. Special Function Endpoints

  

- POST /upload → File uploads

- GET /download/report.pdf → File downloads

- GET /search?q=keyword → Search functionality

  

## Why Endpoints Matter in Security Testing

  

Each endpoint is a potential **attack surface**. During a **web application penetration test**, you should:

  

- Discover and map all endpoints

- Test for injection and validation issues (e.g., XSS, SQLi)

- Check access control and authentication mechanisms

- Analyze input/output behavior

  

## How to Discover Endpoints

  

- **Burp Suite Crawler**

- **Browser Developer Tools** (Network tab)

- **Manual browsing**

- **Content discovery tools**

    - ffuf

    - dirsearch

    - gobuster

- **JavaScript analysis** (look for hidden/dynamic routes)

  

---

---