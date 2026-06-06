# JavaScript `this` Keyword - Complete Notes

# What is `this`?

The `this` keyword is one of the most important and confusing concepts in JavaScript.

### Definition

`this` refers to the object that is currently executing the function.

In simple words:

```text
Who is calling the function?
          ↓
        this
```

The value of `this` is determined at runtime.

---

# Why is `this` Important?

Imagine:

```javascript
const user = {

   name: "John",

   age: 25
};
```

How can a function inside the object access its own properties?

Using:

```javascript
this.name
```

Example:

```javascript
const user = {

   name: "John",

   show(){

      console.log(this.name);

   }

};

user.show();
```

Output:

```text
John
```

---

# Understanding `this`

Think of:

```javascript
this
```

as:

```text
Current Object
```

Example:

```javascript
const user = {

   name: "John",

   show(){

      console.log(this);

   }

};

user.show();
```

Output:

```javascript
{
   name:"John",
   show:f()
}
```

Because:

```text
user.show()
```

means:

```text
user is calling show()
```

So:

```javascript
this === user
```

---

# Rule #1 - Global Context

### Browser

```javascript
console.log(this);
```

Output:

```javascript
window
```

Because in browsers:

```javascript
this === window
```

---

# Visual

```text
Global Scope
      ↓
    Window
      ↓
     this
```

---

# Rule #2 - Inside Object Method

Example:

```javascript
const user = {

   name:"John",

   show(){

      console.log(this);
   }
};

user.show();
```

Output:

```javascript
user object
```

Because:

```text
user.show()
```

Here:

```javascript
this === user
```

---

# Access Object Properties

```javascript
const user = {

   name:"John",

   age:25,

   show(){

      console.log(this.name);

      console.log(this.age);

   }
};

user.show();
```

Output:

```text
John
25
```

---

# Why Not Use Variable Name?

Bad Practice:

```javascript
const user = {

   name:"John",

   show(){

      console.log(user.name);

   }
};
```

Problem:

If object name changes:

```javascript
const person = user;
```

Code becomes harder to maintain.

Better:

```javascript
this.name
```

---

# Rule #3 - Regular Function

Example:

```javascript
function test(){

   console.log(this);
}

test();
```

### Browser Output

```javascript
window
```

Because regular functions default to global object in non-strict mode.

---

# Strict Mode

```javascript
"use strict";

function test(){

   console.log(this);
}

test();
```

Output:

```javascript
undefined
```

---

# Rule #4 - Arrow Functions

Most Important Interview Topic.

Arrow functions do NOT have their own `this`.

Instead:

```text
Arrow Function
      ↓
Inherits this
From Parent Scope
```

---

# Example

```javascript
const user = {

   name:"John",

   show:()=>{

      console.log(this);

   }

};

user.show();
```

Output:

```javascript
window
```

(Not user)

---

# Why?

Arrow functions don't create their own:

```javascript
this
```

They borrow it from surrounding scope.

---

# Visual

```text
Global Scope
     ↓
window
     ↓
Arrow Function
     ↓
Uses Parent this
```

---

# Compare Normal vs Arrow

## Normal Function

```javascript
const user = {

   name:"John",

   show(){

      console.log(this.name);

   }

};

user.show();
```

Output:

```text
John
```

---

## Arrow Function

```javascript
const user = {

   name:"John",

   show:()=>{

      console.log(this.name);

   }

};

user.show();
```

Output:

```text
undefined
```

Why?

```javascript
this === window
```

And:

```javascript
window.name
```

doesn't exist.

---

# Interview Question

Predict Output:

```javascript
const person = {

   name:"Alex",

   greet(){

      return this.name;
   }
};

console.log(
   person.greet()
);
```

Output:

```text
Alex
```

Because:

```javascript
this === person
```

---

# Nested Function Problem

Example:

```javascript
const user = {

   name:"John",

   show(){

      function inner(){

         console.log(this.name);

      }

      inner();
   }
};

user.show();
```

Output:

```text
undefined
```

