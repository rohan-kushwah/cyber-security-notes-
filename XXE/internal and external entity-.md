.

---

## 🔖 Internal vs External Entities in XML

### 🏠 Internal Entity

The value is **defined directly** inside the DTD itself.

xml

```xml
<!ENTITY greeting "Hello, World!">
```

- Value is hardcoded in the document
- Self-contained, no outside reference
- Safe — parser never leaves the document

---

### 🌍 External Entity

The value is **fetched from an outside source** using `SYSTEM` or `PUBLIC` keyword.

xml

````xml
<!ENTITY name SYSTEM "URI">
```

The URI can point to:
| URI Example | What it accesses |
|---|---|
| `http://example.com/data.xml` | Remote web resource |
| `file:///etc/hostname` | Local file system |
| `ftp://internal-server/` | FTP resource |

---

## 🧠 Quick Identification Rule

When reading XML, just look for the keyword:
```
<!ENTITY ...>         → ask: does it have SYSTEM or PUBLIC?
        ↓                          ↓
       NO                         YES
        ↓                          ↓
   INTERNAL                    EXTERNAL
````

---

## ✅ Simple Memory Trick

> - **Internal** = value in **"quotes"**
> - **External** = value via **SYSTEM/PUBLIC**

That's the only difference! Would you like to know how parsers handle these differently?

<?xml version="1.0" encoding="UTF-8"?><stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>

thus is interna;l orn external

16:09

This is **neither** — it contains **no entities at all**! 😊

---

## 🔍 Analysis of Your XML

xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
  <productId>1</productId>
  <storeId>1</storeId>
</stockCheck>
```

This is just a plain **XML document** with:

|Part|What it is|
|---|---|
|`<?xml version="1.0" encoding="UTF-8"?>`|XML declaration|
|`<stockCheck>`|Root element|
|`<productId>1</productId>`|Child element with value|
|`<storeId>1</storeId>`|Child element with value|

---

## ❌ Why No Entities?

For entities to exist, you'd need a `<!DOCTYPE>` block with `<!ENTITY>` declarations, like:

xml

```xml
<!-- Internal entity added -->
<!DOCTYPE stockCheck [
  <!ENTITY store "MainStore">
]>
```

xml

```xml
<!-- External entity added -->
<!DOCTYPE stockCheck [
  <!ENTITY store SYSTEM "http://example.com/store.xml">
]>
```

---

## 🧠 Key Takeaway

> A plain XML document with just **elements and values** has **no entities** — internal or external.

Entities only exist when explicitly declared in a `<!DOCTYPE [ ]>` block.

Want to learn how to **identify XXE vulnerabilities** in such endpoints conceptually?