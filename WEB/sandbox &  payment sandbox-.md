# Sandbox in Web Application Security

  

A **sandbox** is a **controlled, isolated environment** used to run untrusted or potentially harmful code, files, or programs **safely** without affecting the host system.

  

## Why Use a Sandbox?

  

- Prevent system compromise

- Test untrusted code **safely**

- Analyze malware behavior

- Contain exploits during vulnerability research

- Secure application components (e.g., browser tabs, plugins)

  

## Types of Sandboxes

  

### 1. Browser Sandboxing

  

- Isolates each browser tab or plugin from system-level access.

- Prevents malicious websites from affecting your OS.

  

### 2. Operating System Sandboxes

  

- Tools like:

    - Firejail (Linux)

    - Windows Sandbox

    - AppArmor, SELinux

    - Docker containers (lightweight sandboxing)

  

### 3. Application-Level Sandboxing

  

- Web apps or mobile apps may sandbox user sessions, scripts, or file uploads to prevent escape or abuse.

  

### 4. Online Sandbox Tools

  

- Used for malware analysis or file detonation:

    - Any.Run

    - Joe Sandbox

    - Hybrid Analysis

  

## Sandboxing in Penetration Testing

  

- Pentesters may **bypass or escape** sandboxes to demonstrate privilege escalation.

- Exploiting sandbox escape vulnerabilities (e.g., browser escape, Docker container breakout) is **critical impact**.

  

---
 🔐 What is Payment Sandbox?

  

A **payment sandbox** is a **mock version of the live payment system**, built to allow developers to:

  

- **Integrate** the payment gateway

- **Test** various scenarios (success, failure, refund, fraud, etc.)

- **Avoid real financial impact**

- **Use fake card numbers and credentials**

  

---

  

## ⚙️ How It Works:

  

1. **You create a sandbox account** with the payment provider (e.g., Stripe, Razorpay).

2. You get **sandbox API keys** (test keys).

3. You integrate those keys into your app.

4. You use **test card numbers** (e.g., `4242 4242 4242 4242` for Visa on Stripe).

5. You initiate payments like normal — but:

    - No actual money moves.

    - No banks are contacted.

    - The system **pretends** everything is real.

6. The server responds with **mock success or failure**.

  

---

  

## 💳 Example (Stripe):

  

- **Test Card**: `4242 4242 4242 4242`

- **Any future expiry**, any 3-digit CVV

- **Payment Gateway** behaves exactly like real mode:

    - Generates payment ID

    - Accepts webhooks

    - Redirects to success/failure

    - Can trigger 3D Secure page

  

But **nothing is actually charged**.

  

---

  

## 🔧 Internals (How It's Made):

  

Internally, a sandbox mode is like this:

  

- **Separate environment**

    - Different database

    - Different API endpoint (`https://api.sandbox.razorpay.com`)

- **Simulation engine**

    - It reads card numbers, expiry, CVV

    - If they match known test data → simulate result

- **Webhook simulator**

    - Sends fake events like `payment_success`, `payment_failed`, etc.

- **Logging**

    - Every transaction is logged like real, for debugging

- **Security disabled**

    - Real banking, anti-fraud, KYC, and OTP verification is not active

  

---

  

## 🔄 Switching to Live Mode:

  

After testing is complete:

  

1. Replace **sandbox API keys** with **live keys**

2. All other code remains same

3. Real transactions begin

  

---

  

## 🧪 Why Use It?

  

- Prevents loss from mistakes in logic

- Lets you simulate:

    - Failed payments

    - Card declined

    - Insufficient funds

    - Network errors

- Gives **stable development environment** without risk

  

---

  

## 🧱 Summary:

  

> A **payment sandbox** is a **fake, isolated environment** that mimics the behavior of real payment processing so developers can build and test apps **safely**, without using real money or cards.

  

---

  

## ✅ PART 1: Real vs Sandbox Endpoint

  

### 🔹 Example (PayU):

  

- **Live mode API endpoint** → `https://secure.payu.in` or `https://api.payu.com`

- **Sandbox mode API endpoint** → `https://sandbox.payu.in` or `https://sandbox.api.payu.com`

  

Other gateways also follow the same pattern:

  

|Gateway|Live Endpoint|Sandbox/Test Endpoint|

|---|---|---|

|Razorpay|`https://api.razorpay.com`|`https://api.razorpay.com` (same, but test key)|

|Stripe|`https://api.stripe.com`|`https://api.stripe.com` (same, but test key)|

|PayPal|`https://api.paypal.com`|`https://api.sandbox.paypal.com`|

  

👉 Some use different **URLs**, some use **same URL but different API keys** (test vs live).

  

---

  

## ✅ PART 2: How Sandbox Works on Application Side

  

### 🔸 Application has 2 modes:

  

