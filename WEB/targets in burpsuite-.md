# Target Tab in Burp Suite

---

The **Target** tab is a foundational component of Burp Suite used to organize, explore, and define the scope of your web application testing. It provides visibility into all discovered content, allows precise control over which assets are in scope, and helps manage testing boundaries effectively.

  

## 🌐 Site Map

---

The Site Map provides a hierarchical view of all URLs that Burp Suite has interacted with, either through:

  

*   Manual browsing with the proxy enabled

*   Passive scanning or automated crawling tasks

  

**Features:**

*   Displays:

    *   Hosts, folders, and individual requests

    *   Methods (GET, POST, etc.), response codes, and MIME types

*   Helps visualize the full application structure

*   Useful for:

    *   Identifying unlinked endpoints

    *   Detecting duplicate paths

    *   Navigating quickly to interesting areas

  

**🧪 Filters:**

You can apply filters to display only relevant requests based on:

  

*   Status code (e.g., 200, 404, 500)

*   MIME type (e.g., HTML, JSON, images)

*   Request type (e.g., GET, POST)

*   Search terms or regex

  

**Example:**

```

Filter to only show JavaScript files:

MIME type = application/javascript

```

  

## 🎯 Scope

---

The **Scope** defines the set of URLs and content that Burp considers 'in-scope' for scanning and analysis. Burp restricts automated tools like the Scanner or Intruder to operate only on in-scope targets, unless explicitly configured otherwise.

  

### 🔧 Adding Targets to Scope

You can add targets by:

  

- **Exact URLs**

  

```

https://www.google.com/

```

  

```

https://www.google.co.in/maps

```

  

- **Wildcards**

```

https://*.google.com/

```

  

- **Regular expressions**

```regex

/*\.google\.com$

```

---

### Load Settings Using JSON in `Load Project Options`

  

Burp Suite uses a structured JSON format for importing project options (including scope definitions). While the GUI allows hostname wildcards like *.google.com, the project options file requires explicit fields:

  

**Example: Defining a Scope with a Wildcard Host**

  

```json

{

    "target": {

        "scope": {

            "include": [

                {

                    "enabled": true,

                    "host": "^192\\.168\\.1\\.29$",

                    "port": "^3000$",

                    "protocol": "http"

                },

                {

                    "enabled": true,

                    "host": "^192\\.168\\.1\\.29$",

                    "port": "^3000$",

                    "protocol": "https"

                }

            ],

            "exclude": []

        }

    },

    "proxy": {

        "history": {

            "hide_uninteresting": [

                "css",

                "image",

                "font"

            ]

        }

    }

}

```

  

| Field    | Purpose                                  |

| -------- | ---------------------------------------- |

| protocol | Usually http or https                    |

| host     | Hostname — use *.domain.com if useRegex  |

| file     | Path filter — regex accepted if useRegex |

| useRegex | Set true to allow regex in host and file |

  

In Burp Suite:

1. Go to Project options → Import project options

2. Select your custom JSON config

3. Burp will load the defined scope, match patterns, etc.

  

---

### Header Matching Examples

You can match headers using regex or literal patterns. Useful in advanced scope configurations:

  

```

^User-Agent.*$

User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 5_1 like Mac OS X)...

  

^Host: foo.example.org$

Host: bar.example.org

```

  

### 📁 Scope Management

You can export and import scope settings via:

```

Project > Project Options > Load Project Options

```

  

This allows you to reuse or share consistent scope definitions across multiple projects.

  

---

### ▲ Issue Definitions

***

Burp Suite maintains a built-in set of **Issue Definitions** that categorize and explain vulnerabilities detected during scans.

  

**📋 Purpose:**

*   Define the **name**, **description**, and **severity** of known issues

*   Help automate reporting by mapping findings to known categories

*   Standardize vulnerability labels across team members

  

**🧠 Example Categories:**

*   SQL Injection

*   Cross-Site Scripting (XSS)

*   Insecure Direct Object References (IDOR)

*   Authentication Bypass

*   Security Misconfiguration

*   Information Disclosure

  

Each definition includes:

*   A description of the vulnerability

*   Remediation advice

*   Severity and confidence levels

*   CWE and OWASP mappings (if available)

  

---

---