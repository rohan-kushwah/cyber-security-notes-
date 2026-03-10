sftp se bhej do /var/www/html me file bana da php uski location de do program me ab service restart karo ho jayegi'
## ✅ **Step-by-Step: Transfer Files via SFTP in VS Code**

### 🔧 1. **Install VS Code Extension**

Install the **“SFTP”** extension (by liximomo):

- Open VS Code
    
- Go to **Extensions** (`Ctrl+Shift+X`)
    
- Search for: `SFTP` (Author: liximomo)
    
- Click **Install**
    

> ⚠️ Note: There are other extensions like "Remote - SSH", but `SFTP` is used for direct file transfer, not live remote editing.

---

### 📁 2. **Create an `sftp.json` Configuration File**

In your project folder:

- Press `F1` or `Ctrl+Shift+P`
    
- Type: `SFTP: config`
    
- Press `Enter` to generate a file called `.vscode/sftp.json`
    

### 📝 Sample `sftp.json` Configuration

json


`{   "host": "your.server.ip.or.hostname",   "username": "your-ssh-user",   "password": "your-password",  // OR use "privateKeyPath" if using SSH key   "remotePath": "/home/your-ssh-user/app",  // Remote folder   "protocol": "sftp",   "uploadOnSave": true }`

> 🔐 **Security Tip:** If you're using SSH key authentication (recommended), use this instead of `password`:

json



`"privateKeyPath": "~/.ssh/id_rsa"`

---

### 🚀 3. **Upload Files**

You now have several options:

- **Auto Upload on Save:** If `"uploadOnSave": true`, every file you save will automatically upload.
    
- **Manual Upload:**
    
    - Right-click a file or folder
        
    - Choose `SFTP: Upload`
        
- **Download from Server:**
    
    - Right-click → `SFTP: Download`
        

---

### 🔁 4. **Sync Local and Remote**

The extension also allows syncing:

- `SFTP: Sync Local → Remote`
    
- `SFTP: Sync Remote → Local`
    

You can select these from the command palette (`Ctrl+Shift+P`).

---

### 🛑 Common Troubleshooting

- **Permission denied:** Check that your user has write permissions to the `remotePath`
    
- **Connection refused:** Make sure SSH is running on the server:
    
    
    
    `sudo systemctl status sshd`
    
- **Timeouts or delays:** Check firewall rules or if the port (usually `22`) is open.



# basic program of html=
<!DOCTYPE html>


<html>
<head>


<title>My First HTML Page</title>

</head>
<body>
    <h1>Welcome to HTML</h1>
    <p>This is a paragraph.</p>
    <a href="https://www.example.com">Visit Example.com</a>
</body>
</html>
## 🧠 **Explanation of the Program**

|Line|Code|Purpose|
|---|---|---|
|`<!DOCTYPE html>`|Declares the document type|Tells the browser this is an HTML5 document.|
|`<html>`|Opening HTML tag|Marks the start of the HTML document.|
|`<head>`|Metadata section|Contains data about the document (not visible on the page).|
|`<title>My First HTML Page</title>`|Page title|Displays in the browser tab.|
|`</head>`|End of head|Marks the end of the metadata section.|
|`<body>`|Start of visible content|Everything between `<body>` and `</body>` is shown on the webpage.|
|`<h1>Welcome to HTML</h1>`|Main heading|Displays a large heading (level 1).|
|`<p>This is a paragraph.</p>`|Paragraph text|Displays a block of text.|
|`<a href="https://www.example.com">Visit Example.com</a>`|Hyperlink|Clicking this takes you to "example.com".|
|`</body>`|End of visible content|Closes the body section.|
|`</html>`|End of HTML document|Marks the end of the file.|