# WordPress Migration Guide

  

## 📌 What is WordPress Migration?

  

WordPress migration is the process of moving a WordPress website from one environment to another. This can include transferring the site between:

  

- Different hosting providers (e.g., from Bluehost to SiteGround)

- Localhost to live server (e.g., from a local development environment to a production website)

- One domain to another (e.g., from oldsite.com to newsite.com)

- HTTP to HTTPS (migrating to a secure version of your site)

  

## ✅ Why WordPress Migration Is Necessary

  

- **Changing Hosting Providers** – faster or more reliable hosting.

- **Going Live from Localhost** – moving a local development site online.

- **Changing Domain Name** – rebranding or better SEO.

- **Creating a Staging Environment** – for testing updates before going live.

- **Cloning a Site** – for testing, development, or backup.

  

## 🔧 Manual WordPress Migration (No Plugin)

  

### Step-by-Step

  

1. **Backup Files:** Use FTP or File Manager to download `public_html`.

2. **Export Database:** Use phpMyAdmin > Export > SQL.

3. **Upload Files to New Host:** Use FTP or File Manager.

4. **Create New Database:** In cPanel > MySQL Databases.

5. **Import Database:** phpMyAdmin > Import `.sql` file.

6. **Edit `wp-config.php`:**

   ```php

   define('DB_NAME', 'new_db_name');

   define('DB_USER', 'new_db_user');

   define('DB_PASSWORD', 'new_db_password');

   define('DB_HOST', 'localhost');

   ```

7. **Update Site URLs in Database (if domain changed):**

   ```sql

   UPDATE wp_options SET option_value = REPLACE(option_value, 'http://oldsite.com', 'https://newsite.com') WHERE option_name IN ('siteurl', 'home');

   UPDATE wp_posts SET post_content = REPLACE(post_content, 'http://oldsite.com', 'https://newsite.com');

   UPDATE wp_posts SET guid = REPLACE(guid, 'http://oldsite.com', 'https://newsite.com');

   UPDATE wp_postmeta SET meta_value = REPLACE(meta_value, 'http://oldsite.com', 'https://newsite.com');

   UPDATE wp_options SET option_value = REPLACE(option_value, 'http://oldsite.com', 'https://newsite.com');

   UPDATE wp_usermeta SET meta_value = REPLACE(meta_value, 'http://oldsite.com', 'https://newsite.com');

   ```

8. **Fix Permalinks:** Settings > Permalinks > Save.

  

## 🤖 Automatic Migration (Plugins & Tools)

  

- **All-in-One WP Migration** – Easy export/import.

- **Duplicator** – Clone and deploy entire site.

- **UpdraftPlus** – Backup and restore (premium feature for migration).

- **WP Migrate Lite** – For database + URL replacement.

- **Host-Specific Tools** – SiteGround, Bluehost, WP Engine, etc.

  

## 🧾 SQL Queries for Domain Migration

  

```sql

UPDATE wp_options SET option_value = REPLACE(option_value, 'http://oldsite.com', 'https://newsite.com') WHERE option_name IN ('siteurl', 'home');

UPDATE wp_posts SET post_content = REPLACE(post_content, 'http://oldsite.com', 'https://newsite.com');

UPDATE wp_posts SET guid = REPLACE(guid, 'http://oldsite.com', 'https://newsite.com');

UPDATE wp_postmeta SET meta_value = REPLACE(meta_value, 'http://oldsite.com', 'https://newsite.com');

UPDATE wp_options SET option_value = REPLACE(option_value, 'http://oldsite.com', 'https://newsite.com');

UPDATE wp_usermeta SET meta_value = REPLACE(meta_value, 'http://oldsite.com', 'https://newsite.com');

```

  

## 🛠 Handling Serialized Data (Use Tools Instead)

  

- **WP-CLI**

  ```bash

  wp search-replace 'http://oldsite.com' 'https://newsite.com' --all-tables

  ```

- **Better Search Replace Plugin**

- **WP Migrate DB / WP Migrate Lite**

  

## 📝 Summary Table

  

| Method                   | Difficulty | Plugin Needed | Best For                     |

|--------------------------|------------|----------------|-------------------------------|

| Manual (FTP + SQL)       | High       | No             | Full control, tech-savvy users |

| All-in-One WP Migration  | Easy       | Yes            | Beginners                     |

| Duplicator               | Medium     | Yes            | Site clones and staging       |

| WP Migrate DB            | Medium     | Yes            | Database-focused migrations   |

| Host Tools               | Very Easy  | Sometimes      | Hassle-free, fast migrations  |