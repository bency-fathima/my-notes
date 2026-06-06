# JavaScript Event Delegation - Complete Notes

# What is Event Delegation?

Event Delegation is a technique where instead of attaching event listeners to multiple child elements, we attach a single event listener to their parent element.

The parent handles events for all its children using:

```javascript
event.target
```

Think of it like:

```text
Parent Element
       ↓
One Event Listener
       ↓
Handles All Children
```

---

# Why Do We Need Event Delegation?

Imagine a webpage with 100 buttons.

HTML:

```html
<div id="container">

   <button>Button 1</button>

   <button>Button 2</button>

   <button>Button 3</button>

   ...

   <button>Button 100</button>

</div>
```

---

# Bad Approach

Attach listener to every button.

```javascript
const buttons =
document.querySelectorAll("button");

buttons.forEach(button => {

   button.addEventListener(
      "click",
      () => {

         console.log("Clicked");

      }
   );

});
```

---

# Problems

### More Memory Usage

100 buttons

↓

100 Event Listeners

---

### Slower Performance

Browser must maintain many listeners.

---

### Difficult Maintenance

If new buttons are added later:

```javascript
container.innerHTML +=
"<button>New Button</button>";
```

The new button won't have a listener.

---

# Better Approach

Attach one listener to parent.

```javascript
const parent =
document.getElementById(
   "container"
);

parent.addEventListener(

   "click",

   (e)=>{

      if(
         e.target.tagName ===
         "BUTTON"
      ){

         console.log("Clicked");

      }

   }

);
```

Now:

```text
100 Buttons
      ↓
1 Listener
```

Much more efficient.

---

# How Event Delegation Works

Event Delegation relies on:

```text
Event Bubbling
```

---

# What is Event Bubbling?

When an event occurs on a child element:

```text
Child
  ↑
Parent
  ↑
Body
  ↑
Document
```

The event travels upward.

---

# Example

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

Click button.

Output:

```text
Child
Parent
```

---

# Event Flow Visualization

```text
Button Click
     ↑
Parent
     ↑
Body
     ↑
Document
```

This upward movement is called:

```text
Event Bubbling
```

---

# Event Delegation Uses Bubbling

When button is clicked:

```text
Button Click
      ↓
Event Bubbles Up
      ↓
Parent Listener Receives Event
```

Parent can determine which child triggered it.

---

# event.target

Very important.

Represents:

```text
Actual Element
That Triggered Event
```

Example:

```javascript
parent.addEventListener(
   "click",
   (e)=>{

      console.log(
         e.target
      );

   }
);
```

If Button 3 is clicked:

Output:

```html
<button>
Button 3
</button>
```

---

# Example 1

HTML:

```html
<div id="container">

   <button>Save</button>

   <button>Delete</button>

   <button>Edit</button>

</div>
```

JavaScript:

```javascript
const container =
document.getElementById(
   "container"
);

container.addEventListener(

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
Save
```

or

```text
Delete
```

or

```text
Edit
```

depending on clicked button.

---

# Why Check tagName?

Suppose:

```html
<div id="container">

   <button>Save</button>

   <p>Hello</p>

</div>
```

Clicking paragraph also triggers event.

To prevent that:

```javascript
if(
   e.target.tagName ===
   "BUTTON"
){
   console.log("Clicked");
}
```

Now only buttons work.

---

# Visual Representation

```text
Container
│
├── Button
├── Button
├── Button
└── Paragraph

Only Buttons Allowed
```

---

# Dynamic Elements Problem

Imagine:

```javascript
const btn =
document.createElement(
   "button"
);

btn.innerText =
"New Button";

container.appendChild(btn);
```

New button added after page load.

---

# Without Delegation

```javascript
buttons.forEach(...)
```

New button gets:

```text
No Event Listener
```

Click won't work.

---

# With Delegation

Parent already has listener.

New button automatically works.

No extra code required.

---

# Example

