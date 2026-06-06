# JavaScript Throttling - Complete Notes

# What is Throttling?

Throttling is a technique used to limit how often a function can execute within a given period of time.

Instead of executing a function every time an event occurs, throttling allows execution only once in a specified interval.

Think of it like:

```text
Event Fires Continuously
         ↓
Allow Execution
         ↓
Block Further Calls
         ↓
Wait for Delay
         ↓
Allow Again
```

---

# Why Do We Need Throttling?

Imagine a user scrolling a webpage.

```javascript
window.addEventListener(
   "scroll",
   ()=>{
      console.log("Scrolling");
   }
);
```

When scrolling:

```text
Scroll Event
Scroll Event
Scroll Event
Scroll Event
Scroll Event
Scroll Event
...
```

Hundreds of events fire every second.

Problems:

❌ High CPU Usage

❌ Performance Issues

❌ Excessive Function Calls

❌ Laggy User Experience

---

# Desired Behavior

Instead of:

```text
100 Calls Per Second
```

We want:

```text
1 Call Every Second
```

or

```text
1 Call Every 500ms
```

This is Throttling.

---

# Visual Representation

Without Throttling:

```text
Scroll
 ↓
Function

Scroll
 ↓
Function

Scroll
 ↓
Function

Scroll
 ↓
Function
```

100+ executions.

---

With Throttling:

```text
Scroll
 ↓
Function

Wait

Scroll
 ↓
Ignored

Scroll
 ↓
Ignored

Wait Finished

Scroll
 ↓
Function
```

Much fewer executions.

---

# Basic Throttle Function

```javascript
function throttle(fn,delay){

   let allowed = true;

   return function(){

      if(!allowed) return;

      fn();

      allowed = false;

      setTimeout(()=>{

         allowed = true;

      },delay);

   }

}
```

This is one of the most asked interview questions.

---

# Understanding Step-by-Step

## Step 1

```javascript
let allowed = true;
```

Indicates whether function execution is allowed.

Initially:

```text
allowed = true
```

---

## Step 2

```javascript
if(!allowed) return;
```

Check permission.

If:

```text
allowed = false
```

Function exits immediately.

---

## Step 3

```javascript
fn();
```

Execute original function.

---

## Step 4

```javascript
allowed = false;
```

Block future executions.

---

## Step 5

```javascript
setTimeout(()=>{

   allowed = true;

},delay);
```

After delay:

```text
Execution Allowed Again
```

---

# Timeline Example

Throttle Delay:

```javascript
1000ms
```

User Scrolls:

```text
0ms   → Execute

100ms → Ignored

200ms → Ignored

500ms → Ignored

1000ms → Execute

1100ms → Ignored

2000ms → Execute
```

Output:

```text
Run
Run
Run
```

Only once every second.

---

# Visual Timeline

```text
0ms
 ↓
Execute

100ms
 ↓
Blocked

200ms
 ↓
Blocked

300ms
 ↓
Blocked

1000ms
 ↓
Execute Again
```

---

# Example

```javascript
function print(){

   console.log("Running");

}

const throttledPrint =
throttle(print,2000);
```

Calls:

```javascript
throttledPrint();
throttledPrint();
throttledPrint();
throttledPrint();
```

Output:

```text
Running
```

Only first execution happens.

---

# After Delay

```javascript
setTimeout(()=>{

   throttledPrint();

},3000);
```

Output:

```text
Running
```

Because 2-second delay completed.

---

# Real Scroll Example

Without Throttle:

```javascript
window.addEventListener(

   "scroll",

   ()=>{

      console.log(
         window.scrollY
      );

   }

);
```

Output:

```text
Hundreds Of Logs
```

---

# With Throttle

```javascript
const handleScroll =
throttle(()=>{

   console.log(
      window.scrollY
   );

},500);

window.addEventListener(
   "scroll",
   handleScroll
);
```

Output:

```text
Only Once Every 500ms
```

---

# Window Resize Example

Without Throttle:

```javascript
window.addEventListener(

   "resize",

   ()=>{

      console.log(
         window.innerWidth
      );

   }

);
```

Output:

```text
Hundreds Of Executions
```

---

# With Throttle

```javascript
window.addEventListener(

   "resize",

   throttle(()=>{

      console.log(
         window.innerWidth
      );

   },500)

);
```

Output:

```text
One Execution Every 500ms
```

---

# Better Throttle Function

Professional version:

```javascript
function throttle(fn,delay){

   let lastCall = 0;

   return function(...args){

      const now = Date.now();

      if(
         now - lastCall >= delay
      ){

         lastCall = now;

         fn.apply(
            this,
            args
         );

      }

   };

}
```

