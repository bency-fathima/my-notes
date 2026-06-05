# JavaScript DOM (Document Object Model) - Complete Notes

# What is DOM?

DOM stands for **Document Object Model**.

The DOM is a programming interface that represents an HTML document as a tree of JavaScript objects.

In simple words:

```text
HTML Page
    ↓
Browser
    ↓
DOM
    ↓
JavaScript Can Access & Modify It
```

---

# Why DOM is Needed?

HTML alone creates static pages.

Example:

```html
<h1>Hello</h1>
```

This displays content but cannot change dynamically.

Using JavaScript + DOM:

```javascript
document.querySelector("h1").innerText = "Welcome";
```

Output:

```html
<h1>Welcome</h1>
```

Now the page becomes interactive.

---

# How DOM Works

HTML:

```html
<html>
   <body>
      <h1 id="title">Hello</h1>
      <button>Click Me</button>
   </body>
</html>
```

Browser converts it into:

```text
Document
│
└── html
     │
     └── body
          │
          ├── h1
          │     └── Hello
          │
          └── button
                └── Click Me
```

This structure is called the **DOM Tree**.

---

# Document Object

The top-level object in the DOM.

```javascript
console.log(document);
```

Output:

```text
Entire HTML Document
```

Every DOM operation starts from:

```javascript
document
```

---

# Selecting Elements

Before changing anything, JavaScript must find the element.

---

# 1. getElementById()

HTML:

```html
<h1 id="title">Hello</h1>
```

JavaScript:

```javascript
const heading =
document.getElementById("title");
```

Output:

```javascript
<h1>Hello</h1>
```

---

# Visual

```text
HTML
 ↓
id="title"
 ↓
getElementById()
 ↓
Element Object
```

---

# 2. querySelector()

Selects first matching element.

HTML:

```html
<h1>Hello</h1>
<h1>Welcome</h1>
```

JavaScript:

```javascript
const heading =
document.querySelector("h1");
```

Output:

```html
<h1>Hello</h1>
```

Only first match.

---

# Select By Class

HTML:

```html
<h1 class="title">Hello</h1>
```

JavaScript:

```javascript
const heading =
document.querySelector(".title");
```

---

# Select By ID

```javascript
document.querySelector("#title");
```

---

# 3. querySelectorAll()

Returns all matching elements.

HTML:

```html
<li>Apple</li>
<li>Orange</li>
<li>Mango</li>
```

JavaScript:

```javascript
const items =
document.querySelectorAll("li");
```

Output:

```javascript
NodeList(3)
```

Access:

```javascript
console.log(items[0]);
```

Output:

```html
<li>Apple</li>
```

---

# Changing Content

One of the most common DOM operations.

---

# innerText

HTML:

```html
<h1 id="title">Hello</h1>
```

JavaScript:

```javascript
const heading =
document.getElementById("title");

heading.innerText = "Welcome";
```

Output:

```html
<h1>Welcome</h1>
```

---

# innerHTML

Allows HTML content.

```javascript
heading.innerHTML =
"<span>Welcome</span>";
```

Output:

```html
<h1>
   <span>Welcome</span>
</h1>
```

---

# Difference Between innerText and innerHTML

## innerText

```javascript
heading.innerText =
"<b>Hello</b>";
```

Output:

```html
<b>Hello</b>
```

Shown as plain text.

---

## innerHTML

```javascript
heading.innerHTML =
"<b>Hello</b>";
```

Output:

```html
Hello
```

Displayed in bold.

---

# textContent

Another way to update text.

```javascript
heading.textContent =
"Welcome";
```

Similar to innerText but includes hidden text.

---

# Changing Styles

DOM allows changing CSS.

HTML:

```html
<h1 id="title">Hello</h1>
```

JavaScript:

```javascript
const heading =
document.getElementById("title");

heading.style.color = "red";
```

Output:

```html
Red Heading
```

---

# Change Multiple Styles

```javascript
heading.style.color = "red";

heading.style.backgroundColor = "yellow";

heading.style.fontSize = "40px";
```

---

# Example

```javascript
const heading =
document.getElementById("title");

heading.style.color = "white";

heading.style.backgroundColor = "black";

heading.style.padding = "10px";
```

---

# Working with Classes

Instead of changing styles directly.

HTML:

