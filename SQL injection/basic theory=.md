# SQL Injection (SQLi)

## SQL Injection (SQLi)

SQL Injection (SQLi) is a common web application vulnerability that occurs when untrusted input is concatenated into SQL queries without proper sanitization or parameterization, allowing attackers to manipulate queries and interact with the database in unintended ways. It is not a flaw in the database itself, but in the way web applications handle user input before querying the database.

---

## What SQL Injection Can Do

- Retrieve sensitive information (usernames, passwords, financial data).
- Modify data (insert, update, delete records).
- Execute administrative operations (e.g., shutting down a database).
- Bypass authentication and impersonate users.
- In advanced cases, read system files or execute OS commands if the database privileges allow.

---

## Why SQL Injection Happens

- Developers directly concatenate user input into SQL queries.
- Lack of parameterized queries or stored procedures.
- Inadequate input validation and sanitization.
- Over-reliance on firewalls, IDS/IPS, or SSL without addressing application-layer flaws.

---

## Common Misconceptions

- **"Firewalls protect against SQL Injection"**: Firewalls only protect ports; SQLi happens inside an allowed port (e.g., 80/443).
- **"IDS detects SQL Injection"**: IDS only detects _known_ patterns; customized attacks bypass it.
- **"SSL protects against SQL Injection"**: SSL encrypts traffic but does not stop malicious input—it only hides it from network sniffers.

---

## Types of SQL Injection

### Classic SQL Injection

Direct injection of malicious SQL into input fields.

### Blind SQL Injection

When error messages are hidden; attackers infer data using true/false responses or timing attacks.

### Union-Based SQL Injection

Leveraging the `UNION` operator to retrieve data from other tables.

### Error-Based SQL Injection

Forcing database errors to reveal structural information.

### Out-of-Band SQL Injection

Using external communication (e.g., DNS/HTTP requests) when the application is configured for it.

---

## Prevention Techniques

- Use **parameterized queries** (prepared statements) instead of string concatenation.
- Employ **stored procedures** with strict validation.
- Implement **input validation** (whitelisting acceptable inputs).
- Enforce **least privilege access** on database accounts.
- Use modern **ORM frameworks** carefully (still validate inputs).
- Regularly **conduct penetration tests** to detect vulnerabilities.

---

## Architecture Diagram

```
Port No. 80,443
┌──────────┐
│ Attacker │──────┐
└──────────┘      │
                  ↓
              ┌─────────┐
              │Firewall │──────┐
              └─────────┘      │
                               ↓
                    ┌─────────────────────┐
                    │   ┌───────────────┐ │
                    │   │   Accounts    │ │
                    │   │Administration │ │
                    │   │  E-Commerce   │ │
                    │   └───────────────┘ │
                    │    Custom Code      │──────┐
                    └─────────────────────┘      │
                               ↑                 ↓
                    ┌──────────────┐      ┌──────────┐
                    │  App Server  │      │ Firewall │
                    ├──────────────┤             │
                    │  Web Server  │             ↓
                    ├──────────────┤      ┌──────────┐
                    │      OS      │      │ Database │
                    └──────────────┘      └──────────┘
```

The diagram illustrates how an attacker can bypass firewalls (which only protect ports) and inject malicious SQL through allowed ports into custom code that interacts with the database.

---

## Key Takeaways

1. SQL Injection is an **application-layer vulnerability**, not a network or database issue.
2. Firewalls, IDS, and SSL do **not prevent SQL Injection**.
3. **Parameterized queries** and **input validation** are the primary defenses.
4. Regular security testing is essential to identify and fix vulnerabilities.
5. Follow the **principle of least privilege** for database access.