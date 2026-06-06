# JavaScript Currying - Complete Notes

# What is Currying?

Currying is a functional programming technique where a function that takes multiple arguments is transformed into a sequence of functions that each take one argument.

Normal Function:

```javascript
sum(1,2,3);
```

Curried Function:

```javascript
sum(1)(2)(3);
```

Instead of passing all arguments at once:

```text
sum(a,b,c)
```

We pass arguments one by one:

```text
sum(a)
  ↓
returns function(b)
  ↓
returns function(c)
  ↓
returns result
```

---

# Why is it Called Currying?

The term comes from mathematician:

```text
Haskell Curry
```

Currying is named after him.

---

# Understanding the Problem

Normal Function:

```javascript
function sum(a,b,c){

   return a+b+c;

}

console.log(
   sum(1,2,3)
);
```

Output:

```text
6
```

All arguments are passed together.

---

# Curried Version

```javascript
function sum(a){

   return function(b){

      return function(c){

         return a+b+c;

      };

   };

}
```

Call:

```javascript
console.log(
   sum(1)(2)(3)
);
```

Output:

```text
6
```

---

# How Currying Works Internally

Call:

```javascript
sum(1)(2)(3)
```

Step 1:

```javascript
sum(1)
```

Returns:

```javascript
function(b){

   return function(c){

      return a+b+c;

   };

}
```

At this point:

```text
a = 1
```

is remembered.

---

Step 2:

```javascript
sum(1)(2)
```

Returns:

```javascript
function(c){

   return a+b+c;

}
```

Now:

```text
a = 1
b = 2
```

are remembered.

---

Step 3:

```javascript
sum(1)(2)(3)
```

Returns:

```javascript
1 + 2 + 3
```

Output:

```text
6
```

---

# Visual Representation

```text
sum(1)
 ↓
Store a = 1

sum(1)(2)
 ↓
Store b = 2

sum(1)(2)(3)
 ↓
Store c = 3

1+2+3

 ↓

6
```

---

# Memory Visualization

First Call:

```javascript
sum(1)
```

Memory:

```text
a = 1
```

Function returned.

---

Second Call:

```javascript
(2)
```

Memory:

```text
a = 1
b = 2
```

Function returned.

---

Third Call:

```javascript
(3)
```

Memory:

```text
a = 1
b = 2
c = 3
```

Result:

```text
6
```

---

# Why Does Currying Work?

Because of:

```text
Closures
```

Each inner function remembers variables from its outer function.

Example:

```javascript
function outer(){

   let count = 0;

   return function(){

      count++;

      console.log(count);

   };

}
```

This is closure.

Currying is built on top of closures.

---

# Arrow Function Version

```javascript
const sum =
a => b => c => a+b+c;
```

Usage:

```javascript
console.log(
   sum(1)(2)(3)
);
```

Output:

```text
6
```

This is commonly asked in interviews.

---

# Another Example

Normal Function:

```javascript
function multiply(a,b){

   return a*b;

}
```

Curried:

```javascript
function multiply(a){

   return function(b){

      return a*b;

   };

}
```

Usage:

```javascript
console.log(
   multiply(2)(5)
);
```

Output:

```text
10
```

---

# Real Benefit of Currying

Suppose:

```javascript
multiply(2)(5)
multiply(2)(10)
multiply(2)(20)
```

Instead of repeatedly passing:

```text
2
```

we can create:

```javascript
const double =
multiply(2);
```

Now:

```javascript
double(5);
double(10);
double(20);
```

Output:

```text
10
20
40
```

This is called:

```text
Partial Application
```

---

# Partial Application

One of the biggest uses of currying.

Example:

```javascript
function multiply(a){

   return function(b){

      return a*b;

   };

}
```

Create:

```javascript
const triple =
multiply(3);
```

Now:

```javascript
console.log(
   triple(5)
);
```

Output:

```text
15
```

---

# Real World Example

Tax Calculator:

```javascript
function tax(rate){

   return function(amount){

      return amount * rate;

   };

}
```

Create GST:

