## Nmap Installation Commands

### Commands to install Nmap on various platforms:

**For Debian/Ubuntu-based Linux (APT)**

```
apt install nmap
```

**For Red Hat/CentOS/Fedora-based Linux (YUM)**

```
yum install nmap
```

**For UNIX Platforms (Solaris, FreeBSD, NetBSD, OpenBSD, etc.) — From Source**

- First, download the latest `nmap-*.tar.bz2` source package from [https://nmap.org/download#source](https://nmap.org/download#source).
- Then, run the following commands in order to extract, configure, compile, and install Nmap.

```
bzip2 -dc nmap-*.tar.bz2 | tar xvf -
```

```
cd nmap-*
```

```
./configure
```

```
make
```

```
make install
```

---
**Note:**
-  Use `sudo` for commands that require administrative privileges.
- Package managers like `apt` and `yum` will handle dependencies automatically.
- Building from source is recommended for platforms where pre-compiled binaries are not available or if you need the latest version.

---
## Common Nmap Commands and Usage Examples

### Checking Nmap Version

To check the installed Nmap version:

```
nmap -V
```
or
```
nmap --version
```
### Help and Usage Information

Show Help menu:
```
nmap --help
```

Show shorter help:
```
nmap -h
```
### Basic Scanning Examples

Scan a specific IP address:
```
nmap 192.168.1.35
```

Scan by hostname or domain:
```
nmap target.com
```

Scan with verbose output for detailed information:
```
nmap -v 192.168.1.35
```

Scan the official Nmap test server (authorized for scanning):
```
nmap -v scanme.nmap.org
```
(Scanme.nmap.org is provided by the Nmap project for testing Nmap scans and learning purposes.)

---
---