# Node.js Internals & Fundamentals - Deep Dive

# Introduction to Node.js

Node.js is not a programming language.

Node.js is not a framework.

Node.js is a JavaScript Runtime Environment built on Google's V8 JavaScript Engine.

It allows JavaScript code to execute outside the browser.

```text
JavaScript
      ↓
Node.js Runtime
      ↓
Operating System
```

Before Node.js:

```text
JavaScript → Browser
```

After Node.js:

```text
JavaScript → Browser + Backend + CLI + APIs + Microservices
```

---

# Why Was Node.js Created?

Traditional web servers followed a thread-per-request model.

Example:

```text
User 1 Request
      ↓
   Thread 1

User 2 Request
      ↓
   Thread 2

User 3 Request
      ↓
   Thread 3
```

Problems:

* Huge memory consumption
* Context switching overhead
* Difficult scalability

Ryan Dahl introduced Node.js in 2009 with the idea:

```text
One Thread
+
Event Loop
+
Non Blocking I/O
```

to handle thousands of simultaneous connections.

---

# Node.js Internal Architecture

Node.js consists of several components.

```text
                 Application Code
                         |
                         ▼
                    V8 Engine
                         |
                         ▼
                 Node.js APIs
                         |
                         ▼
                       Libuv
                         |
        --------------------------------
        |              |              |
        ▼              ▼              ▼
   Event Loop     Thread Pool      OS Kernel
```

---

# Components of Node.js

## 1. V8 Engine

V8 is Google's JavaScript Engine written in C++.

Responsibilities:

* Parses JavaScript
* Compiles JavaScript
* Executes JavaScript

Example:

```js
const x = 10;
console.log(x);
```

V8 converts JavaScript into machine code.

---

# How V8 Works

### Step 1

Source Code

```js
const x = 10;
```

### Step 2

Parser

Creates:

```text
AST
(Abstract Syntax Tree)
```

Example:

```text
VariableDeclaration
    |
    └── x = 10
```

### Step 3

Ignition Interpreter

Creates:

```text
Bytecode
```

### Step 4

TurboFan Compiler

Optimizes frequently executed code.

Converts bytecode into:

```text
Machine Code
```

This is called:

```text
JIT Compilation
(Just In Time Compilation)
```

---

# Why Node.js Is Fast

Because:

```text
V8
+
JIT Compilation
+
Optimized Garbage Collection
```

makes execution extremely fast.

---

# Node.js Architecture

Node.js follows:

```text
Event Driven Architecture
```

Meaning:

```text
Action Occurs
      ↓
Event Generated
      ↓
Listener Executes
```

Examples:

* HTTP Request
* Database Response
* File Read
* User Login

Everything is event based.

---

# Understanding Single Threaded Nature

Most asked interview question.

---

## Is Node.js Single Threaded?

Yes.

JavaScript code executes on:

```text
One Main Thread
```

Example:

```js
console.log("A");
console.log("B");
console.log("C");
```

Execution:

```text
A
B
C
```

One statement at a time.

---

# Then Why Is Node.js So Fast?

Because only JavaScript runs on a single thread.

Node.js itself is NOT entirely single-threaded.

Internally it uses:

```text
Libuv Thread Pool
```

for heavy operations.

---

# JavaScript Thread

Handles:

```text
Function Execution
Callbacks
Promises
Events
```

---

# Thread Pool

Handles:

```text
File System
DNS
Compression
Encryption
Crypto
```

Default:

```text
4 Threads
```

Can increase:

```bash
UV_THREADPOOL_SIZE=8
```

---

# Example

```js
const crypto = require("crypto");

crypto.pbkdf2(
 "password",
 "salt",
 100000,
 512,
 "sha512",
 ()=>{
   console.log("Done");
 }
);
```

This does NOT run on the JavaScript thread.

It runs in:

```text
Thread Pool
```

---

# What Is I/O?

I/O means:

```text
Input / Output
```

Examples:

```text
Reading File
Writing File
Database Query
Network Request
API Call
```

---

# Blocking I/O

Blocking means:

```text
Wait Until Task Finishes
```

Example:

```js
const fs = require("fs");

const data =
fs.readFileSync("data.txt");

console.log(data.toString());
```

Execution:

```text
Read File
      ↓
Wait
      ↓
Print
```

Everything stops.

---

# Problems With Blocking I/O

Suppose:

```text
1000 Users
```

request files.

```text
User 1 waits

User 2 waits

User 3 waits
```

Server becomes slow.

---

# Non Blocking I/O

Node.js delegates work.

Example:

```js
fs.readFile(
 "data.txt",
 (err,data)=>{
    console.log(data.toString());
 }
);

console.log("Done");
```

Output:

```text
Done

File Content
```

Execution:

```text
Request File
      ↓
OS Handles File
      ↓
Node Continues
      ↓
File Completes
      ↓
Callback Queue
      ↓
Event Loop Executes Callback
```

This is Non Blocking I/O.

---

# Event Loop Deep Dive

Heart of Node.js.

Without Event Loop:

```text
Node.js Cannot Handle Async Tasks
```

---

# Event Loop Responsibilities

* Execute callbacks
* Manage queues
* Process timers
* Handle async events

---

# Event Loop Workflow