1. **Live Mode**

    - Uses **live API keys**

    - Uses **real payment endpoint**

    - Transactions are real

2. **Sandbox/Test Mode**

    - Uses **test/sandbox API keys**

    - Sends requests to **sandbox endpoint**

    - No real money involved

  

---

  

### 🔧 How does the application know what to use?

  

In the app code (usually in backend like PHP/Node.js):

  

```js

// Example (Node.js / pseudo-code)

if (ENVIRONMENT === "sandbox") {

    gateway_url = "https://sandbox.payu.in";

    api_key = "sandbox_key";

} else {

    gateway_url = "https://secure.payu.in";

    api_key = "live_key";

}

```

  

So the developer **switches keys and endpoint** based on the config.

  

If a developer **forgets to switch from sandbox to live**, the app **stays in test mode**, and you can make fake transactions with sandbox cards.

  

---

  

## ✅ PART 3: Why OWASP Juice Shop Accepts Any Test Card?

  

OWASP Juice Shop is a **deliberately insecure app for hacking practice**.

  

### It simulates a fake payment system.

  

It’s not connected to any real payment gateway like Stripe, Razorpay, PayU.

  

What it actually does:

  

- Accepts **any card number** (even if from Google sandbox lists)

- Pretends that a payment was successful

- No real API call is made to any payment server

  

### 🔍 Internals (Juice Shop):

  

- In backend: card input is probably just checked against regex like `XXXX XXXX XXXX XXXX`

- Then hardcoded logic: “if card valid, say payment success”

- There’s **no real gateway**, no API key, no endpoint

  

That’s why **you can enter any card number from sandbox docs**, and it works — **it just fakes it**.

  

---

  

## ❗️What Happens If a Developer Leaves Sandbox Code in Production?

  

> The site will **pretend to accept payments**, but **no money will actually be charged**.

  

And attacker can:

  

- Use known test card numbers (e.g., `4111 1111 1111 1111`)

- Trigger fake "success" payments

- Get premium features, goods, or account upgrades **without paying**

  

### 🧨 It's a real-world vulnerability:

  

- Often listed under **OWASP A5: Security Misconfiguration**

- It's a form of **bypass due to misconfigured payment flow**

  

---

  

## ✅ Summary

  

|Concept|Meaning|

|---|---|

|Sandbox|Fake/test payment environment with fake cards and no real money|

|App-side handling|Uses sandbox API keys and test endpoints in config/code|

|Juice Shop|Doesn't call real gateways; just simulates the logic|

|Developer mistake|If test mode stays enabled in production, attacker can bypass payment|

|Fake cards work|Because test gateways accept them and no real validation happens|

  

---

---

## 🔵 **International Payment Gateways**

  

|Name|Supported Regions|Notes|

|---|---|---|

|**Stripe**|40+ countries|Developer-friendly, popular in US/EU|

|**PayPal**|Global|Widely used, also supports subscriptions|

|**Square**|US, Canada, Japan, Australia|POS + Online payments|

|**Braintree**|Global (owned by PayPal)|Advanced API, recurring payments, vaulting|

|**Adyen**|Global|Enterprise-grade, used by Uber, Spotify|

|**Checkout.com**|Global|Fast-growing, modern REST API|

|**2Checkout / Verifone**|200+ countries|Good for SaaS billing|

|**Authorize.Net**|US/Canada/UK|Part of Visa, stable enterprise option|

|**Worldpay**|Global|Used by large enterprises|

|**Amazon Pay**|US, UK, India, Japan|Uses Amazon account to pay|

|**Klarna**|EU, US|Buy-now-pay-later support|

|**Mollie**|EU|Easy-to-integrate, Netherlands-based|

|**BlueSnap**|Global|All-in-one platform|

  

---

  

## 🇮🇳 **Payment Gateways in India**

  

|Name|Notes|

|---|---|

|**Razorpay**|Very popular, developer-friendly, great for startups|

|**PayU**|Used by many Indian merchants, strong documentation|

|**Cashfree**|Easy integration, supports payouts + collections|

|**CCAvenue**|One of the oldest in India|

|**Instamojo**|Easy setup for small sellers and creators|

|**Paytm Payment Gateway**|Backed by Paytm ecosystem|

|**BillDesk**|Government-related and older systems|

|**PhonePe for Business**|Newer API for UPI-heavy flows|

|**Google Pay API (India)**|For UPI deep integration|

|**Atom**|Less common but still used in some enterprise setups|

  

---

  

## 🏦 **Bank-Backed Gateways**

  

|Gateway|Backed By|

|---|---|

|**HDFC Payment Gateway**|HDFC Bank|

|**ICICI First Data**|ICICI Bank|

|**Axis Bank Gateway**|Axis Bank|

|**SBIePay**|SBI|

  

These are used mostly in **corporate/government sites**, not startups.

  

---

  

