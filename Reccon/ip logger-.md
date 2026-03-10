kisi ne crime kara
ab samne wale ke pass uska ip gaya ab ip change hota regta hai toh me wo ip jisse fraud hua hai uska isp djhunduhuga\\
aur usser request karunga ki mujhe ye ip ki detaiils chaioye is smay pe kiske pass tha wo mujhe de denga 
ab mujge person pata hau ab me phusing karunga us admi ka publlic ipp jannne ke liuye ab jaise hi usne kisi link pe click kiya uska public ip apne pass ip to location trace kar leneg kahatam kam

# IP Logger

---

## What is an IP Logger?

An IP Logger is a tool or script used to **capture and record the IP address** of visitors who access a specific URL, webpage, or file.

---

## What It Does

When someone visits a link or page containing an IP logger:

- Their **IP address** is recorded
- **Additional data may also be logged:**
    - Timestamp
    - Browser and OS (User-Agent)
    - Referrer (where they came from)
    - Page visited

## Common Use Cases (Ethical & Legal Contexts)

|Use Case|Description|
|---|---|
|Penetration Testing|Red teamers may use IP loggers to confirm if a target visited a phishing page|
|Bug Bounty Recon|Researchers may track if certain links are accessed by security teams|
|Traffic Analysis (with consent)|Site owners may log IPs for analytics, throttling, or regional redirection|
|Illegal Surveillance|Logging IPs without consent can violate privacy laws|

## Example Tools

| Tool                                  | Type                        |
| ------------------------------------- | --------------------------- |
| [Grabify](https://grabify.link/)      | Link shortener + IP logger  |
| [IPLogger.org](https://iplogger.org/) | Image-based and URL loggers |
| Custom PHP script                     | Fully manual logging setup  |

## Ethical Use Warning

Logging someone's IP without **notice or consent** may violate:

- **GDPR** (EU)
- **CCPA** (California)
- **Cybercrime laws** (depending on jurisdiction)

Always use IP loggers **with permission or in legal environments** like:

- Your own website/server
- Pen-testing labs
- CTFs or red-team exercises

---
## IP Logger (PHP)

ip-logger.php

```bash
vim ip-logger.php
```

```php
<?php

date_default_timezone_set('Asia/Kolkata');

function getRealIpAddr() {
    if (!empty($_SERVER['HTTP_CLIENT_IP'])) {
        return $_SERVER['HTTP_CLIENT_IP'];
    } elseif (!empty($_SERVER['HTTP_X_FORWARDED_FOR'])) {
        return explode(',', $_SERVER['HTTP_X_FORWARDED_FOR'])[0];
    } else {
        return $_SERVER['REMOTE_ADDR'];
    }
}

$realIp = getRealIpAddr();
$remoteIp = $_SERVER['REMOTE_ADDR'];
$uri = $_SERVER['REQUEST_URI'] ?? 'Unknown';
$referrer = $_SERVER['HTTP_REFERER'] ?? 'Direct Access / Unknown';
$userAgent = $_SERVER['HTTP_USER_AGENT'] ?? 'Unknown';
$dateTime = date('Y-m-d H:i:s');

$logEntry = <<<LOG
<hr>
<b>Date:</b> $dateTime<br>
<b>IP (REMOTE_ADDR):</b> $remoteIp<br>
<b>IP (Detected):</b> $realIp<br>
<b>Page:</b> $uri<br>
<b>Referrer:</b> $referrer<br>
<b>User-Agent:</b> $userAgent<br>
LOG;

file_put_contents("log.html", $logEntry . PHP_EOL, FILE_APPEND | LOCK_EX);
?>
```

---
## Output Log: log.html

Logs will be written to `log.html` in this format:

log.html

```html
<hr>
<b>Date:</b> 2025-07-03 12:45:22<br>
<b>IP (REMOTE_ADDR):</b> 192.168.1.100<br>
<b>IP (Detected):</b> 192.168.1.100<br>
<b>Page:</b> /ip-logger.php<br>
<b>Referrer:</b> https://example.com<br>
<b>User-Agent:</b> Mozilla/5.0 (Windows NT 10.0; Win64; x64)...
```

---

## Recommended: Protect the Log File

.htaccess

```apache
<Files "log.html">
    Order Allow,Deny
    Deny from all
</Files>
```

---

## Notes

- For **red teaming** or **lab use only**
- Be careful with **user privacy** - logging IPs without consent may violate laws
- You can embed this in a webpage disguised as any file or shortener for bait (only in legal/controlled environments)

---
---