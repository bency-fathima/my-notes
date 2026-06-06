# JavaScript Error Handling - Complete Notes

# What is Error Handling?

Error Handling is the process of detecting, managing, and responding to errors that occur during program execution.

Without error handling:

```javascript
let result = x + y;

console.log(result);
```

Output:

```text
ReferenceError: x is not defined
```

Program crashes and stops execution.

With error handling:

```javascript
try{

   let result = x + y;

}catch(error){

   console.log(error);

}
```

Output:

```text
ReferenceError: x is not defined
```

Program continues running safely.

---

# Why Do We Need Error Handling?

Imagine a MERN application:

```text
User Login
     ↓
API Request
     ↓
Server Error
     ↓
Application Crash
```

Without error handling:

```text
Bad User Experience
```

With error handling:

```text
Show Friendly Message
Continue Application
```

---

# What is an Error?

An error is an unexpected condition that prevents normal execution.

Example:

```javascript
console.log(name);
```

Output:

```text
ReferenceError
```

Because:

```javascript
name
```

doesn't exist.

---

# Common JavaScript Errors

## ReferenceError

Variable not found.

```javascript
console.log(user);
```

Output:

```text
ReferenceError:
user is not defined
```

---

## TypeError

Using value incorrectly.

```javascript
const name = "John";

name();
```

Output:

```text
TypeError:
name is not a function
```

---

## SyntaxError

Invalid JavaScript syntax.

```javascript
if(true{

}
```

Output:

```text
SyntaxError
```

---

## RangeError

Value outside allowed range.

```javascript
const arr = [];

arr.length = -1;
```

Output:

```text
RangeError
```

---

## URIError

Invalid URI operation.

```javascript
decodeURIComponent("%");
```

Output:

```text
URIError
```

---

# try...catch

The most important error handling mechanism.

Syntax:

```javascript
try{

   // risky code

}catch(error){

   // handle error

}
```

---

# Example

```javascript
try{

   console.log(user);

}catch(error){

   console.log(error);

}
```

Output:

```text
ReferenceError:
user is not defined
```

Program does not crash.

---

# How try...catch Works

Step 1:

```javascript
try{
   risky code
}
```

JavaScript executes code.

---

Step 2:

If no error:

```javascript
catch()
```

is skipped.

---

Step 3:

If error occurs:

```javascript
catch()
```

runs.

---

# Visual Representation

```text
try
 │
 ├── No Error
 │      ↓
 │   Continue
 │
 └── Error
        ↓
      catch
```

---

# Example Without Error

```javascript
try{

   console.log("Hello");

}catch(error){

   console.log(error);

}
```

Output:

```text
Hello
```

Catch never runs.

---

# Error Object

Catch receives an error object.

```javascript
try{

   console.log(user);

}catch(error){

   console.log(error);

}
```

Output:

```text
ReferenceError
```

---

# Error Properties

```javascript
error.name
```

Example:

```javascript
console.log(error.name);
```

Output:

```text
ReferenceError
```

---

```javascript
error.message
```

Example:

```javascript
console.log(error.message);
```

Output:

```text
user is not defined
```

---

```javascript
error.stack
```

Shows stack trace.

```javascript
console.log(error.stack);
```

Useful for debugging.

---

# Example

```javascript
try{

   console.log(user);

}catch(error){

   console.log(error.name);

   console.log(error.message);

}
```

Output:

```text
ReferenceError
user is not defined
```

---

# finally Block

Runs regardless of success or failure.

Syntax:

```javascript
try{

}
catch(error){

}
finally{

}
```

---

# Example

```javascript
try{

   console.log("Hello");

}catch(error){

   console.log(error);

}finally{

   console.log(
      "Always Runs"
   );

}
```

Output:

```text
Hello
Always Runs
```

---

# Example with Error

```javascript
try{

   console.log(user);

}catch(error){

   console.log(error);

}finally{

   console.log(
      "Always Runs"
   );

}
```

Output:

```text
ReferenceError
Always Runs
```

---

# Visual Representation

```text
try
 │
 ├── Success
 │      ↓
 │   finally
 │
 └── Error
        ↓
      catch
        ↓
      finally
```

---

# throw Statement

JavaScript allows us to create our own errors.

Syntax:

```javascript
throw new Error(
   "Message"
);
```

---

# Example

```javascript
throw new Error(
   "Something Went Wrong"
);
```

Output:

```text
Error:
Something Went Wrong
```

---

# Why Use throw?

Sometimes JavaScript doesn't detect business logic errors.

Example:

```javascript
const age = 15;
```