---

# Why Better?

Preserves:

```javascript
this
```

and

```javascript
arguments
```

Works correctly in real applications.

---

# How Date-Based Throttle Works

```javascript
const now =
Date.now();
```

Current timestamp.

Example:

```text
1715000000000
```

---

Check:

```javascript
now - lastCall >= delay
```

If enough time passed:

```text
Execute Function
```

Otherwise:

```text
Ignore Call
```

---

# Real World Uses

## Scroll Events

```text
Infinite Scroll
Sticky Navbar
Scroll Tracking
```

---

## Resize Events

```text
Responsive Layout
Dashboard Resize
```

---

## Mouse Movement

```text
Drag & Drop
Drawing Applications
```

---

## Button Spam Prevention

```text
Prevent Multiple Clicks
```

---

## Analytics Tracking

```text
Track User Activity
```

---

# Example: Infinite Scrolling

```javascript
window.addEventListener(

   "scroll",

   throttle(()=>{

      if(
         window.innerHeight +
         window.scrollY >=
         document.body.offsetHeight
      ){

         loadMoreProducts();

      }

   },500)

);
```

Very common in React applications.

---

# Throttling vs Debouncing

Most Important Interview Question.

---

# Debouncing

Waits until user stops.

```text
Typing
Typing
Typing
Stop
 ↓
Execute
```

---

Example:

```text
Search Bar
Auto Suggest
```

---

# Throttling

Executes at fixed intervals.

```text
Typing
 ↓
Execute

Typing
 ↓
Wait

Typing
 ↓
Execute
```

---

Example:

```text
Scroll
Resize
Mouse Move
```

---

# Visual Comparison

Debounce:

```text
User Activity
 ↓
Wait
 ↓
Execute Once
```

---

Throttle:

```text
User Activity
 ↓
Execute
 ↓
Wait
 ↓
Execute
```

---

# Debounce vs Throttle Table

| Feature      | Debounce             | Throttle             |
| ------------ | -------------------- | -------------------- |
| Execution    | After Activity Stops | Every Fixed Interval |
| API Calls    | Excellent            | Good                 |
| Search Bar   | Yes                  | No                   |
| Scroll Event | No                   | Yes                  |
| Resize Event | Sometimes            | Yes                  |
| Auto Suggest | Yes                  | No                   |

---

# Interview Question

Predict Output:

```javascript
const fn =
throttle(()=>{

   console.log("Run");

},1000);

fn();
fn();
fn();
fn();
```

Output:

```text
Run
```

Only first call executes.

---

# Memory Visualization

```text
allowed = true

fn()
 ↓
Execute

allowed = false

fn()
 ↓
Ignored

fn()
 ↓
Ignored

Delay Complete

allowed = true

fn()
 ↓
Execute
```

---

# React Example

```javascript
const handleScroll =
throttle(()=>{

   console.log(
      "Loading More Data"
   );

},500);

useEffect(()=>{

   window.addEventListener(
      "scroll",
      handleScroll
   );

   return ()=>{

      window.removeEventListener(
         "scroll",
         handleScroll
      );

   };

},[]);
```

Common React interview example.

---

# Common Interview Questions

## What is Throttling?

A technique that limits function execution to once within a specified interval.

---

## Why Use Throttling?

To improve performance by reducing excessive function calls.

---

## What Problems Does Throttling Solve?

* Scroll Performance
* Resize Performance
* Mouse Movement Events
* Button Spamming

---

## Difference Between Debouncing and Throttling?

Debounce waits.

Throttle limits frequency.

---

## Which Events Commonly Use Throttling?

* Scroll
* Resize
* Mousemove
* Drag Events

---

## Which Uses More Calls?

### Debounce

```text
Less Calls
```

### Throttle

```text
More Calls
```

because execution happens periodically.

---

# Most Important Topics

✅ Throttling

✅ Event Optimization

✅ Scroll Events

✅ Resize Events

✅ Closures

✅ setTimeout()

✅ Date.now()

✅ Debounce vs Throttle

✅ Performance Optimization

✅ React Event Handling

---

# Throttling Formula

```text
Event Fires
      ↓
Allowed?
   /      \
 Yes       No
  ↓         ↓
Execute   Ignore
  ↓
Wait Delay
  ↓
Allow Again
```

or

```text
Throttling
=
Rate Limiting
+
Closures
+
Timers
```

Throttling is one of the most important JavaScript performance optimization techniques and is widely used in React applications, dashboards, infinite scrolling, resize handling, drag-and-drop systems, and large-scale MERN stack applications.
