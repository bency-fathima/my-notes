# JavaScript Design Patterns - Complete Notes

# What are Design Patterns?

Design Patterns are proven solutions to common software development problems.

Think of them as:

```text
Best Practices
      +
Reusable Solutions
      +
Well Tested Approaches
```

Instead of solving the same problem repeatedly, developers use established patterns.

---

# Real Life Example

Imagine building a house.

Without a blueprint:

```text
Build From Scratch
Every Time
```

With a blueprint:

```text
Follow Proven Design
```

Design Patterns are software blueprints.

---

# Why Do We Need Design Patterns?

Without patterns:

```text
Messy Code
Duplicate Logic
Difficult Maintenance
```

With patterns:

```text
Reusable Code
Scalable Applications
Maintainable Systems
```

---

# Common JavaScript Design Patterns

### Creational Patterns

Create objects.

```text
Singleton
Factory
Constructor
```

---

### Structural Patterns

Organize objects.

```text
Module
Decorator
Adapter
```

---

### Behavioral Patterns

Manage communication.

```text
Observer
Strategy
Iterator
```

---

For JavaScript interviews, the most important are:

✅ Module Pattern

✅ Singleton Pattern

✅ Factory Pattern

✅ Observer Pattern

---

# 1. Module Pattern

One of the oldest and most important JavaScript patterns.

---

# What Problem Does Module Pattern Solve?

Suppose:

```javascript
let count = 0;

function increment(){

   count++;

}
```

Problem:

```text
count is Global
```

Any code can modify it.

Example:

```javascript
count = 1000;
```

Dangerous.

---

# Goal

Create:

```text
Private Variables
```

while exposing only necessary methods.

---

# Module Pattern Solution

```javascript
const Counter = (()=>{

   let count = 0;

   return {

      increment(){

         count++;

      },

      getCount(){

         return count;

      }

   };

})();
```

---

# Understanding Step-by-Step

## Step 1

Immediately Invoked Function Expression (IIFE)

```javascript
(()=>{
})();
```

Runs immediately.

---

# Example

```javascript
(function(){

   console.log(
      "Executed"
   );

})();
```

Output:

```text
Executed
```

---

# Step 2

Private Variable

```javascript
let count = 0;
```

Exists inside IIFE.

Cannot be accessed directly.

---

# Wrong

```javascript
console.log(count);
```

Output:

```text
ReferenceError
```

Because:

```text
count is private
```

---

# Step 3

Return Public Methods

```javascript
return {

   increment(){},

   getCount(){}

}
```

These methods can access:

```javascript
count
```

through closure.

---

# Visual Representation

```text
Counter Module

Private
────────────
count
────────────

Public
────────────
increment()
getCount()
────────────
```

---

# Usage

```javascript
Counter.increment();

Counter.increment();

console.log(
   Counter.getCount()
);
```

Output:

```text
2
```

---

# Why Does This Work?

Because of:

```text
Closures
```

Even after IIFE finishes:

```javascript
count
```

remains available to returned methods.

---

# Memory Visualization

```text
IIFE
 │
 ├── count = 0
 │
 └── Return Methods
        ↓
     Closure
        ↓
Remember count
```

---

# Benefits of Module Pattern

### Encapsulation

Hide internal details.

---

### Data Privacy

Private variables.

---

### Controlled Access

Only public methods can modify data.

---

### Avoid Global Variables

Cleaner applications.

---

# Real Example

Shopping Cart

```javascript
const Cart = (()=>{

   const items = [];

   return {

      addItem(item){

         items.push(item);

      },

      getItems(){

         return items;
      }

   };

})();
```

Usage:

```javascript
Cart.addItem("Phone");

Cart.addItem("Laptop");

console.log(
   Cart.getItems()
);
```

Output:

```javascript
[
   "Phone",
   "Laptop"
]
```

---

# 2. Singleton Pattern

One of the most asked interview patterns.

---

# What is Singleton?

Singleton ensures:

```text
Only One Instance
Of An Object
Can Exist
```

---

# Real Life Examples

### Database Connection

```text
One Database Connection
```

---

### Logger

```text
One Logger
```

---

### Configuration Object

```text
One App Configuration
```

---

# Problem Without Singleton

```javascript
const db1 =
new Database();

const db2 =
new Database();
```

Creates:

```text
Two Instances
```

Sometimes undesirable.

---

# Goal

Always return:

```text
Same Instance
```

---

# Singleton Implementation

```javascript
class Database{

   static instance;

   constructor(){

      if(Database.instance){

         return Database.instance;

      }

      Database.instance = this;

   }

}
```

---

# Understanding Step-by-Step

## First Object

```javascript
const db1 =
new Database();
```

Check:

```javascript
Database.instance
```

Output:

```javascript
undefined
```

No instance exists.

Store:

```javascript
Database.instance =
this;
```

