# Advanced Node.js - Complete Deep Dive

# Introduction

When learning Node.js, most developers understand:

```text
Express
MongoDB
APIs
Authentication
```

But companies hiring developers with:

```text
2+ Years Experience
3+ Years Experience
Senior Node.js Developers
```

expect knowledge of Node.js internals.

These topics answer questions like:

```text
Why is Node.js fast?
How does Node.js handle 10,000 users?
Why doesn't Node.js create a thread per request?
How does memory work?
What happens behind async/await?
```

This document explains those concepts in a simple way.

---

# 1. STREAMS

# The Problem Streams Solve

Imagine a file:

```text
movie.mp4
Size = 5GB
```

Without streams:

```text
Load Entire File
      ↓
Process File
```

Memory Usage:

```text
5GB RAM
```

Bad idea.

---

# Real World Example

Suppose:

```text
Netflix
YouTube
Google Drive
```

need to send large videos.

Can they load entire video into RAM?

```text
No
```

---

# Stream Solution

Instead of:

```text
Load Everything
```

Node.js does:

```text
Chunk 1
Chunk 2
Chunk 3
Chunk 4
```

One piece at a time.

Like drinking water through a straw.

---

# What Is A Stream?

A stream is a continuous flow of data.

```text
Data Source
      ↓
Chunk
      ↓
Chunk
      ↓
Chunk
      ↓
Destination
```

---

# Real Life Analogy

Bucket Method:

```text
Fill Entire Bucket
      ↓
Drink
```

Stream Method:

```text
Drink Continuously
```

Streams work like a water pipe.

---

# Types Of Streams

## Readable Stream

Reads data.

Examples:

```text
File Reading
HTTP Request
Database Results
```

```js
const stream =
fs.createReadStream(
 "movie.mp4"
);
```

---

## Writable Stream

Writes data.

Examples:

```text
Save File
Send Response
Write Logs
```

```js
const writeStream =
fs.createWriteStream(
 "output.txt"
);
```

---

## Duplex Stream

Can:

```text
Read
+
Write
```

Examples:

```text
TCP Socket
WebSocket
```

---

## Transform Stream

Can:

```text
Read
Modify
Write
```

Examples:

```text
Compression
Encryption
Image Processing
```

---

# Stream Flow

```text
File
 ↓
Readable Stream
 ↓
Chunk
 ↓
Chunk
 ↓
Chunk
 ↓
Writable Stream
 ↓
File
```

---

# Why Streams Matter

Without Streams:

```text
Large Memory Usage
```

With Streams:

```text
Constant Memory Usage
```

Even for huge files.

---

# 2. BUFFERS

# Why Buffers Exist

JavaScript understands:

```text
String
Array
Object
```

Computers communicate using:

```text
Binary Data
```

Example:

```text
Image
Video
Audio
PDF
Network Packet
```

are binary.

---

# What Is Buffer?

Buffer is temporary memory storage for binary data.

Think:

```text
Network
      ↓
Buffer
      ↓
JavaScript
```

Buffer translates binary into something Node.js can understand.

---

# Example

```js
const buffer =
Buffer.from("Hello");
```

Output:

```text
<Buffer 48 65 6c 6c 6f>
```

Those numbers are bytes.

---

# Why Buffer Is Needed

Suppose user uploads:

```text
profile.jpg
```

Node receives:

```text
Binary Data
```

Not:

```text
JavaScript Object
```

Buffer stores that binary data.

---

# Real World Usage

Used in:

```text
File Uploads
Images
Video Streaming
Socket Communication
PDF Generation
```

---

# Buffer vs Stream

Buffer:

```text
Store Entire Data
```

Stream:

```text
Process Piece By Piece
```

Example:

```text
100MB File
```

Buffer:

```text
100MB Memory
```

Stream:

```text
Few KB Memory
```

---

# 3. PROCESS OBJECT

# What Is Process?

Every running Node.js application is a process.