---

# Why?

Inside:

```javascript
inner()
```

it becomes a regular function call.

Therefore:

```javascript
this === window
```

Not:

```javascript
user
```

---

# Solution 1 - Store this

Old JavaScript approach.

```javascript
const user = {

   name:"John",

   show(){

      const self = this;

      function inner(){

         console.log(
            self.name
         );

      }

      inner();
   }
};
```

Output:

```text
John
```

---

# Solution 2 - Arrow Function

Modern approach.

```javascript
const user = {

   name:"John",

   show(){

      const inner = ()=>{

         console.log(
            this.name
         );

      };

      inner();
   }
};

user.show();
```

Output:

```text
John
```

Because arrow function inherits:

```javascript
this
```

from:

```javascript
show()
```

---

# this in Event Listeners

HTML:

```html
<button id="btn">
   Click
</button>
```

JavaScript:

```javascript
button.addEventListener(
   "click",
   function(){

      console.log(this);

   }
);
```

Output:

```html
<button>
```

Because:

```javascript
this
```

refers to the element that triggered event.

---

# Arrow Function in Events

```javascript
button.addEventListener(
   "click",
   ()=>{

      console.log(this);

   }
);
```

Output:

```javascript
window
```

Because arrow function doesn't have its own `this`.

---

# Constructor Functions

Example:

```javascript
function User(name){

   this.name = name;
}
```

Create:

```javascript
const user1 =
new User("John");
```

Internally:

```javascript
this === user1
```

---

# Visual

```text
new User()
      ↓
Create Object
      ↓
this → New Object
```

---

# Class Example

```javascript
class User{

   constructor(name){

      this.name = name;
   }

   show(){

      console.log(
         this.name
      );
   }
}

const user =
new User("John");

user.show();
```

Output:

```text
John
```

---

# Explicit Binding

JavaScript allows manually setting `this`.

Using:

```javascript
call()
apply()
bind()
```

Example:

```javascript
function greet(){

   console.log(this.name);
}

const user = {

   name:"John"
};

greet.call(user);
```

Output:

```text
John
```

More details are covered in:

```text
Call, Apply and Bind
```

---

# Common Interview Questions

## What is `this`?

`this` refers to the object that is currently executing the function.

---

## Does Arrow Function Have Its Own `this`?

No.

Arrow functions inherit `this` from their surrounding scope.

---

## What is `this` Inside an Object Method?

The object calling the method.

---

## What is `this` Inside a Regular Function?

### Non-Strict Mode

```javascript
window
```

### Strict Mode

```javascript
undefined
```

---

## What is `this` Inside Event Listener?

The HTML element that triggered the event.

---

## Why Use Arrow Functions Inside Nested Functions?

To preserve parent `this`.

---

# Summary Table

| Scenario               | Value of this      |
| ---------------------- | ------------------ |
| Global Scope (Browser) | window             |
| Object Method          | Current Object     |
| Regular Function       | window / undefined |
| Arrow Function         | Parent Scope this  |
| Event Listener         | Triggered Element  |
| Constructor Function   | New Object         |
| Class Method           | Class Instance     |

---

# Real React Example

```javascript
const App = () => {

   const handleClick = () => {

      console.log(this);

   };

   return (
      <button
         onClick={handleClick}
      >
         Click
      </button>
   );
};
```

React functional components primarily use arrow functions because they avoid `this` confusion.

---

# Most Important Topics

✅ Global `this`

✅ Object Method `this`

✅ Regular Function `this`

✅ Arrow Function `this`

✅ Nested Function Problem

✅ Event Listener `this`

✅ Constructor Function `this`

✅ Class Method `this`

✅ call()

✅ apply()

✅ bind()

---

# this Formula

```text
this
=
Who Calls The Function
```

or

```text
Function Call Site
        ↓
Determines
        ↓
this Value
```

The `this` keyword is one of the most frequently asked JavaScript interview topics and is essential for understanding objects, classes, event handlers, React, Node.js, and advanced JavaScript concepts.
