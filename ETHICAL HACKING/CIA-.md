# 🛡️ CIA Triad in Cybersecurity

The **CIA Triad** is a fundamental model in cybersecurity that guides how organizations protect data and systems.  
It stands for **Confidentiality**, **Integrity**, and **Availability**.

---

## 🔒 Confidentiality
**Definition:**  
Ensuring that information is accessible **only to authorized people** and not disclosed to unauthorized individuals.

**Digital Example:**  
- Using **encryption** (like HTTPS or AES) to protect sensitive data such as passwords or credit card information.

**Real-Life Example:**  
- Locking confidential paper documents in a **safe** so only authorized personnel can access them.

---

## 🧾 Integrity
**Definition:**  
Maintaining the **accuracy and trustworthiness** of data — ensuring it is **not altered or tampered with**, intentionally or accidentally.

**Digital Example:**  
- Using **checksums** or **digital signatures** to verify that a file hasn’t been modified during transfer.

**Real-Life Example:**  
- A **bank ledger** being kept accurate so no one can change the numbers without authorization.

---

## ⚙️ Availability
**Definition:**  
Ensuring that information and systems are **accessible when needed** by authorized users.

**Digital Example:**  
- Using **redundant servers** or **DDoS protection** to keep a website online even during an attack.

**Real-Life Example:**  
- Keeping **backup generators** to ensure an office’s computers stay powered during a blackout.

---

## 🧠 Summary Table

| CIA Principle       | Meaning                | Digital Example              | Real-Life Example         |
|---------------------|------------------------|-------------------------------|----------------------------|
| **Confidentiality** | Keep data private      | Data encryption               | Locking a safe             |
| **Integrity**       | Keep data accurate     | Checksums, hashing            | Accurate bank ledger       |
| **Availability**    | Keep data accessible   | Backup servers, DDoS defense  | Backup generators          |

---

The **CIA Triad** helps balance these three principles to ensure a system’s security, reliability, and trustworthiness.

# common vulneriblity scoring system-
# 🔐 CVSS Example

## Example 1: SQL Injection Vulnerability (Web Application)

**Description:**  
A SQL injection vulnerability in a web application's login form allows attackers to extract all user data.

**CVSS v3.1 Vector String:**  
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:L


### 🧩 Metric Breakdown

| **Metric** | **Meaning** | **Value** |
|-------------|-------------|-----------|
| **AV (Attack Vector)** | Can be exploited remotely over a network | **N (Network)** |
| **AC (Attack Complexity)** | Easy to exploit | **L (Low)** |
| **PR (Privileges Required)** | No authentication needed | **N (None)** |
| **UI (User Interaction)** | No user interaction required | **N (None)** |
| **S (Scope)** | Impacts only the vulnerable component | **U (Unchanged)** |
| **C (Confidentiality)** | High data exposure (e.g., all user data) | **H (High)** |
| **I (Integrity)** | Attackers can modify database content | **H (High)** |
| **A (Availability)** | System remains available but slightly impacted | **L (Low)** |

**CVSS Base Score:** **9.1 (Critical)**

---

## Example 2: Local Privilege Escalation Vulnerability

**Description:**  
A local user can exploit a kernel flaw to gain administrative privileges.

**CVSS v3.1 Vector String:**  
CVSS:3.1/AV:L/AC:H/PR:L/UI:N/S:U/C:H/I:H/A:H


### 🧩 Metric Breakdown

| **Metric** | **Meaning** | **Value** |
|-------------|-------------|-----------|
| **AV (Attack Vector)** | Requires local access | **L (Local)** |
| **AC (Attack Complexity)** | Requires specific conditions | **H (High)** |
| **PR (Privileges Required)** | Requires low privileges | **L (Low)** |
| **UI (User Interaction)** | No user interaction required | **N (None)** |
| **S (Scope)** | Impacts only the vulnerable component | **U (Unchanged)** |
| **C (Confidentiality)** | Full read access to sensitive data | **H (High)** |
| **I (Integrity)** | Attackers can modify system components | **H (High)** |
| **A (Availability)** | Full system compromise possible | **H (High)** |

**CVSS Base Score:** **7.8 (High)**

---

✅ **Summary:**
- Example 1 (SQL Injection): **9.1 – Critical**
- Example 2 (Privilege Escalation): **7.8 – High**
