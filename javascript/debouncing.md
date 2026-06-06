# JavaScript Debouncing - Complete Notes

# What is Debouncing?

Debouncing is a technique used to limit how often a function executes.

Instead of executing a function immediately every time an event occurs, debouncing waits for a specified delay before executing the function.

If the event occurs again before the delay finishes, the timer resets.

Think of it like:

```text
User Keeps Typing
        ↓
Keep Waiting
        ↓
User Stops Typing
        ↓
Execute Function Once
```

---

# Why Do We Need Debouncing?

Imagine a search bar.

HTML:

```html
<input type="text" id="search">
```

JavaScript:

```javascript
search.addEventListener(
   "input",
   ()=>{
      fetchUsers();
   }
);
```

User types:

```text
h
he
hel
hell
hello
```

API Calls:

```text
Call 1
Call 2
Call 3
Call 4
Call 5
```

Total:

```text
5 API Requests
```

This creates:

* Unnecessary network requests
* Server load
* Poor performance
* Higher costs

---

# Desired Behavior

User types:

```text
h
he
hel
hell
hello
```

Waits 500ms.

Then:

```text
One API Call
```

Only after typing stops.

This is Debouncing.

---

# Visual Representation

Without Debouncing:

```text
h      → API Call

he     → API Call

hel    → API Call

hell   → API Call

hello  → API Call
```

Total:

```text
5 Calls
```

---

With Debouncing:

```text
h
he
hel
hell
hello

Wait...

API Call
```

Total:

```text
1 Call
```

---

# Real World Example

Google Search.

When typing:

```text
Java
```

Google does NOT send a request for:

```text
J
Ja
Jav
Java
```

immediately.

Instead:

```text
Wait User Stops Typing
        ↓
Send Request
```

Uses Debouncing.

---

# Basic Debounce Function

```javascript
function debounce(fn,delay){

   let timer;

   return function(){

      clearTimeout(timer);

      timer = setTimeout(
         fn,
         delay
      );

   };
}
```

This is one of the most asked interview questions.

---

# Understanding Step-by-Step

```javascript
let timer;
```

Stores timeout id.

Example:

```javascript
timer = 101
```

---

# clearTimeout(timer)

```javascript
clearTimeout(timer);
```

Cancels previous timer.

Without this:

```text
Timer 1
Timer 2
Timer 3
Timer 4
```

All execute.

Not debouncing.

---

# setTimeout()

```javascript
timer = setTimeout(
   fn,
   delay
);
```

Creates new timer.

Example:

```javascript
setTimeout(
   fetchUsers,
   500
);
```

Meaning:

```text
Wait 500ms
       ↓
Run Function
```

---

# How Debouncing Works Internally

User types:

```text
h
```

Timer starts:

```text
500ms
```

Before 500ms finishes:

```text
he
```

New input arrives.

Old timer:

```text
Cancelled
```

New timer starts.

Again:

```text
hel
```

Old timer:

```text
Cancelled
```

New timer starts.

Eventually:

```text
hello
```

User stops.

500ms passes.

Function executes.

---

# Timeline

```text
Type h
 ↓
Start Timer

Type he
 ↓
Cancel Previous Timer

Type hel
 ↓
Cancel Previous Timer

Type hell
 ↓
Cancel Previous Timer

Type hello
 ↓
Cancel Previous Timer

Wait 500ms
 ↓
Execute Function
```

---

# Practical Example

HTML:

```html
<input id="search">
```

JavaScript:

```javascript
function searchUsers(){

   console.log(
      "Searching..."
   );

}
```

Debounce:

```javascript
const debouncedSearch =
debounce(
   searchUsers,
   500
);
```

Use:

```javascript
search.addEventListener(

   "input",

   debouncedSearch

);
```

Now API executes only once.

---

# Better Debounce Function

Real-world implementation:

```javascript
function debounce(fn,delay){

   let timer;

   return function(...args){

      clearTimeout(timer);

      timer = setTimeout(()=>{

         fn.apply(
            this,
            args
         );

      },delay);

   };

}
```

---

# Why Use ...args?

Suppose:

```javascript
function search(value){

   console.log(value);

}
```

Without:

```javascript
...args
```

Function loses arguments.

With:

```javascript
...args
```

Arguments are preserved.

---

# Why Use apply()?

Preserves:

```javascript
this
```

and

```javascript
arguments
```