```text
Call Stack
      ↓
Async API
      ↓
Callback Queue
      ↓
Event Loop
      ↓
Call Stack
```

---

# Example

```js
console.log("Start");

setTimeout(()=>{
   console.log("Timer");
},0);

console.log("End");
```

Output:

```text
Start
End
Timer
```

Why?

Because:

```text
setTimeout
      ↓
Web API
      ↓
Callback Queue
      ↓
Event Loop
      ↓
Call Stack
```

---

# Event Loop Phases

Node Event Loop has six phases.

```text
1. Timers

2. Pending Callbacks

3. Idle / Prepare

4. Poll

5. Check

6. Close Callbacks
```

---

# Phase 1: Timers

Executes:

```js
setTimeout()

setInterval()
```

Example:

```js
setTimeout(()=>{
   console.log("Hello");
},1000);
```

Runs here.

---

# Phase 2: Pending Callbacks

Executes callbacks postponed from previous cycle.

Examples:

```text
TCP Errors
System Errors
```

---

# Phase 3: Idle / Prepare

Internal Node.js operations.

Developers rarely interact with this phase.

---

# Phase 4: Poll Phase

Most important phase.

Handles:

```text
Incoming Requests
Database Results
File System Results
```

Node spends most of its time here.

---

# Phase 5: Check Phase

Executes:

```js
setImmediate()
```

Example:

```js
setImmediate(()=>{
 console.log("Immediate");
});
```

---

# Phase 6: Close Callbacks

Executes:

```js
socket.on("close")
```

callbacks.

---

# Microtask Queue

Before moving to next phase:

Node processes Microtasks.

Contains:

```js
Promise.then()

catch()

finally()

queueMicrotask()

process.nextTick()
```

---

# Priority Order

```text
Call Stack

↓

process.nextTick()

↓

Promise Queue

↓

Timers

↓

I/O Callbacks

↓

setImmediate()
```

---

# Example

```js
console.log("A");

setTimeout(()=>{
 console.log("B");
});

Promise.resolve()
.then(()=>{
 console.log("C");
});

console.log("D");
```

Output:

```text
A
D
C
B
```

Reason:

```text
Promise Queue
has higher priority
than Timer Queue
```

---

# process.nextTick()

Special Node.js queue.

Highest priority.

Example:

```js
console.log("1");

process.nextTick(()=>{
 console.log("2");
});

console.log("3");
```

Output:

```text
1
3
2
```

---

# Event Emitters Deep Dive

Node.js is Event Driven.

Everything is based on events.

Examples:

```text
HTTP Request

Database Connected

File Uploaded

User Logged In
```

---

# EventEmitter Class

Core module:

```js
const EventEmitter =
require("events");
```

Create instance:

```js
const emitter =
new EventEmitter();
```

---

# Event Flow

```text
Emit Event
      ↓
Event Loop
      ↓
Listener Executes
```

---

# Register Listener

```js
emitter.on(
 "login",
 ()=>{
    console.log("User Logged In");
 }
);
```

---

# Emit Event

```js
emitter.emit("login");
```

Output:

```text
User Logged In
```

---

# Multiple Listeners

```js
emitter.on("order",()=>{
 console.log("Email");
});

emitter.on("order",()=>{
 console.log("Inventory");
});

emitter.on("order",()=>{
 console.log("Invoice");
});
```

Emit:

```js
emitter.emit("order");
```

Output:

```text
Email
Inventory
Invoice
```

---

# Real World Example

E-Commerce Order

```text
Order Created
       ↓
orderCreated Event
       ↓
Send Email

Update Inventory

Generate Invoice

Create Notification

Analytics Tracking
```

One event triggers multiple independent actions.

---

# Node.js Request Lifecycle

```text
Client Request
        ↓
HTTP Server
        ↓
Event Loop
        ↓
Database Query
        ↓
Thread Pool / OS
        ↓
Callback Queue
        ↓
Event Loop
        ↓
Response Sent
```

---

# Most Important Interview Questions

### Why Is Node.js Fast?

* V8 Engine
* Event Loop
* Non Blocking I/O
* JIT Compilation

---

### Is Node.js Truly Single Threaded?

JavaScript execution is single-threaded.

Internally Node uses multiple threads through Libuv.

---

### What Is Event Loop?

Mechanism that continuously checks callback queues and executes pending tasks.

---

### Difference Between setImmediate() and setTimeout()?

setTimeout():

```text
Timers Phase
```

setImmediate():

```text
Check Phase
```

---

### Difference Between process.nextTick() and Promise.then()?

process.nextTick()

```text
Higher Priority
```

than Promise callbacks.

---

### Why Is Non Blocking I/O Better?

Allows Node.js to handle thousands of requests without waiting.

---

# Summary

Node.js Internal Concepts:

✔ V8 Engine

✔ JIT Compilation

✔ AST

✔ Ignition

✔ TurboFan

✔ Libuv

✔ Event Loop

✔ Thread Pool

✔ Blocking I/O

✔ Non Blocking I/O

✔ Callback Queue

✔ Microtask Queue

✔ process.nextTick()

✔ Event Emitters

✔ Event Driven Architecture

✔ Request Lifecycle

✔ Event Loop Phases

These concepts form the foundation for understanding Streams, Clusters, Worker Threads, Child Processes, Express.js Internals, and Node.js Performance Optimization.
