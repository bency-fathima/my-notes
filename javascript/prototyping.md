# JavaScript Prototype Basics - Complete Notes

# What is a Prototype?

Prototype is one of the most important concepts in JavaScript because JavaScript uses **Prototype-Based Inheritance** instead of Classical Inheritance.

### Definition

Every JavaScript object has a hidden property called:

```javascript
[[Prototype]]
```

which points to another object.

In browsers, we can view it as:

```javascript
__proto__
```

Think of a prototype as:

```text
Object
   ↓
Prototype
   ↓
Another Object
   ↓
Another Prototype
```

This chain continues until:

```text
null
```

---

# Why Do Prototypes Exist?

Imagine we have:

```javascript
const user = {
   name: "John"
};
```

JavaScript objects can access:

```javascript
user.toString()
user.hasOwnProperty()
user.valueOf()
```

But:

```javascript
const user = {
   name:"John"
};
```

doesn't contain these methods.

So where do they come from?

Answer:

```text
Prototype
```

---

# Every Object Has a Prototype

Example:

```javascript
const user = {
   name:"John"
};

console.log(user.__proto__);
```

Output:

```javascript
{
   constructor: f(),
   hasOwnProperty: f(),
   toString: f(),
   ...
}
```

These methods come from:

```javascript
Object.prototype
```

---

# Visual Representation

```text
user
 │
 ▼
Object.prototype
 │
 ▼
null
```

---

# Understanding Property Lookup

Example:

```javascript
const user = {
   name:"John"
};

console.log(user.name);
```

Output:

```text
John
```

JavaScript searches:

```text
user object
```

Property found.

Stop searching.

---

# What if Property Doesn't Exist?

```javascript
console.log(user.age);
```

Output:

```javascript
undefined
```

Search Process:

```text
user
 ↓
Object.prototype
 ↓
null
```

Not found.

Returns:

```javascript
undefined
```

---

# Prototype Chain

A prototype chain is the sequence JavaScript follows while searching for properties.

Example:

```javascript
const user = {
   name:"John"
};

console.log(
   user.toString()
);
```

Search:

```text
user
 ↓
No toString
 ↓
Object.prototype
 ↓
Found toString
```

Output:

```text
[object Object]
```

---

# Visualizing the Chain

```text
user
 │
 ▼
Object.prototype
 │
 ▼
null
```

Property Search:

```text
Search Current Object
        ↓
Not Found
        ↓
Search Prototype
        ↓
Not Found
        ↓
Continue Up Chain
        ↓
null
        ↓
undefined
```

---

# Creating Custom Prototype Inheritance

Example:

```javascript
const person = {

   greet(){

      console.log("Hello");

   }

};
```

Create another object:

```javascript
const user =
Object.create(person);
```

Now:

```javascript
console.log(user);
```

Output:

```javascript
{}
```

Empty object.

But:

```javascript
user.greet();
```

Output:

```text
Hello
```

---

# Why Does This Work?

Memory:

```text
user
 │
 ▼
person
 │
 ▼
Object.prototype
 │
 ▼
null
```

When:

```javascript
user.greet();
```

runs:

Search:

```text
user
 ↓
No greet
 ↓
person
 ↓
Found greet
```

Execute method.

Output:

```text
Hello
```

---

# Example with Properties

```javascript
const person = {

   country:"India",

   greet(){

      console.log("Hello");
   }

};

const user =
Object.create(person);

user.name = "John";
```

Now:

```javascript
console.log(user.name);
```

Output:

```text
John
```

Found directly.

---

```javascript
console.log(user.country);
```

Output:

```text
India
```

Found through prototype.

---

# Property Lookup Visualization

```text
user.country

Search:

user
 ↓
country ? No
 ↓
person
 ↓
country Found
```

Output:

```text
India
```

---

# Checking Prototype

Modern way:

```javascript
Object.getPrototypeOf(user);
```

Output:

```javascript
person
```

---

# Setting Prototype

```javascript
Object.setPrototypeOf(
   user,
   person
);
```

Now:

```javascript
user
```

inherits from:

```javascript
person
```

---

# Constructor Functions and Prototypes

Before ES6 classes, inheritance was done using constructor functions.

Example:

```javascript
function User(name){

   this.name = name;
}
```

Add method:

```javascript
User.prototype.greet =
function(){

   console.log(
      `Hello ${this.name}`
   );

};
```

