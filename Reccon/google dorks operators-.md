## Google Dorks – Exploit Search Techniques
---
The following are powerful Google Dork queries used to locate exposed directories, files, services, credentials, subdomains, and more. These are typically used for ethical hacking, OSINT, and reconnaissance.

Source: [Exploit-DB Google Hacking Database](https://www.exploit-db.com/google-hacking-database)

### Exposed Credentials and Secrets
---
Searches for `.env` files that often contain secrets like API keys or database credentials.
```
inurl:.env DB_PASSWORD
```

Locates JSON files with possible API keys.
```
filetype:json intext:api_key
```

Searches for `.ini` files containing DB configurations.
```
filetype:ini intext:db_user
```

Targets YAML config files with credentials.
```
filetype:yaml intext:password
```

Searches for AWS keys in source code.
```
intext:"AWS_SECRET_ACCESS_KEY"
```

### Technology & Framework Detection
---
Finds WordPress admin panels.
```
inurl:wp-admin
```

Searches for exposed Laravel debug pages.
```
inurl:/_ignition/ intitle:"Laravel"
```

Finds Django error pages which can leak debug info.
```
intitle:"OperationalError at" inurl:/admin/
```

Locates Magento admin login pages.
```
inurl:admin/login site:.*.* -demo
```

Targets exposed `.git` folders.
```
intitle:"index of /.git"
```

### Login Panels & Access Portals
---
General login portals:
```
intitle:"Login Page" inurl:login
```

Admin login pages:
```
intitle:"admin login"
```

CPanel login panels:
```
inurl:cpanel filetype:php
```

Remote desktop logins:
```
intitle:"Remote Web Workplace"
```

Router/web UI logins:
```
intitle:"Router setup" OR intitle:"web interface login"
```

### Misconfigurations & Debug Info
---
PHP info pages:
```
inurl:phpinfo.php
```

Apache default pages:
```
intitle:"Apache2 Ubuntu Default Page"
```

Jenkins dashboard:
```
intitle:"Dashboard [Jenkins]"
```

Kibana dashboard exposed:
```
intitle:"Kibana" inurl:/app/kibana
```

Exposed .DS_Store files:
```
intitle:"Index of" ".DS_Store"
```

### Log, Backup, and Database Files
---
Searches for log files with debug output:
```
filetype:log intext:"Exception"
```

Finds SQL backups:
```
filetype:sql "dump" OR "backup"
```

Locates .bak database backups:
```
filetype:bak intext:"CREATE TABLE"
```

ZIP and RAR archives labeled as config or backup:
```
filetype:zip OR filetype:rar "config" OR "backup"
```

### Error Messages and Stack Traces
---
Java exceptions:
```
intext:"java.lang.NullPointerException"
```

PHP errors:
```
intext:"Fatal error" filetype:php
```

MySQL errors:
```
intext:"You have an error in your SQL syntax"
```

ASP.NET stack trace:
```
intext:"System.Web.HttpException" filetype:aspx
```

### Publicly Accessible Services and Devices
---
Open webcams:
```
inurl:view/index.shtml
```

VNC portals:
```
intitle:"VNC viewer for Java"
```

JIRA dashboards:
```
intitle:"Dashboard - JIRA"
```

GitLab web interface:
```
intitle:"Sign in . GitLab"
```

### Source Code Exposure
---
Exposed .env:
```
inurl:.env intext:"DB_PASSWORD"
```

Exposed config.php:
```
inurl:config.php intext:mysql_connect
```

Source code files:
```
filetype:php OR filetype:asp OR filetype:js "password"
```

GitHub gists with passwords:
```
site:github.com intext:password
```

### OSINT & Recon
---
Company email leaks:
```
intext:"@company.com" filetype:xls OR filetype:csv
```

Phone numbers & contact info:
```
intext:"contact" AND "phone" site:example.com
```

Job portals with internal tools or tech stack:
```
site:careers.example.com intext:"developer"
```

### More Examples
---
Searches for open directories containing DCIM folders (common in cameras/mobile storage).
```
index.of.dcim
```

Targets open directories potentially containing files or folders with the term "password" in their names.
```
index.of.password
```

Looks for for open directories labeled as "backup," often used for stored or archived data.
```
index.of.backup
```

Searches for open directories named "private," potentially indicating sensitive or restricted content.
```
index.of.private
```

Combines open directory searching with the filetype operator to locate SQL database backup files in directories labeled as "backup."
```
index.of.backup filetype:sql
```

Specifically looks for `.zip` files in open directories labeled as "backup," often containing compressed data.
```
index.of.backup.zip
```

Targets open directories that specifically mention "DCIM" in their structure.
```
intitle:"index of" "parent directory" "DCIM"
```

Searches for directories with "confidential" in their structure, potentially indicating sensitive data.
```
intitle:"index of" "parent directory" "confidential"
```

Looks for open directories containing log files.
```
intitle:"index of" "parent directory" "logs"
```

Focuses on open directories meant for downloadable files.
```
intitle:"index of" "parent directory" "downloads"
```

Searches for SQL backup files in open directories.
```
intitle:"index of" "parent directory" filetype:sql "backup"
```

Locates Excel or CSV files with the term "contacts," often containing sensitive information.
```
intitle:"index of" filetype:xlsx OR filetype:csv "contacts"
```

Finds PDF files labeled as "invoice" in open directories.
```
intitle:"index of" filetype:pdf "invoice"
```

Searches for plaintext files potentially containing passwords.
```
intitle:"index of" filetype:txt "password"
```

Targets compressed backup files in `.zip` or `.tar` formats.
```
intitle:"index of" "backup" filetype:zip OR filetype:tar
```

Looks for `.bak` files, commonly used as database backups.
```
intitle:"index of" "backup" filetype:bak
```

Finds `.gz` or `.7z` compressed backup files.
```
intitle:"index of" "backup" (filetype:gz OR filetype:7z)
```

Looks for `.env` files, which often contain application secrets like API keys and database credentials.
```
intitle:"index of" filetype:env
```

Searches for configuration files mentioning databases.
```
intitle:"index of" filetype:conf "database"
```

Finds `.json` files potentially containing configuration data.
```
intitle:"index of" filetype:json "config"
```

Searches for video files in open directories.
```
intitle:"index of" filetype:mp4 OR filetype:avi
```

Locates Word or PowerPoint files labeled as "proposal."
```
intitle:"index of" filetype:docx OR filetype:pptx "proposal"
```

Finds `.log` files, often containing debugging or server information.
```
intitle:"index of" filetype:log
```

Locates `.ini` files, used for configurations.
```
intitle:"index of" filetype:ini
```

Searches for YAML configuration files.
```
intitle:"index of" filetype:yaml OR filetype:yml
```

Finds shell script files in open directories.
```
intitle:"index of" filetype:sh "script"
```

Searches for `.txt` files that may contain leaked usernames and passwords for Gmail accounts.
```
filetype:txt username password @gmail.com
```

Targets `.txt` files with potential login credentials for Hotmail users.
```
filetype:txt username password @hotmail.com
```

Finds leaked Yahoo email account details stored in `.txt` files.
```
filetype:txt username password @yahoo.com
```

Locates plaintext credentials associated with Ymail accounts.
```
filetype:txt username password @ymail.com
```

Searches for Rediffmail user credentials in `.txt` format.
```
filetype:txt username password @rediffmail.com
```

Targets Facebook-associated accounts listed in `.txt` dumps.
```
filetype:txt username password @facebook.com
```

Searches for Gmail credentials in Excel spreadsheets.
```
filetype:xls username password @gmail.com
```

Locates Excel files with Hotmail account credentials.
```
filetype:xls username password @hotmail.com
```

Finds Yahoo login credentials inside `.xls` spreadsheets.
```
filetype:xls username password @yahoo.com
```

Finds Ymail credentials in `.xls` format.
```
filetype:xls username password @ymail.com
```

Locates Rediffmail credentials in spreadsheet format.
```
filetype:xls username password @rediffmail.com
```

Targets Facebook-related credentials saved in Excel files.
```
filetype:xls username password @facebook.com
```

Searches for web-accessible printer interfaces with `mainframe.cgi`.
```
inurl:webarch/mainframe.cgi
```

Finds status pages of UPS (Uninterruptible Power Supply) systems.
```
intitle:"multimon ups status page"
```

Searches for management interfaces of SpeedStream routers.
```
intitle:"SpeedStream Router Management Interface"
```

Finds browser-accessible VNC remote desktop sessions using Java.
```
intitle:VNC Viewer From Java
```

Alternate syntax to locate VNC viewer Java applets.
```
intitle:"VNC viewer for Java"
```

Locates web interfaces for configuration of Sipura (VoIP) SPA devices.
```
intitle:"Sipura.SPA.Configuration"
```

Uses `site:` with exclusions to help uncover hidden or lesser-known Apple subdomains (removing known common ones).
```
site:apple.com -www -discussionschinese -podcasts -trailers -music -tv -machinelearning -books -support -discussions -searchads -consultants -developer -discussionskorea -testflight -apps
```

Alternative subdomain enumeration by excluding different popular services and regions.
```
site:apple.com -www -business -machinelearning -podcasters -support -stocks -ads -searchads -music -tv -discussionschinese -discussionsjapan -discussionskorea -testflight
```
---
---