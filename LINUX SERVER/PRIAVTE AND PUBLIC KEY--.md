# SSH Key-Based Authentication on CentOS

## ✨ What SSH Keys Protect Against

### 🛡️ Common Threats Prevented

#### 1. **Brute-force Password Attacks**

- SSH Keys can't be brute-forced since no password prompt exists.
    

#### 2. **Credential Theft**

- Private keys are encrypted and never sent over the network.
    

#### 3. **Man-in-the-Middle (MitM) Attacks**

- SSH verifies identity with cryptographic signatures.
    

#### 4. **Replay Attacks**

- Each session uses unique session keys.
    

#### 5. **Phishing**

- No passwords involved, making phishing ineffective.
    

---

## 🔐 Strong Authentication with SSH Keys

- Public key = shared with the server
    
- Private key = kept secret by the user
    
- Even if the public key is exposed, it can't be used alone.
    
- Private keys are locked down and permission-restricted (`chmod 600`).
    

---

## ⚖️ Server Configuration: `/etc/ssh/sshd_config`

```
PermitRootLogin yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding yes
Subsystem sftp /usr/libexec/openssh/sftp-server

# Optional Enhancements
AllowUsers armour
# AllowGroups sshusers
Port 2222
ListenAddress 192.168.1.38
MaxAuthTries 3
```

> ✅ Recommended: Use `PermitRootLogin prohibit-password` or `no` in production

---

## ⚙️ Generate SSH Key Pair

```
# View options
ssh-keygen -help

# Generate RSA key pair
ssh-keygen -t rsa

# Or generate ED25519 key (modern & secure)
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -C "your_email@example.com"
```

This creates `id_rsa` (private key) and `id_rsa.pub` (public key) in `~/.ssh/`.

---

## 🔐 Set Up Key Authentication

```
# Go to .ssh directory
cd /root/.ssh/

# Copy public key to authorized_keys
cp id_rsa.pub authorized_keys

# Permissions
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

> 🔐 This file is used by sshd to validate login attempts via public key.

---

## 🚀 Restart SSH

```
# Apply changes
systemctl restart sshd.service

# Verify service
netstat -nltup | grep sshd
```

---

## 💾 Use Private Key to Connect

```
# Save key and set permissions
vim id_rsa_centos
chmod 600 id_rsa_centos

# Connect using private key
ssh -i id_rsa_centos root@192.168.1.38
```

---

## 👤 DSA Key Pair for User `armour`

```
# Generate key with passphrase
ssh-keygen -t dsa -f /tmp/id_dsa -N "armour123"

# Prepare .ssh directory
mkdir /home/armour/.ssh
cp /tmp/id_dsa.pub /home/armour/.ssh/authorized_keys
chmod 700 /home/armour/.ssh
chmod 600 /home/armour/.ssh/authorized_keys

# Use private key to connect
vim id_dsa_armour
chmod 600 id_dsa_armour
ssh -i id_dsa_armour armour@<hostname or IP>
```

---

## 🔑 Manage Keys with `ssh-add`

```
# Start ssh-agent and add a key
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa

# List loaded public keys
ssh-add -L

# Remove specific key
ssh-add -d ~/.ssh/id_rsa

# Remove all keys
ssh-add -D

# Add key with timeout
ssh-add -t 600 ~/.ssh/id_ed25519

# Add key with confirmation
ssh-add -c ~/.ssh/id_rsa

# Add key from smartcard (PKCS#11)
ssh-add -s /path/to/pkcs11.so

# Remove key from smartcard
ssh-add -e /path/to/pkcs11.so
```

---

## ✨ Tips & Best Practices

- Always use strong passphrases when generating private keys.
    
- Do not share private keys; they must remain secret.
    
- Change the default SSH port from 22 to obscure from scans.
    
- Use `fail2ban` or a firewall (like `ufw` or `iptables`) to reduce brute force login attempts.
    
- Back up `.ssh` directories securely for recovery.
    
- Use `AllowUsers` or `AllowGroups` in `sshd_config` to restrict access.
    
- Periodically audit authorized keys and SSH logs.