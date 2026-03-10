# 🧠 Proxy Server Configuration Inside Virtual Machine Environment (CentOS 9 ➔ Windows Client)

---

# 🛠️ Prerequisites:

|Requirement|Details|
|---|---|
|**1 VM (Server)**|CentOS 9 Stream or RHEL 9|
|**1 VM (Client)**|Windows 10 or 11|
|**Virtualization Software**|VMware / VirtualBox / Hyper-V|
|**Network Type**|**Bridged** or **Internal Network** (both VMs must be in same network/subnet)|

---

# ✨ Step-by-Step Guide

---

## 1. VM Network Setting (Most Important)

### ❗ Set Network Adapters:

✅ For both **CentOS** and **Windows** VMs:

- In **VirtualBox**:
    
    - Network → Attached to: **Bridged Adapter** (for internet access from host and VMs)
        
    - OR
        
    - **Internal Network** if fully isolated network needed.
        
- In **VMware**:
    
    - Choose **Bridged Network** or **Custom Host-Only Network**.
        

✅ Both machines should be able to **ping** each other:

bash

CopyEdit

`# From CentOS VM: ping <Windows_VM_IP>  # From Windows VM (cmd): ping <CentOS_VM_IP>`

---

## 2. Install Squid Proxy on CentOS 9

bash

CopyEdit

`# Update system dnf update -y  # Install Squid dnf install squid -y`

---

## 3. Start and Enable Squid

bash

CopyEdit

`# Enable on boot systemctl enable squid  # Start service systemctl start squid  # Check service status systemctl status squid`

✅ **Status: active (running)** aana chahiye.

---

## 4. Configure Squid to Allow Windows Client Network

Edit file:


`nano /etc/squid/squid.conf`

### Changes to do:

✅ Comment this line:



`# http_access deny all`

✅ Add your network IP range:

Suppose your VMs are in `192.168.1.0/24` network:


`acl mynetwork src 192.168.1.0/24 http_access allow mynetwork`

✅ Confirm squid is listening on default port:

]

`http_port 3128`

✅ Save and exit.

---

## 5. Allow Squid Port in Firewall



`firewall-cmd --permanent --add-port=3128/tcp firewall-cmd --reload`

✅ If no firewall, skip this step.

---

## 6. Restart Squid Service




`systemctl restart squid`

---

## 7. Set Proxy on Windows Client VM

👉 On Windows VM:

- Go to **Settings** → **Network & Internet** → **Proxy**.
    
- Under **Manual Proxy Setup**:
    
    - Turn ON.
        
    - Set **IP Address** = IP of your CentOS Proxy Server (e.g., 192.168.1.100)
        
    - Set **Port** = `3128`
        
- Save the settings.
    

---

## 8. Test Internet Access

- Open Chrome / Edge browser on Windows.
    
- Try to browse `google.com`.
    
- It should pass through the CentOS Proxy now!
    

✅ To confirm proxy usage:



`tail -f /var/log/squid/access.log`

(You will see your Windows IP making proxy requests.)

---

# 🔥 Important Commands (Summary)




`dnf update -y dnf install squid -y systemctl enable squid systemctl start squid nano /etc/squid/squid.conf   # Configure network firewall-cmd --permanent --add-port=3128/tcp firewall-cmd --reload systemctl restart squid`

---

# 🚀 Pro Tips:

|Tip|Command|
|---|---|
|See real-time proxy logs|`tail -f /var/log/squid/access.log`|
|Check Squid errors|`tail -f /var/log/squid/cache.log`|
|Restart Squid after changes|`systemctl restart squid`|
|If port blocked|`firewall-cmd --permanent --add-port=3128/tcp && firewall-cmd --reload`|

---

# 📚 Final Notes:

- Make sure VMs are in **same network** or **routable** to each other.
    
- Firewall must allow port **3128**.
    
- Windows client must manually set proxy settings.
    

---

# ✅ After Following These, Your Setup Will Look Like:

javascript

CopyEdit

`Windows VM (Client) → Proxy Set → CentOS VM (Proxy Server) → Internet`

---

# ✍️ Would you also like:

- How to block websites (like block YouTube)?
    
- How to add username/password authentication?
    
- How to log which website Windows client visited?
    

⚡ Main ready templates de sakta hoon agar chaho!

**Batao, kya next chahiye?** 🔥

4o

  

Search

Rea