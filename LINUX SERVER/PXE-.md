# PXE Boot Server Full Configuration on CentOS/RHEL

## 📦 PXE Boot Server – Explained Briefly

### ✅ What is PXE?

**PXE (Preboot Execution Environment)** allows a computer to boot from a **network server** rather than a local disk. It is commonly used to install operating systems remotely or perform system recovery.

---

## 🛠️ How PXE Boot Works

1. **Client BIOS/UEFI** is configured to boot from network (PXE).
    
2. On boot, the client sends a **DHCP request** asking for network configuration and a boot file.
    
3. The **DHCP server** responds with:
    
    - IP configuration
        
    - Boot filename (e.g., `pxelinux.0`)
        
    - TFTP server IP
        
4. The client downloads the bootloader via **TFTP**.
    
5. Bootloader fetches a **Linux kernel & initrd** or a Windows PE image.
    
6. The client starts booting from the downloaded OS installer image.
    

---

## 🧪 Real-Life Example: Linux PXE Boot Server on CentOS

### Scenario:

You’re setting up a PXE server to automatically install CentOS 9 on multiple machines without USB or DVD.

### Components:

- **DHCP Server** (provides IP & boot file info)
    
- **TFTP Server** (serves boot files like `pxelinux.0`, `vmlinuz`, `initrd.img`)
    
- **HTTP/FTP Server** (hosts installation ISO or Kickstart file)
    
- Optional: **Kickstart File** for unattended install
    

---

### Example Setup

#### 1. **Install required packages**

bash

CopyEdit

`dnf install dhcp-server tftp-server syslinux httpd`

#### 2. **Configure DHCP**

Example `/etc/dhcp/dhcpd.conf`:

conf

CopyEdit

`subnet 192.168.1.0 netmask 255.255.255.0 {   range 192.168.1.100 192.168.1.200;   option routers 192.168.1.1;   filename "pxelinux.0";   next-server 192.168.1.10;  # IP of TFTP server }`

#### 3. **Configure TFTP**

Enable and configure `/etc/xinetd.d/tftp` or `/etc/systemd/system/tftp.service`

#### 4. **Add bootloader**

bash

CopyEdit

`cp /usr/share/syslinux/pxelinux.0 /var/lib/tftpboot/ mkdir /var/lib/tftpboot/pxelinux.cfg`

#### 5. **Configure PXE menu**

`/var/lib/tftpboot/pxelinux.cfg/default`:

text

CopyEdit

`DEFAULT linux LABEL linux   KERNEL vmlinuz   APPEND initrd=initrd.img inst.repo=http://192.168.1.10/centos9/`

#### 6. **Place ISO contents under `/var/www/html/centos9`** and start Apache:

bash

CopyEdit

`mount -o loop CentOS-Stream-9-x86_64-dvd1.iso /mnt cp -r /mnt/* /var/www/html/centos9/ systemctl start httpd`

---

### 🎯 Outcome

When a new PC boots with PXE:

- It gets an IP from the PXE/DHCP server
    
- Downloads bootloader and menu
    
- Loads CentOS installer
    
- Installs from network
    

---

## 🧠 Common Use Cases

- Mass OS deployments in data centers or schools
    
- Automated installations using Kickstart
    
- Recovery environments (e.g., booting Clonezilla or GParted Live)





This guide walks through setting up a PXE boot server to install Linux over a network.

  

---

  

## 🔧 Required Packages

  

```bash

sudo dnf install dhcp-server tftp-server syslinux httpd xinetd -y

```

  

---

  

## 📁 Directory Structure

  

- TFTP root: `/var/lib/tftpboot/`

- HTTP root: `/var/www/html/centos9/`

  

---

  

## 📄 DHCP Server Configuration

  

File: `/etc/dhcp/dhcpd.conf`

  

```conf

subnet 192.168.1.0 netmask 255.255.255.0 {

  range 192.168.1.100 192.168.1.200;

  option routers 192.168.1.1;

  filename "pxelinux.0";

  next-server 192.168.1.10;  # PXE/TFTP Server IP

}

```

  

Enable and start DHCP:

```bash

systemctl enable --now dhcpd

```

  

---

  

## 📄 TFTP Server Configuration

  

### Systemd

  

Enable TFTP in `/etc/xinetd.d/tftp`:

```conf

service tftp

{

        socket_type             = dgram

        protocol                = udp

        wait                    = yes

        user                    = root

        server                  = /usr/sbin/in.tftpd

        server_args             = -s /var/lib/tftpboot

        disable                 = no

}

```

  

Then start services:

```bash

systemctl enable --now xinetd

```

  

---

  

## 📁 PXE Bootloader Files

  

```bash

cp /usr/share/syslinux/pxelinux.0 /var/lib/tftpboot/

mkdir -p /var/lib/tftpboot/pxelinux.cfg

```

  

---

  

## 📄 PXE Menu Configuration

  

File: `/var/lib/tftpboot/pxelinux.cfg/default`

  

```text

DEFAULT menu.c32

PROMPT 0

TIMEOUT 100

ONTIMEOUT linux

  

LABEL linux

  MENU LABEL Install CentOS 9

  KERNEL vmlinuz

  APPEND initrd=initrd.img inst.repo=http://192.168.1.10/centos9/

```

  

---

  

## 🌐 HTTP Server for Installation Files

  

1. Mount ISO and copy content:

  

```bash

mount -o loop CentOS-Stream-9-x86_64-dvd1.iso /mnt

mkdir -p /var/www/html/centos9

cp -r /mnt/* /var/www/html/centos9/

```

  

2. Start Apache:

  

```bash

systemctl enable --now httpd

```

  

---

  

## ⬇️ Download Kernel & Initrd to TFTP root

  

```bash

cp /mnt/images/pxeboot/{vmlinuz,initrd.img} /var/lib/tftpboot/

```

  

---

  

## ✅ Final Check

  

- Boot client PC with PXE enabled in BIOS/UEFI

- Ensure it's on the same network

- PXE menu should appear and start CentOS installation

  

---

  

## 🎯 Optional: Kickstart for Unattended Install

  

You can add a `ks=` line to the `APPEND` in pxelinux.cfg/default for automated installs.

  

---

  

**Done! PXE boot environment is now ready.**