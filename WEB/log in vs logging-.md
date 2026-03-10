## 🔑 Login vs 📋 Logging

***

| Term        | Meaning                                                                | Example                            | Usage Context                     |
| ----------- | ---------------------------------------------------------------------- | ---------------------------------- | --------------------------------- |
| **Login**   | The act of signing into a system with credentials (username/password). | "User login was successful."       | Authentication / Access Control   |
| **Logging** | The process of recording system events, actions, errors, or messages.  | "Error logged in /var/log/syslog." | Monitoring / Debugging / Auditing |

<br>

### 🔑 Login
***
*   Noun: "Enter your **login** to access the system."
*   Verb (often written as "log in"): "Please **log in** using your credentials."

Examples:
```bash
ssh user@192.168.1.10
# You are performing a login here
```

<br>

### 📋 Logging
***
*   Refers to recording events like user activity, system errors, or debug info.
*   Handled by logging frameworks (e.g., syslog, Log4j, Winston, etc.)

Examples:
```python
import logging
logging.warning("Login failed for user admin")
```

```bash
tail -f /var/log/auth.log
# Viewing login attempts and system logging info
```

<br>

### 👀 Quick Tip to Remember
***
*   "Login" = user action to *access* a system
*   "Logging" = system action to *record* that or any other event

---
---