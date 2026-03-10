# SSL-Bump-with-transparent Proxy


# configuration steps of transparent proxy--
To configure a **transparent proxy**, you need to intercept network traffic (usually HTTP/HTTPS) without requiring client devices to configure proxy settings. This is typically done on a **gateway** or **router** with a proxy server like **Squid**. Here's a general step-by-step guide for setting up a **transparent proxy** using **Squid** on a **Linux-based system** (e.g., CentOS, Ubuntu):

---

## 🔧 Requirements

- A Linux machine with 2 NICs (for routing) or appropriate IP forwarding enabled.
    
- Squid installed.
    
- IP forwarding and firewall rules set up to redirect traffic to Squid.
    
- Clients on the same network segment.
    

---

## ✅ Step-by-Step Configuration of Transparent Proxy (Squid)

### 1. **Install Squid**

On CentOS/RHEL:



`sudo dnf install squid -y`

On Ubuntu/Debian:



`sudo apt install squid -y`

---

### 2. **Enable IP Forwarding**

Edit `/etc/sysctl.conf`:



`net.ipv4.ip_forward = 1`

Apply changes:



`sudo sysctl -p`

---

### 3. **Configure Squid for Transparent Mode**

Edit `/etc/squid/squid.conf`. Make sure these options are included:


`http_port 3128 transparent`

You can also control access:



`acl localnet src 192.168.1.0/24 http_access allow localnet http_access deny all`

---

### 4. **Configure Firewall (iptables/nftables/firewalld)**

#### Example using iptables:

Redirect HTTP traffic to Squid's port:



`iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 3128`

(Adjust `eth0` to your LAN interface and ensure you're only redirecting from internal network sources.)

Make rules persistent:


`sudo apt install iptables-persistent      # Debian-based sudo service iptables save                # RHEL/CentOS`

---

### 5. **Start and Enable Squid**



`sudo systemctl enable --now squid`

---

### 6. **Testing**

- Ensure client devices are **not configured with any manual proxy settings**.
    
- Open a browser and try accessing a website.
    
- Monitor logs:
    


`tail -f /var/log/squid/access.log`

---

### 🔒 Optional: HTTPS Interception (Advanced)

Intercepting HTTPS requires:

- `ssl-bump` configuration in Squid.
    
- Your own CA certificate installed on clients.
    

> ⚠️ This can break many services and is considered invasive unless done in controlled environments (e.g., schools or enterprises).



#  configurartion of ssl bump --
`ssl_bump`** is a feature in **Squid Proxy** used for **intercepting and inspecting HTTPS traffic** in a **transparent proxy** setup. Normally, HTTPS is encrypted end-to-end, so a standard proxy cannot see the contents. With `ssl_bump`, Squid acts as a **man-in-the-middle (MITM)** proxy: it decrypts the client's HTTPS request, inspects or filters it, and then re-encrypts it when forwarding it to the destination.

---

### 🔍 **What `ssl_bump` is used for:**

|Purpose|Description|
|---|---|
|**Content Filtering**|To block or allow specific websites over HTTPS.|
|**Logging & Monitoring**|To log URLs or inspect encrypted traffic for auditing.|
|**Data Loss Prevention (DLP)**|To detect sensitive data leaks (e.g., passwords, confidential documents).|
|**Malware Detection**|To scan HTTPS downloads for viruses or suspicious activity.|
|**Policy Enforcement**|To restrict file types, enforce time limits, or bandwidth usage even over encrypted traffic.|

---

### 🧱 **How `ssl_bump` works:**

1. Client connects to `https://example.com` via Squid.
    
2. Squid **pretends to be `example.com`**, generating a fake SSL certificate signed by your internal CA.
    
3. Client trusts this certificate **only if your CA is installed on the client**.
    
4. Squid decrypts the traffic, applies filters/rules, then re-encrypts it before forwarding it to the real `example.com`.
    

---




## 🧱 OVERVIEW

- **Platform**: CentOS 9 Stream (Squid 5+), Windows client (e.g., Windows 10/11)
    
- **Goal**: Transparent proxy that intercepts HTTP **and** HTTPS using `ssl_bump`
    
- **Tools**: Squid, OpenSSL, iptables (for redirect), a custom CA cert
    

---

## 🔧 STEP 1: Install Squid & OpenSSL

bash

CopyEdit

`sudo dnf install squid openssl -y`

---

## 📁 STEP 2: Create a Custom CA Certificate

bash

CopyEdit

`sudo mkdir -p /etc/squid/ssl_cert cd /etc/squid/ssl_cert  sudo openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 \   -keyout myCA.pem -out myCA.pem \   -subj "/C=US/ST=Example/L=Proxy/O=MyProxyCA/CN=MyProxyCA"`

Split it into key and cert:

bash

CopyEdit

`sudo cp myCA.pem myCA.crt sudo openssl x509 -in myCA.crt -outform DER -out myCA.der`

This `.der` file will be imported to Windows later.

---

## 🔑 STEP 3: Configure Squid for SSL Bumping


`sudo nano /etc/squid/squid.conf`

Add or edit:



`http_port 3128 ssl-bump cert=/etc/squid/ssl_cert/myCA.pem key=/etc/squid/ssl_cert/myCA.pem generate-host-certificates=on dynamic_cert_mem_cache_size=4MB  # Define ACLs acl step1 at_step SslBump1 acl step2 at_step SslBump2 acl step3 at_step SslBump3  # SSL Bump Steps ssl_bump peek step1 ssl_bump bump step2 ssl_bump splice step3  # Allow localnet acl localnet src 192.168.1.0/24 http_access allow localnet http_access deny all  # Cert Generation Helper sslcrtd_program /usr/lib64/squid/security_file_certgen -s /var/lib/ssl_db -M 4MB`

---

## 🏗️ STEP 4: Initialize SSL Cert DB

bash

CopyEdit

`sudo /usr/lib64/squid/security_file_certgen -c -s /var/lib/ssl_db -M 4MB sudo chown -R squid:squid /var/lib/ssl_db`

---

## 🔥 STEP 5: Redirect HTTPS Traffic with iptables

Enable IP forwarding:

bash

CopyEdit

`echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf sudo sysctl -p`

Redirect traffic (for clients on 192.168.1.0/24):

bash

CopyEdit

`sudo firewall-cmd --permanent --add-masquerade sudo firewall-cmd --reload  sudo iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 443 -j REDIRECT --to-port 3128`

Replace `eth0` with your LAN interface name (`ip a` can show you that).

---

## 🚀 STEP 6: Start and Enable Squid

bash

CopyEdit

`sudo systemctl enable --now squid`

Check logs:

bash

CopyEdit

`sudo tail -f /var/log/squid/access.log`

---

## 🖥️ STEP 7: Import CA Cert on Windows Client

1. Transfer the `myCA.der` file to Windows.
    
2. Open **Run > `mmc` > File > Add Snap-In > Certificates > Computer Account**
    
3. Navigate to:
    
    - **Trusted Root Certification Authorities > Certificates**
        
4. Right-click > All Tasks > **Import**
    
5. Import `myCA.der`
    
6. Restart browser or PC.
    

---

## ✅ TEST

Open a browser on Windows (no proxy config needed).  
Try visiting `https://example.com`. If no certificate warning shows up and Squid logs the request, SSL bumping is working.
---

