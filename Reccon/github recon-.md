## 🌍 GitHub Recon
---
### 🐙 GitHub
---
#### 📖 What is GitHub?
GitHub is a web-based platform for version control and collaboration, built around **Git**, a distributed version control system. GitHub hosts millions of repositories containing open-source and private code, documents, configurations, and data.

#### ⭐ GitHub Provides:
*   Code repositories
*   Issue tracking
*   Documentation (Wikis)
*   Actions for CI/CD (Automation)
*   GitHub Pages (Static website hosting)
*   Collaboration tools for developers

#### 📖 What is GitHub Recon?
---
GitHub Recon refers to gathering publicly accessible information from GitHub repositories, which may include:

*   🔑 Secrets (API keys, credentials)
*   📂 Sensitive files (config files, database dumps)
*   ☁️ Infrastructure information
*   📄 Internal documentation
*   👤 Developer info (emails, usernames)

A key part of OSINT (Open-Source Intelligence), GitHub Recon is used by:
*   ✅ Penetration testers
*   ✅ Bug bounty hunters
*   ✅ Red team operators
*   ✅ Threat hunters

### 🎯 Why GitHub Recon?
---

| 🔥 Objective                     | ✅ Benefit                              |
| -------------------------------- | -------------------------------------- |
| Find sensitive files             | Configs, credentials, backups          |
| Discover API keys and tokens     | AWS, Google, Azure, Twilio, etc.       |
| Identify internal infrastructure | Domains, IPs, databases                |
| Map developers                   | Emails, usernames, contributor details |
| Gather historical sensitive data | Even if deleted from recent commits    |

---
### 🔧 How to Perform GitHub Recon

#### 1️⃣ Define the Target

*   Organization, user, or project.

*   Example: `org:examplecorp` or `user:username`

#### 2️⃣ Use GitHub Search Dorks

**🔑 Look for Secrets and Credentials**

```
filename:.env
```

```
filename:config.json
```

```
filename:credentials
```

```
path:**/.env
```

```
path:**/config.json
```

```
path:**/credentials
```

```
"aws_access_key_id"
```

```
"aws_secret_access_key"
```

```
"secret_key"
```

```
"api_key"
```

```
"client_secret"
```

**🎯 Target Specific Users or Organizations**

```
org:examplecorp "password"
```

```
org:examplecorp filename:config
```

```
user:username path:"/config" "key"
```

**🔐 Search for Common Password Patterns**
```
"db_password"
```

```
"ftp_password"
```

```
"connectionstring"
```

```
"security_credentials"
```

**📂 File Types**
```
extension:sql mysql dump
```

```
extension:json api|forecast.io
```

```
extension:yaml mongolab.com
```

```
filename:id_rsa
```

```
filename:sshd_config
```

```
filename:wp-config.php
```

**🕓 Date Filters**

```
created:>2023-01-01
```

```
created:2020-01-01..2022-12-31
```

### 🔑 Advanced GitHub Search Syntax

| Operator     | Description                    | Example                       |
| :----------- | :----------------------------- | :---------------------------- |
| `filename:`  | Search by filename             | `filename:config.json`        |
| `path:`      | Search by path                 | `path:config`                 |
| `extension:` | Search by file extension       | `extension:sql`               |
| `org:`       | Search within an organization  | `org:google`                  |
| `user:`      | Search within a user account   | `user:admin`                  |
| `language:`  | Filter by programming language | `language:python`             |
| `in:`        | Search in specific fields      | `in:name, in:login, in:email` |
| `created:`   | Filter by creation date        | `created:>2022-01-01`         |

### 🧠 Example Dorks

| Category                 | Example Queries                                                 |
| :----------------------- | :-------------------------------------------------------------- |
| 🤫 Secrets & Credentials | `"aws_access_key_id"`, `"api_key"`, `"client_secret"`           |
| 🔑 Password Patterns     | `"db_password"`, `"ftp_password"`, `"security_credentials"`     |
| 📂 Sensitive Files       | `filename:.env`, `filename:config.json`, `filename:credentials` |
| 📄 File Extensions       | `extension:sql mysql dump`, `filename:id_rsa`                   |
| 🎯 User/Org Targeting    | `org:examplecorp "password"`, `user:username path:"/config"`    |
| 📅 Date-Based Search     | `created:>2023-01-01`                                           |

### 🛠️ Tools for GitHub Recon
---

