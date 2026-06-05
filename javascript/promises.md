# JavaScript Promises - Complete Notes

# What is a Promise?

A Promise is a JavaScript object used to handle asynchronous operations.

A Promise represents a value that may be available:

* Now
* Later
* Never

Think of a Promise like ordering food online.

```text
Order Food
     ↓
Waiting
     ↓
Delivered ✅
or
Cancelled ❌
```

Similarly:

```text
Promise Created
      ↓
Pending
      ↓
Resolved ✅
or
Rejected ❌
```

---

# Why Do We Need Promises?

JavaScript is Single Threaded.

It executes one line at a time.

Example:

```javascript
console.log("Start");

console.log("End");
```

Output:

```text
Start
End
```

---

Now imagine:

```javascript
console.log("Start");

setTimeout(()=>{
   console.log("Done");
},2000);

console.log("End");
```

Output:

```text
Start
End
Done
```

Why?

Because:

```javascript
setTimeout()
```

is asynchronous.

JavaScript doesn't wait.

It continues executing the next line.

---

# Synchronous vs Asynchronous

## Synchronous

```javascript
console.log("A");
console.log("B");
console.log("C");
```

Output:

```text
A
B
C
```

Executed one after another.

---

## Asynchronous

```javascript
console.log("A");

setTimeout(()=>{
   console.log("B");
},1000);

console.log("C");
```

Output:

```text
A
C
B
```

Because timeout runs later.

---

# The Problem Before Promises

Imagine:

1. Download File
2. Process File
3. Send Email

Using Callbacks:

```javascript
downloadFile(function(){

   processFile(function(){

      sendEmail(function(){

         console.log("Done");

      });

   });

});
```

This creates:

```text
Callback Hell
```

Also called:

```text
Pyramid of Doom
```

Difficult to read and maintain.

---

# Promise Solution

Promises solve callback hell.

```javascript
downloadFile()
   .then(processFile)
   .then(sendEmail)
   .then(()=>{
      console.log("Done");
   });
```

Cleaner and easier to understand.

---

# Creating a Promise

Syntax:

```javascript
const promise =
new Promise(
   (resolve,reject)=>{

   }
);
```

---

# Understanding resolve and reject

Promise has two possible outcomes:

### Success

```javascript
resolve()
```

### Failure

```javascript
reject()
```

---

Example:

```javascript
const promise =
new Promise((resolve,reject)=>{

   let success = true;

   if(success){

      resolve("Success");

   }else{

      reject("Failed");

   }

});
```

---

# Promise States

A Promise has three states.

```text
1. Pending
2. Fulfilled (Resolved)
3. Rejected
```

---

## Pending

Promise is waiting.

```text
Promise Created
```

---

## Fulfilled

Operation completed successfully.

```javascript
resolve()
```

---

## Rejected

Operation failed.

```javascript
reject()
```

---

# Visual Representation

```text
            Promise
                │
        ┌───────┴───────┐
        │               │
    resolve()       reject()
        │               │
   Fulfilled       Rejected
```

---

# Consuming a Promise

Creating promise alone is not enough.

We must consume it.

---

## Using then()

```javascript
promise.then(result=>{

   console.log(result);

});
```

Output:

```text
Success
```

---

## Using catch()

```javascript
promise.catch(error=>{

   console.log(error);

});
```

Output:

```text
Failed
```

---

# Complete Example

```javascript
const promise =
new Promise((resolve,reject)=>{

   let success = true;

   if(success){

      resolve("Success");

   }else{

      reject("Failed");

   }

});

promise
.then(result=>{

   console.log(result);

})
.catch(error=>{

   console.log(error);

});
```

Output:

```text
Success
```

---

# What Happens Internally?

### Step 1

Promise created.

```text
Pending
```

---

### Step 2

Condition checked.

```javascript
success = true
```

---

### Step 3

Execute:

```javascript
resolve("Success");
```

State becomes:

```text
Fulfilled
```

---

### Step 4

then() executes.

```javascript
.then(result=>{
   console.log(result);
})
```

Output:

```text
Success
```

---

# Real Example with setTimeout

```javascript
const promise =
new Promise((resolve,reject)=>{

   setTimeout(()=>{

      resolve("Data Loaded");

   },2000);

});
```

Consume:

```javascript
promise.then(result=>{

   console.log(result);

});
```

