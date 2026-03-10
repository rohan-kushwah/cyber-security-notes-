  
# 🚀 SSH, SCP, and RSYNC COMMANDS CHEAT SHEET

  

A complete and practical reference for securely accessing, copying, and synchronizing files between machines using **SSH**, **SCP**, and **RSYNC**. Designed for developers, sysadmins, and network engineers.

  

---

  

## 🔐 SSH (Secure Shell)

  

### ✅ Basic Commands

```bash

ssh user@remote # Login to remote host

ssh -p 2222 user@remote # Use a specific port

ssh -i ~/.ssh/id_rsa user@remote # Use a private key

```

  

### 🔍 Verbose Modes (Debugging)

```bash

ssh -v user@remote # Basic debug

ssh -vv user@remote # More verbose

ssh -vvv user@remote # Maximum debug

```

  

### 🖥️ Run Remote Commands

```bash

ssh user@remote 'ls -la'

ssh user@remote 'df -h'

ssh user@remote 'uptime'

```

  

### 🔁 Port Forwarding

```bash

ssh -L 8080:localhost:80 user@remote # Local port forward

ssh -R 2222:localhost:22 user@remote # Remote port forward

ssh -D 1080 user@remote # Dynamic SOCKS proxy (VPN)

```

  

---

  

## 📁 SCP (Secure Copy)

  

### 🔄 Transfer Files

```bash

scp file.txt user@remote:/path/ # Copy single file to remote

scp file1.txt file2.jpg user@remote:/path/ # Copy multiple files

scp -r folder/ user@remote:/path/ # Copy directory recursively

scp user@remote:/path/file.txt ./ # Copy from remote to local

```

  

### 🔐 Authentication and Ports

```bash

scp -i ~/.ssh/id_rsa file.txt user@remote:/path/ # Use SSH identity file

scp -P 2222 file.txt user@remote:/path/ # Use custom port

scp -l 500 file.txt user@remote:/path/ # Limit bandwidth (in Kbps)

```

  

### 📦 Preserve Attributes

```bash

scp -p file.txt user@remote:/path/ # Preserve timestamps, permissions

```

  

---

  

## ⚡ RSYNC (Remote Sync)

  

### 🔃 Sync Local ↔ Remote

```bash

rsync -avz folder/ user@remote:/path/ # Local to Remote

rsync -avz user@remote:/path/ ./local_folder/ # Remote to Local

```

  

### 🔧 Advanced Options

```bash

rsync -avz --progress folder/ user@remote:/path/ # Show progress

rsync -avz -e "ssh -p 2222" folder/ user@remote:/path/ # Use custom SSH port

rsync -avz -e "ssh -i ~/.ssh/id_rsa" folder/ user@remote:/path/ # Use identity file

rsync -avz --exclude 'node_modules' app/ user@remote:/path/ # Exclude files

rsync -avz --delete folder/ user@remote:/path/ # Delete extra files in dest

rsync -avz --dry-run folder/ user@remote:/path/ # Preview changes

rsync -avz --bwlimit=500 folder/ user@remote:/path/ # Limit bandwidth (in KB/s)

```

  

---

  

## 📝 Summary Table

  

| Tool | Feature | Example Command |

|--------|----------------------------|--------------------------------------------------|

| ssh | Basic login | `ssh user@host` |

| ssh | Verbose mode | `ssh -vvv user@host` |

| ssh | Remote command | `ssh user@host 'uptime'` |

| ssh | Port forwarding | `ssh -L 8080:localhost:80 user@host` |

| scp | Copy file | `scp file.txt user@host:/path/` |

| scp | Copy folder | `scp -r folder/ user@host:/path/` |

| scp | With identity file | `scp -i ~/.ssh/id_rsa file user@host:/path/` |

| scp | Bandwidth limit | `scp -l 500 file.txt user@host:/path/` |

| rsync | Sync local to remote | `rsync -avz folder/ user@host:/path/` |

| rsync | Sync remote to local | `rsync -avz user@host:/path/ ./local/` |

