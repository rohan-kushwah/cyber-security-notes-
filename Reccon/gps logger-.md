# GPS IP Logger with Google Map Preview (PHP + JS)

This setup captures a visitor's **IP address**, **GPS location** (with browser permission), and **User-Agent**, then logs it to a file along with a **Google Maps preview link**.

## 🧱 File Structure

```
/gps-logger/
├── index.html
└── logger.php
```

## 📄 index.html

Requests the user's GPS location and sends it to the PHP logger:

```bash
vim index.html
```

```html
<!DOCTYPE html>
<html>
<head>
    <title>GPS IP Logger</title>
</head>
<body>
    <h2>Getting your GPS location ...</h2>
    <p id="status">Waiting for user permission ...</p>

    <script>
        if ("geolocation" in navigator) {
            navigator.geolocation.getCurrentPosition(
                function(position) {
                    const lat = position.coords.latitude;
                    const lon = position.coords.longitude;

                    // Send to logger.php
                    fetch('logger.php', {
                        method: 'POST',
                        headers: {'Content-Type': 'application/x-www-form-urlencoded'},
                        body: `lat=${lat}&lon=${lon}`
                    });

                    document.getElementById("status").innerHTML = `Location sent: ${lat}, ${lon}`;
                },
                function(error) {
                    document.getElementById("status").innerHTML = `Error: ${error.message}`;
                }
            );
        } else {
            document.getElementById("status").innerHTML = "Geolocation not supported by your browser.";
        }
    </script>
</body>
</html>
```

## 📄 logger.php

Receives the GPS coordinates, logs IP, time, and user-agent, and generates a Google Maps link.
		
```bash
vim logger.php
```

```php
<?php
date_default_timezone_set('Asia/Kolkata');

$lat = $_POST['lat'] ?? 'Not provided';
$lon = $_POST['lon'] ?? 'Not provided';
$ip = $_SERVER['REMOTE_ADDR'];
$ua = $_SERVER['HTTP_USER_AGENT'] ?? 'Unknown';
$time = date('Y-m-d H:i:s T');

$mapLink = "https://www.google.com/maps?q=$lat,$lon";

$log = <<<LOG
<hr>
<b>Time:</b> $time<br>
<b>IP:</b> $ip<br>
<b>Latitude:</b> $lat<br>
<b>Longitude:</b> $lon<br>
<b>Google Map:</b> <a href="$mapLink" target="_blank">View Location</a><br>
<b>User-Agent:</b> $ua<br>
LOG;

file_put_contents('gps-log.html', $log . PHP_EOL, FILE_APPEND | LOCK_EX);
?>
```

## 🗂️ Sample Output in gps-log.html

```html
<hr>
<b>Time:</b> 2025-07-04 17:37:02 IST<br>
<b>IP:</b> 192.168.1.213<br>
<b>Latitude:</b> 22.6957902<br>
<b>Longitude:</b> 75.8338161<br>
<b>Google Map:</b> <a href="https://www.google.com/maps?q=22.6957902,75.8338161" target="_blank">View Location</a><br>
<b>User-Agent:</b> Mozilla/5.0 (Android 12; Mobile; rv:140.0) Gecko/140.0 Firefox/140.0<br>
```

```bash
cat gps-log.html
```

---

## 🔒 Important Notes

- 📍 **GPS access requires user permission.**
- 🌐 **Must be served over HTTPS to work in most browsers.**
- 🚫 **Do not use this for unauthorized surveillance.**
- 📱 **Works best on mobile devices with GPS enabled.**

## 🔐 Optional: Protect the Log File

To restrict access to the log file:

**.htaccess**

```apache
<Files "gps-log.html">
    Order Allow,Deny
    Deny from all
</Files>
```

## 🚀 Deployment Tips

- Use a free host like **Netlify**, **Vercel**, or **[GitHub Pages (with PHP support via backend)]**
- Or run locally using PHP dev server:

```bash
php -S localhost:8000
```

**Note:** `localhost` will not trigger GPS unless you use HTTPS.

## ✅ Done!

You now have a fully working **GPS-based IP logger** with **Google Map preview** built using vanilla PHP and JavaScript.

---
---