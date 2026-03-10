## Burp Suite

  

**Burp Suite** is a powerful integrated platform used primarily for **web application penetration testing**. It's developed by **PortSwigger** and offers a range of tools to help security professionals identify and exploit vulnerabilities in web applications.

  

### What Is Burp Suite?

  

Burp Suite acts as an **intercepting proxy** that sits between your browser and the target web application. It allows you to:  

* Inspect and modify HTTP(S) traffic on the fly  

* Automate scans for common web vulnerabilities  

* Manually test for complex security issues  

* Reproduce and refine attacks like SQLi, XSS, CSRF, etc.  

  

### Core Tools in Burp Suite

  

| Tool         | Functionality                                                |

|--------------|--------------------------------------------------------------|

| **Proxy**    | Intercepts traffic between browser and server                |

| **Intruder** | Automates customized attacks (fuzzing, brute force, etc.)    |

| **Repeater** | Manually modify and resend HTTP requests                      |

| **Scanner**  | Automatically scans for security issues (Pro version)        |

| **Decoder**  | Encodes/decodes and transforms data (Base64, URL, hex, etc.) |

| **Comparer** | Diffs between requests or responses                           |

| **Extender** | Adds plugins or extensions (via BApp Store or custom code)   |

  

### Typical Workflow

  

1. Set browser proxy to **127.0.0.1:8080**  

2. Start Burp Suite and **enable the proxy**  

3. Intercept requests from the browser (e.g., visiting `http://example.com`)  

4. Use **Repeater** to modify and replay interesting requests  

5. Use **Intruder** to automate payload testing  

6. Review results and look for security issues  

  

### HTTPS Interception

  

To intercept HTTPS:  

* Install **Burp's CA certificate** into your browser or system (as in your earlier notes)  

* This allows Burp to decrypt and view HTTPS traffic without browser warnings  

  

### Use Cases

  

* Testing login forms  

* Discovering hidden parameters  

* Exploiting IDOR, SSRF, and other logic flaws  

* Performing fuzzing and brute-force attacks  

* Auditing API endpoints  

  

### Burp Suite Versions

  

| Edition       | Features                                          |

|---------------|--------------------------------------------------|

| **Community** | Free, limited features                            |

| **Professional** | Paid, includes scanner and automation tools    |

| **Enterprise**  | Designed for large-scale automated scanning     |

  

### Burp Suite Versions – Comparison

  

| Feature/Capability                    | Community             | Professional             | Enterprise                          |

|-------------------------------------|-----------------------|--------------------------|-----------------------------------|

| **Price**                           | Free                  | Paid (per user)          | Paid (per site, scalable)          |

| **Intercepting Proxy**              | ✅                    | ✅                      | ✅                                |

| **Manual Testing Tools (Repeater, Decoder)** | ✅          | ✅                      | ✅                                |

| **Extensibility (BApp Store)**     | ✅                    | ✅                      | ✅                                |

| **Web Vulnerability Scanner**      | ❌                    | ✅                      | ✅                                |

| **Advanced Manual Tools (e.g., Intruder)** | Limited          | ✅ (full capabilities)   | ✅                                |

| **Automation / Scheduling**         | ❌                    | ❌                      | ✅ (CI/CD integration, scheduling)|

| **CI/CD Integration**               | ❌                    | ❌                      | ✅                                |

| **Team Collaboration**             | ❌                    | ❌                      | ✅ (dashboard, results sharing, etc.) |

| **Support & Updates**               | Community only        | Premium Support         | Premium Support                   |

| **Use Case**                       | Learning, hobbyists   | Professional pen testers | Enterprises with large app portfolios |

  

### Chrome Browser Setup

  

Navigate to:  

```

chrome://settings/security

```

  

- **Always use secure connections → Turn OFF**

  

### How HTTPS Works

  

HTTPS relies on **TLS (Transport Layer Security)** using **asymmetric encryption**:

  

| Key Type      | Description                                                                |

|---------------|----------------------------------------------------------------------------|

| **Private Key** | Stored securely on the web server. Decrypts data encrypted with public key. |

