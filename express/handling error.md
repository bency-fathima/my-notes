# Express.js Error Handling - Complete Deep Dive

# Introduction

One of the biggest differences between a beginner backend developer and an experienced backend developer is:

```text
Error Handling
```

Most beginners write APIs like:

```js
app.get("/users", async(req,res)=>{

   const users = await User.find();

   res.json(users);

});
```

Looks fine.

But what happens if:

```text
Database Connection Fails?

MongoDB Server Stops?

User Sends Invalid Data?

File Upload Fails?

JWT Token Is Invalid?
```

Without proper error handling:

```text
Application Crashes
```

or

```text
Users Receive Unclear Errors
```

A professional backend application should:

```text
Never Crash Unexpectedly

Return Proper Error Messages

Log Errors

Handle All Edge Cases
```

---

# What is Error Handling?

Error handling means:

```text
Detect Error
      ↓
Process Error
      ↓
Send Meaningful Response
```

instead of crashing the application.

---

# Why Error Handling Is Important

Without Error Handling:

```text
Request
   ↓
Error Occurs
   ↓
Application Crashes
```

---

With Error Handling:

```text
Request
   ↓
Error Occurs
   ↓
Error Middleware
   ↓
Proper Response
```

---

# Types of Errors in Node.js

Generally:

```text
1. Operational Errors

2. Programming Errors
```

---

# Operational Errors

Expected errors.

Examples:

```text
Database Down

Invalid Password

Missing File

Unauthorized User

Validation Failure
```

These should be handled.

---

# Programming Errors

Developer mistakes.

Examples:

```js
user.name.toUpperCase();
```

when:

```js
user = null;
```

Error:

```text
Cannot read property 'name'
```

These indicate bugs.

---

# Error Handling Flow

```text
Client Request
      ↓
Controller
      ↓
Error Occurs
      ↓
next(error)
      ↓
Global Error Handler
      ↓
JSON Response
```

---

# Basic Error Example

```js
app.get("/test",(req,res)=>{

   throw new Error(
      "Something Went Wrong"
   );

});
```

Error:

```text
Application Crashes
```

or Express returns default HTML error.

Not professional.

---

# Proper Error Handling

```js
app.get("/test",(req,res,next)=>{

   try{

      throw new Error(
         "Something Went Wrong"
      );

   }
   catch(error){

      next(error);

   }

});
```

---

# What is next(error)?

Normally:

```js
next();
```

means:

```text
Go To Next Middleware
```

---

But:

```js
next(error);
```

means:

```text
Go To Error Middleware
```

Immediately.

---

# GLOBAL ERROR HANDLER

# What is Global Error Handler?

A single middleware responsible for handling all errors.

Instead of:

```text
Handling Errors
In Every Controller
```

we centralize them.

---

# Why Use Global Error Handler?

Without Global Error Handler:

```js
res.status(500).json(...)
```

repeated everywhere.

Code duplication.

---

With Global Error Handler:

```text
One Place
Handles Everything
```

---

# Structure

Normal Middleware:

```js
(req,res,next)
```

---

Error Middleware:

```js
(err,req,res,next)
```

Notice:

```text
4 Parameters
```

This is how Express identifies it.

---

# Basic Global Error Handler

```js
app.use(

 (err,req,res,next)=>{

   res.status(500).json({

      success:false,

      message:err.message

   });

 }

);
```

---

# Flow

```text
Route
 ↓
Error
 ↓
next(error)
 ↓
Global Handler
 ↓
Response
```

---

# Example

Controller:

```js
app.get("/users",

(req,res,next)=>{

   try{

      throw new Error(
       "Database Error"
      );

   }
   catch(error){

      next(error);

   }

});
```

Response:

```json
{
  "success":false,
  "message":"Database Error"
}
```

---

# Why Global Handler Is Powerful

Suppose project contains:

```text
50 Controllers
```

Without global handler:

```text
50 Error Responses
```

Need maintenance.

---

With global handler:

```text
1 Error Response Structure
```

Used everywhere.

---

# Professional Global Error Handler

```js
app.use(

 (err,req,res,next)=>{

   const statusCode =
   err.statusCode || 500;

   const message =
   err.message ||
   "Internal Server Error";

   res.status(statusCode)
   .json({

      success:false,

      message

   });

 }

);
```

---

# Benefits

```text
Consistent Response

Easy Debugging

Clean Controllers
```

---

# ASYNC ERROR HANDLING

# Why Async Errors Are Different

Most Express APIs use:

```js
async/await
```

Example:

```js
app.get("/users",

async(req,res)=>{

 const users =
 await User.find();

 res.json(users);

});
```

Looks good.

---

# Problem

Suppose:

```text
MongoDB Connection Lost
```

Then:

```js
await User.find()
```

throws error.

Express may not automatically catch it.

---

# Solution 1 - Try Catch

```js
app.get("/users",

async(req,res,next)=>{

 try{

   const users =
   await User.find();

   res.json(users);

 }
 catch(error){

   next(error);

 }

});
```

Works.

---

# Problem

Imagine:

```text
100 Routes
```

Every route:

```js
try{
}
catch{
}
```

Code duplication.

---

# Solution 2 - Async Wrapper

Professional projects use:

```text
Async Handler
```

---

# Create Async Handler

```js
const asyncHandler =
(fn)=>{

 return (req,res,next)=>{

   Promise.resolve(

      fn(req,res,next)

   ).catch(next);

 };

};
```

---

# How It Works