| rsync | Custom port | `rsync -avz -e "ssh -p 2222" folder/ user@host:/`|

| rsync | Dry run | `rsync -avz --dry-run folder/ user@host:/` |

| rsync | Bandwidth limit | `rsync -avz --bwlimit=500 folder/ user@host:/` |

  

---

  

# 📋 Examples of `rsync` commands with switches:

|Switch|Example Command|Explanation|
|---|---|---|
|`-a`|`rsync -a /home/user/docs/ /backup/docs/`|Copy all files from `docs/` to backup, preserving permissions, timestamps, symlinks, etc.|
|`-v`|`rsync -av /home/user/music/ /backup/music/`|Copy music files and **show detailed output**.|
|`-z`|`rsync -avz /home/user/videos/ root@192.168.1.5:/backup/videos/`|Copy videos to remote server with compression.|
|`-e ssh`|`rsync -avz -e ssh /var/www/ user@server:/var/www/`|Use SSH tunnel to securely copy website files to server.|
|`-r`|`rsync -r /home/user/images/ /backup/images/`|Recursively copy directories and files (without full archive mode).|
|`-p`|`rsync -p file1.txt /backup/`|Copy file while preserving permissions.|
|`-t`|`rsync -t file1.txt /backup/`|Copy file while preserving **modification time**.|
|`-g`|`rsync -g /home/user/project/ /backup/project/`|Preserve **group ownership** during copy.|
|`-o`|`rsync -o /home/user/project/ /backup/project/`|Preserve **file owner** (needs root permission).|
|`-D`|`rsync -D /dev/sdX /backup/devices/`|Copy special device files too (like block/char devices).|
|`--delete`|`rsync -av --delete /source/ /destination/`|Delete files in destination that are missing from source (perfect sync).|
|`--progress`|`rsync -avz --progress /home/user/bigfile.iso /backup/`|Show live progress for big file transfer.|
|`--dry-run`|`rsync -avz --dry-run /home/user/docs/ /backup/docs/`|**Simulate** copying files, no changes made (safe preview).|
|`--update`|`rsync -avz --update /source/ /destination/`|Only copy if source file is **newer** than destination.|
|`--exclude='*.tmp'`|`rsync -avz --exclude='*.tmp' /source/ /backup/`|Skip copying all `.tmp` temporary files.|
|`--include='*.jpg'`|`rsync -avz --include='*.jpg' --exclude='*' /photos/ /backup/photos/`|Only copy `.jpg` files, exclude everything else.|
|`-P`|`rsync -avzP /home/user/Downloads/ /backup/Downloads/`|Show progress + allow resuming interrupted transfers.|
|`--partial`|`rsync -avz --partial /largefiles/ /backup/largefiles/`|Save partially transferred files to resume later.|
---

### 🔥 Some **popular combinations** people use:

- `-avz` → archive + verbose + compress
    
- `-avz --delete` → archive + verbose + compress + delete old files at destination
    
- `-avzP` → archive + verbose + compress + show progress + keep partial

-n for no transfer


Here's a clear explanation:

- **PuTTYgen** (PuTTY Key Generator):
    
    - It is used to **generate SSH key pairs** (private and public keys).
        
    - These keys are mainly used for **SSH authentication** — a more secure way to log into servers instead of using a password.
        
    - PuTTYgen can create keys in formats compatible with **PuTTY** (a popular SSH client for Windows) and can also export OpenSSH keys if needed.
        
- **WinSCP**:
    
    - It is a **file transfer program** for Windows that allows you to securely **upload, download, and manage files** on a remote server.
        
    - It uses protocols like **SFTP**, **SCP**, or **FTP** over an SSH connection.
        
    - It is often used alongside PuTTY — for example, after using PuTTY to log in to a Linux server, you can use WinSCP to transfer files easily.
        

**In short:**

- Use **PuTTYgen** to create SSH keys for secure authentication.
    
- Use **WinSCP** to **transfer files** securely between your computer and a server.
    

Would you like me to show you a simple workflow example using PuTTYgen and WinSCP together? 🚀