Create object:

```javascript
const user1 =
new User("John");

user1.greet();
```

Output:

```text
Hello John
```

---

# Why Use Prototype Here?

Bad Approach:

```javascript
function User(name){

   this.name = name;

   this.greet = function(){

      console.log("Hello");

   };
}
```

Each object gets its own copy.

```text
user1.greet
user2.greet
user3.greet
```

Memory waste.

---

# Better Approach

```javascript
User.prototype.greet =
function(){

   console.log("Hello");
};
```

Now:

```text
One Method
Shared By All Objects
```

Memory efficient.

---

# Memory Visualization

Without Prototype:

```text
user1 → greet()
user2 → greet()
user3 → greet()
```

Three copies.

---

With Prototype:

```text
user1
   ↓

user2
   ↓

user3
   ↓

User.prototype
   ↓
greet()
```

One copy.

---

# Prototype and Arrays

Arrays also use prototypes.

Example:

```javascript
const arr = [1,2,3];

console.log(arr.__proto__);
```

Output:

```javascript
Array.prototype
```

Methods like:

```javascript
map()
filter()
reduce()
push()
pop()
```

come from:

```javascript
Array.prototype
```

---

# Example

```javascript
const arr = [1,2,3];

arr.map(num => num*2);
```

Search:

```text
arr
 ↓
No map
 ↓
Array.prototype
 ↓
Found map
```

---

# Prototype and Functions

Functions also have prototypes.

```javascript
function test(){}

console.log(test.__proto__);
```

Output:

```javascript
Function.prototype
```

---

# Built-in Prototype Chains

### Object

```text
Object
 ↓
Object.prototype
 ↓
null
```

---

### Array

```text
Array
 ↓
Array.prototype
 ↓
Object.prototype
 ↓
null
```

---

### Function

```text
Function
 ↓
Function.prototype
 ↓
Object.prototype
 ↓
null
```

---

# hasOwnProperty()

Important interview question.

Example:

```javascript
const person = {

   country:"India"
};

const user =
Object.create(person);

user.name = "John";
```

Check:

```javascript
console.log(
   user.hasOwnProperty("name")
);
```

Output:

```text
true
```

---

Check:

```javascript
console.log(
   user.hasOwnProperty("country")
);
```

Output:

```text
false
```

Why?

Because:

```text
country
```

belongs to prototype.

Not user itself.

---

# ES6 Classes Use Prototypes

Example:

```javascript
class User{

   constructor(name){

      this.name = name;
   }

   greet(){

      console.log(
         this.name
      );
   }
}
```

Internally JavaScript converts it into:

```javascript
function User(name){

   this.name = name;
}

User.prototype.greet =
function(){

   console.log(this.name);
};
```

Classes are syntactic sugar over prototypes.

---

# Common Interview Questions

## What is a Prototype?

A prototype is an object from which another object inherits properties and methods.

---

## What is Prototype Chain?

The chain JavaScript follows while searching for a property.

---

## What is **proto**?

A reference to an object's prototype.

---

## Difference Between **proto** and prototype?

### **proto**

Exists on objects.

```javascript
user.__proto__
```

---

### prototype

Exists on constructor functions.

```javascript
User.prototype
```

---

## Why Use Prototypes?

To share methods among multiple objects and save memory.

---

## Do Classes Use Prototypes?

Yes.

ES6 classes are built on top of prototypes.

---

# Real MERN Stack Relevance

Understanding prototypes helps with:

* JavaScript Internals
* Classes
* Inheritance
* React Components
* Node.js Frameworks
* Memory Optimization
* Object-Oriented Programming

---

# Most Important Topics

✅ Prototype

✅ **proto**

✅ prototype

✅ Prototype Chain

✅ Object.create()

✅ Object.getPrototypeOf()

✅ Constructor Functions

✅ Shared Methods

✅ Array Prototype

✅ Function Prototype

✅ hasOwnProperty()

✅ ES6 Classes and Prototypes

---

# Prototype Formula

```text
Object
   +
Prototype
   =
Inheritance
```

or

```text
Property Lookup

Current Object
      ↓
Prototype
      ↓
Prototype Chain
      ↓
null
```

Prototypes are the foundation of JavaScript inheritance. A strong understanding of prototypes is essential for mastering classes, inheritance, object-oriented programming, React internals, Node.js, and advanced JavaScript interviews.
