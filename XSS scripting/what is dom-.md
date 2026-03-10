# 🧩 What is the DOM (in a Simple Way)?

**DOM (Document Object Model) is the browser’s way of representing a webpage so JavaScript can read and change it.**

Think of the DOM as a **tree structure** made from your HTML.

Example:

HTML:

`<h1>Hello</h1> <p>Welcome!</p>`

DOM (tree):

`Document  ├── <h1>  └── <p>`

Because of the DOM, JavaScript can:

- change text
    
- change colors
    
- add or remove elements
    
- react to clicks
    

Example:

`document.getElementById("title").innerText = "New Title";`

This changes the webpage **without reloading** — thanks to the DOM.

---

# 🧠 Ultra-Simple Definition

**DOM = The structure of the webpage that JavaScript can control.**