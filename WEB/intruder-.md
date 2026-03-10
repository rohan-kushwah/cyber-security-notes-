 Intruder

  

The **Intruder tool in Burp Suite** is a powerful feature used for **automated testing of web application inputs**. It allows you to **send multiple payloads** to one or more positions in a request to perform tasks like:

  

*   Brute-forcing logins

*   Enumerating user accounts

*   Testing for SQL injection, XSS, and other injection flaws

*   Discovering hidden parameters or directories

  

---

>**Note:** Password lists are at `/usr/share/seclists`.

## 🛠️ Intruder Tool - Overview

  

### 🔑 Use Case

The Intruder tool is used to **automate customized payload attacks** against specific request fields (e.g., cookies, POST data, URL params, headers).

  

### 📋 How It Works

  

1.  **Capture a request** using Burp's Proxy.

2.  **Send it to Intruder**: Right-click the request -> "Send to Intruder".

3.  **Configure attack positions**:

    *   Highlight the parts of the request you want to target.

    *   Burp will wrap them with `§` symbols (e.g., `username=§admin§`).

4.  **Set the attack type**:

    *   **Sniper** – One payload at a time, one position.

    *   **Battering ram** – Same payload in all positions.

    *   **Pitchfork** – Multiple payload sets, synchronized across positions.

    *   **Cluster bomb** – Multiple payload sets, all combinations.

  

### 🎯 Attack Types Explained

  

| Type | Description |

| :--- | :--- |

| **Sniper** | Good for single-parameter testing like XSS/SQLi. |   ek payload ek jagah

| **Battering Ram** | Sends same payload to multiple positions. Useful for repeated auth  eheaders, tokens. |  ek payload multiple jagah

| **Pitchfork** | Sends one value from each list to each position. Useful for multi-field forms. |   2 payload do position pe toh ye line by line dono payload mne se dono position pe bharega

| **Cluster Bomb** | Tries all combinations of payloads across positions. Best for multi-step brute-force. |  do positions pe do payloadd har sequence try karega

  

---

### 🔍 Example: Brute-force Login

1. Intercept login request and send to Intruder.

2. Set username and password fields as positions.

3. Choose **Cluster Bomb** attack type.

4. Load a **user list** and a **password list** as payloads.

5. Start attack and analyze **HTTP status codes**, **lengths**, or **response messages** to detect valid logins.

  

---

### 📎 Payload Options

You can use:

* Custom wordlists (like rockyou.txt)

* Numbers, dates, brute-force sequences

* Character fuzzing

* Pre-built payload types (e.g., SQLi, XSS payloads)

* Burp Extensions (for advanced payloads)

  

---

### 📊 Response Analysis

Look at the **response length**, **status code**, or **response content** to identify successful or interesting responses.

  

Use columns like:

* Status

* Length

* Words

* Lines

* Custom Grep

  

---

### ⚠️ Notes

* Intruder is **rate-limited** in the free/community version.

* For **high-speed attacks**, use **Burp Suite Professional**.

* Combine with **Grep Extract** or **Match and Replace** to extract tokens or modify request parts dynamically.

  

---

---

## **How your payloads are generated**.

  

### List-Based Payload Types

  

These are used when you already have a list of things you want to test.

  

1.  **Simple list**

    *   **What it is:** The most common and straightforward type. You provide a fixed list of strings by pasting them, typing them in, or loading them from a file.

    *   **When to use it:** You have a predefined wordlist. For example, a list of common passwords (`rockyou.txt`), usernames, directories (`/admin`, `/login`), or SQL injection payloads.

  

2.  **Runtime file**

    *   **What it is:** Similar to a simple list, but instead of loading the entire file into memory, Burp reads it one line at a time during the attack.

    *   **When to use it:** When your wordlist is **huge** (millions or billions of entries). Using a "Simple list" would consume too much RAM and could crash Burp. This is the memory-efficient option.

  

### Generator Payload Types

  

These are for when you need to create payloads based on a pattern.

  

3.  **Numbers**

    *   **What it is:** Generates a sequence of numbers. You can define the start, end, step (e.g., count by 1, 2, 5), and number format (decimal or hex).

    *   **When to use it:** Brute-forcing numeric values like user IDs (`/user?id=1`, `id=2`, ...), PIN codes, or One-Time Passwords (OTPs).

  