| Tool                   | Description                              | Link                                                        |
| :--------------------- | :--------------------------------------- | :---------------------------------------------------------- |
| GitLeaks               | Scan repositories for secrets            | [GitLeaks](https://github.com/gitleaks/gitleaks)            |
| TruffleHog             | Find secrets in Git history              | [TruffleHog](https://github.com/trufflesecurity/truffleHog) |
| Gitrob                 | Recon repositories for sensitive files   | [Gitrob](https://github.com/michenriksen/gitrob)            |
| Gittyleaks             | Search for secrets in GitHub             | [Gittyleaks](https://github.com/kootenpv/gittyleaks)        |
| Githound               | Hunt for sensitive information in GitHub | [Githound](https://github.com/tillson/git-hound)            |
| Wraith                 | Scan GitHub orgs for secrets             | [Wraith](https://github.com/Nandaja/Wraith)                 |
| GitHub Dorks           | Dork collections for GitHub              | [github-dorks](https://github.com/techgaun/github-dorks)    |
| Random Robbie Keywords | Useful keyword list for searching        | [keywords](https://github.com/random-robbie/keywords)       |

### 🔗 Resources
*   🔥 [GitHub Dorks Collection](https://github.com/techgaun/github-dorks)
*   🔥 [Random Robbie's Keyword List](https://github.com/random-robbie/keywords)
*   🔥 [GitHub Recon Guide on Medium](https://infosecwriteups.com/a-guide-to-github-recon-9805459e1302)


---
----
# GitHub dorks

Find sensitive information accidentally pushed to public repos. 
These are **advanced search queries** crafted to find:
- API keys
- Passwords
- Config files
- Sensitive endpoints
- Internal docs
- Private URLs

---
### 🔍 GitHub Dorks for Recon (2025 Edition)

> Use them on:  
> [https://github.com/search](https://github.com/search)

---

#### 🔑 API Keys / Tokens

```bash
"api_key" OR "apikey" OR "api-key" language:json
```

```
"secret_key" OR "access_token" OR "auth_token"
```

```
"Authorization: Bearer" language:python
```

```
"PRIVATE_KEY" language:env
```

```
"firebaseio.com" "authDomain"
```

```
"client_secret" filename:*.json
```

---

#### 🔐 Passwords & Credentials

```bash
"password" filename:.env
```

```
"password" extension:php
```

```
"passwd" OR "pwd" OR "user" OR "login" language:yaml
```

```
"ftp" "username" "password"
```

```
"mysql_connect" OR "pg_connect"
```

```
filename:config.json password
```

---

#### ☁️ Cloud Keys (AWS, GCP, Azure)

```bash
AWS_SECRET_ACCESS_KEY
```

```
"aws_access_key_id" "aws_secret_access_key"
```

```
"accessKeyId" AND "secretAccessKey"
```

```
"google_api_key" OR "gcp_credentials"
```

```
"AZURE_STORAGE_KEY"
```

---

#### ⚙️ Config / Env Files

```bash
filename:.env
```

```
filename:.git-credentials
```

```
filename:.npmrc _auth
```

```
filename:config.yml password
```

```
filename:settings.py SECRET_KEY
```

---

#### 🧠 Tech Stack Info

```bash
filename:package.json
```

```
filename:requirements.txt
```

```
filename:composer.json
```

```
filename:pom.xml
```

---

#### 🔗 Hidden/Internal URLs

```bash
"internal-use-only" OR "do not distribute"
```

```
site:github.com inurl:"/internal"
```

```
"admin" inurl:login
```

```
"staging" OR "test" OR "dev" inurl:subdomain
```

---

#### 🛠 Hardcoded Credentials / URLs

```bash
"jdbc:mysql://"
```

```
"mongodb://"
```

```
"POSTGRES_URL"
```

```
"url" AND "token"
```

---

#### 📱 Mobile App Secrets

```bash
"key" filename:google-services.json
```

```
"client_secret" filename:*.plist
```

```
"base64" AND "secret"
```

---

### ⛏ Tips for Use:

- Use `user:orgname` to limit dorks to a company/org.
    
    ```
    "api_key" user:examplecorp
    ```
    
- Use `repo:username/repo` to target a specific project.
    
- Use filters like `created:>` or `pushed:>` to get recent leaks.
    

---

### 🚨 Tools That Automate GitHub Recon:

| Tool              | Purpose                   |
| ----------------- | ------------------------- |
| `github-dork.py`  | Automate GitHub dorking   |
| `GitHound`        | Finds secrets via dorks   |
| `TruffleHog`      | Scans repos for secrets   |
| `GHunt`           | Info from GitHub profiles |
| `repo-supervisor` | Prevent leaks in repos    |

---
---