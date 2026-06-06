# JavaScript Call, Apply & Bind - Complete Notes

# Why Do We Need call(), apply() and bind()?

Before understanding call, apply and bind, we must understand a problem with `this`.

Example:

```javascript
const user = {

   name: "John"
};

function greet(){

   console.log(this.name);
}

greet();
```

Output (Browser):

```text
undefined
```

Why?

Because:

```javascript
this
```

depends on how the function is called.

Here:

```javascript
greet();
```

is a regular function call.

So:

```javascript
this === window
```

and:

```javascript
window.name
```

does not exist.

Output:

```text
undefined
```

---

# The Solution

JavaScript provides:

```javascript
call()
apply()
bind()
```

These methods allow us to manually set the value of:

```javascript
this
```

---

# Think of it Like This

Normally:

```javascript
this
```

is decided automatically.

Using:

```javascript
call()
apply()
bind()
```

we decide it manually.

---

# Example Setup

```javascript
const user = {

   name:"John"
};

function greet(city){

   console.log(
      this.name,
      city
   );
}
```

---

# call()

The `call()` method invokes a function immediately and allows us to set the value of `this`.

Syntax:

```javascript
functionName.call(
   thisArg,
   arg1,
   arg2,
   arg3
);
```

---

# Example

```javascript
greet.call(
   user,
   "Kochi"
);
```

Output:

```text
John Kochi
```

---

# What Happens Internally?

JavaScript treats it like:

```javascript
user.greet = greet;

user.greet("Kochi");
```

Now:

```javascript
this === user
```

Therefore:

```javascript
this.name
```

becomes:

```javascript
user.name
```

Output:

```text
John Kochi
```

---

# Multiple Arguments

```javascript
function greet(city,country){

   console.log(
      this.name,
      city,
      country
   );
}
```

Call:

```javascript
greet.call(
   user,
   "Kochi",
   "India"
);
```

Output:

```text
John Kochi India
```

---

# Visual Representation

```text
greet.call(user)

      ↓

this = user

      ↓

user.name

      ↓

John
```

---

# apply()

`apply()` works almost exactly like `call()`.

Difference:

```text
call()  → Arguments Separately

apply() → Arguments Inside Array
```

---

# Syntax

```javascript
functionName.apply(
   thisArg,
   [arg1,arg2,arg3]
);
```

---

# Example

```javascript
greet.apply(
   user,
   ["Kochi"]
);
```

Output:

```text
John Kochi
```

---

# Multiple Arguments

```javascript
function greet(city,country){

   console.log(
      this.name,
      city,
      country
   );
}
```

Apply:

```javascript
greet.apply(
   user,
   ["Kochi","India"]
);
```

Output:

```text
John Kochi India
```

---

# call vs apply

### call

```javascript
greet.call(
   user,
   "Kochi",
   "India"
);
```

---

### apply

```javascript
greet.apply(
   user,
   ["Kochi","India"]
);
```

---

# Visual Comparison

```text
call()
↓

Arguments Separate

John
Kochi
India
```

```text
apply()
↓

Arguments in Array

[
 Kochi,
 India
]
```

---

# bind()

Most important among the three.

Unlike:

```javascript
call()
apply()
```

`bind()` does NOT execute immediately.

Instead:

```text
Creates New Function
          ↓
Returns It
```

---

# Syntax

```javascript
const newFunction =
originalFunction.bind(
   thisArg
);
```

---

# Example

```javascript
const fn =
greet.bind(user);
```

Nothing happens yet.

Output:

```text
No Output
```

Because bind only returns a new function.

---

# Execute Later

```javascript
fn("Kochi");
```

Output:

```text
John Kochi
```

---

# Visual Representation

```text
bind()
   ↓
Create New Function
   ↓
Store in Variable
   ↓
Execute Later
```

---

# Why bind() is Useful?

Suppose:

```javascript
const user = {

   name:"John",

   show(){

      console.log(this.name);
   }
};
```

---

# Problem

```javascript
const display =
user.show;

display();
```

Output:

```text
undefined
```

Why?

Because:

```javascript
display()
```

is no longer called through:

```javascript
user
```

Therefore:

```javascript
this !== user
```

---

# Solution Using bind()

```javascript
const display =
user.show.bind(user);

display();
```

Output:

```text
John
```

Now:

```javascript
this === user
```

forever.

---

# Real World Example - Event Handlers

Example:

```javascript
class User{

   constructor(){

      this.name = "John";
   }

   show(){

      console.log(this.name);
   }
}
```

---

Problem:

```javascript
button.addEventListener(
   "click",
   user.show
);
```

Output:

```text
undefined
```

Because:

```javascript
this
```

becomes button.

---

Solution:

```javascript
button.addEventListener(

   "click",

   user.show.bind(user)

);
```

Output:

```text
John
```

---

# Borrowing Methods

One object can use another object's method.

Example:

```javascript
const user1 = {

   name:"John"
};

const user2 = {

   name:"Alex"
};

function greet(){

   console.log(this.name);
}
```

Call:

```javascript
greet.call(user1);
```

Output:

```text
John
```

---

Call:

```javascript
greet.call(user2);
```

Output:

```text
Alex
```

This is called:

```text
Function Borrowing
```

---

# Interview Question

Predict Output:

```javascript
const person = {

   name:"John"
};

function show(){

   console.log(this.name);
}

const fn =
show.bind(person);

fn();
```

Output:

```text
John
```

---

# Partial Application using bind()

```javascript
function multiply(a,b){

   return a*b;
}
```

Bind:

```javascript
const double =
multiply.bind(
   null,
   2
);
```

Now:

```javascript
console.log(
   double(5)
);
```

Output:

```text
10
```

Because:

```javascript
multiply(2,5)
```

---

# call(), apply(), bind() with Arrow Functions

Example:

```javascript
const greet = ()=>{

   console.log(this);
};
```

Now:

```javascript
greet.call(user);
```

Output:

```javascript
window
```

Why?

Arrow functions do not have their own:

```javascript
this
```

Therefore:

```javascript
call()
apply()
bind()
```

cannot change arrow function's `this`.

---

# Important Interview Question

Can call, apply, and bind change arrow function's this?

Answer:

```text
No
```

Because arrow functions use lexical this.

---

# Comparison Table

| Feature              | call()   | apply() | bind()   |
| -------------------- | -------- | ------- | -------- |
| Sets this            | Yes      | Yes     | Yes      |
| Executes Immediately | Yes      | Yes     | No       |
| Returns Function     | No       | No      | Yes      |
| Arguments            | Separate | Array   | Separate |

---

# Quick Examples

### call()

```javascript
greet.call(
   user,
   "Kochi"
);
```

Output:

```text
John Kochi
```

---

### apply()

```javascript
greet.apply(
   user,
   ["Kochi"]
);
```

Output:

```text
John Kochi
```

---

### bind()

```javascript
const fn =
greet.bind(user);

fn("Kochi");
```

Output:

```text
John Kochi
```

---

# Memory Visualization

```text
call()
   ↓
Execute Now

apply()
   ↓
Execute Now

bind()
   ↓
Create Function
   ↓
Execute Later
```

---

# Real MERN Stack Usage

call(), apply(), and bind() are commonly used in:

* React Class Components
* Event Handlers
* Function Borrowing
* Custom Libraries
* JavaScript Frameworks
* OOP Programming

---

# Common Interview Questions

## What is call()?

Calls function immediately and sets `this`.

---

## What is apply()?

Calls function immediately and sets `this`, but arguments are passed as an array.

---

## What is bind()?

Returns a new function with fixed `this`.

---

## Difference Between call and apply?

```javascript
call(user,a,b);
```

```javascript
apply(user,[a,b]);
```

---

## Difference Between call and bind?

### call()

Executes immediately.

### bind()

Returns function and executes later.

---

## Can bind() be used for partial application?

Yes.

Very commonly asked in interviews.

---

# Most Important Topics

✅ this Keyword

✅ call()

✅ apply()

✅ bind()

✅ Function Borrowing

✅ Event Handlers

✅ Partial Application

✅ Arrow Functions and this

✅ Method Sharing

---

# Formula

```text
call()
=
Set this
+
Execute Immediately
```

```text
apply()
=
Set this
+
Execute Immediately
+
Arguments as Array
```

```text
bind()
=
Set this
+
Return New Function
+
Execute Later
```

Understanding call, apply, and bind is essential for mastering JavaScript's `this` keyword, event handling, object-oriented programming, React class components, and advanced JavaScript interviews.
