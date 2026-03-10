#  LeakIX Recon

LeakIX is an open-source intelligence (OSINT) engine for discovering exposed services, credentials, and data leaks across the internet.
[Visit LeakIX](https://leakix.net)

#### What Is LeakIX?

LeakIX is similar to Shodan or Censys but focuses more on:
- Exposed credentials
- Open directories
- Misconfigured applications
- Leaky databases
- Files with secrets (env, config, backup, etc.)

#### Key Features

| Feature                | Description                                         |
|------------------------|-----------------------------------------------------|
| **Full-text Search**   | Search by IP, domain, keywords, banners             |
| **Leak Detection**     | Finds `.env`, `.git`, backups, admin panels, etc.   |
| **Database Leak Reports** | MongoDB, Elasticsearch, Redis, etc.             |
| **Fingerprinting**     | Shows services (HTTP, SSH, DB) and software versions|
| **Public & Private Scans** | Anonymous search + authenticated deeper scan  |

#### Basic Usage

- Go to: [LeakIX](https://leakix.net)
- Use the search bar to query:

**Examples:**

- `.env`
- `index of /backup`
- `mysql password`
- `site:.gov`
- `product:elasticsearch`

#### Common Search Queries

| Query                   | Purpose                                |
| ----------------------- | -------------------------------------- |
| `.env`                  | Find leaked environment variables      |
| `site:*.gov`            | Filter to government sites             |
| `product:elasticsearch` | Find exposed databases                 |
| `index of /backup`      | Open directory listing backup files    |
| `password`              | Search for passwords in HTTP responses |
| `config.json`           | Look for config file exposures         |
| `mongo / redis`         | Unsecured databases                    |
### Examples

- `.env AND password`
- `product:elasticsearch country:IN`
- `site:*.edu filetype:bak`

### Authenticated Scanning (Optional)

You can register and get access to:
- API access (in beta)
- Submit your own scans
- Monitor exposure over time

### Legal Reminder

Always ensure you have authorization before investigating or testing any system. LeakIX is meant for defensive and research purposes only.

----
----