When you execute:

```bash
node app.js
```

Operating System creates:

```text
Node Process
```

---

# Process Object

Node exposes process information through:

```js
process
```

global object.

---

# Process Architecture

```text
Operating System
       ↓
Node Process
       ↓
process Object
```

---

# Get Process ID

```js
console.log(process.pid);
```

Output:

```text
4512
```

Unique process ID.

---

# Current Directory

```js
console.log(process.cwd());
```

Output:

```text
/project
```

---

# Environment Variables

```js
console.log(
 process.env.PORT
);
```

Used for:

```text
API Keys
Database URLs
Secrets
```

---

# Exit Process

```js
process.exit();
```

Immediately terminates application.

---

# Memory Usage

```js
console.log(
 process.memoryUsage()
);
```

Output:

```js
{
 rss: 50000000,
 heapTotal: 20000000,
 heapUsed: 15000000
}
```

Used for monitoring.

---

# Process Events

```js
process.on(
 "exit",
 ()=>{
   console.log("Bye");
 }
);
```

Runs before application exits.

---

# 4. WORKER THREADS

# The Problem

Node.js executes JavaScript on:

```text
One Main Thread
```

Suppose:

```js
while(true){}
```

Server freezes.

Why?

Because:

```text
CPU Task
Blocks Event Loop
```

---

# Examples Of CPU Intensive Tasks

```text
Image Compression
Video Processing
Machine Learning
Large Loops
Encryption
```

---

# Solution

Worker Threads.

---

# Architecture

```text
Main Thread
      ↓
Worker Thread
      ↓
Heavy Computation
```

Main thread remains free.

---

# Example

```js
const {
 Worker
}
=
require(
 "worker_threads"
);
```

Create Worker:

```js
new Worker(
 "./worker.js"
);
```

Worker runs separately.

---

# Communication

Main Thread:

```js
worker.postMessage(
 "Hello Worker"
);
```

Worker:

```js
parentPort.on(
 "message",
 (msg)=>{
   console.log(msg);
 }
);
```

---

# When To Use Workers

Good:

```text
Image Processing
AI Tasks
Video Encoding
PDF Generation
```

Bad:

```text
Simple Database Query
Simple API
```

---

# 5. CLUSTERING

# Problem

Suppose machine has:

```text
8 CPU Cores
```

Node uses:

```text
1 Core
```

Only:

```text
12.5%
```

CPU utilization.

---

# Why?

One Node process = One CPU core.

---

# Solution

Cluster Module.

---

# Architecture

Without Cluster:

```text
CPU Core 1 → Node
CPU Core 2 → Idle
CPU Core 3 → Idle
CPU Core 4 → Idle
```

---

With Cluster:

```text
Master
  |
------------------
| | | |
W W W W
```

Worker per CPU.

---

# Benefits

```text
More Requests
Better CPU Usage
High Availability
```

---

# Worker Crash?

Master detects:

```text
Worker Dead
```

Creates new worker automatically.

Production-ready systems use this.

---

# Worker Threads vs Cluster

Worker Threads:

```text
CPU Tasks
```

Cluster:

```text
More HTTP Requests
```

---

# 6. MEMORY MANAGEMENT

# What Is Memory?

Every application needs RAM.

Node.js stores:

```text
Variables
Objects
Functions
Arrays
```

inside memory.

---

# Memory Areas

```text
Stack Memory

Heap Memory
```

---

# Stack Memory

Stores:

```text
Function Calls
Primitive Values
```

Example:

```js
let name = "John";
```

Stored in stack.

---

# Heap Memory

Stores:

```text
Objects
Arrays
Functions
```

Example:

```js
const user = {
 name:"John"
};
```

Stored in heap.

---

# Stack vs Heap

Stack:

```text
Fast
Small
```

Heap:

```text
Large
Slower
```

---

# Memory Leak

Very important interview topic.

Memory leak means:

```text
Memory Allocated
But Never Released
```

---

# Example

```js
const users = [];

setInterval(()=>{

 users.push(
  new Array(100000)
 );

},1000);
```

Memory keeps growing.

Application may crash.

---

# Symptoms

```text
High RAM Usage
Slow Response
Application Crash
```

---

# 7. GARBAGE COLLECTION

# Problem

Suppose:

```js
let user = {
 name:"John"
};
```

Memory allocated.

Later:

```js
user = null;
```

Object is no longer needed.

Who removes it?

---

# Answer

Garbage Collector.

---

# What Is Garbage Collection?

Automatic memory cleanup mechanism.

```text
Unused Object
      ↓
Detected
      ↓
Deleted
```

---

# V8 Garbage Collector

Node uses:

```text
V8 Garbage Collector
```

---

# Mark And Sweep Algorithm

Most asked interview question.

---

# Step 1

Mark reachable objects.

```text
Global Object
       ↓
User Object
```

Reachable.

---

# Step 2

Objects not reachable:

```text
Unused Object
```

Marked as garbage.

---

# Step 3

Memory freed.

---

# Example

```js
let user = {
 name:"John"
};

user = null;
```

Now:

```text
Object Unreachable
```

Garbage collector removes it.

---

# 8. EVENT LOOP INTERNALS

Most important Node.js topic.

---

# Why Event Loop Exists

Node.js has:

```text
One Main Thread
```

Yet handles:

```text
Thousands Of Users
```

How?

Event Loop.

---

# Event Loop Job

Continuously checks:

```text
Any Callback Ready?
```

If yes:

```text
Execute Callback
```

---

# Complete Flow

```text
Call Stack
      ↓
Async Operation
      ↓
OS / Thread Pool
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
console.log("A");

setTimeout(()=>{
 console.log("B");
},0);

console.log("C");
```

Output:

```text
A
C
B
```

---

# Event Loop Phases

```text
1. Timers

2. Pending Callbacks

3. Idle

4. Poll

5. Check

6. Close
```

---

# Timers

Executes:

```js
setTimeout()

setInterval()
```

---

# Poll

Most important phase.

Handles:

```text
File System
Database Results
Network Responses
```

---

# Check

Executes:

```js
setImmediate()
```

---

# Microtask Queue

Higher priority.

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
Promises
      ↓
Timers
      ↓
I/O
      ↓
setImmediate()
```

---

# Example

```js
console.log("A");

Promise.resolve()
.then(()=>{

 console.log("B");

});

setTimeout(()=>{

 console.log("C");

});

console.log("D");
```

Output:

```text
A
D
B
C
```

Because:

```text
Promise Queue
has higher priority
than Timer Queue
```

---

# Advanced Interview Questions

### Why Is Node.js Fast?

* V8 Engine
* Event Loop
* Non Blocking I/O
* Streams

---

### Stream vs Buffer?

Buffer:

```text
Entire Data
```

Stream:

```text
Chunk By Chunk
```

---

### Worker Thread vs Cluster?

Worker Thread:

```text
CPU Intensive Task
```

Cluster:

```text
Handle More Requests
```

---

### What Causes Memory Leaks?

Unused references kept in memory.

---

### Which Garbage Collection Algorithm Does V8 Use?

Mark and Sweep.

---

### Why Doesn't Node.js Create A Thread Per Request?

Thread creation is expensive.

Node uses Event Loop and async I/O instead.

---

# Final Summary

Advanced Node.js Concepts:

✔ Streams

✔ Buffers

✔ Process Object

✔ Worker Threads

✔ Cluster

✔ Memory Management

✔ Memory Leaks

✔ Garbage Collection

✔ Event Loop Internals

✔ Microtasks

✔ Thread Pool

✔ V8 Internals

✔ Non Blocking I/O

These concepts are commonly asked in Node.js interviews for developers with 2–6 years of experience and are crucial for understanding how Node.js works internally.
