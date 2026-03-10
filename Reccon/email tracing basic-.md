# Email Tracing & Protocols

## What is E-mail?

E-mail (Electronic Mail) allows users to send and receive messages over the internet.

## Three Parts of an Email Address

**Example:** sachin@armourinfosec.com

1. **sachin** - Account name or username
2. **@** - Separator
3. **armourinfosec.com** - Domain (could represent company, group, or department)

## Mail User Agent (MUA)

A Mail User Agent (MUA) is software that allows users to send and receive emails.

### MUA Programs:

- Microsoft Outlook
- Mozilla Thunderbird

### MUA Web-Based Clients:

- Gmail
- Yahoo Mail
- Outlook.com

---

# Mail User Agent (MUA) Programs

A Mail User Agent (MUA) is a software application that enables users to compose, send, receive, and manage their email.

## Desktop-Based MUA Programs

|MUA Program|Platform|Description|
|---|---|---|
|Microsoft Outlook|Windows, macOS|Feature-rich client, supports Exchange, POP3, IMAP|
|Mozilla Thunderbird|Windows, macOS, Linux|Free, open-source; supports add-ons and multiple accounts|
|eM Client|Windows, macOS|Simple UI, calendar integration|
|Mailbird|Windows|Lightweight, supports third-party apps|
|Apple Mail|macOS, iOS|Default client for Apple devices|
|The Bat!|Windows|Advanced filtering and encryption support|
|Claws Mail|Linux, Windows|Lightweight and fast, focused on performance|
|Evolution|Linux|GNOME desktop default email client with calendar and tasks|

## Web-Based MUA Clients (FYI)

While not installed programs, popular web-based MUAs include:

- Gmail (via browser)
- Outlook.com
- Yahoo Mail
- ProtonMail

---

# Mail Delivery Agent (MDA)

A Mail Delivery Agent (MDA) is a software component responsible for receiving email messages from the Mail Transfer Agent (MTA) and delivering them to the recipient's mailbox.

## MDA Workflow

1. [Sender MUA] → [Sender MTA] → [Recipient MTA] → **[MDA]** → [Recipient Mailbox]

- The MTA routes the email
- The MDA handles the final delivery to the correct mailbox (e.g., `/var/mail/username` or a maildir)

## Common MDA Protocols

|Protocol|Description|Default Port|Secure Port|
|---|---|---|---|
|POP3|Downloads email from server to client and deletes from server|110|995 (SSL/TLS)|
|IMAP|Accesses email stored on server; syncs across devices|143|993 (SSL/TLS)|

## Popular MDA Software

|MDA Tool|Description|
|---|---|
|Procmail|One of the oldest MDAs, supports filtering and mail sorting|
|Maildrop|Lightweight MDA, used with Courier mail server|
|Dovecot|Commonly used for IMAP/POP3 and mailbox delivery|
|Postfix (Local)|Postfix can act as an MDA when configured with `local`|
|Fetchmail|Technically a remote mail retriever, often used with MDA for local delivery|

## Key Points

- The MDA does not send emails; it only delivers them locally
- It may perform actions like:
    - Filtering spam
    - Sorting into folders
    - Triggering scripts
- MDA is often integrated with email servers like Dovecot or Postfix

---

# Mail Transfer Agent (MTA)

A Mail Transfer Agent (MTA) is a software application responsible for sending, receiving, and routing email messages between mail servers.

## What Does an MTA Do?

- Accepts outgoing mail from a Mail User Agent (MUA)
- Routes email to the destination MTA (or relays it through other MTAs)
- Hands off messages to a Mail Delivery Agent (MDA) for final delivery

## MTA Workflow

1. [Sender MUA] → **[Sender MTA]** → [Internet] → **[Recipient MTA]** → [MDA] → [Recipient Mailbox]

## Protocol Used

- SMTP (Simple Mail Transfer Protocol)

|Protocol|Purpose|Port (Unencrypted)|Port (Encrypted)|
|---|---|---|---|
|SMTP|Sending/relaying mail|25|465 (SSL), 587 (STARTTLS)|

