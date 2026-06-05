# JavaScript Functions - Complete Notes

## What is a Function?

A function is a reusable block of code that performs a specific task.

### Without Function

```javascript
console.log("Welcome John");
console.log("Welcome Alex");
console.log("Welcome Bob");
```

This causes repetition.

### With Function

```javascript
function welcome(name){
   console.log(`Welcome ${name}`);
}

welcome("John");
welcome("Alex");
welcome("Bob");
```

### Output

```text
Welcome John
Welcome Alex
Welcome Bob
```

Now one function handles multiple users.

---

# Function Declaration

Also called Function Statement.

```javascript
function greet() {
   console.log("Hello");
}

greet();
```

### Output

```text
Hello
```

---

## How JavaScript Stores It

```javascript
function greet(){
   console.log("Hello");
}
```

During memory creation phase:

```text
Memory
----------------
greet → Entire Function
----------------
```

Unlike variables, functions are completely stored in memory.

---

# Function with Parameters

```javascript
function greet(name){
   console.log("Hello " + name);
}

greet("John");
```

### Output

```text
Hello John
```

---

# Parameters vs Arguments

One of the most asked interview questions.

```javascript
function add(a,b){
   return a+b;
}

add(10,20);
```

---

## Parameters

Variables defined while creating a function.

```javascript
function add(a,b)
```

Here:

```text
a,b
```

are Parameters.

---

## Arguments

Values passed during function call.

```javascript
add(10,20);
```

Here:

```text
10,20
```

are Arguments.

---

## Think Like This

```text
Function Definition
↓
Parameters

Function Call
↓
Arguments
```

### Example

```javascript
function welcome(name){

}

welcome("John");
```

```text
Parameter = name

Argument = John
```

---

# Return Keyword

Many beginners confuse `console.log()` and `return`.

---

## Without Return

```javascript
function add(a,b){
   console.log(a+b);
}

const result = add(10,20);

console.log(result);
```

### Output

```text
30
undefined
```

### Why?

Because `console.log()` only prints.

Nothing is returned.

---

## With Return

```javascript
function add(a,b){
   return a+b;
}

const result = add(10,20);

console.log(result);
```

### Output

```text
30
```

---

## Return Stops Execution

```javascript
function test(){

   console.log("A");

   return;

   console.log("B");
}

test();
```

### Output

```text
A
```

Code after `return` never executes.

---

# Function Expression

Function assigned to a variable.

```javascript
const greet = function(){
   console.log("Hello");
};

greet();
```

### Output

```text
Hello
```

---

## How It Works

```javascript
const greet = function(){

};
```

Function becomes a value.

Just like:

```javascript
const age = 25;
```

```javascript
const greet = function(){};
```

Both are values.

---

# Difference Between Declaration and Expression

## Function Declaration

```javascript
greet();

function greet(){
   console.log("Hello");
}
```

### Output

```text
Hello
```

Works because of hoisting.

---

## Function Expression

```javascript
greet();

const greet = function(){
   console.log("Hello");
}
```

### Output

```text
ReferenceError
```

Because variable isn't initialized yet.

---

# Anonymous Function

Function without a name.

```javascript
const greet = function(){

};
```

This part:

```javascript
function(){

}
```

is an anonymous function.

---

# Named Function Expression

```javascript
const greet = function hello(){

   console.log("Hello");
}
```

Function has a name.

```text
hello
```

Used mainly for debugging.

---

# Arrow Functions

Introduced in ES6.

---

## Normal Function

```javascript
function add(a,b){
   return a+b;
}
```

---

## Arrow Function

```javascript
const add = (a,b)=>{
   return a+b;
}
```

---

## Short Form

If one line:

```javascript
const add = (a,b)=>a+b;
```

Same as:

```javascript
const add = (a,b)=>{
   return a+b;
}
```

---

## Single Parameter

```javascript
const square = num => num*num;
```

No brackets needed.

---

## No Parameters

```javascript
const greet = () => {
   console.log("Hello");
}
```

---

# Arrow Function vs Normal Function

Very Important Interview Question.

---

## Normal Function has its own this

```javascript
const user = {

   name:"John",

   show:function(){
       console.log(this.name);
   }
};

user.show();
```

### Output

```text
John
```

---

## Arrow Function Doesn't Have its Own this

```javascript
const user = {

   name:"John",

   show:()=>{
       console.log(this.name);
   }
};

user.show();
```

### Output

```text
undefined
```

### Why?

Arrow function inherits `this` from surrounding scope.

---

# First Class Functions

JavaScript treats functions like values.

Meaning functions can:

### Be stored in variables

```javascript
const greet = function(){};
```

### Be passed as arguments

```javascript
process(greet);
```

### Be returned from functions

```javascript
function test(){

   return function(){

   };
}
```

---

# Callback Functions

A function passed into another function.

```javascript
function greet(callback){

   console.log("Hello");

   callback();
}

function bye(){

   console.log("Bye");
}

greet(bye);
```

### Output

```text
Hello
Bye
```

---

# Why Callbacks?

Used when one task should execute after another.

### Example

```javascript
downloadFile(()=>{
   sendEmail();
});
```

```text
Download Complete
↓
Send Email
```

---

# Higher Order Functions (HOF)

A function that:

1. Accepts another function
2. Returns another function

---

## Accepting Function

```javascript
function process(callback){

   callback();
}

process(()=>{
   console.log("Running");
});
```

### Output

```text
Running
```

---

## Returning Function

```javascript
function outer(){

   return function(){

       console.log("Inner");
   };
}

const fn = outer();

fn();
```

### Output

```text
Inner
```

---

# Real Examples of Higher Order Functions

## map()

```javascript
const nums = [1,2,3];

const doubled = nums.map(num=>num*2);

console.log(doubled);
```

### Output

```text
[2,4,6]
```

---

## filter()

```javascript
const nums = [10,20,30,40];

const result = nums.filter(
   num => num > 20
);

console.log(result);
```

### Output

```text
[30,40]
```

---

## reduce()

```javascript
const nums = [1,2,3,4];

const total = nums.reduce(
   (acc,curr)=>acc+curr,
   0
);

console.log(total);
```

### Output

```text
10
```

---

# Function Execution Context

When a function runs:

```javascript
function add(a,b){

   let result = a+b;

   return result;
}

add(10,20);
```

JavaScript creates:

```text
Function Execution Context
```

Containing:

```text
Memory Area
Code Area
```

### Memory

```text
a = 10
b = 20
result = 30
```

After execution:

```text
Context Destroyed
```

Memory is released.

---

# Closures Preview

Function returning function.

```javascript
function outer(){

   let count = 0;

   return function(){

       count++;

       console.log(count);
   }
}
```

Even after `outer()` finishes:

```javascript
const counter = outer();

counter();
counter();
```

### Output

```text
1
2
```

The inner function remembers variables from the outer function.

This concept is called a **Closure**.

---

# Important Function Concepts for Interviews

* Function Declaration
* Function Expression
* Arrow Functions
* Parameters vs Arguments
* Return Statement
* Anonymous Functions
* Named Function Expressions
* First Class Functions
* Callback Functions
* Higher Order Functions
* Function Execution Context
* Closures
* this Keyword
* map()
* filter()
* reduce()

These concepts form the foundation of React, Node.js, Express.js, and MERN Stack development.
