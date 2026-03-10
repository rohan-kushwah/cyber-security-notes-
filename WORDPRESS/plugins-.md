# 🧭 WordPress Plugin Browsing History & Setup Log

This document tracks the browsing and installation history of the following plugins:

---
whaT is plugins

###### 

### 🔌 What Is a Plugin in WordPress?

A **plugin** in WordPress is a piece of software that you can add to your WordPress website to extend its functionality or add new features—**without writing any code**.

---

### 🔧 Think of it like this:

> A **WordPress website** is your smartphone.  
> A **plugin** is like an **app** that adds new capabilities.

---

### ✅ Examples of What Plugins Can Do:

|Feature|Plugin Example|
|---|---|
|SEO Optimization|Yoast SEO|
|Contact Forms|Contact Form 7|
|Security|Wordfence Security|
|E-commerce|WooCommerce|
|Site Backup|UpdraftPlus|
|Website Migration|All-in-One WP Migration|
|Login Security|Limit Login Attempts Reloaded|

---

### 📦 How Plugins Work:

- WordPress has a **core system**.
    
- Plugins are built using PHP and can **"hook"** into this core.
    
- They **modify or extend** existing behavior without altering the core code.
    

---

### 📁 Where Plugins Are Stored:

On your server, plugins are saved in:

bash

CopyEdit

`/wp-content/plugins/`

Each plugin usually has its own folder with all necessary files.

---

### 🛠️ How to Manage Plugins:

- **Install**: Add new functionality.
    
- **Activate**: Turn the plugin on.
    
- **Deactivate**: Turn the plugin off (but keep it installed).
    
- **Delete**: Remove the plugin files completely.
    

---

### 🔐 Are Plugins Safe?

- **Yes**, if downloaded from trusted sources like:
    
    - https://wordpress.org/plugins/
        
    - Reputable developers or marketplaces (e.g., CodeCanyon)
        

✅ Always:

- Keep plugins **updated**
    
- Use only **necessary** plugins
    
- Avoid **conflicting** or poorly rated plugins


## 1. 🔐 Limit Login Attempts Reloaded

**🔎 Browsed:**  
[https://wordpress.org/plugins/limit-login-attempts-reloaded/](https://wordpress.org/plugins/limit-login-attempts-reloaded/)


**📝 Notes:**  
- Read about brute-force protection features  
- Checked last update & active installs  
- Reviewed 5-star user reviews  
- Confirmed compatibility with latest WordPress version

**✅ Actions Taken:**
- Installed via `Plugins > Add New`
- Activated
- Configured max retries = 3, lockout = 20 min, notify on lock


---

## 2. 🔁 WP Reset

**🔎 Browsed:**  
[https://wordpress.org/plugins/wp-reset/](https://wordpress.org/plugins/wp-reset/)

**📝 Notes:**  
- Verified plugin resets WP to factory state  
- Snapshot feature reviewed  
- Noted warning: cannot be undone

**✅ Actions Taken:**
- Installed from dashboard
- Activated plugin
- Navigated to `Tools > WP Reset`
- Created snapshot before reset
- Executed site reset by typing “reset”

---

## 3. 📦 All-in-One WP Migration

**🔎 Browsed:**  
[https://wordpress.org/plugins/all-in-one-wp-migration/](https://wordpress.org/plugins/all-in-one-wp-migration/)

**📝 Notes:**  
- Reviewed export/import file types  
- Learned about `.wpress` format  
- Noted extension required for larger backups

**✅ Actions Taken:**
- Installed via dashboard
- Exported site to local `.wpress` file
- Tested import on test WP instance

---

## 4. 🧩 Duplicate Page

**🔎 Browsed:**  
[https://wordpress.org/plugins/duplicate-page/](https://wordpress.org/plugins/duplicate-page/)

**📝 Notes:**  
- Verified compatibility with Gutenberg  
- Read about use with custom post types

**✅ Actions Taken:**
- Installed and activated
- Configured: status = Draft, editor = Classic
- Duplicated pages successfully from list view

---

## 5. 🔐 WPS Hide Login

**🔎 Browsed:**  
[https://wordpress.org/plugins/wps-hide-login/](https://wordpress.org/plugins/wps-hide-login/)

**📝 Notes:**  
- Reviewed how it changes login URLs securely  
- Read FAQ: how to recover if URL forgotten  
- Confirmed minimal resource usage

**✅ Actions Taken:**
- Installed and activated
- Set new login path to `/secureadmin`
- Verified login functionality at new path

---

## ✅ Summary Log Table

| Plugin                   | Browsed URL                                           | Action Summary                      |
|--------------------------|--------------------------------------------------------|-------------------------------------|
| Limit Login Attempts     | wordpress.org/plugins/limit-login-attempts-reloaded/  | Installed, activated, configured    |
| WP Reset                 | wordpress.org/plugins/wp-reset/                       | Installed, snapshot, reset run      |
| All-in-One WP Migration  | wordpress.org/plugins/all-in-one-wp-migration/        | Export/import tested                |
| Duplicate Page           | wordpress.org/plugins/duplicate-page/                 | Duplicated posts and pages          |
| WPS Hide Login           | wordpress.org/plugins/wps-hide-login/                 | Changed login URL                   |

---

🗒️ *Document created for plugin tracking and training purposes.*
