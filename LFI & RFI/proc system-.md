# 🧠 proc Filesystem Abuse in Web Exploitation
### *Local File Inclusion (LFI) → Process Enumeration → Remote Code Execution (RCE)*

---

## 📁 What is `/proc`?
The **`/proc` filesystem** in Linux is a **virtual, kernel-generated filesystem**.  
It exposes **live system and process information** and is **not stored on disk**.

> `/proc` is invaluable for administrators — and extremely useful for attackers  
> when combined with **Local File Inclusion (LFI)** vulnerabilities.

---

## 🔑 Key `/proc` Files of Interest

### 1️⃣ `/proc/self/environ`
**📌 Content**
- Environment variables of the current process  
- Null-byte (`\x00`) separated

**⚔️ Offensive Use**
- Inject payloads via HTTP headers (e.g., `User-Agent`)
- Trigger execution through LFI inclusion

**🛡️ Defensive Focus**
- Sanitize logged headers
- Monitor access to `/proc/self/environ`
- Audit environment variables in web processes

---

### 2️⃣ `/proc/self/cmdline`
**📌 Content**
- Command-line arguments used to start the process

**⚔️ Offensive Use**
- Leak sensitive startup configs
- Reveal credentials, paths, flags

**🛡️ Defensive Focus**
- Avoid secrets in CLI arguments
- Use secure secret managers

---

### 3️⃣ `/proc/[PID]/fd/[FD]`
**📌 Content**
- File descriptors opened by a process  
- Symlinks to files, sockets, pipes

**⚔️ Offensive Use**
- Locate active access logs
- Abuse writable descriptors for log poisoning
- Achieve LFI → RCE

**🛡️ Defensive Focus**
- Limit process file permissions
- Monitor unusual reads of `/proc/*/fd/*`

---

## 🔥 LFI + PID = Remote Code Execution (RCE)

### 🧩 Why This Works
- LFI allows local file inclusion
- `/proc` exposes **live, writable files**
- Web servers keep logs **open via file descriptors**

---

## 🧪 Example Attack Flow (High-Level)

### 🔹 Step 1: LFI Vulnerability Exists
- Application includes files based on user input

### 🔹 Step 2: Identify Web Server PID
- Each process has a directory under `/proc/[PID]/`

### 🔹 Step 3: Enumerate File Descriptors
- Access `/proc/[PID]/fd/`
- Identify access log symlink

### 🔹 Step 4: Log Poisoning
- Inject code via HTTP headers
- Payload written into access logs

### 🔹 Step 5: Trigger Inclusion
- LFI includes the poisoned file descriptor
- Payload executes in server context

---

## 🗺️ System-Wide Recon via `/proc`

| Path | Information Exposed |
|-----|--------------------|
| `/proc/net/*` | Network connections |
| `/proc/net/arp` | Local network hosts |
| `/proc/net/tcp` | Active TCP sessions |
| `/proc/version` | Kernel version |
| `/proc/mounts` | Mounted filesystems |
| `/proc/sched_debug` | Scheduler internals |

> ⚠️ These paths assist in **lateral movement**, **container escapes**, and **kernel targeting**

---

## 🎯 Common `/proc` Targets in LFI Exploits

| Path | Purpose |
|----|----|
| `/proc/self/environ` | Environment injection |
| `/proc/self/cmdline` | Config leakage |
| `/proc/[PID]/fd/[FD]` | Log file inclusion |
| `/proc/net/tcp` | Network mapping |

---

## 🧠 Exploitation Summary

- LFI alone is dangerous
- `/proc` **amplifies impact**
- Combining LFI + PID knowledge often leads to **RCE**
- Dynamic files (logs, env, sockets) are prime targets

---

## 🛡️ Defensive Recommendations

### 🔐 Web Application Hardening
- Strict allow-lists for file inclusion
- Disable dynamic includes where possible

### 🔒 System Hardening
- Restrict `/proc` visibility
- Use AppArmor / SELinux
- Containerize services securely

### 👀 Detection & Monitoring
- Alert on access to `/proc/*/fd/*`
- Correlate LFI attempts with log anomalies
- Monitor header injection patterns

---

## 📌 Key Takeaway
> **LFI + `/proc` = One of the most reliable paths to RCE in Linux web environments**

Understanding this chain is **critical for OSCP, real-world pentesting, and blue-team defense**.

---

🧑‍💻 *Educational reference for web application security testing*
