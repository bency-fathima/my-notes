# JavaScript Variables and Scope

## What is a Variable?

A variable is a named container in memory used to store data.

### Example

```javascript
let name = "John";
```

Here:

```text
Variable Name: name
Value: John
```

### Memory Representation

```text
Memory
-------------
name → "John"
-------------
```

When we access the variable:

```javascript
console.log(name);
```

Output:

```text
John
```

---

## Why Do We Need Variables?

### Without Variables

```javascript
console.log("John");
console.log("John");
console.log("John");
```

### With Variables

```javascript
let name = "John";

console.log(name);
console.log(name);
console.log(name);
```

Variables make code:

* Reusable
* Readable
* Maintainable

---

# Ways to Declare Variables

JavaScript provides three ways:

```javascript
var
let
const
```

---

# 1. var

Before ES6 (2015), developers mainly used `var`.

### Example

```javascript
var name = "John";

console.log(name);
```

Output:

```text
John
```

---

## var Can Be Redeclared

```javascript
var name = "John";

var name = "Alex";

console.log(name);
```

Output:

```text
Alex
```

Because redeclaration is allowed.

---

## var Can Be Reassigned

```javascript
var age = 20;

age = 25;

console.log(age);
```

Output:

```text
25
```

---

## var is Function Scoped

### Example

```javascript
function test() {

   var age = 20;

   console.log(age);
}

test();
```

Output:

```text
20
```

Outside function:

```javascript
console.log(age);
```

Output:

```text
ReferenceError
```

Because `var` exists only inside the function.

---

## var Ignores Block Scope

### Example

```javascript
if(true){

   var a = 10;
}

console.log(a);
```

Output:

```text
10
```

Many beginners expect:

```text
ReferenceError
```

But `var` ignores blocks.

Internally:

```javascript
var a;

if(true){
  a = 10;
}

console.log(a);
```

Output:

```text
10
```

This behavior often causes bugs.

---

# 2. let

ES6 introduced `let`.

```javascript
let age = 25;
```

---

## let Cannot Be Redeclared

```javascript
let age = 25;

let age = 30;
```

Output:

```text
SyntaxError
```

Because redeclaration isn't allowed in the same scope.

---

## let Can Be Reassigned

```javascript
let age = 25;

age = 30;

console.log(age);
```

Output:

```text
30
```

---

## let is Block Scoped

```javascript
if(true){

   let a = 10;

   console.log(a);
}
```

Output:

```text
10
```

Outside block:

```javascript
console.log(a);
```

Output:

```text
ReferenceError
```

### Memory

```text
Block
--------
a = 10
--------
```

After block execution:

```text
Block Destroyed
```

So the variable no longer exists.

---

# 3. const

Used for values that shouldn't change.

```javascript
const PI = 3.14;
```

---

## const Cannot Be Reassigned

```javascript
const PI = 3.14;

PI = 4;
```

Output:

```text
TypeError
```

---

## const Must Be Initialized

Wrong:

```javascript
const age;
```

Output:

```text
SyntaxError
```

Correct:

```javascript
const age = 20;
```

---

## const is Block Scoped

```javascript
if(true){

   const x = 10;

   console.log(x);
}
```

Outside:

```javascript
console.log(x);
```

Output:

```text
ReferenceError
```

---

## const and Objects (Very Important)

Many developers misunderstand this.

```javascript
const user = {
  name: "John"
};
```

### Can we change properties?

Yes.

```javascript
user.name = "Alex";

console.log(user);
```

Output:

```javascript
{
  name: "Alex"
}
```

---

### Can we reassign?

```javascript
user = {};
```

Output:

```text
TypeError
```

Why?

```text
Object Contents → Can Change

Reference → Cannot Change
```

---

# Scope

Scope determines:

```text
Where a variable can be accessed.
```

Think of scope as rooms in a house.

Some variables are accessible everywhere.

Some are accessible only inside a specific room.

---

# Types of Scope

1. Global Scope
2. Function Scope
3. Block Scope

---

# 1. Global Scope

Variables declared outside functions.

```javascript
let company = "OpenAI";

function show(){

  console.log(company);
}
```

Output:

```text
OpenAI
```

Accessible from anywhere.

### Memory

```text
Global Memory
--------------
company = OpenAI
--------------
```

---

### Example

```javascript
let name = "John";

function greet(){
  console.log(name);
}

function welcome(){
  console.log(name);
}
```

Both functions access `name`.

---

# 2. Function Scope

Variables exist only inside a function.

```javascript
function test(){

   let age = 20;

   console.log(age);
}

test();
```

Output:

```text
20
```

Outside:

```javascript
console.log(age);
```

Output:

```text
ReferenceError
```

---

### Memory Visualization

```javascript
function test(){

  let age = 20;
}
```

Execution:

```text
Function Memory
--------------
age = 20
--------------
```

After function completes:

```text
Memory Destroyed
```

---

# 3. Block Scope

A block means:

```javascript
{
}
```

Examples:

* if
* for
* while
* switch

---

### Example

```javascript
if(true){

   let city = "Kochi";

   console.log(city);
}
```

Output:

```text
Kochi
```

Outside:

```javascript
console.log(city);
```

Output:

```text
ReferenceError
```

---

### Another Example

```javascript
for(let i = 0; i < 3; i++){

   console.log(i);
}

console.log(i);
```

Output:

```text
0
1
2
ReferenceError
```

Because `i` belongs to the loop block.

---

# Scope Chain

JavaScript searches variables in a chain.

### Example

```javascript
let company = "OpenAI";

function outer(){

   let department = "Engineering";

   function inner(){

       let employee = "John";

       console.log(company);
       console.log(department);
       console.log(employee);
   }

   inner();
}

outer();
```

Output:

```text
OpenAI
Engineering
John
```

---

## How JavaScript Searches

When looking for:

```javascript
company
```

Search order:

```text
inner()
↓
outer()
↓
Global
```

This is called the:

```text
Scope Chain
```

---

# Shadowing

Inner variable hides outer variable.

```javascript
let name = "John";

function test(){

   let name = "Alex";

   console.log(name);
}

test();
```

Output:

```text
Alex
```

JavaScript finds the nearest variable first.

---

# Difference Between var, let, and const

| Feature   | var      | let   | const |
| --------- | -------- | ----- | ----- |
| Scope     | Function | Block | Block |
| Redeclare | Yes      | No    | No    |
| Reassign  | Yes      | Yes   | No    |
| Hoisted   | Yes      | Yes   | Yes   |
| TDZ       | No       | Yes   | Yes   |

---

# Best Practice

### Use const by default

```javascript
const PI = 3.14;
```

### Use let if value changes

```javascript
let count = 0;
```

### Avoid var

```javascript
var name = "John";
```

Modern JavaScript projects (React, Node.js, Next.js, MERN) rarely use `var`.

---

# JavaScript Functions

Functions are one of the most important concepts in JavaScript. Almost everything in JavaScript revolves around functions.

Think of a function as a machine:

```text
Input → Function → Output
```

### Example

```javascript
function add(a, b) {
   return a + b;
}

console.log(add(10, 20));
```

Visualization:

```text
10,20 → add() → 30
```
