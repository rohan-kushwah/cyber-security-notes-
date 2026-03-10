

**Waybackurls** is a powerful tool by **Tomnomnom** for retrieving archived URLs from the Wayback Machine. It is particularly useful for security assessments, as it helps uncover endpoints that may no longer be accessible on the live site but are still archived.

- Pulls **historical URLs** from the **Wayback Machine (archive.org)**.
- Gets URLs that were **publicly accessible in the past**, even if deleted now.
- No live crawling — works purely from archived data.
- Cannot detect dynamic or JavaScript-generated URLs.
- Might give **dead or outdated URLs**.
- Useful for **OSINT**, bug bounty, and discovering forgotten endpoints.    
- Very fast and low-resource.

---
## Installation

1. **Ensure Go is installed:**

```bash
apt install golang
```

2. **Install Waybackurls using Go:**

```bash
go install github.com/tomnomnom/waybackurls@latest
```

3. **Move the binary to a location in your system's PATH:**

```bash
cp -v /root/go/bin/waybackurls /usr/local/bin/
```

---
## Basic Usage

Check available options:

```bash
waybackurls -h
```

---
## Retrieve URLs for a Single Domain

```bash
waybackurls armourinfosec.com
```

Or using the full URL:

```bash
waybackurls https://www.armourinfosec.com
```

---
## Using a File with Multiple Domains

Create a file, e.g. `domains.txt`:

```bash
vim domains.txt
```

Contents:

```
armourinfosec.com
thehackersworld.com
```

Then run:

```bash
cat domains.txt | waybackurls
```

---
## Output to a File

```bash
waybackurls https://www.tesla.com > /tmp/tesla-urls.txt
```

---