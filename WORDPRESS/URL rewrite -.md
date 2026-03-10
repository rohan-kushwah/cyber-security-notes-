k## 🔄 How URL Rewrite Works (In Simple Terms)

**Example:**

### Before Rewrite:



`http://example.com/index.php?page=products&id=123`

### After Rewrite:



`http://example.com/products/123`

The rewritten URL is easier to read and better for SEO. The web server internally maps this clean URL back to the original query string.

---

## ⚙️ Configuration in Linux (Apache using `.htaccess` and `mod_rewrite`)

### Step 1: Enable `mod_rewrite` module


`sudo a2enmod rewrite sudo systemctl restart apache2`

### Step 2: Update Apache config to allow `.htaccess`

In `/etc/apache2/sites-available/000-default.conf`:



`<Directory /var/www/html>     AllowOverride All </Directory>`

### Step 3: Create `.htaccess` in your site root


`RewriteEngine On RewriteRule ^products/([0-9]+)$ index.php?page=products&id=$1 [L]`

This rule translates:


`http://yourdomain.com/products/123 ➡ index.php?page=products&id=123`


####

- **Ensure `mod_rewrite` is installed**  
    It usually comes bundled with `httpd`, but confirm:
    
    bash
    
    CopyEdit
    
    `sudo yum install httpd -y sudo httpd -M | grep rewrite`
    
    If you see output like `rewrite_module (shared)`, it’s already loaded.
    
- **Manually Enable `mod_rewrite`**  
    Edit the Apache config or virtual host config (like `/etc/httpd/conf/httpd.conf` or `/etc/httpd/conf.d/your-site.conf`), and make sure these lines exist or are added:
    
    apache
    
    CopyEdit
    
    `LoadModule rewrite_module modules/mod_rewrite.so`
    
    Also, in the `<Directory>` block for your site (e.g., `/var/www/html`), allow `.htaccess` overrides:
    
    apache
    
    CopyEdit
    
    `<Directory "/var/www/html">     AllowOverride All     Require all granted </Directory>`
    
- **Restart Apache**  
    After editing the configuration:
    
    bash
    
    CopyEdit
    
    `sudo systemctl restart httpd`
    
- **Test Rewrite**  
    Add a `.htaccess` in `/var/www/html` with a simple rule:
    
    apache
    
    CopyEdit
    
    `RewriteEngine On RewriteRule ^test$ test.html`
    
    Then create a `test.html` file. Access `http://your-server-ip/test` and see if it works.

---

## ⚙️ Configuration in Windows (IIS using URL Rewrite Module)

### Step 1: Install IIS URL Rewrite Module

Download from Microsoft:

- https://www.iis.net/downloads/microsoft/url-rewrite
    

### Step 2: Configure Rule via IIS Manager

1. Open **IIS Manager**
    
2. Select your site → open **URL Rewrite**
    
3. Add **Inbound Rule** → **Blank Rule**
    
4. Configure:
    
    - **Pattern**: `^products/([0-9]+)$`
        
    - **Action**: Rewrite
        
        - **Rewrite URL**: `index.php?page=products&id={R:1}`
            
    - Check “Append query string”: **No**
        

### Alternative: Use `web.config`

Create or update `web.config` in your site root:



`<configuration>   <system.webServer>     <rewrite>       <rules>         <rule name="CleanProductURL" stopProcessing="true">           <match url="^products/([0-9]+)$" />           <action type="Rewrite" url="index.php?page=products&amp;id={R:1}" />         </rule>       </rules>     </rewrite>   </system.webServer> </configuration>`

---

## ✅ Real Life Use Cases

- Clean URLs in content management systems (WordPress, Joomla, Laravel)
    
- Redirecting old links after site restructuring
    
- SEO optimization
    
- Masking internal directory structure from users