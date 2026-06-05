# JavaScript Async / Await - Complete Notes

# What is Async / Await?

Async/Await is a modern JavaScript feature introduced in ES8 (2017).

It provides a cleaner and more readable way to work with Promises.

Think of it as:

```text
Promises
      ↓
Cleaner Syntax
      ↓
Async / Await
```

---

# Why Async/Await Was Introduced?

Before Async/Await, developers used Promise chaining.

Example:

```javascript
fetch(url)
.then(res => res.json())
.then(data => {

   console.log(data);

})
.catch(error => {

   console.log(error);

});
```

Works perfectly.

But when multiple asynchronous operations are involved:

```javascript
fetchUser()
.then(user => {

   return fetchOrders(user.id);

})
.then(orders => {

   return fetchPayment(orders);

})
.then(payment => {

   console.log(payment);

});
```

Code becomes harder to read.

---

# Async/Await Solution

```javascript
async function getData(){

   const user =
   await fetchUser();

   const orders =
   await fetchOrders(user.id);

   const payment =
   await fetchPayment(orders);

   console.log(payment);
}
```

Looks like synchronous code.

Much easier to understand.

---

# The Two Keywords

Async/Await consists of two keywords:

```javascript
async
await
```

---

# 1. async

The `async` keyword makes a function asynchronous.

Example:

```javascript
async function greet(){

   return "Hello";
}
```

---

# What Does async Actually Do?

JavaScript automatically wraps the return value in a Promise.

```javascript
async function greet(){

   return "Hello";
}
```

Internally:

```javascript
function greet(){

   return Promise.resolve("Hello");
}
```

---

Example:

```javascript
async function greet(){

   return "Hello";
}

greet()
.then(result => {

   console.log(result);

});
```

Output:

```text
Hello
```

---

# 2. await

The `await` keyword waits for a Promise to resolve.

Example:

```javascript
const promise =
Promise.resolve("Success");

async function test(){

   const result =
   await promise;

   console.log(result);
}
```

Output:

```text
Success
```

---

# Important Rule

`await` can only be used inside an async function.

Wrong:

```javascript
const result =
await fetch(url);
```

Output:

```text
SyntaxError
```

Correct:

```javascript
async function getData(){

   const result =
   await fetch(url);

}
```

---

# Understanding await Step-by-Step

Example:

```javascript
async function test(){

   console.log("A");

   await Promise.resolve();

   console.log("B");
}

test();

console.log("C");
```

Output:

```text
A
C
B
```

---

Why?

Execution Flow:

```text
A Printed
      ↓
await Found
      ↓
Pause Function
      ↓
Continue Main Thread
      ↓
Print C
      ↓
Promise Resolved
      ↓
Print B
```

---

# First Real Example

Without Async/Await:

```javascript
fetch(url)
.then(response => {

   return response.json();

})
.then(data => {

   console.log(data);

});
```

---

With Async/Await:

```javascript
async function getData(){

   const response =
   await fetch(url);

   const data =
   await response.json();

   console.log(data);
}
```

Much cleaner.

---

# What Happens Internally?

```javascript
const response =
await fetch(url);
```

Equivalent to:

```javascript
fetch(url)
.then(response => {

});
```

---

And:

```javascript
const data =
await response.json();
```

Equivalent to:

```javascript
response.json()
.then(data => {

});
```

---

# Real API Example

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

getUsers();
```

Output:

```javascript
[
   {id:1,name:"Leanne"},
   {id:2,name:"Ervin"}
]
```

---

# Error Handling with try/catch

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
Unhandled Error
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

# Visual Representation

```text
try
 │
 ├── Success
 │      ↓
 │   Execute Code
 │
 └── Error
        ↓
     catch
