# Katana

Katana is a modern web crawler and spidering tool developed by ProjectDiscovery. It is designed for security researchers and penetration testers, offering speed, modularity, and the ability to handle modern web applications effectively.
- Crawls the **live website** to discover URLs in real-time.
- Extracts links from:
    - HTML
    - JavaScript files (`-js`)
    - JavaScript-executed DOM content (`-jc`)
- Can reveal **hidden, dynamic, or client-side URLs**.
- Useful for **modern recon**, especially SPAs and JS-heavy sites.
- Requires more time/resources than passive tools.    
	- Finds only currently reachable URLs — not old ones.

----
## Key Features of Katana

- **High Performance:** Built for speed and efficiency, leveraging modern concurrency techniques. It can handle large-scale web applications quickly.
    
- **Modularity:** Supports multiple modules to extend functionality, including file discovery, parameter enumeration, and subdomain discovery. Users can create custom modules to meet specific requirements.
    
- **JavaScript-Aware Crawling:** Handles single-page applications (SPA) by parsing JavaScript and dynamically loaded content. It can identify endpoints and parameters hidden within JavaScript.
    
- **Integration with Other Tools:** Works seamlessly with tools like Nuclei, HTTPX, and Naabu for vulnerability detection, HTTP probing, and port scanning. Output can be piped directly into these tools for streamlined workflows.
    
- **Customizable and Scriptable:** Provides options to filter and customize the scope of crawling. Supports various output formats for easy parsing and reporting.

----
## Installation

Install Katana using Go:

```
go install github.com/projectdiscovery/katana/cmd/katana@latest
```

----
## Basic Usage

Check the available options:

```
katana -h
```

----
## Simple Crawl

To perform a basic crawl:

```
katana -u https://www.armourinfosec.com
```

Save the output to a file:

```
katana -u https://www.armourinfosec.com -o armourinfosec.com-katana-output.txt
```

----
## JavaScript Crawling

Enable JavaScript-aware crawling:

```
katana -u https://www.armourinfosec.com -jc
```

----
## Depth Limitation

Control the crawling depth using the `-d` flag:

```
katana -u https://www.armourinfosec.com -d 3
```

----
## Post-Processing Output

Remove entries containing `www.armourinfosec.com`:

```
cat armourinfosec.com-katana-output.txt | grep -v www\.armourinfosec\.com
```

List unique JavaScript files (remove query parameters, sort, and deduplicate):

```
cat armourinfosec.com-katana-output.txt | grep '\.js' | cut -d '?' -f 1 | sort | uniq
```


---
---