Important in React and JavaScript libraries.

---

# Search API Example

Without Debounce:

```javascript
input.addEventListener(

   "input",

   async(e)=>{

      await fetch(
         `/users?q=${e.target.value}`
      );

   }

);
```

Typing:

```text
hello
```

Creates:

```text
5 Requests
```

---

With Debounce:

```javascript
const searchUser =
debounce(

   async(e)=>{

      await fetch(
         `/users?q=${e.target.value}`
      );

   },

   500

);
```

Now:

```text
1 Request
```

---

# Window Resize Example

Without Debounce:

```javascript
window.addEventListener(

   "resize",

   ()=>{

      console.log(
         "Resizing"
      );

   }

);
```

Dragging browser:

```text
Hundreds of Calls
```

---

With Debounce:

```javascript
window.addEventListener(

   "resize",

   debounce(()=>{

      console.log(
         "Resize Finished"
      );

   },500)

);
```

Only executes once.

---

# Real World Uses

### Search Bars

```text
Amazon
Google
YouTube
```

---

### Auto Suggestions

```text
Search Suggestions
```

---

### Window Resize

```text
Responsive Layouts
```

---

### Form Validation

```text
Validate After User Stops Typing
```

---

### API Calls

```text
Reduce Server Requests
```

---

### Filtering Large Data

```text
Data Tables
```

---

# Debouncing vs Normal Execution

Without Debounce:

```text
User Types 10 Keys
        ↓
10 Function Calls
```

---

With Debounce:

```text
User Types 10 Keys
        ↓
1 Function Call
```

---

# Debouncing vs Throttling

Most Asked Interview Question.

---

# Debouncing

```text
Wait Until User Stops
```

Example:

```text
Typing Search Query
```

---

# Throttling

```text
Execute Every Fixed Interval
```

Example:

```text
Scroll Events
```

---

# Visual Comparison

Debounce:

```text
Typing
Typing
Typing
Typing
Stop
 ↓
Execute
```

---

Throttle:

```text
Typing
 ↓
Execute

Typing
 ↓
Execute

Typing
 ↓
Execute
```

---

# Interview Question

Predict Output:

```javascript
const fn =
debounce(
   ()=>{

      console.log("Run");

   },
   1000
);

fn();
fn();
fn();
fn();
```

Output:

```text
Run
```

Only once.

Because every call resets timer.

---

# Memory Visualization

```text
timer = null

fn()
 ↓
timer = 101

fn()
 ↓
clear 101
 ↓
timer = 102

fn()
 ↓
clear 102
 ↓
timer = 103

Only Timer 103 Executes
```

---

# React Example

```javascript
const searchUser =
debounce(

   async(value)=>{

      const response =
      await fetch(
         `/users?q=${value}`
      );

   },

   500

);
```

Usage:

```javascript
<input
   onChange={(e)=>{

      searchUser(
         e.target.value
      );

   }}
/>
```

Common interview scenario.

---

# Common Interview Questions

## What is Debouncing?

A technique that delays function execution until a specified delay has passed since the last event.

---

## Why Use Debouncing?

To reduce unnecessary function calls.

---

## What Problem Does Debouncing Solve?

Excessive:

* API Calls
* Search Requests
* Resize Events
* Validation Calls

---

## How Does Debouncing Work?

Using:

```javascript
clearTimeout()
```

and

```javascript
setTimeout()
```

---

## Difference Between Debounce and Throttle?

Debounce waits.

Throttle limits frequency.

---

## Where is Debouncing Used?

* Search Bars
* Auto Suggestions
* Forms
* Resize Events
* API Requests

---

# Most Important Topics

✅ Debouncing

✅ clearTimeout()

✅ setTimeout()

✅ Closures

✅ Higher Order Functions

✅ Search Optimization

✅ API Optimization

✅ Resize Optimization

✅ Debounce vs Throttle

✅ React Search Inputs

---

# Debouncing Formula

```text
Event Fires
      ↓
Cancel Previous Timer
      ↓
Start New Timer
      ↓
No New Events?
      ↓
Execute Function
```

or

```text
Debouncing
=
clearTimeout()
+
setTimeout()
+
Closure
```

Debouncing is a very important JavaScript optimization technique and is commonly used in React applications, search bars, dashboards, filtering systems, form validation, and API-driven MERN stack applications. It is one of the most frequently asked JavaScript interview topics for 2+ years experienced developers.