## 🧾 Special-Purpose Gateways

  

|Name|Use-case|

|---|---|

|**Crypto Gateways** (Coinbase Commerce, BitPay)|Accept BTC, ETH, etc.|

|**Apple Pay**|iOS/macOS payments only|

|**Google Pay API**|Android/UPI based|

|**FastSpring**|Software and SaaS billing|

|**Paddle**|Subscription billing for SaaS|

|**Recurly / Chargebee**|Subscription management|

  

---

  

## 💡 How to Discover What Gateway a Site Uses

  

- Look at payment page URLs (`checkout.stripe.com`, `sandbox.razorpay.com`)

- DevTools → Network tab → See JS files loaded from Stripe, Razorpay, etc.

- Payment form’s hidden inputs often contain gateway keys or references

- Look for `rzp_key`, `stripe_key`, `payu_key` in JS or HTML

  

---

---

## ✅ Do All Payment Gateways Work the Same Way?

  

### ✔️ **Yes, conceptually all work similarly**:

  

|Feature|Description|

|---|---|

|**2 Modes**|All gateways offer a **sandbox (test)** and **live (real)** mode. You switch using different **API keys** or **endpoint URLs**.|

|**API-based integration**|You create orders, accept card/UPI info, confirm payments via their API.|

|**Webhook**|They send a webhook callback to notify you of real payment success.|

|**Test Cards**|Each has public card numbers to simulate transactions in **sandbox**.|

  

> So Razorpay, Stripe, PayPal, PayU, etc. all follow this model:

  

```

1. Frontend: Collect card/UPI details

2. Backend: Send payment to gateway API

3. Gateway: Shows 3D Secure / OTP if needed

4. Gateway: Calls your webhook with status

5. You verify → give access

```

  

**The main difference is in syntax, currencies, and 3D Secure requirements.**

  

---

  

## 🔶 What is RuPay?

  

### 🔹 RuPay is an **Indian domestic card scheme**, like Visa or MasterCard — but built by **NPCI** (National Payments Corporation of India).

  

|Feature|Detail|

|---|---|

|Type|Debit/Credit Card (16-digit)|

|Issuers|SBI, PNB, Axis, etc.|

|Processor|NPCI (Not Visa/MasterCard)|

|Works With|Razorpay, PayU, Stripe (India)|

  

RuPay is cheaper and preferred for **domestic Indian transactions**. Banks issue RuPay cards because the transaction fees are **lower than Visa/MasterCard**.

  

> RuPay cards support **3D Secure** via **OTP** in sandbox and live mode, like any other card.

  

---

  

## 🔷 What is UPI?

  

### 🔹 UPI = **Unified Payments Interface**

  

Built by **NPCI**, it’s a real-time payment system in India.

  

|Feature|Description|

|---|---|

|Based On|Bank-to-Bank transfer using VPA (UPI ID)|

|Requires|UPI PIN, Mobile Number linked with Bank|

|Works With|PhonePe, Google Pay, Paytm, BHIM, Razorpay UPI|

|UPI ID Format|`yourname@bank` or `mobile@upi`|

  

UPI **does not use cards** — it directly debits and credits bank accounts over IMPS.

  

---

  

## 🤔 Why is UPI Free?

  

UPI is **government-backed**. The **RBI and NPCI** mandated that:

  

- No **MDR** (Merchant Discount Rate) is charged for UPI or RuPay payments.

- So merchants don't pay transaction fees like they do with Visa/MasterCard.

- It’s part of India’s **Digital India** initiative to promote cashless transactions.

  

This is **why**:

  

|Gateway|MDR (fees) for merchant|

|---|---|

|UPI|₹0|

|RuPay|₹0|

|Visa/Master|~1.5–2.5%|

|Stripe/PayPal|~2.9–3.5%|

  

---

  

## 🛠️ How Gateways Integrate UPI and RuPay?

  

### Razorpay UPI flow:

  

```js

// JS Integration

Razorpay.createPayment({

  method: "upi",

  vpa: "rishabh@okaxis",  // user's UPI ID

});

```

  

### Razorpay RuPay card flow:

  

```json

{

  "card": {

    "number": "5081 7300 0000 0000",

    "expiry": "12/30",

    "cvv": "123"

  }

}

```

  

Gateway detects the BIN (first 6 digits `508173`) and routes it via **RuPay switch** instead of Visa.

  

---

  

## 🧪 Summary: Key Differences

  

|Method|Works Like|Costs|Used For|Gateway Handling|

|---|---|---|---|---|

|**Visa/MC**|Card-based|Paid|Global payments|Card flow → 3D Secure|

|**RuPay**|Card-based (India)|Free|Indian domestic card|Same as Visa, but via NPCI|

|**UPI**|Bank-based, no card|Free|Instant India bank transfers|VPA + UPI PIN, via IMPS|

  

---

---