```javascript
container.addEventListener(

   "click",

   (e)=>{

      if(
         e.target.tagName ===
         "BUTTON"
      ){

         console.log(
            e.target.innerText
         );

      }

   }

);
```

Works for:

```text
Existing Buttons
+
Future Buttons
```

---

# Real World Example

Todo App

HTML:

```html
<ul id="tasks">

   <li>Task 1</li>

   <li>Task 2</li>

</ul>
```

JavaScript:

```javascript
tasks.addEventListener(

   "click",

   (e)=>{

      if(
         e.target.tagName ===
         "LI"
      ){

         console.log(
            e.target.innerText
         );

      }

   }

);
```

Click:

```text
Task 1
```

Output:

```text
Task 1
```

---

# Example: Delete Button

HTML:

```html
<div id="users">

   <button data-id="1">
      Delete
   </button>

   <button data-id="2">
      Delete
   </button>

</div>
```

JavaScript:

```javascript
users.addEventListener(

   "click",

   (e)=>{

      if(
         e.target.tagName ===
         "BUTTON"
      ){

         const id =
         e.target.dataset.id;

         console.log(id);

      }

   }

);
```

Output:

```text
1
```

or

```text
2
```

depending on clicked button.

---

# Event Delegation vs Normal Events

## Normal Approach

```javascript
button1.addEventListener();
button2.addEventListener();
button3.addEventListener();
button4.addEventListener();
button5.addEventListener();
```

Total:

```text
5 Listeners
```

---

## Delegation Approach

```javascript
parent.addEventListener();
```

Total:

```text
1 Listener
```

---

# Memory Comparison

```text
1000 Buttons

Without Delegation
↓
1000 Listeners

With Delegation
↓
1 Listener
```

Huge performance improvement.

---

# Closest Method

Sometimes click happens inside nested element.

Example:

```html
<button>
   <span>Edit</span>
</button>
```

User clicks:

```html
<span>
```

Now:

```javascript
e.target
```

is:

```html
<span>
```

not button.

---

# Solution

Use:

```javascript
closest()
```

Example:

```javascript
const button =
e.target.closest(
   "button"
);
```

Now JavaScript finds nearest button.

---

# Professional Example

```javascript
container.addEventListener(

   "click",

   (e)=>{

      const button =
      e.target.closest(
         "button"
      );

      if(!button) return;

      console.log(
         button.innerText
      );

   }

);
```

Widely used in production applications.

---

# Common Interview Questions

## What is Event Delegation?

Attaching one event listener to a parent element to handle events from child elements.

---

## Why Use Event Delegation?

* Better Performance
* Less Memory Usage
* Works for Dynamic Elements

---

## What Does Event Delegation Depend On?

```text
Event Bubbling
```

---

## What is event.target?

The actual element that triggered the event.

---

## What is the Advantage Over Multiple Listeners?

One listener can manage hundreds or thousands of child elements.

---

## Why is Event Delegation Useful for Dynamic Elements?

Newly created elements automatically inherit parent event handling.

---

# Real React Usage

React internally uses concepts similar to event delegation through its Synthetic Event System.

Examples:

* Lists
* Tables
* Menus
* Dynamic Forms
* Todo Applications
* Data Grids

---

# Most Important Topics

✅ Event Bubbling

✅ event.target

✅ Parent Listener

✅ Child Elements

✅ Dynamic Elements

✅ Performance Optimization

✅ closest()

✅ tagName Checking

✅ Memory Optimization

---

# Event Delegation Formula

```text
Many Child Elements
          ↓
Single Parent Listener
          ↓
event.target
          ↓
Handle Correct Child
```

or

```text
Event Delegation
=
Event Bubbling
+
Parent Listener
+
event.target
```

Event Delegation is one of the most important DOM concepts in JavaScript and is frequently used in React applications, dynamic lists, dashboards, tables, and large-scale web applications because it improves performance, reduces memory usage, and simplifies event management.
