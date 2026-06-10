# Express.js Routing - Complete Deep Dive

# Introduction to Express.js

Express.js is the most popular Node.js framework used for building:

```text id="qkwxku"
REST APIs
Web Applications
Microservices
Backend Servers
```

Node.js provides:

```text id="ymbk0g"
HTTP Module
```

but writing routes using only Node.js is complex.

Example:

Without Express:

```js id="g0qvq7"
const http = require("http");

http.createServer((req,res)=>{

   if(req.url === "/"){
      res.end("Home");
   }

});
```

As application grows:

```text id="1ejj1v"
More Routes
More Conditions
More Complexity
```

Express simplifies this.

---

# What is Routing?

Routing means:

```text id="uqy18r"
Determining
Which Function Should Execute
For A Particular URL
```

Think of routing as:

```text id="k8m6dd"
Traffic Police
```

It decides:

```text id="gm5tza"
Request
      ↓
Correct Controller
      ↓
Response
```

---

# Real World Example

E-Commerce Application

```text id="1j8a31"
GET /products
       ↓
Get Products

GET /products/1
       ↓
Get Single Product

POST /products
       ↓
Create Product

DELETE /products/1
       ↓
Delete Product
```

Each URL is a route.

---

# Request Lifecycle

```text id="ehm09d"
Client
   ↓
HTTP Request
   ↓
Express Route
   ↓
Controller Logic
   ↓
Database
   ↓
Response
```

---

# Creating Express Server

Install Express:

```bash id="h5m2b8"
npm install express
```

---

# Basic Setup

```js id="jtw8jx"
const express =
require("express");

const app =
express();

app.listen(3000);
```

---

# First Route

```js id="6g0wb9"
app.get("/",(req,res)=>{

   res.send("Home Page");

});
```

URL:

```text id="x1eovg"
http://localhost:3000/
```

Response:

```text id="2h7j6v"
Home Page
```

---

# Understanding HTTP Methods

Express routes usually correspond to HTTP methods.

| Method | Purpose                 |
| ------ | ----------------------- |
| GET    | Read Data               |
| POST   | Create Data             |
| PUT    | Update Entire Resource  |
| PATCH  | Update Partial Resource |
| DELETE | Delete Resource         |

---

# GET Route

Used to fetch data.

```js id="6l5q3q"
app.get("/users",
(req,res)=>{

   res.send("Users");

});
```

---

# POST Route

Used to create data.

```js id="a7v6ci"
app.post("/users",
(req,res)=>{

   res.send("User Created");

});
```

---

# PUT Route

Used to replace data.

```js id="2eqe9y"
app.put("/users/1",
(req,res)=>{

   res.send("Updated");

});
```

---

# DELETE Route

```js id="h13v6e"
app.delete("/users/1",
(req,res)=>{

   res.send("Deleted");

});
```

---

# Route Parameters

One of the most important Express concepts.

---

# What Are Route Parameters?

Route parameters are dynamic values embedded inside URL paths.

Example:

```text id="k8xjtl"
/users/10

/users/20

/users/30
```

IDs change dynamically.

---

# Problem Without Route Parameters

Bad Approach:

```js id="8lsz0y"
app.get("/users/1");

app.get("/users/2");

app.get("/users/3");
```

Impossible to manage.

---

# Route Parameter Solution

```js id="m8r6ef"
app.get(
"/users/:id",
(req,res)=>{

});
```

---

# How It Works

URL:

```text id="8buwfx"
/users/101
```

Express stores:

```js id="ycr6px"
req.params
```

Output:

```js id="jqzk0r"
{
   id:"101"
}
```

---

# Example

```js id="lcf17w"
app.get(
"/users/:id",
(req,res)=>{

 console.log(
  req.params.id
 );

 res.send("User");

});
```

Request:

```text id="t4xjvk"
/users/25
```

Output:

```text id="j68u0n"
25
```

---

# Multiple Route Parameters

```js id="z7gklt"
app.get(
"/users/:userId/orders/:orderId",
(req,res)=>{

 console.log(req.params);

});
```

URL:

```text id="ig48y8"
/users/5/orders/100
```

Output:

```js id="yjlwmc"
{
 userId:"5",
 orderId:"100"
}
```

---

# Real World Examples

Get Product

```text id="jlwmzt"
/products/10
```

Get User

```text id="zkvhmw"
/users/5
```

Get Order

```text id="5nlyww"
/orders/100
```

All use route parameters.

---

# Route Parameter Validation

```js id="fxo29n"
app.get(
"/users/:id",
(req,res)=>{

 const id =
 Number(req.params.id);

 if(isNaN(id)){

   return res
   .status(400)
   .send("Invalid ID");

 }

});
```

Always validate.

---

# Query Parameters

Another frequently asked interview topic.

---

# What Are Query Parameters?

Query parameters are key-value pairs added after:

```text id="6y9jv4"
?
```

symbol.

Example:

```text id="1vk5zr"
/products?page=1
```

---

# Structure

```text id="4t7p5m"
URL
  ?
key=value
```

Example:

```text id="ezl7jm"
/products?page=2&limit=10
```

---

# Accessing Query Parameters

```js id="jtdc2z"
app.get(
"/products",
(req,res)=>{

 console.log(
  req.query
 );

});
```

