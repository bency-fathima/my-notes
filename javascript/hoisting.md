# JavaScript Hoisting - Complete Notes

# What is Hoisting?

Hoisting is one of the most important and frequently asked JavaScript interview topics.

### Definition

Hoisting is JavaScript's behavior of moving declarations to the top of their scope during the memory creation phase before code execution.

In simple words:

```text
JavaScript reads the code first
        ↓
Allocates memory for variables and functions
        ↓
Then starts executing code
```

Many developers think JavaScript physically moves code.

It does NOT.

JavaScript only allocates memory before execution.

---

# Why Does Hoisting Happen?

JavaScript executes code in two phases:

## Phase 1: Memory Creation Phase

JavaScript scans entire code and allocates memory.

## Phase 2: Code Execution Phase

JavaScript executes code line by line.

---

# Visual Representation

```text
JavaScript Execution
│
├── Phase 1
│    Memory Creation
│
└── Phase 2
     Code Execution
```

---

# Example 1 - var Hoisting

```javascript
console.log(a);

var a = 10;
```

Many beginners expect:

```text
ReferenceError
```

Actual Output:

```text
undefined
```

---

# What Happens Internally?

JavaScript Memory Creation Phase:

```javascript
var a;
```

Memory:

```text
Global Memory
--------------
a = undefined
--------------
```

---

Code Execution Phase:

```javascript
console.log(a);
```

Output:

```text
undefined
```

Then:

```javascript
a = 10;
```

Memory becomes:

```text
Global Memory
--------------
a = 10
--------------
```

---

# Internal Representation

JavaScript treats it like:

```javascript
var a;

console.log(a);

a = 10;
```

Output:

```text
undefined
```

---

# Important Point

Only declaration is hoisted.

Initialization is NOT hoisted.

Example:

```javascript
var a;

a = 10;
```

Not:

```javascript
var a = 10;
```

---

# Memory Visualization

```javascript
console.log(a);

var a = 10;
```

### Before Execution

```text
Memory
-----------
a = undefined
-----------
```

### During Execution

```javascript
console.log(a);
```

Output:

```text
undefined
```

### After Assignment

```text
Memory
-----------
a = 10
-----------
```

---

# Hoisting with let

Example:

```javascript
console.log(age);

let age = 25;
```

Output:

```text
ReferenceError
```

---

# Why?

Many developers think:

```text
let is not hoisted
```

This is WRONG.

`let` is hoisted.

But it behaves differently.

---

# Memory Creation

```javascript
let age;
```

Memory is reserved.

However:

```text
Cannot Access Yet
```

Because it is inside:

```text
Temporal Dead Zone (TDZ)
```

---

# Temporal Dead Zone (TDZ)

TDZ is the period between:

```text
Variable Creation
       ↓
Variable Initialization
```

Example:

```javascript
console.log(age);

let age = 25;
```

Timeline:

```text
Memory Created
       ↓
TDZ Starts
       ↓
console.log(age)
       ↓
ReferenceError
       ↓
age = 25
       ↓
TDZ Ends
```

---

# Visual Representation

```text
let age

│
│  TDZ
│
│
▼
age = 25
```

Accessing variable inside TDZ:

```text
ReferenceError
```

---

# Hoisting with const

Example:

```javascript
console.log(PI);

const PI = 3.14;
```

Output:

```text
ReferenceError
```

Reason:

```text
const also lives inside TDZ
```

---

# var vs let vs const Hoisting

## var

```javascript
console.log(a);

var a = 10;
```

Output:

```text
undefined
```

---

## let

```javascript
console.log(a);

let a = 10;
```

Output:

```text
ReferenceError
```

---

## const

```javascript
console.log(a);

const a = 10;
```

Output:

```text
ReferenceError
```

---

# Comparison Table

| Feature                   | var       | let   | const |
| ------------------------- | --------- | ----- | ----- |
| Hoisted                   | Yes       | Yes   | Yes   |
| Initial Value             | undefined | TDZ   | TDZ   |
| Access Before Declaration | Allowed   | Error | Error |

---

# Function Hoisting

Functions behave differently.

---

# Function Declaration Hoisting

Example:

```javascript
greet();

function greet(){

   console.log("Hello");
}
```

Output:

```text
Hello
```

---

# Why?

During Memory Creation:

```text
Memory
--------------------
greet → Entire Function
--------------------
```