Output after 2 seconds:

```text
Data Loaded
```

---

# Returning Data from Promise

```javascript
const getUser =
new Promise((resolve)=>{

   resolve({
      id:1,
      name:"John"
   });

});
```

Consume:

```javascript
getUser.then(user=>{

   console.log(user.name);

});
```

Output:

```text
John
```

---

# Promise Chaining

One of the biggest advantages.

```javascript
Promise.resolve(10)

.then(num=>{

   return num * 2;

})

.then(num=>{

   return num + 5;

})

.then(result=>{

   console.log(result);

});
```

Output:

```text
25
```

---

# How Chaining Works

```text
10
↓
10 * 2
↓
20
↓
20 + 5
↓
25
```

Each `.then()` receives data from the previous one.

---

# Promise.catch()

Handles errors.

Example:

```javascript
const promise =
new Promise((resolve,reject)=>{

   reject("Server Error");

});
```

Consume:

```javascript
promise
.then(result=>{

   console.log(result);

})
.catch(error=>{

   console.log(error);

});
```

Output:

```text
Server Error
```

---

# Promise.finally()

Runs regardless of success or failure.

```javascript
promise
.then(result=>{

})
.catch(error=>{

})
.finally(()=>{

   console.log("Completed");

});
```

Output:

```text
Completed
```

---

# Real API Example

Fetching users.

```javascript
fetch("/users")

.then(response=>{

   return response.json();

})

.then(data=>{

   console.log(data);

})

.catch(error=>{

   console.log(error);

});
```

This is one of the most common Promise patterns.

---

# Promise Methods

---

## Promise.resolve()

Creates successful promise.

```javascript
Promise.resolve("Success")
.then(data=>{

   console.log(data);

});
```

Output:

```text
Success
```

---

## Promise.reject()

Creates failed promise.

```javascript
Promise.reject("Failed")
.catch(error=>{

   console.log(error);

});
```

Output:

```text
Failed
```

---

# Promise.all()

Run multiple promises together.

```javascript
const p1 =
Promise.resolve("A");

const p2 =
Promise.resolve("B");

const p3 =
Promise.resolve("C");
```

```javascript
Promise.all([
   p1,p2,p3
])
.then(result=>{

   console.log(result);

});
```

Output:

```text
["A","B","C"]
```

---

# Promise.race()

Returns first completed promise.

```javascript
Promise.race([
   p1,
   p2,
   p3
])
.then(result=>{

   console.log(result);

});
```

Returns whichever finishes first.

---

# Common Interview Questions

## What is a Promise?

A Promise is an object representing the eventual completion or failure of an asynchronous operation.

---

## What are Promise States?

```text
Pending
Fulfilled
Rejected
```

---

## Difference Between resolve and reject?

### resolve()

Success.

### reject()

Failure.

---

## What is Promise Chaining?

Using multiple `.then()` calls where output from one becomes input to the next.

---

## Difference Between then and catch?

### then()

Handles success.

### catch()

Handles failure.

---

## What is finally()?

Runs regardless of success or failure.

---

# Promise vs Callback

| Callback                 | Promise                    |
| ------------------------ | -------------------------- |
| Callback Hell            | Cleaner                    |
| Hard to Read             | Easy to Read               |
| Nested Code              | Chaining                   |
| Error Handling Difficult | Centralized Error Handling |

---

# Real MERN Stack Usage

Promises are used in:

### API Calls

```javascript
fetch()
```

### MongoDB Queries

```javascript
User.find()
```

### Axios Requests

```javascript
axios.get()
```

### Authentication

```javascript
loginUser()
```

### File Uploads

```javascript
uploadFile()
```

---

# Most Important Promise Topics

✅ Asynchronous JavaScript

✅ Promise States

✅ resolve()

✅ reject()

✅ then()

✅ catch()

✅ finally()

✅ Promise Chaining

✅ Promise.all()

✅ Promise.race()

✅ API Calls

✅ Error Handling

✅ Callback Hell

---

# Promise Formula

```text
Promise
=
Asynchronous Task
+
Success Handling
+
Error Handling
```

or

```text
Promise
=
Pending
   ↓
Resolved OR Rejected
```

Promises are one of the most important JavaScript concepts and form the foundation for Async/Await, Fetch API, React API Calls, Node.js Backend Development, and MERN Stack applications.
