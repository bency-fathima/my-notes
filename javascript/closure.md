# JavaScript Closures - Complete Notes

# What is a Closure?

A closure is one of the most important concepts in JavaScript.

### Definition

A closure is created when a function remembers and can access variables from its outer scope even after the outer function has finished executing.

In simple words:

```text
Inner Function
        +
Outer Function Variables
        =
      Closure
```

---

# Why Closures Are Important?

Normally, local variables disappear after a function finishes execution.

Example:

```javascript
function test(){

   let age = 25;

   console.log(age);
}

test();
```

After execution:

```text
Function Ends
      ↓
Memory Destroyed
      ↓
age Removed
```

But closures behave differently.

They allow inner functions to remember variables even after the outer function has completed.

---

# Basic Closure Example

```javascript
function counter(){

   let count = 0;

   return function(){

      count++;

      console.log(count);

   };
}

const c = counter();

c();
c();
c();
```

### Output

```text
1
2
3
```

---

# Why Does This Work?

Many beginners expect:

```text
1
1
1
```

But output is:

```text
1
2
3
```

Let's understand step-by-step.

---

# Step 1

```javascript
const c = counter();
```

Execution:

```javascript
function counter(){

   let count = 0;

   return function(){

      count++;

      console.log(count);

   };
}
```

Memory:

```text
counter()
--------------
count = 0
--------------
```

Returns:

```javascript
function(){

   count++;

   console.log(count);

}
```

Stored inside:

```javascript
c
```

---

# Step 2

```javascript
c();
```

Execution:

```javascript
count++;
```

Count becomes:

```text
1
```

Output:

```text
1
```

---

# Step 3

```javascript
c();
```

Closure remembers:

```text
count = 1
```

Now:

```javascript
count++;
```

Becomes:

```text
2
```

Output:

```text
2
```

---

# Step 4

```javascript
c();
```

Closure remembers:

```text
count = 2
```

Now:

```text
3
```

Output:

```text
3
```

---

# Visual Representation

```text
counter()
│
├── count = 0
│
└── returns inner function
          │
          ▼
       Closure
          │
          ▼
 remembers count forever
```

---

# Important Point

The outer function has finished executing.

Normally memory should be removed.

But because the inner function still needs:

```javascript
count
```

JavaScript keeps it alive.

This preserved memory is called a:

```text
Closure
```

---

# Another Example

```javascript
function greet(){

   let name = "John";

   return function(){

      console.log(name);

   };
}

const show = greet();

show();
```

### Output

```text
John
```

---

# How Scope Chain Works with Closures

```javascript
function outer(){

   let company = "OpenAI";

   function inner(){

      console.log(company);

   }

   return inner;
}

const fn = outer();

fn();
```

### Output

```text
OpenAI
```

---

Search Process:

```text
inner()
   ↓
outer()
   ↓
Global Scope
```

This is called:

```text
Lexical Scope
```

Closures use lexical scope.

---

# Closure with Parameters

```javascript
function multiply(x){

   return function(y){

      return x * y;

   };
}

const double = multiply(2);

console.log(
   double(5)
);
```

### Output

```text
10
```

---

Explanation:

```javascript
multiply(2)
```

Returns:

```javascript
function(y){

   return 2 * y;
}
```

Now:

```javascript
double(5)
```

Becomes:

```javascript
2 * 5
```

Output:

```text
10
```

---

# Real World Example - Private Variables

Closures help create private variables.

```javascript
function bankAccount(){

   let balance = 1000;

   return {

      getBalance(){

         return balance;
      },

      deposit(amount){

         balance += amount;
      }

   };
}
```

Usage:

```javascript
const account =
bankAccount();

console.log(
   account.getBalance()
);

account.deposit(500);

console.log(
   account.getBalance()
);
```

### Output

```text
1000
1500
```

---

Cannot access directly:

```javascript
console.log(
   account.balance
);
```

Output:

```text
undefined
```

Because balance is private.

---

# Closure in Event Handlers

Very common in JavaScript.

```javascript
function setupButton(){

   let clicks = 0;

   button.addEventListener(
      "click",
      ()=>{

         clicks++;

         console.log(clicks);

      }
   );
}
```

Every click remembers previous count.

Output:

```text
1
2
3
4
5
```

---

# Closure in setTimeout()

```javascript
function test(){

   let message = "Hello";

   setTimeout(()=>{

      console.log(message);

   },2000);
}

test();
```

Output after 2 seconds:

```text
Hello
```

Even though:

```javascript
test()
```

already finished.

Why?

Closure remembers:

```javascript
message
```

---

# Common Interview Question

## Predict Output

```javascript
function outer(){

   let count = 0;

   return function(){

      count++;

      console.log(count);

   };
}

const c1 = outer();

const c2 = outer();

c1();
c1();
c2();
```

### Output

```text
1
2
1
```

Why?

Each call creates a separate closure.

Memory:

```text
Closure 1
count = 2

Closure 2
count = 1
```

---

# Closure vs Normal Function

### Normal Function

```javascript
function test(){

   let x = 10;
}
```

After execution:

```text
Memory Destroyed
```

---

### Closure

```javascript
function test(){

   let x = 10;

   return function(){

      console.log(x);

   };
}
```

After execution:

```text
Memory Preserved
```

Because inner function needs it.

---

# Closure in React

React Hooks heavily use closures.

Example:

```javascript
function Counter(){

   const [count,setCount] =
   useState(0);

   function increment(){

      setCount(count + 1);

   }

   return (
      <button
         onClick={increment}
      >
         {count}
      </button>
   );
}
```

The event handler remembers:

```javascript
count
```

through closures.

---

# Closure in Currying

```javascript
function add(a){

   return function(b){

      return a + b;

   };
}

console.log(
   add(5)(10)
);
```

Output:

```text
15
```

Closure remembers:

```javascript
a = 5
```

---

# Advantages of Closures

### Data Privacy

```javascript
Private Variables
```

---

### State Preservation

Remember values between function calls.

---

### Event Handlers

Maintain data across user interactions.

---

### Functional Programming

Used in:

* Currying
* Memoization
* Higher Order Functions

---

### React Hooks

Foundation of React's state behavior.

---

# Disadvantages of Closures

### Memory Usage

Closures keep variables alive.

```text
More Memory Consumption
```

if used carelessly.

---

### Harder Debugging

Nested closures can be difficult to understand.

---

# Most Important Closure Interview Questions

## What is a Closure?

A closure is a function that remembers variables from its outer scope even after the outer function has completed execution.

---

## Why does count persist?

Because the returned function keeps a reference to the outer variable.

---

## Where are Closures Used?

* React Hooks
* Event Handlers
* setTimeout
* Private Variables
* Currying
* Memoization

---

# Closure Formula

```text
Closure
=
Function
+
Lexical Environment
```

or

```text
Closure
=
Inner Function
+
Remembered Outer Variables
```

---

# Topics You Must Master

✅ Lexical Scope

✅ Scope Chain

✅ Function Returning Function

✅ State Preservation

✅ Private Variables

✅ Event Handlers

✅ setTimeout Closures

✅ React Hooks

✅ Currying

✅ Closure Memory Behavior

Closures are one of the most frequently asked JavaScript interview topics and are considered a core concept for React, Node.js, and MERN Stack developers.