No JavaScript error.

But business rule says:

```text
Minimum Age = 18
```

We create our own error.

---

# Custom Error Example

```javascript
const age = 15;

if(age < 18){

   throw new Error(
      "Not Eligible"
   );

}
```

Output:

```text
Error:
Not Eligible
```

---

# Handling Custom Error

```javascript
try{

   const age = 15;

   if(age < 18){

      throw new Error(
         "Not Eligible"
      );

   }

}catch(error){

   console.log(
      error.message
   );

}
```

Output:

```text
Not Eligible
```

---

# Real Login Example

```javascript
try{

   const password =
   "";

   if(!password){

      throw new Error(
         "Password Required"
      );

   }

}catch(error){

   console.log(
      error.message
   );

}
```

Output:

```text
Password Required
```

---

# Real Registration Example

```javascript
try{

   const email =
   "";

   if(!email){

      throw new Error(
         "Email Required"
      );

   }

}catch(error){

   console.log(
      error.message
   );

}
```

Output:

```text
Email Required
```

---

# Nested try...catch

```javascript
try{

   try{

      throw new Error(
         "Inner Error"
      );

   }
   catch(error){

      console.log(
         "Inner Catch"
      );

   }

}catch(error){

   console.log(
      "Outer Catch"
   );

}
```

Output:

```text
Inner Catch
```

---

# Error Handling with Async/Await

Very Important.

Without Error Handling:

```javascript
async function getData(){

   const response =
   await fetch(url);

}
```

If API fails:

```text
Unhandled Promise Rejection
```

---

# Using try...catch

```javascript
async function getData(){

   try{

      const response =
      await fetch(url);

      const data =
      await response.json();

      console.log(data);

   }
   catch(error){

      console.log(error);

   }

}
```

---

# Real Fetch Example

```javascript
async function getUsers(){

   try{

      const response =
      await fetch("/users");

      if(!response.ok){

         throw new Error(
            "Failed To Fetch Users"
         );

      }

      const users =
      await response.json();

      console.log(users);

   }

   catch(error){

      console.log(
         error.message
      );

   }

}
```

---

# Creating Custom Error Types

```javascript
class ValidationError
extends Error{

   constructor(message){

      super(message);

      this.name =
      "ValidationError";

   }

}
```

Use:

```javascript
throw new ValidationError(
   "Invalid Email"
);
```

Output:

```text
ValidationError:
Invalid Email
```

---

# Common Interview Questions

## What is Error Handling?

The process of managing runtime errors without crashing the application.

---

## What is try?

Contains risky code.

---

## What is catch?

Handles errors.

---

## What is finally?

Runs regardless of success or failure.

---

## What is throw?

Used to create custom errors.

---

## What does new Error() do?

Creates an Error object.

---

## Difference Between Syntax Error and Runtime Error?

### Syntax Error

Code cannot execute.

```javascript
if(true{
}
```

---

### Runtime Error

Occurs while code runs.

```javascript
console.log(user);
```

---

## Can finally run without catch?

Yes.

```javascript
try{

}
finally{

}
```

Valid JavaScript.

---

# Error Handling in MERN Stack

Frontend:

```javascript
try{

   const response =
   await fetch("/users");

}catch(error){

   toast.error(
      error.message
   );

}
```

Backend:

```javascript
try{

   const users =
   await User.find();

   res.json(users);

}catch(error){

   res.status(500).json({

      message:error.message

   });

}
```

---

# Best Practices

### Always Handle Async Errors

```javascript
try{
}
catch(error){
}
```

---

### Show User-Friendly Messages

Bad:

```javascript
ReferenceError
```

Good:

```text
Unable to Load Users
```

---

### Use Custom Errors

```javascript
throw new Error(
   "Invalid Password"
);
```

---

### Log Errors for Debugging

```javascript
console.error(error);
```

---

# Most Important Topics

✅ try

✅ catch

✅ finally

✅ throw

✅ Error Object

✅ error.message

✅ error.name

✅ Runtime Errors

✅ Custom Errors

✅ Async Error Handling

✅ Fetch Error Handling

✅ MERN Error Handling

---

# Error Handling Formula

```text
Risky Code
      ↓
try
      ↓
Error?
   /      \
 Yes       No
  ↓         ↓
catch    Continue
  ↓
finally
```

or

```text
Error Handling
=
try
+
catch
+
finally
+
throw
```

Error Handling is one of the most important JavaScript concepts and is essential for building robust React applications, Node.js APIs, Express servers, MongoDB integrations, and production-ready MERN stack applications.
