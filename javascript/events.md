# JavaScript Events - Complete Notes

# What is an Event?

An event is an action or occurrence that happens in the browser.

Events are generated when users interact with a webpage.

Examples:

* Clicking a button
* Typing in an input field
* Submitting a form
* Hovering over an element
* Pressing a keyboard key
* Scrolling the page

Think of it like:

```text
User Action
      ↓
    Event
      ↓
JavaScript Responds
```

---

# Why Do We Need Events?

Without events, web pages would be static.

HTML:

```html
<button>Click Me</button>
```

Nothing happens when clicked.

Using Events:

```javascript
button.addEventListener("click",()=>{
   console.log("Clicked");
});
```

Now the page responds to user interaction.

---

# How Events Work

HTML:

```html
<button id="btn">
   Click Me
</button>
```

JavaScript:

```javascript
const button =
document.getElementById("btn");

button.addEventListener(
   "click",
   ()=>{
      console.log("Clicked");
   }
);
```

Flow:

```text
User Clicks Button
        ↓
Click Event Fires
        ↓
Event Listener Executes
        ↓
Console Output
```

---

# Event Listener

The most common way to handle events.

Syntax:

```javascript
element.addEventListener(
   eventType,
   callbackFunction
);
```

Example:

```javascript
button.addEventListener(
   "click",
   ()=>{
      console.log("Button Clicked");
   }
);
```

---

# Common Events

| Event     | Triggered When      |
| --------- | ------------------- |
| click     | User clicks         |
| dblclick  | Double click        |
| mouseover | Mouse enters        |
| mouseout  | Mouse leaves        |
| keydown   | Key pressed         |
| keyup     | Key released        |
| input     | Input changes       |
| change    | Value changes       |
| submit    | Form submitted      |
| focus     | Element focused     |
| blur      | Element loses focus |
| scroll    | Page scrolls        |

---

# Click Event

Most common event.

HTML:

```html
<button id="btn">
   Click Me
</button>
```

JavaScript:

```javascript
const button =
document.getElementById("btn");

button.addEventListener(
   "click",
   ()=>{
      console.log("Clicked");
   }
);
```

Output:

```text
Clicked
```

every time the button is clicked.

---

# Multiple Actions on Click

```javascript
button.addEventListener(
   "click",
   ()=>{

      console.log("Clicked");

      alert("Welcome");

      document.body.style.backgroundColor =
      "yellow";

   }
);
```

---

# Mouse Events

---

# Mouse Over

Triggered when cursor enters an element.

```javascript
button.addEventListener(
   "mouseover",
   ()=>{

      console.log("Mouse Entered");

   }
);
```

---

# Mouse Out

Triggered when cursor leaves.

```javascript
button.addEventListener(
   "mouseout",
   ()=>{

      console.log("Mouse Left");

   }
);
```

---

# Example

```javascript
button.addEventListener(
   "mouseover",
   ()=>{

      button.style.backgroundColor =
      "green";

   }
);

button.addEventListener(
   "mouseout",
   ()=>{

      button.style.backgroundColor =
      "white";

   }
);
```

---

# Keyboard Events

Used frequently in forms.

---

# keydown

Fires when key is pressed.

```javascript
document.addEventListener(
   "keydown",
   (e)=>{

      console.log(e.key);

   }
);
```

If user presses:

```text
A
```

Output:

```text
a
```

---

# keyup

Fires when key is released.

```javascript
document.addEventListener(
   "keyup",
   (e)=>{

      console.log(e.key);

   }
);
```

---

# Input Event

Triggered whenever input value changes.

HTML:

```html
<input id="name">
```

JavaScript:

```javascript
const input =
document.getElementById("name");

input.addEventListener(
   "input",
   (e)=>{

      console.log(
         e.target.value
      );

   }
);
```

Typing:

```text
John
```

Output:

```text
J
Jo
Joh
John
```

Real-time updates.

---

# Change Event

Fires when input loses focus after value changes.

```javascript
input.addEventListener(
   "change",
   (e)=>{

      console.log(
         e.target.value
      );

   }
);
```

---

# Form Events

Very important in React and JavaScript interviews.

---

# Submit Event

HTML:

```html
<form id="form">

   <input type="text">

   <button>
      Submit
   </button>

</form>
```

JavaScript:

