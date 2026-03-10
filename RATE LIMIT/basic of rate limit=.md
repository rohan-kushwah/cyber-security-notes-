# 🚦 Rate Limiting – Web Application Security Guide  
  
---  
  
## 📖 What is Rate Limiting?  
  
**Rate Limiting** controls how many requests a client can make to a web application or API within a specific time window.  
  
Example:  

100 requests per minute per IP

  
It helps protect systems from:  
  
- 🔑 Brute-force attacks  
- 🤖 Bots and scraping  
- 💥 Denial-of-Service (DoS)  
- ⚡ Resource exhaustion  
  
---  
  
## 🌍 Why Rate Limiting Matters (2025+)  
  
Modern applications expose:  
  
- Public APIs  
- Microservices  
- High-traffic endpoints  
  
Without rate limiting, a small number of abusive clients can degrade service for everyone.  
  
Common platforms implementing rate limiting:  
  
- Cloudflare  
- AWS API Gateway  
- Azure Front Door  
- Akamai  
- Vercel Edge  
- NGINX / Envoy  
  
---  
  
# 🔑 Typical Keys Used for Rate Limiting  
  
Rate limiting works by identifying **who is sending requests**.  
  
Common identifiers:  
  
### 🌐 IP Address  
Used for general abuse prevention.  

Example:  
10 requests / second per IP

  
Often implemented at:  
  
- CDN  
- WAF  
- Reverse Proxy  
  
---  
  
### 🔑 API Key / Client ID  
Used for public APIs.  
  
Example:  

1000 requests/day per API key

  
---  
  
### 👤 Authenticated User ID  
  
Used when users are logged in.  
  
Example:  

5 password attempts per user per 5 minutes

  
---  
  
### 🧬 Combined Fingerprints  
  
Advanced systems combine:  

IP + User-Agent + TLS fingerprint (JA3/JA4)

  
This prevents bypass using VPNs or botnets.  
  
---  
  
# ⚙️ Core Rate Limiting Algorithms  
  
## 1️⃣ Fixed Window Counter  
  
Counts requests within a fixed time interval.  
  
Example:  

Limit: 100 requests per minute

00:00 - 00:59 → 100 requests allowed  
01:00 → counter resets

  
### ✔ Advantages  
- Simple  
- Fast  
- Easy to implement  
  
### ❌ Problem  
Burst attacks near window boundaries.  
  
Example:  

100 requests at 00:59  
100 requests at 01:00  
= 200 requests in 2 seconds

  
---  
  
## 2️⃣ Sliding Window  
  
Uses rolling timestamps.  
  
Example:  

Last 60 seconds window

  
### ✔ Advantages  
- More accurate  
- Prevents burst abuse  
  
---  
  
## 3️⃣ Token Bucket  
  
Requests consume tokens from a bucket.  
  
Example:  

Bucket size: 20  
Refill rate: 5 tokens/sec

  
Allows **short bursts but enforces long-term limits**.  
  
---  
  
## 4️⃣ Leaky Bucket  
  
Works like a queue.  
  
Requests are processed at a **constant rate**.  
  
If queue is full:  

Requests are dropped or delayed

  
Good for protecting backend systems.  
  
---  
  
# 📍 Where Rate Limiting is Implemented  
  
Rate limiting can be applied at multiple layers.  
  
---  
  
## 🌐 Edge / WAF / CDN  
  
Examples:  
  
- Cloudflare  
- Akamai  
- AWS WAF  
  
Purpose:  

Block bots before reaching backend

  
Example rule:  

> 50 requests / minute → block IP

  
---  
  
## 🚪 API Gateway / Service Mesh  
  
Examples:  
  
- Kong  
- Envoy  
- AWS API Gateway  
- Apigee  
  
Used for:  
  
- Per API key limits  
- Per service limits  
- Plan-based limits  
  
Example:  

Free plan → 1000 requests/day  
Premium → 10000 requests/day

  
---  
  
## 🧠 Application Layer (Middleware)  
  
Framework examples:  
  
### Node.js  

express-rate-limit

  
### Django  

