# JavaScript Fetch API - Complete Notes

# What is Fetch API?

The Fetch API is a modern JavaScript interface used to make HTTP requests to servers and APIs.

Think of Fetch as:

```text
JavaScript
     ↓
Fetch API
     ↓
Server/API
     ↓
Response Data
```

Fetch allows JavaScript to:

* Get Data (GET)
* Create Data (POST)
* Update Data (PUT/PATCH)
* Delete Data (DELETE)

---

# Why Do We Need Fetch API?

Modern applications rarely store all data in HTML.

Instead:

```text
Frontend
    ↓
API Call
    ↓
Backend
    ↓
Database
```

Example:

Instagram

```text
Open App
     ↓
Fetch User Posts
     ↓
Display Posts
```

Amazon

```text
Open Product
     ↓
Fetch Product Data
     ↓
Display Product
```

React and MERN applications use Fetch extensively.

---

# What is an API?

API stands for:

```text
Application Programming Interface
```

An API acts as a bridge between applications.

Example:

```text
React App
      ↓
API
      ↓
Node.js Server
      ↓
MongoDB
```

---

# What is fetch()?

The fetch() function sends HTTP requests.

Syntax:

```javascript
fetch(url)
```

Returns:

```javascript
Promise
```

This is why Fetch is usually used with:

```javascript
async / await
```

or

```javascript
.then()
```

---

# First Fetch Example

```javascript
fetch(
   "https://jsonplaceholder.typicode.com/users"
);
```

This sends a GET request.

---

# Using Async/Await

```javascript
const response =
await fetch(
   "https://jsonplaceholder.typicode.com/users"
);

const data =
await response.json();

console.log(data);
```

Output:

```javascript
[
  {
     id:1,
     name:"Leanne"
  },
  {
     id:2,
     name:"Ervin"
  }
]
```

---

# Understanding Step-by-Step

```javascript
const response =
await fetch(url);
```

Fetch sends request to server.

---

Server responds:

```text
HTTP Response
```

Stored in:

```javascript
response
```

---

# What is Response Object?

```javascript
const response =
await fetch(url);
```

Response contains:

```text
status
headers
body
ok
url
```

Example:

```javascript
console.log(response);
```

Output:

```javascript
Response {
   status:200,
   ok:true
}
```

---

# response.json()

Server sends data as JSON.

Example:

```json
{
   "name":"John"
}
```

JavaScript cannot directly use it.

Convert:

```javascript
const data =
await response.json();
```

Now:

```javascript
{
   name:"John"
}
```

becomes JavaScript object.

---

# Visual Representation

```text
Server
   ↓
JSON String
   ↓
response.json()
   ↓
JavaScript Object
```

---

# Complete GET Request

```javascript
async function getUsers(){

   const response =
   await fetch(
      "https://jsonplaceholder.typicode.com/users"
   );

   const users =
   await response.json();

   console.log(users);

}
```

---

# GET Request Flow

```text
Browser
    ↓
GET Request
    ↓
Server
    ↓
JSON Response
    ↓
response.json()
    ↓
JavaScript Object
```

---

# Using .then()

Before Async/Await:

```javascript
fetch(
   "https://jsonplaceholder.typicode.com/users"
)
.then(response=>{

   return response.json();

})
.then(data=>{

   console.log(data);

});
```

Same result.

---

# HTTP Methods

Fetch supports different methods.

| Method | Purpose             |
| ------ | ------------------- |
| GET    | Read Data           |
| POST   | Create Data         |
| PUT    | Update Entire Data  |
| PATCH  | Update Partial Data |
| DELETE | Remove Data         |

---

# GET Request

Default method.

```javascript
fetch(url);
```

Equivalent to:

```javascript
fetch(url,{
   method:"GET"
});
```

---

# POST Request

Used to create data.

Example:

```javascript
await fetch(url,{

   method:"POST",

   headers:{
      "Content-Type":
      "application/json"
   },

   body:JSON.stringify({

      name:"John"

   })

});
```

---

# Understanding POST Request

```javascript
method:"POST"
```

Means:

```text
Create New Data
```

---

# Why Headers?

```javascript
headers:{
   "Content-Type":
   "application/json"
}
```

Tells server:

```text
I am sending JSON data
```

---

# Why JSON.stringify()?

JavaScript Object:

```javascript
{
   name:"John"
}
```

Must be converted into JSON string.

```javascript
JSON.stringify({
   name:"John"
});
```

Output:

```json
{
   "name":"John"
}
```

---

# Complete POST Example