```text
Route
   ↓
Promise
   ↓
Error?
   ↓
next(error)
```

Automatically.

---

# Usage

Instead of:

```js
async(req,res,next)=>{

 try{

 }
 catch(error){

 }

}
```

Use:

```js
app.get(

 "/users",

 asyncHandler(

  async(req,res)=>{

     const users =
     await User.find();

     res.json(users);

  }

 )

);
```

Cleaner.

---

# Execution Flow

```text
Request
   ↓
Async Route
   ↓
Error
   ↓
Async Handler
   ↓
next(error)
   ↓
Global Handler
```

---

# Real Project Structure

```text
middleware

 ├── errorHandler.js

 ├── asyncHandler.js
```

---

# asyncHandler.js

```js
module.exports =

(fn)=>{

 return (req,res,next)=>{

   Promise.resolve(

      fn(req,res,next)

   ).catch(next);

 };

};
```

---

# CUSTOM ERROR CLASSES

# Problem

Suppose:

```text
User Not Found

Invalid Password

Unauthorized User

Product Not Found
```

All errors return:

```json
{
 "message":"Error"
}
```

Not informative.

---

# Solution

Custom Error Classes.

---

# What is Custom Error Class?

Custom class extending JavaScript Error.

Allows:

```text
Custom Status Codes

Custom Messages

Better Control
```

---

# Basic Example

```js
class AppError
extends Error{

 constructor(
   message,
   statusCode
 ){

   super(message);

   this.statusCode =
   statusCode;

 }

}
```

---

# Creating Error

```js
throw new AppError(

 "User Not Found",

 404

);
```

---

# Error Object

```js
{
 message:
 "User Not Found",

 statusCode:
 404
}
```

---

# Use With Global Handler

```js
app.use(

(err,req,res,next)=>{

 res.status(

   err.statusCode || 500

 ).json({

   success:false,

   message:err.message

 });

});
```

---

# Example

Controller:

```js
app.get("/users/:id",

(req,res)=>{

 throw new AppError(

   "User Not Found",

   404

 );

});
```

Response:

```json
{
 "success":false,
 "message":"User Not Found"
}
```

Status:

```text
404
```

---

# Real World Error Classes

## Validation Error

```js
class ValidationError
extends Error{

 constructor(message){

   super(message);

   this.statusCode=400;

 }

}
```

---

## Unauthorized Error

```js
class UnauthorizedError
extends Error{

 constructor(message){

   super(message);

   this.statusCode=401;

 }

}
```

---

## Forbidden Error

```js
class ForbiddenError
extends Error{

 constructor(message){

   super(message);

   this.statusCode=403;

 }

}
```

---

## Not Found Error

```js
class NotFoundError
extends Error{

 constructor(message){

   super(message);

   this.statusCode=404;

 }

}
```

---

# Practical Example

```js
const user =
await User.findById(id);

if(!user){

 throw new NotFoundError(

   "User Not Found"

 );

}
```

---

# Professional Error Response Format

```json
{
  "success":false,
  "statusCode":404,
  "message":"User Not Found"
}
```

---

# Production vs Development Errors

Professional applications show different errors.

---

# Development

```json
{
  "success":false,
  "message":"User Not Found",
  "stack":"..."
}
```

Developer can debug.

---

# Production

```json
{
  "success":false,
  "message":"User Not Found"
}
```

Hide stack trace.

Never expose internal details.

---

# Handling Unhandled Rejections

Example:

```js
Promise.reject(
 new Error("DB Error")
);
```

Handle globally:

```js
process.on(

 "unhandledRejection",

 (err)=>{

   console.log(err);

   process.exit(1);

 }

);
```

---

# Handling Uncaught Exceptions

```js
process.on(

 "uncaughtException",

 (err)=>{

   console.log(err);

   process.exit(1);

 }

);
```

---

# Complete Error Flow

```text
Client Request
      ↓
Route
      ↓
Controller
      ↓
Error Occurs
      ↓
Async Handler
      ↓
next(error)
      ↓
Global Error Handler
      ↓
JSON Response
```

---

# Interview Questions

### What is Global Error Handler?

Central middleware handling all application errors.

---

### How Does Express Identify Error Middleware?

By:

```js
(err,req,res,next)
```

4 parameters.

---

### Why Use next(error)?

To pass error to error middleware.

---

### Why Async Errors Need Special Handling?

Because async functions return promises.

Errors must be caught and forwarded.

---

### What is Async Handler?

Wrapper that automatically catches async errors.

---

### What is Custom Error Class?

A class extending Error used to create structured application errors.

---

### Why Use Custom Errors?

```text
Custom Messages

Custom Status Codes

Cleaner Code
```

---

### Difference Between Operational And Programming Errors?

Operational:

```text
Expected Errors
```

Examples:

```text
Validation Failure
User Not Found
```

Programming:

```text
Code Bugs
```

Examples:

```text
Undefined Variable
Null Reference
```

---

# Summary

Express Error Handling Concepts:

✔ Error Handling Fundamentals

✔ Operational Errors

✔ Programming Errors

✔ next(error)

✔ Global Error Handler

✔ Error Middleware

✔ Async Error Handling

✔ Async Wrapper

✔ Custom Error Classes

✔ AppError

✔ ValidationError

✔ NotFoundError

✔ Production Error Handling

✔ Unhandled Rejections

✔ Uncaught Exceptions

These concepts are mandatory in production-grade Node.js applications and are among the most frequently asked Express.js interview topics for developers with 2–6 years of experience.
