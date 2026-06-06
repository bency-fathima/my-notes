# JavaScript Memoization - Complete Notes

# What is Memoization?

Memoization is an optimization technique used to improve performance by storing the results of expensive function calls and reusing them when the same inputs occur again.

Instead of:

```text
Calculate Again
```

we do:

```text
Calculate Once
      ↓
Store Result
      ↓
Reuse Result
```

Think of it like:

```text
First Time
   ↓
Compute Answer
   ↓
Save Answer

Second Time
   ↓
Use Saved Answer
```

---

# Why Do We Need Memoization?

Imagine a heavy calculation:

```javascript
function square(num){

   console.log("Calculating");

   return num * num;

}

console.log(square(5));
console.log(square(5));
console.log(square(5));
```

Output:

```text
Calculating
25

Calculating
25

Calculating
25
```

The same calculation runs repeatedly.

This wastes:

* CPU Time
* Memory Resources
* Performance

---

# Desired Behavior

First call:

```text
Calculate
```

Second call:

```text
Use Cached Result
```

Third call:

```text
Use Cached Result
```

Output:

```text
Calculating
25

Cached
25

Cached
25
```

This is Memoization.

---

# Real Life Example

Imagine a math teacher.

Student asks:

```text
What is 500 × 500?
```

Teacher calculates:

```text
250000
```

Teacher writes answer in notebook.

Later:

```text
What is 500 × 500?
```

Teacher checks notebook instead of calculating again.

Notebook = Cache

This is Memoization.

---

# Basic Memoization Example

```javascript
function memoized(){

   let cache = {};

   return function(num){

      if(cache[num]){

         console.log("Cached");

         return cache[num];

      }

      cache[num] = num * num;

      return cache[num];

   };

}
```

---

# Understanding Step-by-Step

## Step 1

Create cache.

```javascript
let cache = {};
```

Initially:

```javascript
{}
```

Empty object.

---

## Step 2

Call function.

```javascript
memoFn(5);
```

Check:

```javascript
cache[5]
```

Output:

```javascript
undefined
```

Not found.

---

## Step 3

Calculate.

```javascript
5 * 5
```

Result:

```javascript
25
```

Store:

```javascript
cache[5] = 25;
```

Cache:

```javascript
{
   5:25
}
```

---

## Step 4

Call again.

```javascript
memoFn(5);
```

Check:

```javascript
cache[5]
```

Output:

```javascript
25
```

Found.

Return directly.

No recalculation.

---

# Complete Example

```javascript
function memoized(){

   let cache = {};

   return function(num){

      if(cache[num]){

         console.log("Cached");

         return cache[num];

      }

      console.log("Calculating");

      cache[num] = num * num;

      return cache[num];

   };

}

const square =
memoized();

console.log(square(5));

console.log(square(5));

console.log(square(5));
```

Output:

```text
Calculating
25

Cached
25

Cached
25
```

---

# Visual Representation

First Call:

```text
square(5)

Cache Empty
     ↓
Calculate
     ↓
Store

Cache:
{
 5:25
}
```

---

Second Call:

```text
square(5)

Cache Exists
     ↓
Return 25
```

No calculation.

---

# Why Does Memoization Work?

Because of:

```text
Closures
```

Example:

```javascript
function memoized(){

   let cache = {};

   return function(num){

   };

}
```

Even after:

```javascript
memoized()
```

finishes,

the returned function still remembers:

```javascript
cache
```

This is closure.

---

# Memory Visualization

```text
memoized()
      ↓

cache = {}

      ↓

Returned Function
      ↓

Remembers Cache
```

Even after outer function completes.

---

# Expensive Calculation Example

Without Memoization:

```javascript
function factorial(n){

   console.log(
      "Calculating"
   );

   if(n === 0)
      return 1;

   return n *
   factorial(n-1);

}
```

Every call recalculates.

---

With Memoization:

```javascript
function memoFactorial(){

   const cache = {};

   return function factorial(n){

      if(cache[n]){

         return cache[n];

      }

      let result;

      if(n === 0){

         result = 1;

      }else{

         result =
         n *
         factorial(n-1);

      }

      cache[n] = result;

      return result;

   };

}
```

Repeated calls become much faster.

---

# Generic Memoization Function

Professional approach.

```javascript
function memoize(fn){

   const cache = {};

   return function(...args){

      const key =
      JSON.stringify(args);

      if(cache[key]){

         return cache[key];

      }

      const result =
      fn(...args);

      cache[key] =
      result;

      return result;

   };

}
```

---

# Example

Normal Function:

```javascript
function add(a,b){

   console.log(
      "Calculating"
   );

   return a+b;

}
```

Memoized:

```javascript
const memoAdd =
memoize(add);
```

Usage:

