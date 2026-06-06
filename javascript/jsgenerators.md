# JavaScript Generators - Complete Notes

# What are Generators?

Generators are special functions that can:

```text
Pause Execution
      ↓
Save State
      ↓
Resume Later
```

Unlike normal functions, generators don't execute completely in one go.

They execute step-by-step.

---

# Why Do We Need Generators?

Normal Function:

```javascript
function test(){

   console.log("A");

   console.log("B");

   console.log("C");

}

test();
```

Output:

```text
A
B
C
```

Execution happens all at once.

No way to pause.

---

# Generator Function

```javascript
function* generate(){

   yield 1;

   yield 2;

   yield 3;

}
```

Notice:

```javascript
function*
```

The asterisk:

```javascript
*
```

makes it a generator function.

---

# Important Difference

Normal Function:

```javascript
function greet(){}
```

Generator Function:

```javascript
function* greet(){}
```

---

# Calling a Generator

Normal Function:

```javascript
const result = test();
```

Output:

```text
Function Executes Immediately
```

---

Generator:

```javascript
const g = generate();
```

Output:

```text
Nothing Executes Yet
```

Why?

Because:

```javascript
generate()
```

returns a:

```text
Generator Object
```

not the final result.

---

# Generator Object

```javascript
const g = generate();

console.log(g);
```

Output:

```javascript
Generator {}
```

Think of it as:

```text
Remote Control
For Function Execution
```

---

# The next() Method

To execute generator code:

```javascript
g.next();
```

Each call moves execution forward.

---

# Example

```javascript
function* generate(){

   yield 1;

   yield 2;

   yield 3;

}

const g = generate();

console.log(g.next());
```

Output:

```javascript
{
   value:1,
   done:false
}
```

---

# Understanding What Happened

Execution:

```javascript
yield 1;
```

Generator pauses.

Returns:

```javascript
{
   value:1,
   done:false
}
```

Meaning:

```text
Current Value = 1

Generator Not Finished
```

---

# Second next()

```javascript
console.log(
   g.next()
);
```

Output:

```javascript
{
   value:2,
   done:false
}
```

Execution resumes from:

```javascript
yield 2;
```

---

# Third next()

```javascript
console.log(
   g.next()
);
```

Output:

```javascript
{
   value:3,
   done:false
}
```

Execution pauses again.

---

# Fourth next()

```javascript
console.log(
   g.next()
);
```

Output:

```javascript
{
   value:undefined,
   done:true
}
```

Generator completed.

---

# Visual Representation

Generator:

```javascript
function* generate(){

   yield 1;

   yield 2;

   yield 3;

}
```

Execution Flow:

```text
Start
 ↓

yield 1
 ↓
Pause

next()
 ↓

yield 2
 ↓
Pause

next()
 ↓

yield 3
 ↓
Pause

next()
 ↓

Done
```

---

# Understanding yield

The most important generator keyword.

Think of:

```javascript
yield
```

as:

```text
return
+
pause
```

Normal return:

```javascript
return 1;
```

Returns value and ends function.

---

Yield:

```javascript
yield 1;
```

Returns value but pauses function.

Can continue later.

---

# Return vs Yield

Return:

```javascript
function test(){

   return 1;

   return 2;

}
```

Output:

```text
1
```

Function ends.

---

Yield:

```javascript
function* test(){

   yield 1;

   yield 2;

}
```

Output:

```text
1
2
```

Execution continues.

---

# Memory Visualization

Generator State:

```text
Generator

Current Line
     ↓
yield 1

Variables
     ↓
Stored
```

When paused:

```text
State Saved
```

When resumed:

```text
Continue From Last Position
```

---

# Example with Variables

```javascript
function* counter(){

   let count = 1;

   yield count;

   count++;

   yield count;

   count++;

   yield count;

}
```

Usage:

```javascript
const c =
counter();

console.log(
   c.next()
);

console.log(
   c.next()
);

console.log(
   c.next()
);
```

Output:

```javascript
{value:1,done:false}

{value:2,done:false}

{value:3,done:false}
```

---

# Why Does Count Persist?

Because generators save their state.

Normal functions destroy memory after execution.

Generators remember:

```text
Variables
Execution Position
State
```

---

# Infinite Sequence Example

Very common interview example.

```javascript
function* infiniteCounter(){

   let count = 1;

   while(true){

      yield count++;

   }

}
```

Usage:

```javascript
const counter =
infiniteCounter();

console.log(
   counter.next().value
);

console.log(
   counter.next().value
);

console.log(
   counter.next().value
);
```

Output:

```text
1
2
3
```

Can continue forever.

---

# Why Generators Are Useful Here?

Without generators:

```javascript
while(true){

}
```

Infinite loop.

Application freezes.

---

With generators:

```text
Generate Values
One At A Time
```

No freeze.

---

# Passing Values Into Generators

Generators can receive data.

Example:

```javascript
function* test(){

   const value =
   yield "Enter Value";

   console.log(value);

}
```

Usage:

```javascript
const g = test();

console.log(
   g.next()
);

g.next("John");
```

Output:

```javascript
{
   value:"Enter Value",
   done:false
}
```

Then:

```text
John
```

---

# How It Works

First:

```javascript
g.next();
```

Stops at:

```javascript
yield "Enter Value";
```

---

Second:

```javascript
g.next("John");
```

Value:

```javascript
"John"
```

is assigned to:

```javascript
value
```

---

# Iterating Generators

Generators are iterable.

Example:

```javascript
function* numbers(){

   yield 1;

   yield 2;

   yield 3;

}
```

Use:

```javascript
for(const num of numbers()){

   console.log(num);

}
```

Output:

```text
1
2
3
```

---

# Spread Operator with Generators

```javascript
function* numbers(){

   yield 1;

   yield 2;

   yield 3;

}
```

Convert to array:

```javascript
console.log(
   [...numbers()]
);
```

Output:

```javascript
[1,2,3]
```

---

# Real World Use Cases

## Large Data Processing

Instead of:

```text
Load 1 Million Records
```

Load:

```text
One Record At A Time
```

---

## Infinite Sequences

```text
IDs
Counters
Streams
```

---

## Pagination

```text
Load Data
Page By Page
```

---

## Custom Iterators

```text
Collections
Trees
Graphs
```

---

## Async Programming

Before Async/Await:

```text
Generators
+
Promises
```

were used.

---

# Generator vs Normal Function

| Feature                  | Normal Function | Generator |
| ------------------------ | --------------- | --------- |
| Executes Immediately     | Yes             | No        |
| Pauses Execution         | No              | Yes       |
| Resumes Execution        | No              | Yes       |
| Uses yield               | No              | Yes       |
| Returns Generator Object | No              | Yes       |

---

# Generator vs Return

Return:

```javascript
return value;
```

Output:

```text
Value Returned
Function Ends
```

---

Yield:

```javascript
yield value;
```

Output:

```text
Value Returned
Function Paused
```

---

# Interview Question

Predict Output:

```javascript
function* test(){

   yield 1;

   yield 2;

}

const g =
test();

console.log(
   g.next()
);

console.log(
   g.next()
);

console.log(
   g.next()
);
```

Output:

```javascript
{
 value:1,
 done:false
}

{
 value:2,
 done:false
}

{
 value:undefined,
 done:true
}
```

---

# Common Interview Questions

## What is a Generator?

A special function that can pause and resume execution.

---

## What Keyword Creates a Generator?

```javascript
function*
```

---

## What Does yield Do?

Returns a value and pauses execution.

---

## What Does next() Do?

Resumes execution until the next yield.

---

## What Does done Mean?

```javascript
done:true
```

Generator completed.

---

## Can Generators Be Infinite?

Yes.

Example:

```javascript
while(true){

   yield count++;

}
```

---

## Are Generators Iterable?

Yes.

Can be used with:

```javascript
for...of
```

and

```javascript
spread operator (...)
```

---

# Real MERN Stack Relevance

Generators are less common in modern React applications but are useful for:

### Redux Saga

```text
Side Effects
API Calls
```

Redux Saga heavily uses generators.

Example:

```javascript
function* fetchUsers(){

   yield call(api);

}
```

---

### Data Streaming

```text
Large Files
Large Datasets
```

---

### Custom Iterators

```text
Complex Data Structures
```

---

# Most Important Topics

✅ Generator Functions

✅ function*

✅ yield

✅ next()

✅ Generator Objects

✅ done Property

✅ Infinite Generators

✅ Iterables

✅ for...of

✅ Redux Saga

---

# Generator Formula

```text
Generator Function
        ↓
Generator Object
        ↓
next()
        ↓
yield
        ↓
Pause
        ↓
next()
        ↓
Resume
```

or

```text
Generators
=
Function
+
yield
+
Pause & Resume
```

Generators are an advanced JavaScript feature that allow functions to pause and resume execution while preserving their internal state. They are useful for lazy evaluation, custom iterators, infinite sequences, Redux Saga, and memory-efficient data processing, making them an important topic for intermediate and advanced JavaScript developers.