---

# Second Object

```javascript
const db2 =
new Database();
```

Check:

```javascript
Database.instance
```

Already exists.

Return:

```javascript
Database.instance
```

instead of creating new object.

---

# Verify

```javascript
console.log(
   db1 === db2
);
```

Output:

```text
true
```

Same object.

---

# Visual Representation

First Creation:

```text
new Database()
      ↓
Create Instance
      ↓
Store Instance
```

---

Second Creation:

```text
new Database()
      ↓
Instance Exists
      ↓
Return Existing Instance
```

---

# Memory Visualization

```text
Database.instance
        ↓
      Object

db1 ───┘

db2 ───┘
```

Both point to same object.

---

# Real World Example

Application Settings

```javascript
class Settings{

   static instance;

   constructor(){

      if(Settings.instance){

         return Settings.instance;
      }

      this.theme =
      "dark";

      Settings.instance =
      this;
   }

}
```

Usage:

```javascript
const s1 =
new Settings();

const s2 =
new Settings();
```

Check:

```javascript
console.log(
   s1 === s2
);
```

Output:

```text
true
```

---

# Modern Singleton Pattern

Using static method.

```javascript
class Database{

   static instance;

   static getInstance(){

      if(!Database.instance){

         Database.instance =
         new Database();
      }

      return Database.instance;

   }

}
```

Usage:

```javascript
const db1 =
Database.getInstance();

const db2 =
Database.getInstance();
```

Output:

```text
Same Instance
```

---

# Factory Pattern

Another common interview topic.

Creates objects without exposing creation logic.

Example:

```javascript
function createUser(name){

   return {

      name,

      greet(){

         console.log(
            `Hello ${name}`
         );
      }

   };

}
```

Usage:

```javascript
const user =
createUser("John");
```

Output:

```javascript
{
   name:"John"
}
```

---

# Observer Pattern

Used heavily in React and JavaScript.

Example:

```javascript
class Subject{

   constructor(){

      this.observers = [];
   }

   subscribe(fn){

      this.observers.push(fn);

   }

   notify(){

      this.observers.forEach(
         fn => fn()
      );

   }

}
```

Used in:

```text
React State Updates
Redux
Event Systems
```

---

# Design Patterns in React

### Module Pattern

```javascript
Custom Hooks
```

---

### Singleton Pattern

```javascript
API Client
Axios Instance
```

---

### Observer Pattern

```javascript
Redux Store
Context API
```

---

### Factory Pattern

```javascript
Component Factories
```

---

# Common Interview Questions

## What is a Design Pattern?

A reusable solution to common software design problems.

---

## What is Module Pattern?

A pattern that provides private variables and public methods using closures.

---

## What is Singleton Pattern?

A pattern that allows only one instance of an object.

---

## Why Use Module Pattern?

* Encapsulation
* Data Hiding
* Avoid Global Variables

---

## Why Use Singleton Pattern?

* Database Connections
* Configuration Management
* Logging Systems

---

## What JavaScript Concept Makes Module Pattern Possible?

```text
Closures
```

---

## How Can You Verify Singleton?

```javascript
db1 === db2
```

Output:

```text
true
```

---

# Module Pattern vs Singleton Pattern

| Feature           | Module Pattern    | Singleton Pattern   |
| ----------------- | ----------------- | ------------------- |
| Purpose           | Encapsulation     | Single Instance     |
| Uses Closures     | Yes               | Sometimes           |
| Private Variables | Yes               | Not Necessarily     |
| Object Count      | One Module Object | One Instance Only   |
| Common Use        | Utilities         | Database Connection |

---

# Real MERN Stack Usage

### Module Pattern

```javascript
Auth Module
Cart Module
Utility Module
```

---

### Singleton Pattern

```javascript
MongoDB Connection
Axios Client
Logger Service
```

---

### Observer Pattern

```javascript
Redux
Socket Events
Notifications
```

---

# Most Important Topics

✅ Design Patterns

✅ Module Pattern

✅ Singleton Pattern

✅ Factory Pattern

✅ Observer Pattern

✅ Closures

✅ Encapsulation

✅ Private Variables

✅ Shared Instance

✅ React Design Patterns

---

# Design Pattern Formula

```text
Problem
   ↓
Proven Solution
   ↓
Design Pattern
```

---

# Module Pattern Formula

```text
IIFE
 +
Closure
 +
Private Variables
 =
Module Pattern
```

---

# Singleton Pattern Formula

```text
One Class
     ↓
One Instance
     ↓
Shared Everywhere
```

Design Patterns are advanced JavaScript concepts that help developers write scalable, maintainable, reusable, and production-ready applications. Understanding Module Pattern, Singleton Pattern, Factory Pattern, and Observer Pattern is important for React, Node.js, MERN Stack development, and JavaScript interviews for 2+ years experienced developers.
