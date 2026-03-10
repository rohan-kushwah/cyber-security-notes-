## GitHound – GitHub Recon Tool
---
**GitHound** is an advanced tool for hunting sensitive information across GitHub repositories. It searches for credentials, API keys, secrets, and sensitive files that may have been leaked in public repositories.

🔗 [GitHub Repository](https://github.com/tillson/git-hound)
🔗 [Latest Release v1.7.2](https://github.com/tillson/git-hound/releases/tag/v1.7.2)

---
### 🛠️ Installation
---

**1️⃣ Download GitHound Binary**

```bash
wget https://github.com/tillson/git-hound/releases/download/v1.7.2/git-hound_1.7.2_linux_amd64.tar.gz
```

**2️⃣ Extract the Archive**

```bash
tar -xvf git-hound_1.7.2_linux_amd64.tar.gz
```

**3️⃣ Move Binary to System Path**

```bash
cp -v git-hound /usr/local/bin/
```

### ✅ Basic Usage
---
**Run GitHound:**

```bash
git-hound
```

**Show Help:**

```bash
git-hound -h
```

### ⚙️ Configure GitHound
---
**Download Example Configuration:**

```bash
wget https://raw.githubusercontent.com/tillson/git-hound/main/config.example.yml -O config.yml
```

**🔧 `config.yml` Includes:**
*   Whitelist keywords to reduce false positives
*   Blacklist domains (exclude your own company's domains)
*   Regex patterns for secret detection

Edit `config.yml` to fine-tune searches for your needs.

### 🔍 Example Usage
---
**Search for leaks related to an organization:**

```bash
git-hound --subdomain example.com
```

**Search using keywords:**

```bash
git-hound --dig-keyword "aws_access_key_id"
```

**Combine with config:**

```bash
git-hound --config config.yml --dig-keyword "api_key"
```

### 🔥 Features
---
*   Regex-based secret searching
*   Keyword searching
*   Domain-specific hunting (`--subdomain`)
*   Supports whitelisting/blacklisting
*   Fast and efficient scanning
---
---