```javascript
memoAdd(2,3);

memoAdd(2,3);

memoAdd(2,3);
```

Output:

```text
Calculating
5

5

5
```

Only first call calculates.

---

# Visual Representation

```text
add(2,3)

Cache Empty
    ↓
Calculate
    ↓
Store

Cache:
{
 "[2,3]":5
}
```

Next Call:

```text
Cache Hit
    ↓
Return 5
```

---

# Cache Hit vs Cache Miss

Very common interview topic.

### Cache Miss

Data not found.

```text
Cache Empty
     ↓
Calculate
```

---

### Cache Hit

Data found.

```text
Result Exists
     ↓
Return Directly
```

---

# Memoization vs Caching

Often used interchangeably.

### Caching

General storage mechanism.

Example:

```text
Browser Cache
API Cache
Database Cache
```

---

### Memoization

Caching specifically for function results.

```javascript
memoizedFunction()
```

---

# React and Memoization

Very important for interviews.

React applications often perform unnecessary calculations.

Example:

```javascript
const total =
calculateTotal(products);
```

If component re-renders:

```text
Calculate Again
```

even if data didn't change.

---

# React useMemo()

React provides built-in memoization.

Example:

```javascript
const total =
useMemo(()=>{

   return calculateTotal(
      products
   );

},[products]);
```

Now calculation runs only when:

```javascript
products
```

changes.

---

# React.memo()

Memoizes components.

```javascript
const User =
React.memo(
   function User(){
      return <div>User</div>;
   }
);
```

Avoids unnecessary renders.

---

# Real World Uses

## Heavy Calculations

```text
Factorial
Fibonacci
Mathematics
```

---

## API Data Processing

```text
Filtering
Sorting
Searching
```

---

## React Optimization

```text
useMemo
React.memo
```

---

## Dashboard Calculations

```text
Reports
Analytics
Charts
```

---

## Data Tables

```text
Large Datasets
```

---

# Fibonacci Example

Without Memoization:

```javascript
function fib(n){

   if(n<=1)
      return n;

   return fib(n-1)
      + fib(n-2);

}
```

Very slow.

---

With Memoization:

```javascript
function fibMemo(){

   const cache = {};

   return function fib(n){

      if(cache[n]){

         return cache[n];

      }

      if(n<=1)
         return n;

      cache[n] =
      fib(n-1) +
      fib(n-2);

      return cache[n];

   };

}
```

Huge performance improvement.

---

# Common Interview Questions

## What is Memoization?

An optimization technique that stores function results and reuses them for repeated inputs.

---

## Why Use Memoization?

To avoid expensive recalculations.

---

## What Concept Makes Memoization Possible?

```text
Closures
```

---

## What is a Cache?

Storage used to save computed results.

---

## Difference Between Memoization and Caching?

Memoization caches function outputs.

Caching is a broader concept.

---

## What React Hooks Use Memoization?

```javascript
useMemo()
```

and

```javascript
React.memo()
```

---

## What is Cache Hit?

Result found in cache.

---

## What is Cache Miss?

Result not found.

Function must execute.

---

# Memoization vs Debouncing vs Throttling

| Feature                  | Memoization   | Debouncing      | Throttling      |
| ------------------------ | ------------- | --------------- | --------------- |
| Purpose                  | Store Results | Delay Execution | Limit Frequency |
| Uses Cache               | Yes           | No              | No              |
| Performance Optimization | Yes           | Yes             | Yes             |
| Search Bars              | Sometimes     | Yes             | No              |
| Heavy Calculations       | Yes           | No              | No              |
| Scroll Events            | No            | No              | Yes             |

---

# Real MERN Stack Usage

Memoization is commonly used in:

### React Components

```javascript
useMemo()
```

### Dashboard Calculations

```javascript
Sales Reports
```

### Data Tables

```javascript
Filtering
Sorting
Pagination
```

### Analytics

```javascript
Charts
Graphs
```

### Expensive API Processing

```javascript
Transforming Large Data
```

---

# Most Important Topics

✅ Memoization

✅ Closures

✅ Cache

✅ Cache Hit

✅ Cache Miss

✅ Function Optimization

✅ React useMemo

✅ React.memo

✅ Expensive Calculations

✅ Fibonacci Optimization

---

# Memoization Formula

```text
Function Call
      ↓
Result In Cache?
   /           \
 Yes            No
  ↓              ↓
Return       Calculate
Result           ↓
                Store
                  ↓
              Return
```

or

```text
Memoization
=
Closure
+
Cache
+
Function Optimization
```

Memoization is one of the most important JavaScript performance optimization techniques. It is heavily used in React applications, dashboard systems, analytics platforms, large-scale MERN applications, and technical interviews for developers with 2+ years of experience.