```

---

# Example

```javascript
async function test(){

   try{

      throw new Error(
         "Something went wrong"
      );

   }
   catch(error){

      console.log(
         error.message
      );

   }
}
```

Output:

```text
Something went wrong
```

---

# Async Function Always Returns Promise

Example:

```javascript
async function add(){

   return 10;
}
```

Output:

```javascript
Promise {10}
```

Consume:

```javascript
add()
.then(result => {

   console.log(result);

});
```

Output:

```text
10
```

---

# Multiple Awaits

```javascript
async function process(){

   const user =
   await getUser();

   const orders =
   await getOrders();

   const payment =
   await getPayment();

   console.log(payment);
}
```

Execution:

```text
Wait User
      ↓
Wait Orders
      ↓
Wait Payment
```

Runs one after another.

---

# Problem with Sequential Await

Imagine:

```javascript
const user =
await getUser();

const products =
await getProducts();
```

Time:

```text
User → 2 sec

Products → 2 sec

Total → 4 sec
```

Slow.

---

# Solution - Promise.all()

Run simultaneously.

```javascript
const [
   user,
   products
] = await Promise.all([

   getUser(),
   getProducts()

]);
```

Execution:

```text
User → 2 sec

Products → 2 sec

Total → 2 sec
```

Much faster.

---

# Promise.all with Async/Await

```javascript
async function getData(){

   const [
      users,
      products
   ] = await Promise.all([

      fetchUsers(),
      fetchProducts()

   ]);

   console.log(users);

   console.log(products);
}
```

Very common in React applications.

---

# Async/Await with setTimeout

Create Promise:

```javascript
function delay(){

   return new Promise(
      resolve => {

         setTimeout(
            resolve,
            2000
         );

      }
   );
}
```

Use:

```javascript
async function test(){

   console.log("Start");

   await delay();

   console.log("End");
}

test();
```

Output:

```text
Start
End
```

(after 2 seconds)

---

# Async/Await in React

Example:

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

# Async/Await in Node.js

Example:

```javascript
app.get("/users",
async(req,res)=>{

   const users =
   await User.find();

   res.json(users);

});
```

MongoDB queries return Promises.

Async/Await makes code cleaner.

---

# Common Interview Questions

## What is Async/Await?

A cleaner syntax for working with Promises.

---

## What does async do?

Makes a function return a Promise.

---

## What does await do?

Pauses execution until a Promise resolves.

---

## Can await be used outside async?

No.

```javascript
await fetch(url);
```

Produces:

```text
SyntaxError
```

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

## Does await block JavaScript?

No.

It pauses only the current async function.

The rest of JavaScript continues executing.

---

# Promise vs Async/Await

| Promise        | Async/Await     |
| -------------- | --------------- |
| .then()        | await           |
| .catch()       | try/catch       |
| Chaining       | Sequential Code |
| Harder to Read | Easier to Read  |
| Nested Logic   | Cleaner Logic   |

---

# Real MERN Stack Usage

Async/Await is used in:

### API Calls

```javascript
await fetch()
```

### Axios Requests

```javascript
await axios.get()
```

### MongoDB Queries

```javascript
await User.find()
```

### Authentication

```javascript
await loginUser()
```

### File Uploads

```javascript
await uploadFile()
```

### Payment Integration

```javascript
await createOrder()
```

---

# Execution Flow Visualization

```text
Start
  ↓
await fetch()
  ↓
Pause Function
  ↓
Continue Other Tasks
  ↓
Promise Resolves
  ↓
Resume Function
  ↓
End
```

---

# Most Important Topics

✅ async Keyword

✅ await Keyword

✅ Promise Conversion

✅ try/catch

✅ Error Handling

✅ Fetch API

✅ Promise.all()

✅ Sequential vs Parallel Execution

✅ React API Calls

✅ Node.js Async Operations

✅ MongoDB Queries

---

# Async/Await Formula

```text
Async Function
       +
Await Promise
       =
Readable Asynchronous Code
```

or

```text
Promise
      ↓
Async / Await
      ↓
Cleaner Code
```

Async/Await is one of the most important JavaScript concepts and is used daily in React, Node.js, Express.js, MongoDB, APIs, and MERN Stack applications. Mastering Async/Await is essential for modern JavaScript development.