```html
<h1 id="title">Hello</h1>
```

CSS:

```css
.active{
   color:white;
   background:black;
}
```

JavaScript:

```javascript
heading.classList.add("active");
```

---

# Remove Class

```javascript
heading.classList.remove("active");
```

---

# Toggle Class

```javascript
heading.classList.toggle("active");
```

Very common in:

* Menus
* Sidebars
* Dark Mode

---

# Creating Elements

DOM allows creating new HTML elements dynamically.

---

# createElement()

```javascript
const p =
document.createElement("p");
```

Creates:

```html
<p></p>
```

Not yet visible.

---

# Add Content

```javascript
p.innerText =
"New Paragraph";
```

Now:

```html
<p>New Paragraph</p>
```

Still not visible.

---

# appendChild()

Adds element to page.

```javascript
document.body.appendChild(p);
```

Now visible.

---

# Complete Example

```javascript
const p =
document.createElement("p");

p.innerText =
"Hello DOM";

document.body.appendChild(p);
```

Output:

```html
<p>Hello DOM</p>
```

---

# Creating Lists Dynamically

```javascript
const ul =
document.createElement("ul");

const li =
document.createElement("li");

li.innerText = "Apple";

ul.appendChild(li);

document.body.appendChild(ul);
```

Output:

```html
<ul>
   <li>Apple</li>
</ul>
```

---

# Removing Elements

HTML:

```html
<p id="msg">
   Hello
</p>
```

JavaScript:

```javascript
const msg =
document.getElementById("msg");

msg.remove();
```

Element disappears.

---

# Replacing Elements

```javascript
const newHeading =
document.createElement("h1");

newHeading.innerText =
"Welcome";
```

Replace:

```javascript
oldHeading.replaceWith(
   newHeading
);
```

---

# Parent and Child Elements

HTML:

```html
<div id="parent">

   <h1>Hello</h1>

   <p>World</p>

</div>
```

---

# Access Parent

```javascript
const h1 =
document.querySelector("h1");

console.log(
   h1.parentElement
);
```

Output:

```html
<div id="parent">
```

---

# Access Children

```javascript
const parent =
document.getElementById("parent");

console.log(
   parent.children
);
```

Output:

```javascript
HTMLCollection(2)
```

---

# Traversing DOM

## First Child

```javascript
parent.firstElementChild
```

---

## Last Child

```javascript
parent.lastElementChild
```

---

## Next Sibling

```javascript
element.nextElementSibling
```

---

## Previous Sibling

```javascript
element.previousElementSibling
```

---

# Common DOM Methods

| Method             | Purpose            |
| ------------------ | ------------------ |
| getElementById()   | Select by ID       |
| querySelector()    | Select first match |
| querySelectorAll() | Select all matches |
| createElement()    | Create element     |
| appendChild()      | Add element        |
| remove()           | Remove element     |
| replaceWith()      | Replace element    |

---

# Real MERN Example

Display users from API.

```javascript
const users = [
   {
      name:"John"
   },
   {
      name:"Alex"
   }
];
```

```javascript
users.forEach(user=>{

   const p =
   document.createElement("p");

   p.innerText =
   user.name;

   document.body
   .appendChild(p);

});
```

Output:

```html
<p>John</p>
<p>Alex</p>
```

---

# Interview Questions

## What is DOM?

DOM is a tree-like representation of an HTML document that JavaScript can manipulate.

---

## Difference Between innerText and innerHTML?

### innerText

Treats content as plain text.

### innerHTML

Parses content as HTML.

---

## Difference Between querySelector and querySelectorAll?

### querySelector

Returns first matching element.

### querySelectorAll

Returns all matching elements.

---

## How to Create an Element?

```javascript
const div =
document.createElement("div");
```

---

## How to Add Element to Page?

```javascript
document.body.appendChild(div);
```

---

# Most Important DOM Topics

✅ DOM Tree

✅ document Object

✅ getElementById()

✅ querySelector()

✅ querySelectorAll()

✅ innerText

✅ innerHTML

✅ textContent

✅ style Property

✅ classList

✅ createElement()

✅ appendChild()

✅ remove()

✅ replaceWith()

✅ Parent & Child Nodes

✅ DOM Traversal

These concepts form the foundation for Events, Event Delegation, Forms, React Virtual DOM, and Frontend Development.
