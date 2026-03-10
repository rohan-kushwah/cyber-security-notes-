# 🧃 OWASP Juice Shop

  

## 🚩 What is OWASP Juice Shop?

OWASP Juice Shop is a deliberately insecure web application developed by the OWASP community. It is designed for:

* ✔ Security testing

* ✔ Vulnerability discovery

* ✔ Ethical hacking practice

* ✔ Capture the Flag (CTF) exercises

* ✔ Security training for developers and testers

  

Juice Shop mimics a real-world e-commerce website (an online juice store) but is intentionally riddled with vulnerabilities of varying difficulty.

  

## 🔥 Key Features

* ✔ Covers the OWASP Top 10 and beyond.

* ✔ Over **100+** security challenges – beginner to advanced.

* ✔ Includes a **built-in Capture The Flag (CTF) mode**.

* ✔ Open-source and actively maintained by the OWASP community.

* ✔ Available via **local install, Docker, cloud deployment, or online demo**.

* ✔ Suitable for **individual learning, classroom training, and professional workshops**.

  

## 🛠 Common Vulnerabilities

* Injection Attacks (SQLi, NoSQLi, Command Injection)

* Broken Authentication

* Cross-Site Scripting (XSS)

* Cross-Site Request Forgery (CSRF)

* Security Misconfiguration

* Broken Access Control

* Sensitive Data Exposure

* Insecure Deserialization

* Server-Side Request Forgery (SSRF)

* Cross-Origin Resource Sharing (CORS) Misconfigurations

* Weak Session Management

  

## 🗓 Usage Scenarios

  

| Scenario                       | ✔ Purpose                                           |

| :----------------------------- | :-------------------------------------------------- |

| Penetration Testing Practice   | Practice exploiting vulnerabilities safely          |

| Security Awareness Training    | Educate developers and testers about risks          |

| CTF Competitions               | Built-in scoreboard and gamified challenges         |

| Tool Testing                   | Test with Burp Suite, OWASP ZAP, etc.               |

| DevSecOps Training             | Integrate security into CI/CD pipelines             |

  

## 🌎 Deployment Options

*   **Run Locally:** On Linux, Windows, or macOS.

*   **Docker:**

    ```bash

    docker run --rm -p 3000:3000 bkimminich/juice-shop

    ```

*   **Cloud:** Deploy on AWS, Azure, GCP, Heroku, etc.

*   **CTF Mode:** Enable via `/ctf/key` route for gamified experiences.

  

## 🔗 Official Resources

*   🌐 [OWASP Juice Shop Website](https://owasp-juice.shop/)

*   💾 [GitHub Repository](https://github.com/bkimminich/juice-shop)

*   📚 [Official Documentation](https://pwning.owasp-juice.shop/)

*   ▶️ [Live Demo (Public Sandbox)](https://demo.owasp-juice.shop/)

  

## ⚠️ Important Note

> OWASP Juice Shop is intended for **legal and ethical use only**.

> Always use it in controlled environments – such as your local machine, private labs, or cloud servers where you have explicit permission.



# 
# OWASP Juice Shop Installation

---

## 1. Prerequisites

  

*   **Official Website:**

    https://owasp.org/www-project-juice-shop/

*   **GitHub Repository:**

    https://github.com/juice-shop/juice-shop

*   **Node.js (Version 22.x Recommended):**

    https://nodejs.org/en/download/package-manager/

*   **Juice Shop Releases:**

    https://github.com/juice-shop/juice-shop/releases

  

## 2. System Update and Setup

  

Update the package list and upgrade existing packages:

```bash

apt update

apt upgrade

```

  

Install `curl` if not installed:

```bash

apt install curl

```

  

Edit the hostname (optional):

```bash

vim /etc/hostname

```

  

Reboot the system to apply changes:

```bash

reboot

```

  

## 3. Install Node.js (Version 22.x)

  

Ensure compatibility with Node.js version for Juice Shop.

Visit: [Node.js Version Compatibility](https://github.com/juice-shop/juice-shop#node-version-compatibility)

  

Set up the Node.js repository:

```bash

curl -fsSL https://deb.nodesource.com/setup_22.x | bash -

apt install -y nodejs

```

  

Verify Node.js and npm installation:

```bash

node -v

npm -v

```

  

Juice Shop releases page:

[Juice Shop Releases](https://github.com/juice-shop/juice-shop/releases)

  

Download the appropriate Juice Shop release (version 18.0.0 for Node 22.x):

```bash

wget https://github.com/juice-shop/juice-shop/releases/download/v18.0.0/juice-shop-18.0.0_node22_linux_x64.tgz

```

  

Verify the integrity of the downloaded file:

```bash

sha256sum juice-shop-18.0.0_node22_linux_x64.tgz

```

  

## 4. Extract and Move Juice Shop Files

  

Extract the downloaded Juice Shop archive:

```bash

tar -xvf juice-shop-18.0.0_node22_linux_x64.tgz

```

  

Move the extracted Juice Shop directory to `/opt`:

```bash

mv -v juice-shop-18.0.0 /opt/

```

  

## 5. Start Juice Shop

  

Change to the Juice Shop directory:

```bash

cd /opt/juice-shop_18.0.0

```

  

Start Juice Shop:

```bash

npm start

```

  

Access Juice Shop via the web browser:

```http

http://192.168.1.23:3000/#/

```

  

## 6. (Optional) Create a Start Script

  

Create the script:

```bash

vim /usr/local/bin/juice-shop-start.sh

```

  

Add:

```bash

#!/bin/bash

cd /opt/juice-shop_18.0.0

npm start

```

  

Make it executable:

```bash

chmod +x /usr/local/bin/juice-shop-start.sh

```

  

Run anytime with:

```bash

juice-shop-start.sh

```

  

## 7. (Optional) Create a Systemd Service

  

Create a service file:

```bash

vim /etc/systemd/system/juice-shop.service

```

  

Add the following:

```ini

[Unit]

Description=OWASP Juice Shop

After=network.target

  

[Service]

Type=simple

ExecStart=/usr/bin/npm start

WorkingDirectory=/opt/juice-shop_18.0.0

Restart=always

User=root

  

[Install]

WantedBy=multi-user.target

```

  

Reload and enable the service:

```bash

systemctl daemon-reload

```

  

```bash

systemctl enable juice-shop

```

  

```bash

systemctl start juice-shop

```

  

Check the service status:

```bash

systemctl status juice-shop

```

  

## 8. Default Login Information

  

> **Note**: Juice Shop does not have default credentials. Users typically register manually or exploit authentication vulnerabilities intentionally built into the application for practice.

  

## Important Reminder

  

> **OWASP Juice Shop is for legal, educational, and ethical hacking purposes only.**

> Use it only in controlled environments where you have full permission.

  

----

---