```javascript
async function createUser(){

   const response =
   await fetch(url,{

      method:"POST",

      headers:{
         "Content-Type":
         "application/json"
      },

      body:JSON.stringify({

         name:"John",
         age:25

      })

   });

   const data =
   await response.json();

   console.log(data);
}
```

---

# PUT Request

Replace entire resource.

```javascript
await fetch(url,{

   method:"PUT",

   headers:{
      "Content-Type":
      "application/json"
   },

   body:JSON.stringify({

      name:"Alex"

   })

});
```

---

# PATCH Request

Update only specific fields.

```javascript
await fetch(url,{

   method:"PATCH",

   headers:{
      "Content-Type":
      "application/json"
   },

   body:JSON.stringify({

      age:30

   })

});
```

---

# DELETE Request

Remove data.

```javascript
await fetch(url,{

   method:"DELETE"

});
```

---

# Error Handling

Very Important.

Bad:

```javascript
const response =
await fetch(url);
```

If server fails:

```text
Application Error
```

---

# Using try/catch

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

# Checking Response Status

Example:

```javascript
const response =
await fetch(url);

console.log(
   response.status
);
```

Output:

```text
200
```

---

# Common Status Codes

| Code | Meaning      |
| ---- | ------------ |
| 200  | Success      |
| 201  | Created      |
| 400  | Bad Request  |
| 401  | Unauthorized |
| 403  | Forbidden    |
| 404  | Not Found    |
| 500  | Server Error |

---

# response.ok

Shortcut for checking success.

```javascript
if(response.ok){

   console.log("Success");

}
```

Output:

```text
Success
```

---

# Example

```javascript
const response =
await fetch(url);

if(!response.ok){

   throw new Error(
      "Request Failed"
   );

}
```

---

# Fetch Multiple APIs

Sequential:

```javascript
const users =
await fetchUsers();

const products =
await fetchProducts();
```

Slow.

---

# Faster with Promise.all()

```javascript
const [
   users,
   products
] = await Promise.all([

   fetchUsers(),
   fetchProducts()

]);
```

Runs simultaneously.

---

# Real React Example

```javascript
useEffect(()=>{

   async function fetchUsers(){

      const response =
      await fetch("/users");

      const data =
      await response.json();

      setUsers(data);

   }

   fetchUsers();

},[]);
```

Very common interview question.

---

# Real Node.js Backend Example

Frontend:

```javascript
const response =
await fetch(
   "http://localhost:5000/users"
);

const users =
await response.json();
```

Backend:

```javascript
app.get("/users",
async(req,res)=>{

   const users =
   await User.find();

   res.json(users);

});
```

---

# Common Interview Questions

## What is Fetch API?

A JavaScript API used to make HTTP requests.

---

## What does fetch() return?

```javascript
Promise
```

---

## Why use response.json()?

To convert JSON response into JavaScript object.

---

## Difference Between GET and POST?

### GET

Fetch data.

### POST

Send/Create data.

---

## Why JSON.stringify()?

Converts JavaScript object into JSON string.

---

## How to Handle Errors?

Using:

```javascript
try{
}
catch(error){
}
```

---

## What is response.ok?

Returns:

```javascript
true
```

if request succeeded.

---

# Fetch API vs Axios

| Fetch                | Axios                  |
| -------------------- | ---------------------- |
| Built into Browser   | External Library       |
| Uses response.json() | Automatic JSON Parsing |
| Smaller              | More Features          |
| Native               | Popular in React       |

---

# Real MERN Stack Usage

Fetch is used for:

✅ Login

✅ Registration

✅ Fetch Users

✅ Fetch Products

✅ Create Orders

✅ Payment Integration

✅ File Upload APIs

✅ Dashboard Data

---

# Most Important Fetch Topics

✅ fetch()

✅ GET Request

✅ POST Request

✅ PUT Request

✅ PATCH Request

✅ DELETE Request

✅ response.json()

✅ Headers

✅ JSON.stringify()

✅ Error Handling

✅ response.ok

✅ Status Codes

✅ Promise.all()

✅ React API Calls

---

# Fetch API Formula

```text
Fetch API
     =
HTTP Request
     +
Promise
     +
Server Response
```

or

```text
Client
   ↓
fetch()
   ↓
Server
   ↓
JSON Response
   ↓
response.json()
   ↓
JavaScript Object
```

The Fetch API is one of the most important JavaScript concepts and is used daily in React, Node.js, Express.js, MongoDB applications, REST APIs, and MERN Stack development.