The complete function is stored.

---

# Internal Representation

```javascript
function greet(){

   console.log("Hello");
}

greet();
```

Output:

```text
Hello
```

---

# Memory Visualization

```text
Global Memory
---------------------
greet → function(){}
---------------------
```

Function is available before execution.

---

# Function Expression Hoisting

Example:

```javascript
greet();

var greet = function(){

   console.log("Hello");
};
```

Output:

```text
TypeError:
greet is not a function
```

---

# Why?

Memory Phase:

```javascript
var greet;
```

Memory:

```text
greet = undefined
```

Execution:

```javascript
greet();
```

Becomes:

```javascript
undefined();
```

Output:

```text
TypeError
```

---

# Visual

```text
Memory Creation
---------------
greet = undefined
---------------
```

Execution:

```javascript
greet();
```

Equivalent:

```javascript
undefined();
```

Invalid.

---

# Function Expression with let

```javascript
greet();

let greet = function(){

   console.log("Hello");
};
```

Output:

```text
ReferenceError
```

Because:

```text
greet is inside TDZ
```

---

# Arrow Function Hoisting

Example:

```javascript
sayHello();

const sayHello = () => {

   console.log("Hello");
};
```

Output:

```text
ReferenceError
```

Because arrow functions are usually stored in:

```javascript
const
```

and const has TDZ.

---

# Interview Question

Predict Output:

```javascript
console.log(a);

var a = 10;

console.log(a);
```

Output:

```text
undefined
10
```

---

# Explanation

Memory Phase:

```text
a = undefined
```

Execution:

```javascript
console.log(a);
```

Output:

```text
undefined
```

Then:

```javascript
a = 10;
```

Now:

```javascript
console.log(a);
```

Output:

```text
10
```

---

# Interview Question

Predict Output:

```javascript
var a = 10;

function test(){

   console.log(a);

   var a = 20;
}

test();
```

Output:

```text
undefined
```

---

# Why?

Inside function:

Memory Creation:

```text
Function Memory
---------------
a = undefined
---------------
```

Execution:

```javascript
console.log(a);
```

Output:

```text
undefined
```

Then:

```javascript
a = 20;
```

---

# Internal Representation

```javascript
function test(){

   var a;

   console.log(a);

   a = 20;
}
```

---

# Hoisting and Execution Context

Every execution context has:

```text
Memory Phase
Code Phase
```

Example:

```javascript
var a = 10;

function greet(){

   var name = "John";
}
```

Memory Phase:

```text
Global Memory
------------------
a = undefined
greet = function(){}
------------------
```

Execution Phase:

```text
a = 10
```

---

# Common Interview Questions

## What is Hoisting?

Hoisting is JavaScript's behavior of allocating memory for variables and functions before code execution.

---

## Is let Hoisted?

Yes.

But it remains inside the Temporal Dead Zone until initialization.

---

## Is const Hoisted?

Yes.

But it also remains inside the Temporal Dead Zone.

---

## Why does var return undefined?

Because memory is allocated with:

```javascript
undefined
```

during creation phase.

---

## Why are Function Declarations Hoisted?

Because the entire function is stored in memory before execution.

---

## Difference Between Hoisting of Function Declaration and Function Expression?

### Function Declaration

```javascript
greet();

function greet(){}
```

Works.

---

### Function Expression

```javascript
greet();

var greet = function(){};
```

Produces:

```text
TypeError
```

---

# Real MERN Stack Relevance

Understanding hoisting helps with:

* React Components
* React Hooks
* Node.js Execution Flow
* Closures
* Scope
* Execution Context
* Debugging Undefined Errors

---

# Most Important Topics

✅ Memory Creation Phase

✅ Code Execution Phase

✅ var Hoisting

✅ let Hoisting

✅ const Hoisting

✅ Temporal Dead Zone (TDZ)

✅ Function Hoisting

✅ Function Expressions

✅ Arrow Functions

✅ Execution Context

---

# Hoisting Formula

```text
Hoisting
=
Memory Allocation
Before
Code Execution
```

or

```text
JavaScript Execution
         ↓
Memory Creation
         ↓
Hoisting Happens
         ↓
Code Execution
```

Hoisting is one of the foundational concepts of JavaScript. A strong understanding of hoisting is essential for mastering scope, closures, execution contexts, React, Node.js, and advanced JavaScript interviews.
