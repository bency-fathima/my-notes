# Node.js Core Modules - Beginner to Advanced Deep Dive

# Introduction

Node.js comes with many built-in modules called **Core Modules**.

These modules are already available inside Node.js.

You don't need to install them using npm.

Example:

```js
const fs = require("fs");
const path = require("path");
```

These modules help us perform:

* File Operations
* Path Handling
* Operating System Information
* Security & Encryption
* Data Streaming
* Binary Data Handling
* Running Other Programs
* Multithreading
* Multi-Core Utilization

---

# 1. FS MODULE (File System)

# What is fs?

The fs module allows Node.js to interact with files and folders.

Think of fs as a bridge between:

```text
Node.js
   ↓
Files & Folders
```

Without fs:

```text
Node.js cannot read files.
Node.js cannot create files.
Node.js cannot upload files.
```

---

# Real Life Example

Suppose:

```text
User uploads resume.pdf
```

Backend must:

```text
Receive File
      ↓
Save File
      ↓
Store Path In Database
```

The saving part is done using fs.

---

# Common fs Operations

```text
Read File
Write File
Append Data
Delete File
Rename File
Create Folder
Delete Folder
```

---

# Reading Files

Imagine a file:

```text
welcome.txt

Hello User
Welcome To Node.js
```

### Synchronous Reading

```js
const fs = require("fs");

const data =
fs.readFileSync("welcome.txt","utf8");

console.log(data);

console.log("Done");
```

Output:

```text
Hello User
Welcome To Node.js

Done
```

Execution:

```text
Read File
    ↓
Wait
    ↓
Print Data
    ↓
Done
```

Program waits.

This is called:

```text
Blocking Operation
```

---

# Why Blocking Is Bad?

Suppose:

```text
1000 Users
```

request files simultaneously.

Every request waits.

Server becomes slow.

---

# Asynchronous Reading

```js
fs.readFile(
 "welcome.txt",
 "utf8",
 (err,data)=>{

   console.log(data);

 }
);

console.log("Done");
```

Output:

```text
Done

Hello User
Welcome To Node.js
```

Notice:

```text
Done
```

prints first.

Node doesn't wait.

This is:

```text
Non Blocking I/O
```

---

# Writing File

```js
fs.writeFile(
 "note.txt",
 "Learning Node.js",
 (err)=>{
   console.log("Saved");
 }
);
```

Creates:

```text
note.txt
```

with content:

```text
Learning Node.js
```

---

# Appending Data

Suppose file contains:

```text
Node.js
```

Append:

```js
fs.appendFile(
 "note.txt",
 "\nExpress.js",
 ()=>{}
);
```

Now:

```text
Node.js
Express.js
```

---

# Delete File

```js
fs.unlink(
 "note.txt",
 ()=>{}
);
```

Deletes file.

---

# Interview Question

### Difference Between readFileSync() and readFile()?

readFileSync()

```text
Blocking
```

readFile()

```text
Non Blocking
```

---

# 2. PATH MODULE

# Why Do We Need Path Module?

Windows Path:

```text
C:\Users\Admin\file.txt
```

Linux Path:

```text
/home/admin/file.txt
```

Different operating systems use different formats.

Path module handles this automatically.

---

# path.join()

Most used function.

```js
const path = require("path");

const filePath =
path.join(
 __dirname,
 "uploads",
 "profile.jpg"
);

console.log(filePath);
```

Output:

```text
project/uploads/profile.jpg
```

or

```text
project\uploads\profile.jpg
```

depending on OS.

---

# Why Use join()?

Bad:

```js
const path =
__dirname + "/uploads/profile.jpg";
```

Can break on different OS.

Good:

```js
path.join(...)
```

---

# basename()

Returns filename.

```js
path.basename(
 "/home/user/photo.png"
);
```

Output:

```text
photo.png
```

---

# extname()

Returns extension.

```js
path.extname(
 "photo.png"
);
```

Output:

```text
.png
```

Useful for:

```text
File Upload Validation
```

---

# dirname()

Returns folder path.

```js
path.dirname(
 "/home/user/photo.png"
);
```

Output:

```text
/home/user
```

---

# Real Project Usage

When user uploads image:

```text
Get Extension
       ↓
Validate
       ↓
Generate Path
       ↓
Store Image
```

Path module handles this.

---

# 3. OS MODULE

# What is OS Module?

Provides information about the system where Node.js is running.

Think:

```text
Node.js
     ↓
Operating System Details
```

---

# Platform

```js
const os = require("os");

console.log(os.platform());
```

Output:

```text
win32
linux
darwin
```

---

# CPU Information

```js
console.log(
 os.cpus()
);
```

Returns:

```js
[
 {
  model:"Intel Core i7",
  speed:2600
 }
]
```

---

# Total Memory

```js
console.log(
 os.totalmem()
);
```

Output:

```text
17179869184
```

bytes.

---

# Free Memory

```js
console.log(
 os.freemem()
);
```

Useful for:

```text
Server Monitoring
```

---

# Real World Usage

Admin Dashboard:

```text
CPU Usage
RAM Usage
Server Status
```

