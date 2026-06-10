# Express.js Middleware - Complete Deep Dive

# What is Middleware?

Middleware is one of the most important concepts in Express.js.

Many developers can build APIs without fully understanding middleware, but middleware is actually the heart of Express.

Everything in Express works because of middleware.

---

# Simple Definition

Middleware is a function that executes:

```text id="x1c7d9"
Before Request Reaches Route

OR

Before Response Goes Back To Client
```

---

# Real Life Example

Imagine entering an airport.

You don't directly board the airplane.

You pass through:

```text id="y8j5r2"
Security Check
       ↓
Passport Verification
       ↓
Luggage Check
       ↓
Boarding Gate
       ↓
Flight
```

Each step processes you before reaching the destination.

Middleware works exactly like this.

---

# Express Request Lifecycle

Without Middleware:

```text id="m3k8n1"
Client
   ↓
Route
   ↓
Response
```

---

With Middleware:

```text id="n9v2w4"
Client
   ↓
Middleware 1
   ↓
Middleware 2
   ↓
Middleware 3
   ↓
Route
   ↓
Response
```

---

# Middleware Function Structure

Every middleware has access to:

```js id="a7m4t9"
(req, res, next)
```

---

# Understanding Parameters

### req

Request Object

Contains:

```text id="q2h6k8"
Headers
Body
Params
Query
Cookies
```

---

### res

Response Object

Used to send response.

```js id="j4p7r3"
res.send()
```

---

### next

Most important part.

Passes control to next middleware.

```js id="z6d1n5"
next()
```

---

# Example Middleware

```js id="b8w3f7"
app.use((req,res,next)=>{

   console.log("Middleware Running");

   next();

});
```

---

# Request Flow

```text id="l1x9c4"
Request
   ↓
Middleware Executes
   ↓
next()
   ↓
Route Executes
```

---

# What Happens If next() Is Missing?

Example:

```js id="v5n2k7"
app.use((req,res,next)=>{

   console.log("Middleware");

});
```

Request:

```text id="r3m8p1"
/users
```

Execution:

```text id="f7c6w2"
Middleware Runs
      ↓
Stops Here
```

Route never executes.

Browser keeps loading.

---

# Why Middleware Exists

Used for:

```text id="g9q4t8"
Authentication

Authorization

Logging

Validation

Error Handling

Parsing Request Body

Rate Limiting

File Uploads
```

---

# Middleware Execution Order

Very important interview question.

---

Example:

```js id="u2y5m8"
app.use((req,res,next)=>{

 console.log("M1");

 next();

});

app.use((req,res,next)=>{

 console.log("M2");

 next();

});

app.get("/",(req,res)=>{

 res.send("Home");

});
```

Output:

```text id="c5v7n1"
M1

M2

Home
```

Middleware executes in order.

---

# Types Of Middleware

Express has:

```text id="t8p2w6"
1. Application Middleware

2. Router Middleware

3. Error Middleware

4. Third Party Middleware

5. Built-in Middleware
```

---

# 1. APPLICATION MIDDLEWARE

# What Is Application Middleware?

Middleware attached directly to:

```js id="k4m9s2"
app
```

using:

```js id="x7r3v8"
app.use()
```

---

# Why Use It?

Runs for:

```text id="d6n5y1"
Every Request
```

or

```text id="w2p8c7"
Specific Routes
```

---

# Global Application Middleware

```js id="m8q3r6"
app.use((req,res,next)=>{

 console.log(
   "Request Received"
 );

 next();

});
```

---

Request:

```text id="n1v7k3"
/users
```

Output:

```text id="z5c8t2"
Request Received
```

---

Request:

```text id="y4m2p9"
/products
```

Output:

```text id="h7q6r1"
Request Received
```

Runs for all routes.

---

# Route-Specific Application Middleware

```js id="c9x5w8"
app.use(
 "/admin",
 middleware
);
```

Only runs for:

```text id="v3k7m2"
/admin
/admin/users
/admin/settings
```

---

Not for:

```text id="j6n4q8"
/products
```

---

# Example

```js id="r8y3t6"
app.use(
 "/admin",
 (req,res,next)=>{

   console.log(
    "Admin Middleware"
   );

   next();

 });
```

---

# Real World Use Cases

```text id="q4m8w1"
Logging

Authentication

Rate Limiting

Security Checks
```

---

# Authentication Middleware Example

```js id="n7c5p2"
const auth =
(req,res,next)=>{

 const token =
 req.headers.authorization;

 if(!token){

   return res.status(401)
   .send("Unauthorized");

 }

 next();

};
```

Apply:

```js id="x2v9r7"
app.use("/admin",auth);
```

---

# Request Flow

```text id="m6p3k9"
Request
   ↓
Auth Middleware
   ↓
Token Valid?
   ↓
Yes → Route

No → Error
```

---

# 2. ROUTER MIDDLEWARE

# What Is Router Middleware?

Used with:

```js id="b4n8t3"
express.Router()
```

Allows middleware to apply only to specific routers.

---

# Why Needed?

Large applications contain:

```text id="p9w6k1"
Users Routes

Products Routes

Orders Routes

Admin Routes
```

We don't want middleware everywhere.

---

# Create Router

```js id="g3m7q2"
const router =
express.Router();
```

---

# Router Middleware

```js id="u8c4v1"
router.use(
 (req,res,next)=>{

   console.log(
    "Router Middleware"
   );

   next();

 });
```

---

# Example Structure

```text id="y7n5p4"
routes

├── userRoutes.js

├── adminRoutes.js

├── productRoutes.js
```

---

# Admin Middleware