```javascript
const form =
document.getElementById("form");

form.addEventListener(
   "submit",
   ()=>{

      console.log(
         "Form Submitted"
      );

   }
);
```

---

# Default Browser Behavior

When form submits:

```text
Form Submitted
      ↓
Page Refreshes
```

This often causes problems.

---

# Prevent Form Refresh

Use:

```javascript
e.preventDefault();
```

Example:

```javascript
form.addEventListener(
   "submit",
   (e)=>{

      e.preventDefault();

      console.log(
         "Form Submitted"
      );

   }
);
```

Now:

```text
Form Submitted
      ↓
No Refresh
```

Very important for:

* React Forms
* Login Pages
* Registration Pages
* Search Forms

---

# Event Object

Whenever an event occurs, JavaScript creates an event object.

Example:

```javascript
button.addEventListener(
   "click",
   (e)=>{

      console.log(e);

   }
);
```

Output:

```text
MouseEvent Object
```

---

# Common Event Object Properties

```javascript
e.target
```

Element that triggered event.

---

```javascript
e.type
```

Event type.

Example:

```text
click
```

---

```javascript
e.preventDefault()
```

Stops default browser action.

---

# Example

```javascript
button.addEventListener(
   "click",
   (e)=>{

      console.log(
         e.target
      );

   }
);
```

Output:

```html
<button>
```

---

# Event Bubbling

Very Important Interview Question.

HTML:

```html
<div id="parent">

   <button id="child">
      Click
   </button>

</div>
```

JavaScript:

```javascript
parent.addEventListener(
   "click",
   ()=>{
      console.log("Parent");
   }
);

child.addEventListener(
   "click",
   ()=>{
      console.log("Child");
   }
);
```

Click Button:

Output:

```text
Child
Parent
```

Why?

Event travels upward.

```text
Button
   ↑
Parent
   ↑
Body
   ↑
Document
```

This is called Event Bubbling.

---

# Stop Event Bubbling

```javascript
child.addEventListener(
   "click",
   (e)=>{

      e.stopPropagation();

      console.log("Child");

   }
);
```

Output:

```text
Child
```

Parent won't execute.

---

# Event Delegation

Attach one listener to parent instead of many children.

HTML:

```html
<ul id="list">

   <li>Apple</li>

   <li>Orange</li>

   <li>Mango</li>

</ul>
```

JavaScript:

```javascript
const list =
document.getElementById("list");

list.addEventListener(
   "click",
   (e)=>{

      console.log(
         e.target.innerText
      );

   }
);
```

Output:

```text
Apple
```

or

```text
Orange
```

or

```text
Mango
```

depending on clicked item.

---

# Why Event Delegation?

Instead of:

```javascript
item1.addEventListener();
item2.addEventListener();
item3.addEventListener();
```

Use:

```javascript
parent.addEventListener();
```

Better performance.

Used heavily in:

* Dynamic Lists
* Tables
* React Applications

---

# Real Login Form Example

HTML:

```html
<form id="loginForm">

   <input id="email">

   <button>
      Login
   </button>

</form>
```

JavaScript:

```javascript
const form =
document.getElementById(
   "loginForm"
);

form.addEventListener(
   "submit",
   (e)=>{

      e.preventDefault();

      const email =
      document.getElementById(
         "email"
      ).value;

      console.log(email);

   }
);
```

---

# Common Interview Questions

## What is an Event?

An action that occurs in the browser and can be handled using JavaScript.

---

## What is addEventListener()?

A method used to attach event handlers to elements.

---

## What is preventDefault()?

Stops default browser behavior.

Example:

```javascript
e.preventDefault();
```

Used in forms.

---

## What is Event Bubbling?

Event propagation from child to parent.

---

## What is stopPropagation()?

Stops event bubbling.

---

## What is Event Delegation?

Handling child events using a parent event listener.

---

# Most Important Event Topics

✅ Click Events

✅ Input Events

✅ Keyboard Events

✅ Submit Events

✅ Event Object

✅ preventDefault()

✅ Event Bubbling

✅ stopPropagation()

✅ Event Delegation

✅ addEventListener()

✅ Mouse Events

✅ Form Handling

These concepts are heavily used in JavaScript, React, Node.js frontends, forms, dashboards, admin panels, and MERN Stack applications.
