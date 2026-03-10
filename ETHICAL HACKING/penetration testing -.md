# 🛡️ Penetration Testing Types (Easy Guide)

Penetration testing (or "pen testing") means testing your systems like a hacker would — to find and fix weak points before real hackers can exploit them.

---

## 🔌 1. Network Penetration Testing
**What it tests:** Computers, servers, and devices connected in a network.  
**Goal:** Find open doors (called "ports") or weak settings that hackers could use.  
**Risks:** Weak passwords, old software, unsafe services.  
**Common Tools:** Nmap, Nessus, Metasploit.

🧠 **Definitions:**
- **Port:** A virtual door on a computer where data comes in or out.
- **Service:** A program running in the background like email or file sharing.

---

## 🌐 2. Web Application Penetration Testing
**What it tests:** Websites or apps you access through a browser.  
**Goal:** Find bugs in login pages, forms, and how data is handled.  
**Risks:** Hackers stealing data or logging in without permission.  
**Common Tools:** Burp Suite, OWASP ZAP.

🧠 **Definitions:**
- **SQL Injection (SQLi):** Trick that lets hackers mess with a database.
- **XSS (Cross-Site Scripting):** Lets hackers run bad scripts on websites.
- **CSRF:** Tricks a user into doing something they didn’t mean to.

---

## 📶 3. Wireless Penetration Testing
**What it tests:** Wi-Fi networks.  
**Goal:** Check how easy it is to break into the wireless network.  
**Risks:** Hackers spying on traffic or pretending to be your Wi-Fi.  
**Common Tools:** Aircrack-ng, Kismet, Wireshark.

🧠 **Definitions:**
- **Access Point (AP):** The device that gives you Wi-Fi.
- **MITM (Man-in-the-Middle):** When a hacker secretly watches or changes data between two people.

---

## 🤖 4. IoT (Internet of Things) Penetration Testing
**What it tests:** Smart devices like cameras, doorbells, and sensors.  
**Goal:** Check for weak passwords or poor coding.  
**Risks:** Hackers controlling your devices.  
**Common Tools:** Shodan, Binwalk.

🧠 **Definitions:**
- **Firmware:** Software inside smart devices.
- **Default credentials:** Factory-set username and password (like "admin/admin").

---

## 🔗 5. API Penetration Testing
**What it tests:** APIs — the bridges that connect different software together.  
**Goal:** Make sure users can only access their own data.  
**Risks:** Stealing private info or breaking the app.  
**Common Tools:** Postman, Burp Suite.

🧠 **Definitions:**
- **API (Application Programming Interface):** Lets two programs talk to each other.
- **IDOR (Insecure Direct Object Reference):** When a user can access another user's info just by changing a number in the link.

---

## 🏢 6. Physical Security Testing
**What it tests:** Your building or office's physical security.  
**Goal:** See if someone can get inside without permission.  
**Risks:** A person stealing devices or plugging in bad USBs.  
**Common Tools:** RFID cloners, USB drop devices.

🧠 **Definitions:**
- **Tailgating:** When someone follows you into a secure area without a badge.
- **USB drop:** A hacker leaves a USB stick hoping someone plugs it in.

---

## 📱 7. Mobile Application Testing
**What it tests:** Apps on phones and tablets.  
**Goal:** Make sure data is safe and not stored in the wrong place.  
**Risks:** Hackers stealing data or spying on users.  
**Common Tools:** MobSF, Frida.

🧠 **Definitions:**
- **APK:** The installable file for Android apps.
- **Hardcoded secrets:** Passwords or keys written into the app code.

---

## 💻 8. Desktop Application Testing
**What it tests:** Programs you install on computers.  
**Goal:** Find bugs that let hackers change files or break the app.  
**Risks:** Hackers taking over your computer or stealing local data.  
**Common Tools:** IDA Pro, x64dbg.

🧠 **Definitions:**
- **Buffer overflow:** A bug that lets hackers mess with memory.
- **DLL Injection:** A way to make a program do something it shouldn’t.

---

## 📊 Summary Table

| Type                  | What it Tests            | Risks                               |
|-----------------------|---------------------------|--------------------------------------|
| Network               | Devices and servers       | Weak passwords, open ports          |
| Web App               | Websites and forms        | Data leaks, login bypass            |
| Wireless              | Wi-Fi networks            | Hacked Wi-Fi, spying                |
| IoT                   | Smart gadgets             | Hijacked devices                    |
| API                   | Software connectors       | Data theft, broken permissions      |
| Physical Security     | Offices and doors         | Unauthorized entry, USB traps       |
| Mobile App            | Phone apps                | Insecure storage, leaked info       |
| Desktop App           | PC software               | Local attacks, memory bugs          |