## Popular MTA Software

|MTA Tool|Description|
|---|---|
|Postfix|Secure, fast, and popular open-source MTA (Linux default)|
|Sendmail|One of the earliest MTAs; powerful but complex|
|Exim|Flexible, often used in cPanel hosting environments|
|Qmail|Secure, modular design with less complexity|
|Microsoft Exchange Server|Enterprise-grade mail server with MTA capabilities|

## MTA Security Features

- TLS encryption for secure transmission
- SPF/DKIM/DMARC validation for sender authenticity
- Anti-spam and rate-limiting for outgoing/incoming messages

## Key Responsibilities

- Queue and retry failed email delivery attempts
- Perform DNS lookups for recipient domains
- Apply routing rules and filters
- Log all email activity for monitoring

---

# Diagram: Email Flow – MUA → MTA → MDA

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Sender's  │ ──→│   Sender's  │ ──→│ Recipient's │ ──→│ Recipient's │
│     MUA     │    │     MTA     │    │     MTA     │    │     MDA     │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
      (1)             (2) SMTP           (3) SMTP         (4) POP3/IMAP
 Compose & Send   Transfer to Internet  Receive via MX Record  Deliver to Mailbox
```

## Legend:

1. **MUA** (Mail User Agent): Email client like Outlook or Thunderbird
2. **MTA** (Mail Transfer Agent): Mail servers like Postfix, Sendmail
3. **MDA** (Mail Delivery Agent): Handles mailbox delivery using Dovecot, Procmail
4. **Protocols:**
    - MUA → MTA: uses SMTP
    - MTA → MTA: uses SMTP
    - MTA → MDA: usually local delivery
    - MDA → MUA (for reading): uses POP3 or IMAP

---

# How to Check MTA Logs

## Postfix Logs (Linux)

Postfix logs are usually located in:

```
/var/log/mail.log
```

Example command:

```
sudo grep postfix /var/log/mail.log
```

To filter specific activities (e.g., email sent):

```
sudo grep "status=sent" /var/log/mail.log
```

## Exim Logs

Common log files:

- **Main log:** `/var/log/exim/mainlog`
- **Reject log:** `/var/log/exim/rejectlog`
- **Panic log:** `/var/log/exim/paniclog`

Example to view latest email deliveries:

```
sudo tail -f /var/log/exim/mainlog
```

---

# Simple Mail Transfer Protocol (SMTP)

Used for sending emails from a client to a server or between servers.

- **Port 25** – Default (non-encrypted)
- **Port 465 (TCP)** – Secure (SSL/TLS)
- **Port 587 (TCP)** – Secure (STARTTLS)

# Post Office Protocol v3 (POP3)

Used for downloading emails from a remote server to a local client.

- **Port 110** – Default (non-encrypted)
- **Port 995 (TCP)** – Secure (encrypted)

# Internet Message Access Protocol (IMAP)

Used to access email on a remote server without downloading.

- **Port 143** – Default (non-encrypted)
- **Port 993 (TCP)** – Secure (encrypted)

---

# What is Email Tracing?

Email Tracing is the process of monitoring and analyzing email communication to gather information such as:

- Sender's IP address
- Geographic location of the sender
- Email service provider used
- Time and date of sending
- Mail server hops (via headers)

This is especially useful in cyber forensics, incident response, and phishing investigations.

# How Email Tracing Works

When someone sends an email, each mail server it passes through adds a "Received" header. These headers help trace the route the email took.

By examining the email headers, investigators can:

- Identify the source IP address of the sender
- Check time stamps of each mail server that handled the message
- Detect forged headers or spoofing attempts

## Example Email Header (Simplified)

```
Received: from unknown (HELO mail.example.com) (192.168.1.10)
    by mail.yourdomain.com with SMTP; Thu, 1 Jul 2025 10:00:00 +0530
```

- **192.168.1.10** — IP address of the sender (may vary depending on relay or masking)
- **mail.example.com** — Hostname used by sender
- Timestamps show delivery path and delay between servers

---