| **Public Key**  | Shared with clients. Used to encrypt data sent to the server.             |

  

---

  

## Installing Burp CA Certificate on Kali Linux

  

#### Step 1: Intercept in Burp  

  

- Open browser with **Burp proxy enabled**  

- Visit: [http://burp](http://burp)  

- Click on **"CA Certificate"** to download `cacert.der`  

  

#### Step 2: Use `curl` With Proxy

  

```bash

curl https://www.google.com --proxy 127.0.0.1:8080

```

  

#### Step 3: Convert DER to CRT

  

```bash

openssl x509 -in cacert.der -inform DER -out cacert.crt

```

  

#### Step 4: Add to System Store

  

```bash

cp -v cacert.crt /usr/local/share/ca-certificates/

```

  

```bash

update-ca-certificates

```

  

#### Step 5: Verify with `curl`

  

```bash

curl https://www.google.com --proxy 127.0.0.1:8080 --cacert /path/to/cacert.crt

```

  

#### Step 6: Using With Tools (e.g., dirsearch)

  

```bash

dirsearch -u https://www.google.com --proxy=127.0.0.1:8080

```

  

#### Step 7: Remove Certificate (Cleanup)

  

```bash

rm -fv /usr/local/share/ca-certificates/cacert.crt

```

  

```bash

update-ca-certificates --fresh

```

  

### Burp Suite Runtime Configuration

  

**File:** `/opt/BurpSuitePro/BurpSuitePro.vmoptions`

  

Open the file with:

  

```bash

vim /opt/BurpSuitePro/BurpSuitePro.vmoptions

```

  

Add or edit the following lines:

  

```vmoptions

-Xmx16000m

-XX:MaxRAMPercentage=50

-include-options settings.vmoptions

-include-options user.vmoptions

```

  

- `-Xmx16000m` : Allocates max 16 GB RAM  

- `-XX:MaxRAMPercentage=50` : Caps Burp's RAM usage to 50% of total available  

- `-include-options` : Merges other config files  

  

Reference: [Burp Suite Runtime Configs](https://portswigger.net/burp/documentation/desktop/settings/runtime-config-options)

  

### Additional Resources

  

- Burp Intruder Positions:  

  [https://portswigger.net/burp/documentation/desktop/tools/intruder/positions](https://portswigger.net/burp/documentation/desktop/tools/intruder/positions)

  

- Blog:  

  [Adding Burp CA Cert to Kali Linux](https://portswigger.net/burp/documentation/desktop/tools/intruder/positions)

  

---

# Scans

  

Burp Suite provides **three types of scans**, each with distinct behavior.

  

## Types of Scans In Burp Suite

  

Burp Suite offers several scanning modes tailored to different phases of web application security testing.

  

### 1. Crawl and Audit

  

- Combines both crawling and active auditing in a single operation.

- Burp will:

    - Automatically **discover** endpoints and links

    - Perform **vulnerability checks** (e.g., XSS, SQLi) on discovered content

- Useful for automated end-to-end testing.

  

### 2. Crawl Only

  

- Focuses purely on **discovering content** without active vulnerability analysis.

- Recursively follows all links and parameters.

- Good for **mapping attack surface** before manual or selective auditing.

  

### 3. API-Only Scan

  

- Designed specifically for **REST APIs, OpenAPI, GraphQL**, etc.

- Allows you to:

    - Import API definitions (e.g., Swagger, Postman collections)

    - Test endpoints without requiring UI interaction

- Ideal for **headless apps** or **mobile backends**.

  

## 4. Live Tasks

  

Live tasks operate in the background and continuously analyze traffic **as you browse**.

  

### Live Audit

  

- Actively audits requests/responses while you explore the application manually.

- Ideal for **real-time vulnerability discovery**.

  

### Live Passive Crawl

  

- Only passively observes and maps new endpoints.

- No active probing or payloads—**stealthier**.

  

## Event Log

  

- Shows real-time events and logs within Burp Suite.

- Includes:

    - Errors

    - Informational messages

    - Output from custom extensions

- **Tip:** Check here if you're having issues intercepting or decoding traffic.

  

---

---