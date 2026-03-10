# WordPress Theme Installation Guide

  

## 🔧 Method 1: Install a WordPress Theme via the Dashboard

  

### ✅ For Free Themes from the WordPress Repository:

  

1. **Log in to your WordPress Admin Dashboard**  

   Go to: `http://yourdomain.com/wp-admin`

  

2. **Navigate to Themes**  

   In the left sidebar: `Appearance` → `Themes`

  

3. **Add New Theme**  

   Click the **“Add New”** button at the top.

  

4. **Search for a Theme**  

   Use the search bar to find a theme (e.g., “Astra”, “OceanWP”).

  

5. **Install and Activate**  

   - Click **Install**

   - Then click **Activate**

  

---

  

### ✅ For Premium or Downloaded Themes (.zip File):

  

1. **Log in to Dashboard**  

   As above, go to `Appearance` → `Themes` → `Add New`

  

2. **Upload Theme**  

   - Click **Upload Theme** (top of the page)

   - Choose your `.zip` file (e.g., `mytheme.zip`)

   - Click **Install Now**

  

3. **Activate the Theme**  

   After installation, click **Activate**

  

---

  

## 💻 Method 2: Install Theme Manually via FTP

  

Use this if your theme fails to upload via the dashboard (e.g., due to file size or server restrictions).

  

### Requirements:

- FTP software (e.g., FileZilla)

- FTP access to your web hosting account

  

### Steps:

  

1. **Extract the Theme ZIP**  

   Unzip the theme on your computer. You’ll get a folder like `mytheme`.

  

2. **Connect to Your Server**  

   Use FTP to connect to your site.

  

3. **Upload the Theme Folder**  

   Navigate to the WordPress theme directory and upload:

  

   ```

   /wp-content/themes/mytheme/

   ```

  

4. **Activate in Dashboard**  

   - Go to WordPress Admin: `Appearance` → `Themes`

   - Find your uploaded theme and click **Activate**

  

---

  

## 📁 Theme Installation Directory

  

When manually uploading a WordPress theme via FTP, it must be placed in the following folder:

  

```

/wp-content/themes/

```

  

### 🔍 Full Path Example:

  

If your WordPress site is in `public_html` or `/var/www/html`, the path might look like:

  

```

/home/username/public_html/wp-content/themes/mytheme/

```

or

```

/var/www/html/wp-content/themes/mytheme/

```

  

### ✅ Required Files:

  

Make sure your theme folder contains at least:

  

- `style.css`

- `index.php`

  

These are essential for WordPress to detect the theme properly.

  

---

  

## 📌 Additional Tips

  

- After activating a theme, check for **required plugins** or **starter templates** the theme might suggest.

- Customize it under `Appearance` → `Customize`

- Always use a **child theme** if you plan to modify the theme’s code.