Request:

```text id="e09a4v"
/products?page=2&limit=10
```

Output:

```js id="3n78ux"
{
 page:"2",
 limit:"10"
}
```

---

# Access Individual Values

```js id="rv4rt9"
const page =
req.query.page;

const limit =
req.query.limit;
```

---

# Route Params vs Query Params

URL:

```text id="yvk0xk"
/products/10
```

Route Parameter:

```js id="1vjlwm"
req.params.id
```

Output:

```text id="4o3nvy"
10
```

---

URL:

```text id="x87knd"
/products?page=2
```

Query Parameter:

```js id="jlwmva"
req.query.page
```

Output:

```text id="h8c5n0"
2
```

---

# When To Use Route Params?

Use when resource is mandatory.

Example:

```text id="q5k6mr"
/users/10
```

Need specific user.

---

# When To Use Query Params?

Use for filtering.

Example:

```text id="6q6f7z"
/users?page=2
```

or

```text id="jlwmvb"
/products?category=mobile
```

---

# Real World Example

Amazon

```text id="8jpbw5"
/products?category=laptop
```

Netflix

```text id="jlwmvc"
/movies?genre=action
```

Instagram

```text id="jlwmvd"
/posts?page=2
```

---

# Pagination Example

```js id="jlwmve"
app.get(
"/products",
(req,res)=>{

 const page =
 Number(req.query.page) || 1;

 const limit =
 Number(req.query.limit) || 10;

});
```

Request:

```text id="jlwmvf"
/products?page=2&limit=20
```

Meaning:

```text id="jlwmvg"
Page 2
20 Records
```

---

# Route Chaining

Most developers learn this later.

Very useful for clean code.

---

# Problem

Without chaining:

```js id="jlwmvh"
app.get("/users",getUsers);

app.post("/users",createUser);

app.put("/users",updateUser);

app.delete("/users",deleteUser);
```

Routes become messy.

---

# Route Chaining Solution

```js id="jlwmvi"
app.route("/users")

.get(getUsers)

.post(createUser)

.put(updateUser)

.delete(deleteUser);
```

Much cleaner.

---

# How Route Chaining Works

```text id="jlwmvj"
/users
    ↓
GET
    ↓
getUsers()

POST
    ↓
createUser()

PUT
    ↓
updateUser()

DELETE
    ↓
deleteUser()
```

---

# Complete Example

```js id="jlwmvk"
app.route("/products")

.get((req,res)=>{

 res.send("Get Products");

})

.post((req,res)=>{

 res.send("Create Product");

});
```

Request:

```text id="jlwmvl"
GET /products
```

Output:

```text id="jlwmvm"
Get Products
```

---

Request:

```text id="jlwmvn"
POST /products
```

Output:

```text id="jlwmvo"
Create Product
```

---

# Route Chaining With Parameters

```js id="jlwmvp"
app.route("/users/:id")

.get(getUser)

.put(updateUser)

.delete(deleteUser);
```

---

# Real Project Structure

```text id="jlwmvq"
Routes
   ↓
Controllers
   ↓
Services
   ↓
Database
```

Example:

```js id="jlwmvr"
app.route("/users")

.get(userController.getUsers)

.post(userController.createUser);
```

Professional structure.

---

# Common Routing Mistakes

## Mistake 1

```js id="jlwmvs"
app.get("/users/:id");
```

```js id="jlwmvt"
app.get("/users/profile");
```

Problem:

```text id="jlwmvu"
/users/profile
```

may be interpreted as:

```text id="jlwmvv"
id = profile
```

Always place specific routes first.

---

## Mistake 2

Using query params instead of route params.

Bad:

```text id="jlwmvw"
/user?id=5
```

Better:

```text id="jlwmvx"
/users/5
```

for resource identification.

---

# Express Routing Flow

```text id="jlwmvy"
Request
   ↓
Route Match
   ↓
Route Handler
   ↓
Controller
   ↓
Response
```

---

# Interview Questions

### What Is Routing?

Mapping URLs to specific handler functions.

---

### What Is Route Parameter?

Dynamic value inside URL path.

Example:

```text id="jlwmvz"
/users/:id
```

---

### What Is Query Parameter?

Key-value pair after ?

Example:

```text id="jlwmwa"
/users?page=1
```

---

### Difference Between Params And Query?

Params:

```text id="jlwmwb"
Mandatory Resource Identifier
```

Example:

```text id="jlwmwc"
/users/5
```

---

Query:

```text id="jlwmwd"
Filtering / Sorting / Pagination
```

Example:

```text id="jlwmwe"
/users?page=2
```

---

### What Is Route Chaining?

Grouping multiple HTTP methods under same route.

---

### Benefits Of Route Chaining?

```text id="jlwmwf"
Cleaner Code

Less Duplication

Better Maintainability
```

---

# Summary

Express Routing Concepts:

✔ Routing

✔ HTTP Methods

✔ GET

✔ POST

✔ PUT

✔ DELETE

✔ Route Parameters

✔ Multiple Parameters

✔ Query Parameters

✔ Pagination Queries

✔ Route Chaining

✔ Route Matching

✔ Express Request Lifecycle

These concepts form the foundation of every Express.js API and are frequently asked in MERN Stack interviews from beginner to experienced levels.