4.  **Dates**

    *   **What it is:** Generates a sequence of dates. You can define a start and end date, a step (e.g., every day, every week), and the format (e.g., `YYYY-MM-DD`, `DD/MM/YY`).

    *   **When to use it:** Testing date-sensitive functionality, like a booking system, to find available slots or to check for vulnerabilities in date parsing.

  

5.  **Brute forcer**

    *   **What it is:** Generates strings of a specified length using a defined character set. For example, "generate all possible 4-character strings using only lowercase letters and numbers."

    *   **When to use it:** When you have no wordlist and need to perform a true brute-force attack on something with a small keyspace, like a 4-digit PIN or a short session token.

  

6.  **Username generator**

    *   **What it is:** Takes a list of names or emails (e.g., "John Smith") and generates common username formats from them.

    *   **When to use it:** Performing username enumeration. If you have a list of employee names, this can generate likely usernames like `jsmith`, `john.smith`, `smithj`, etc., to see which ones are valid.

  

7.  **Character blocks**

    *   **What it is:** Generates strings of the same character, increasing in length. For example, with `A` as the character and a max length of 4, it generates: `A`, `AA`, `AAA`, `AAAA`.

    *   **When to use it:** Fuzzing for buffer overflow vulnerabilities or trying to discover maximum field length restrictions.

  

### Manipulation & Fuzzing Payload Types

  

These types take a base value and "mangle" or modify it to create new payloads.

  

8.  **Character substitution**

    *   **What it is:** Takes a string and applies common substitutions (e.g., `A` -> `4`, `E` -> `3`, `S` -> `5`).

    *   **When to use it:** Password guessing. If you know a user's password is "password123", you can use this to generate variations like `p4ssword123`, `p@ssw0rd123`, etc.

  

9.  **Case modification**

    *   **What it is:** Takes a string and changes its case (e.g., to uppercase, lowercase, or ToGgLe CaSe).

    *   **When to use it:** Testing for case-sensitivity issues or bypassing simple filters. For example, a web application firewall might block `<script>` but not `<sCrIpT>`.

  

10. **Bit flipper**

    *   **What it is:** A very low-level fuzzer. It takes a base string and systematically flips each bit (0 to 1, or 1 to 0) at every position.

    *   **When to use it:** Fuzzing binary data formats, session tokens, or encrypted/serialized data to look for interesting changes in the application's response (e.g., padding oracle attacks).

  

11. **Character frobber**

    *   **What it is:** Similar to the bit flipper but operates on a character level. It takes a base string and iterates through each character position, changing that character to a different one (e.g., changing its ASCII value by +1).

    *   **When to use it:** Fuzzing parameters to see how the application reacts to small, incremental changes in otherwise valid data.

  

12. **Illegal Unicode**

    *   **What it is:** Generates malformed and overlong Unicode character representations.

    *   **When to use it:** Fuzzing an application to see if it can handle malformed text properly. This can sometimes lead to crashes or bypass security filters.

  

13. **Null payloads**

    *   **What it is:** Generates a specified number of null bytes (`\x00`). The payload itself is empty.

    *   **When to use it:** Testing for null byte injection vulnerabilities. In some older applications, a null byte can terminate a string early, potentially leading to logic flaws or bypassing file path checks.

  

### Advanced & Logic-Based Payload Types

  

These are more complex and powerful for specific scenarios.

  

14. **Custom iterator**

    *   **What it is:** A highly flexible payload generator. You can define up to 8 different simple lists and then create a "template" for how to combine them. It's like a mini-Cluster Bomb attack inside a single payload position.

    *   **When to use it:** When you need to generate payloads with a complex, repeating structure. For example, generating `user1-A`, `user1-B`, `user2-A`, `user2-B` from lists `[user1, user2]` and `[A, B]`.

  

15. **Recursive grep**

    *   **What it is:** An extremely powerful type where the payload for the *next* request is taken from the response of the *previous* request.

    *   **When to use it:** Chaining requests together. For example, if a page gives you a `next_page_id` in its body, Recursive Grep can find that ID, use it as the payload for the next request, find the *new* ID in that response, and so on, to crawl through a multi-page process automatically.

  

----

----