django-ratelimit

  
### ASP.NET  

AddRateLimiter()

  
### Spring Boot  

Bucket4j

  
Example rule:  

/login → 5 requests per minute

  
---  
  
# 🔐 Rate Limiting for Brute Force Protection  
  
Rate limiting is **critical for login security**.  
  
Attack example:  

attacker tries 10,000 passwords

  
Without rate limit:  

Attack finishes in seconds

  
With rate limit:  

5 attempts per minute

  
Attack becomes impractical.  
  
---  
  
## 🔑 Endpoints That MUST Have Rate Limits  
  
| Endpoint | Reason |  
|--------|--------|  
| `/login` | Password brute force |  
| `/register` | Bot account creation |  
| `/forgot-password` | Email bombing |  
| `/otp-verify` | OTP brute force |  
| `/api/auth` | Credential stuffing |  
  
---  
  
## Example Secure Login Policy  

5 login attempts per minute per IP  
5 attempts per user account  
lock account for 15 minutes

  
---  
  
# 🧪 How to Check if Rate Limiting Exists (Pentesting)  
  
During **web application testing**, you should verify if rate limiting exists.  
  
---  
  
## Method 1: Burp Suite Intruder  
  
Send multiple login requests.  
  
Example:  

POST /login  
username=admin  
password=123

  
Send **100+ requests quickly**.  
  
### Expected secure behavior  
  
Server returns:  

429 Too Many Requests

  
or  

Account temporarily locked

  
---  
  
## Method 2: Using curl Loop  
  
```bash  
for i in {1..50}  
do  
curl -X POST https://target.com/login \  
-d "username=admin&password=test"  
done

If responses are always **200 OK**, rate limit may be missing.

---

## Method 3: Using ffuf

ffuf -u https://target.com/login \  
-X POST \  
-d "username=admin&password=FUZZ" \  
-w passwords.txt

Watch for:

429 Too Many Requests

---

## Method 4: Python Script

import requests  
  
url = "https://target.com/login"  
  
for i in range(100):  
    r = requests.post(url,data={"username":"admin","password":"test"})  
    print(i,r.status_code)

---

# 🚨 Signs Rate Limiting Exists

Look for headers like:

X-RateLimit-Limit  
X-RateLimit-Remaining  
X-RateLimit-Reset  
Retry-After

Example response:

HTTP/1.1 429 Too Many Requests  
  
Retry-After: 60

---

# 🚨 Signs Rate Limiting is Missing

Indicators:

- Unlimited login attempts
    
- No CAPTCHA
    
- No 429 responses
    
- No delay between requests
    
- No account lockout
    

This leads to:

Brute Force Vulnerability

---

# 🛠 Common Rate Limit Bypass Techniques (For Security Testing)

Attackers may try:

### 1️⃣ IP Rotation

Using proxies:

TOR  
VPN  
Proxy lists

---

### 2️⃣ Header Spoofing

Some apps trust headers like:

X-Forwarded-For  
X-Real-IP

Example:

X-Forwarded-For: 1.1.1.1

---

### 3️⃣ Account Rotation

Attackers try:

username1  
username2  
username3

Instead of targeting one account.

---

# 🧠 Best Practices for Secure Rate Limiting

✔ Use **multiple identifiers**

IP + User ID + Device fingerprint

✔ Apply limits to **sensitive endpoints**

✔ Return proper response codes

429 Too Many Requests

✔ Add **exponential backoff**

✔ Implement **temporary account lockouts**

---

# 📚 Summary

|Feature|Purpose|
|---|---|
|Rate limiting|Prevent abuse|
|Login limits|Stop brute force|
|API limits|Protect infrastructure|
|Sliding window|Accurate limiting|
|Token bucket|Burst-friendly control|

---

# 🔚 Conclusion

Rate limiting is one of the **most important defenses in modern web applications**.

Without it, attackers can perform:

- Brute force attacks
    
- Credential stuffing
    
- API abuse
    
- Denial of service
    

Implementing **multi-layer rate limiting** ensures both **security and performance**.