```javascript
const gst =
tax(0.18);
```

Use:

```javascript
console.log(
   gst(1000)
);
```

Output:

```text
180
```

---

# React Example

Currying is frequently used in React event handlers.

Example:

```javascript
const handleDelete =
(id) => {

   return () => {

      console.log(
         "Delete",
         id
      );

   };

};
```

Usage:

```jsx
<button
   onClick={
      handleDelete(5)
   }
>
Delete
</button>
```

When clicked:

```text
Delete 5
```

---

# Event Handler Currying

Example:

```javascript
function handleClick(id){

   return function(){

      console.log(id);

   };

}
```

Usage:

```javascript
button.onclick =
handleClick(10);
```

Output:

```text
10
```

---

# Generic Curry Example

Normal Function:

```javascript
function add(a,b,c){

   return a+b+c;

}
```

Curried:

```javascript
function curryAdd(a){

   return function(b){

      return function(c){

         return a+b+c;

      };

   };

}
```

Usage:

```javascript
curryAdd(1)(2)(3);
```

Output:

```text
6
```

---

# Currying vs Normal Function

Normal:

```javascript
sum(1,2,3);
```

All arguments together.

---

Curried:

```javascript
sum(1)(2)(3);
```

Arguments one by one.

---

# Visual Comparison

Normal:

```text
1
2
3
 ↓
sum()
 ↓
6
```

---

Curried:

```text
1
 ↓
Function

2
 ↓
Function

3
 ↓
Result

6
```

---

# Using bind() for Currying

JavaScript provides a shortcut.

Example:

```javascript
function multiply(a,b){

   return a*b;

}
```

Create:

```javascript
const double =
multiply.bind(
   null,
   2
);
```

Now:

```javascript
double(5);
```

Output:

```text
10
```

This is a form of currying.

---

# Common Interview Question

Predict Output:

```javascript
function sum(a){

   return function(b){

      return function(c){

         return a+b+c;

      };

   };

}

console.log(
   sum(2)(3)(4)
);
```

Output:

```text
9
```

---

# Common Interview Questions

## What is Currying?

Transforming a function with multiple arguments into a sequence of functions taking one argument at a time.

---

## Why Use Currying?

* Reusability
* Partial Application
* Functional Programming
* Cleaner Code

---

## What Concept Makes Currying Possible?

```text
Closures
```

---

## Difference Between Currying and Normal Functions?

Normal:

```javascript
sum(1,2,3);
```

Curried:

```javascript
sum(1)(2)(3);
```

---

## Where is Currying Used?

* React
* Functional Programming
* Event Handlers
* Utility Functions
* Libraries like Lodash

---

# Real MERN Stack Usage

Currying is commonly used in:

### React Event Handlers

```jsx
onClick={handleDelete(id)}
```

### Form Validation

```javascript
validate("email")(value)
```

### API Utilities

```javascript
fetchData(baseUrl)(endpoint)
```

### Redux Middleware

```javascript
store => next => action
```

Very common in advanced React and Redux codebases.

---

# Currying vs Closures

Currying:

```javascript
sum(1)(2)(3)
```

Purpose:

```text
Break Arguments Into Steps
```

---

Closure:

```javascript
function outer(){

   let count = 0;

   return function(){

      count++;

   };

}
```

Purpose:

```text
Remember Variables
```

Currying uses closures internally.

---

# Most Important Topics

✅ Currying

✅ Closures

✅ Higher Order Functions

✅ Partial Application

✅ Arrow Function Currying

✅ React Event Handlers

✅ Function Reusability

✅ Functional Programming

✅ bind() and Currying

---

# Currying Formula

```text
Function(a,b,c)
          ↓
Currying
          ↓
Function(a)(b)(c)
```

or

```text
Currying
=
Closures
+
Higher Order Functions
+
Partial Application
```

Currying is an advanced JavaScript concept that improves code reusability, readability, and functional programming capabilities. It is frequently asked in JavaScript interviews for developers with 2+ years of experience and is widely used in React, Redux, and modern JavaScript applications.