All use OS module.

---

# 4. CRYPTO MODULE

# What is Crypto?

Crypto provides security features.

Used for:

```text
Password Hashing
Encryption
Token Generation
Digital Signatures
```

---

# Why Hash Passwords?

Bad:

```json
{
 "password":"123456"
}
```

If database leaks:

```text
Everyone's password exposed
```

---

# Hashing

```js
const crypto =
require("crypto");

const hash =
crypto
.createHash("sha256")
.update("123456")
.digest("hex");

console.log(hash);
```

Output:

```text
8d969eef...
```

Password becomes unreadable.

---

# Random Token

```js
crypto
.randomBytes(32)
.toString("hex");
```

Output:

```text
ab34cd56ef....
```

Used for:

```text
Reset Password Links
Email Verification
API Keys
```

---

# Difference Between Encryption and Hashing

Hashing:

```text
One Way
```

Cannot reverse.

---

Encryption:

```text
Two Way
```

Can decrypt.

---

# 5. BUFFER MODULE

# What is Buffer?

JavaScript understands:

```text
String
Number
Object
Array
```

But computers communicate using:

```text
Binary Data
```

Buffer acts as:

```text
Translator
```

between JavaScript and Binary Data.

---

# Example

```js
const buffer =
Buffer.from("Hello");

console.log(buffer);
```

Output:

```text
<Buffer 48 65 6c 6c 6f>
```

---

# Convert Back

```js
console.log(
 buffer.toString()
);
```

Output:

```text
Hello
```

---

# Real World Usage

Whenever user uploads:

```text
Image
Video
PDF
Audio
```

Buffers are involved.

---

# 6. STREAM MODULE

# Problem Without Streams

Suppose:

```text
Movie.mp4
Size = 5GB
```

If Node loads entire file:

```text
5GB RAM Required
```

Huge memory consumption.

---

# Stream Solution

Instead of:

```text
Load Everything
```

Node does:

```text
Small Chunk
      ↓
Process
      ↓
Next Chunk
      ↓
Process
```

---

# Example

```js
const fs = require("fs");

const stream =
fs.createReadStream(
 "movie.mp4"
);
```

Node reads:

```text
Chunk 1
Chunk 2
Chunk 3
...
```

---

# Stream Types

### Readable

Read Data

```js
fs.createReadStream()
```

---

### Writable

Write Data

```js
fs.createWriteStream()
```

---

### Duplex

Read + Write

Example:

```text
Socket Connection
```

---

### Transform

Read + Modify + Write

Example:

```text
Compression
Encryption
```

---

# Pipe

```js
readStream.pipe(writeStream);
```

Flow:

```text
Read
 ↓
Pipe
 ↓
Write
```

No manual handling.

---

# 7. CHILD PROCESS

# Why Child Process?

Node cannot do everything itself.

Sometimes we need:

```text
Run Python Script
Run Git Command
Run Shell Command
```

Child Process helps.

---

# Example

```js
const { exec } =
require("child_process");

exec(
 "dir",
 (err,stdout)=>{
   console.log(stdout);
 }
);
```

Node asks OS:

```text
Run Command
```

OS executes.

Result returned to Node.

---

# Real Example

```js
exec("git status");
```

Backend can run Git commands.

---

# 8. WORKER THREADS

# Problem

Node.js uses:

```text
One Main Thread
```

Suppose:

```js
while(true){}
```

Server freezes.

---

# Why?

CPU-intensive tasks block Event Loop.

Examples:

```text
Image Processing
Video Processing
AI Computation
Large Calculations
```

---

# Solution

Worker Threads.

```text
Main Thread
      ↓
Worker Thread
```

Heavy work moved away.

---

# Example

```js
const {
 Worker
} = require(
 "worker_threads"
);

new Worker(
 "./worker.js"
);
```

Worker executes independently.

Main thread stays responsive.

---

# 9. CLUSTER MODULE

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

of CPU power.

---

# Cluster Solution

Create multiple Node processes.

```text
Master
   |
------------------
| | | | | | | |
W W W W W W W W
```

Each worker:

```text
Separate Node Process
```

---

# Example

```js
const cluster =
require("cluster");

if(cluster.isPrimary){

 cluster.fork();
 cluster.fork();

}
```

Creates workers.

---

# Worker Threads vs Cluster

Worker Threads:

```text
CPU Intensive Tasks
```

Examples:

```text
Image Compression
AI Processing
```

---

Cluster:

```text
Handle More Requests
```

Examples:

```text
API Servers
E-Commerce Backend
```

---

# Quick Interview Revision

### fs

File operations.

### path

Path management.

### os

System information.

### crypto

Security and hashing.

### buffer

Binary data handling.

### stream

Chunk-based data processing.

### child_process

Execute external programs.

### worker_threads

Run CPU-intensive tasks separately.

### cluster

Utilize multiple CPU cores.

---

# Memory Trick

```text
fs      → Files

path    → Paths

os      → Operating System

crypto  → Security

buffer  → Binary Data

stream  → Large Data

child_process → External Programs

worker_threads → Heavy Computation

cluster → Multiple CPU Cores
```