```js id="v6m3r8"
router.use(auth);
```

Only admin routes protected.

---

# Flow

```text id="x1k9m6"
/admin/users
       ↓
Router Middleware
       ↓
Route
```

---

# Benefits

```text id="n5v8q2"
Cleaner Code

Modular Structure

Reusable Middleware
```

---

# 3. ERROR HANDLING MIDDLEWARE

# What Is Error Middleware?

Special middleware used for handling errors.

---

# Normal Middleware

```js id="q7r3m5"
(req,res,next)
```

3 parameters.

---

# Error Middleware

```js id="d4v8n2"
(err,req,res,next)
```

4 parameters.

---

# Why Special?

Express identifies:

```text id="m9k5p7"
4 Parameters
```

as error middleware.

---

# Example

```js id="h3n7v1"
app.use(
 (err,req,res,next)=>{

   res.status(500)
   .json({

     message:
     err.message

   });

 });
```

---

# Throw Error

```js id="w8m2c5"
app.get(
 "/test",
 (req,res)=>{

   throw new Error(
    "Something Wrong"
   );

 });
```

---

# Flow

```text id="k4q9v6"
Route Error
      ↓
Error Middleware
      ↓
Response
```

---

# Custom Error Middleware

```js id="z1v8n4"
app.use(
 (err,req,res,next)=>{

   console.error(err);

   res.status(
    err.status || 500
   )
   .json({

      success:false,

      message:
      err.message

   });

 });
```

---

# Why Error Middleware Matters

Without it:

```text id="c8p3m7"
Server Crash

Unclear Errors

Bad User Experience
```

---

# Real Project Error Flow

```text id="r6m2k8"
Request
   ↓
Controller
   ↓
Database Error
   ↓
next(error)
   ↓
Error Middleware
   ↓
JSON Response
```

---

# Passing Errors

```js id="y9q4v1"
try{

}
catch(error){

 next(error);

}
```

Express automatically sends it to error middleware.

---

# 4. THIRD PARTY MIDDLEWARE

# What Is Third Party Middleware?

Middleware created by other developers.

Installed using:

```bash id="p7m4c8"
npm install
```

---

# Why Use It?

Avoid writing common functionality.

Examples:

```text id="t5v2n9"
Authentication

Logging

File Uploads

Compression

Security
```

already available.

---

# Popular Third Party Middleware

| Package            | Purpose               |
| ------------------ | --------------------- |
| morgan             | Logging               |
| cors               | Cross Origin Requests |
| helmet             | Security              |
| compression        | Response Compression  |
| multer             | File Upload           |
| express-rate-limit | Rate Limiting         |

---

# Morgan Middleware

Install:

```bash id="q2m8v5"
npm install morgan
```

Use:

```js id="x7n3k1"
const morgan =
require("morgan");

app.use(
 morgan("dev")
);
```

Output:

```text id="v8r5m2"
GET /users 200
```

Logs requests.

---

# CORS Middleware

Problem:

Frontend:

```text id="z3p7v4"
localhost:3000
```

Backend:

```text id="m5k8n1"
localhost:5000
```

Browser blocks request.

---

Install:

```bash id="c4v9q2"
npm install cors
```

Use:

```js id="n6m3v7"
const cors =
require("cors");

app.use(cors());
```

Allows requests.

---

# Helmet Middleware

Adds security headers.

Install:

```bash id="t9p5m1"
npm install helmet
```

Use:

```js id="k8v3n6"
app.use(
 helmet()
);
```

Protects against:

```text id="r2m7q4"
XSS

Clickjacking

Content Injection
```

---

# Multer Middleware

Used for:

```text id="j5v8n3"
File Uploads
```

Example:

```js id="w7m2p6"
const multer =
require("multer");

const upload =
multer({
 dest:"uploads/"
});
```

---

# Compression Middleware

Compresses responses.

```js id="m1q8v4"
const compression =
require(
 "compression"
);

app.use(
 compression()
);
```

Benefits:

```text id="c7n3v5"
Smaller Response

Faster API
```

---

# Complete Middleware Flow

```text id="p4v7m2"
Client Request
      ↓
Morgan
      ↓
CORS
      ↓
Helmet
      ↓
Authentication
      ↓
Validation
      ↓
Route
      ↓
Controller
      ↓
Database
      ↓
Response
```

---

# Middleware vs Route Handler

Middleware:

```text id="x6m9v1"
Processes Request
```

Route Handler:

```text id="k2p7m5"
Generates Response
```

---

# Interview Questions

### What Is Middleware?

Function that executes between request and response cycle.

---

### What Does next() Do?

Passes control to next middleware.

---

### What Happens If next() Is Missing?

Request hangs.

---

### Difference Between Application And Router Middleware?

Application Middleware:

```text id="n4v8m3"
Attached To App
```

Router Middleware:

```text id="r7m2v5"
Attached To Router
```

---

### How Does Express Identify Error Middleware?

By:

```js id="z9m4v2"
(err,req,res,next)
```

4 parameters.

---

### Why Use Third Party Middleware?

Avoid reinventing common functionality.

---

# Summary

Express Middleware Concepts:

✔ Middleware Fundamentals

✔ next()

✔ Request Lifecycle

✔ Application Middleware

✔ Router Middleware

✔ Error Middleware

✔ Third Party Middleware

✔ Morgan

✔ CORS

✔ Helmet

✔ Multer

✔ Compression

✔ Authentication Middleware

✔ Middleware Chaining

✔ Error Flow

These concepts are heavily used in every real-world Express.js application and are among the most frequently asked Express interview topics.
