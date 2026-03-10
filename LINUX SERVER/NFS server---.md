An **NFS server** (Network File System server) is a server that uses the **NFS protocol** to allow **file sharing** over a network, primarily in **Linux and UNIX environments**.

### 🔧 What It Does:

An NFS server lets other computers (clients) **mount directories** from the server as if they were local directories. This allows for:

- Shared storage across systems
    
- Centralized file management
    
- Easier backups and data access
    

### 🧱 How It Works:

1. **The NFS server** exports (shares) directories.
    
2. **The NFS clients** connect and mount those shared directories.
    
3. Clients can then read/write files depending on permissions.
    

### 📁 Example Use Case:

You're managing a team of developers. Instead of copying code across everyone's machine, you set up an NFS server with a shared `/projects` folder. Everyone mounts it and works on the same files in real time.

### 🔐 Common Features:

- Supports **permissions** (via UNIX file permissions)
    
- Can be **read-only** or **read/write**
    
- Integrates with system users/groups
    
- Works over **TCP or UDP**
    

### 📦 Example Services/Commands:

On **Linux**, the NFS server is typically powered by:

- `nfs-utils` package
    
- Config file: `/etc/exports`
    
- Service: `nfs-server` or `nfs`


# use karte hai in the lan file transfer in the linux networkk-

# **Network File System (NFS) Setup**

> This guide explains how to install, configure, and secure an NFS server on RHEL/CentOS using **firewalld**.

---

## **Step 1: Install NFS Packages**

### ✅ Check if NFS is already installed:


`rpm -qa | grep nfs`

### 📦 Install NFS and dependencies:


`yum install -y nfs-utils rpcbind`

### 🔍 Verify installation:


`rpm -qi nfs-utils
rpm -ql nfs-utils 
rpm -qc nfs-utils
rpm -qd nfs-utils
rpm -qi rpcbind
rpm -ql rpcbind


rpc port maping karta hai base bana ta hai service ko run karne ke liye

## 🔧 **Step 2: Enable and Start NFS Services**

### 🔁 Enable services to start on boot:


`systemctl enable rpcbind 
`systemctl enable nfs-server
`systemctl enable nfs-lock 
`systemctl enable nfs-idmap`

### 🚀 Start services now:


`systemctl start rpcbind
`systemctl start nfs-server`
`systemctl start nfs-lock
`systemctl start nfs-idmap`




## 📁 **Step 3: Create a Shared Directory**


`mkdir -p /data chmod 777 /data`

---

## 📤 **Step 4: Export Shared Directory**

📝 Edit the `/etc/exports` file:


`vim /etc/exports`

### Example entries:


`# Allow read-write with sync /data/ 192.168.1.0/24(rw,sync) 
 Read-only access #/data/ 192.168.1.0/24(ro,sync) 
#Allow root privileges (use with caution) /data/ 192.168.1.0/24(rw,sync,no_root_squash,no_all_squash)`

💾 Apply export changes:


`exportfs -rav`

### 🔥 Enable and Start firewalld


`# Enable and start firewalld systemctl enable firewalld systemctl start firewalld`

---

### 🔐 Add NFS Services to firewalld Permanently


`firewall-cmd --permanent --add-service=nfs firewall-cmd --permanent --add-service=mountd firewall-cmd --permanent --add-service=rpc-bind`

---

### 🔄 Reload firewalld to Apply Changes


`firewall-cmd --reload`

---

### ✅ Optional: Verify Open Ports


`firewall-cmd --list-all`

### 🔗 Step 6: NFS Client (Example)

On a client machine, mount the shared directory:


`mount -t nfs 192.168.1.10:/data /mnt`

_Replace `192.168.1.10` with your NFS server IP._

---

### 📝 Notes

- `rw` – Writable access
    
- `ro` – Read-only
    
- `sync` – Ensures data is written to disk before responding
    
- `no_root_squash` – Allows root access on client (⚠️ insecure in prod)
    
- `no_all_squash` – Preserves UID/GID from client

### 🔐 Optional (SELinux)

If SELinux is enabled:


`chcon -Rt nfs_t /data`

showmount -e then ip --- to show which data will be shared by server

## 🖥️ NFS Client Setup and Mounting Guide

This section describes how to install NFS client tools, connect to an NFS server, mount shared directories, and optionally set up automatic mounting on boot.

---

### 📦 Step 1: Install NFS Client Utilities

Install required packages depending on your Linux distribution:

---

**Debian/Ubuntu:**

`apt install rpcbind nfs-common`

_Installs the required RPC service (`rpcbind`) and NFS tools (`nfs-common`)._

---

**RHEL/CentOS:**


`yum install rpcbind nfs-common`

_Same functionality for RHEL/CentOS systems._

(Optional) Install `showmount` to view exports from an NFS server:


`yum install nfs-utils`


Step 2: Discover Available NFS Shares  
Check available exports from the server:


```
showmount -e 192.168.1.31
```

Lists the directories exported by the NFS server at 192.168.1.31

Step 3: Mount NFS Shares Manually  
Mount the root of the NFS server (not recommended in production):


```
mount -t nfs 192.168.1.31://mnt -o nolock
```

Unmount the share:


```
umount /mnt
```

Create local directories for mounting multiple shares:


```
mkdir /mnt/d1 /mnt/d2
```

Mount specific shares:


```
mount -t nfs 192.168.1.31:/data /mnt/d1/mount -t nfs 192.168.1.31:/backup /mnt/d2/
```


Unmount the shares:

- `umount /mnt/dh`
- `umount /mnt/d2`

Step 4: Auto-Mount NFS Shares via `/etc/fstab`  
Edit the file to configure persistent NFS mounts:

- `vim /etc/fstab`

Add the following line to mount `/data` on boot:

- `192.168.1.31:/data /mnt/d1 nfs rw,sync,hard,intr 0 0`
Apply and verify:


```
mount -adf -h
```

Step 5: More Mount Examples  
Various mount syntax variations for NFS:


```
mount -t nfs 192.168.1.31:/var/armour_share/ /mnt -o nolockmount -t nfs -o vers=3,nolock 192.168.1.31:/data /mnt/db
```

Step 6: Restart NFS Server (on server-side)  
If needed, restart the NFS server:


```
systemctl restart nfs-server.service
```

111 - Portmapper / rpcbind
Protocol: TCP/UDP
Purpose:
    rpcbind (formerly portmapper) maps RPC program numbers to network port numbers.
    Essential for services like NFS, mountd, and statd to communicate over the network.
    Required by: All RPC-based services including NFS.

2049 - NFS Server (nfsd)
Protocol: TCP/UDP
Purpose:
    The main port used by NFS servers to handle file-sharing requests.
    This is the default and well-known port for NFS.

20048 - mountd
Protocol: TCP/UDP
Purpose:
    Handles initial mount requests from NFS clients.
    Helps negotiate which directories are available to be mounted.
    Port can vary unless fixed using configuration.
![[Pasted image 20250416130359.png]]
