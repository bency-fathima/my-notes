# Mongoose Advanced Concepts - Complete Guide

# What is Mongoose?

Mongoose is an ODM (Object Data Modeling) library for MongoDB and Node.js.

ODM acts as a bridge between:

```text
Node.js Application
        ↕
     Mongoose
        ↕
     MongoDB
```

It provides:

* Schema Validation
* Middleware (Hooks)
* Population
* Virtual Fields
* Transactions
* Query Helpers
* Plugins

---

# Why Use Mongoose?

Without Mongoose:

```js
const user = {
  name: "John",
  email: "john@gmail.com"
};
```

No validation.

No structure.

No hooks.

---

With Mongoose:

```js
const userSchema = new mongoose.Schema({
  name: String,
  email: {
    type: String,
    required: true
  }
});
```

Benefits:

* Data consistency
* Validation
* Relationships
* Middleware support

---

# Mongoose Middleware (Hooks)

# What is Middleware?

Middleware is a function that executes:

```text
Before
or
After
```

a database operation.

Think of it as:

```text
Request
   ↓
Hook Executes
   ↓
Database Operation
```

---

# Why Use Middleware?

Common use cases:

* Password hashing
* Logging
* Validation
* Soft delete
* Auditing
* Sending emails

---

# Types of Middleware

1. Document Middleware
2. Query Middleware
3. Aggregate Middleware
4. Model Middleware

---

# Document Middleware

Runs on document methods.

Examples:

```js
save()
validate()
remove()
deleteOne()
```

---

# Pre Middleware

Runs BEFORE operation.

Syntax:

```js
schema.pre("save", function(next) {
   next();
});
```

Example:

```js
userSchema.pre("save", function(next) {

   console.log("Before Save");

   next();

});
```

Execution:

```text
Save User
   ↓
Before Save
   ↓
Database Save
```

---

# Password Hashing Example

Most asked interview question.

```js
const bcrypt = require("bcrypt");
```

```js
userSchema.pre("save", async function(next){

   if(!this.isModified("password"))
      return next();

   this.password =
      await bcrypt.hash(this.password,10);

   next();

});
```

Before:

```json
{
  "password":"123456"
}
```

After:

```json
{
  "password":"$2b$10..."
}
```

---

# Post Middleware

Runs AFTER operation.

Syntax:

```js
schema.post("save", function(doc){

});
```

Example:

```js
userSchema.post("save", function(doc){

   console.log("User Created");

});
```

Execution:

```text
Save User
   ↓
MongoDB Saves
   ↓
Post Hook Runs
```

---

# Query Middleware

Runs on:

```js
find()
findOne()
updateOne()
deleteOne()
findOneAndUpdate()
```

Example:

```js
userSchema.pre("find", function(){

   console.log(this.getQuery());

});
```

Useful for:

* Query logging
* Multi-tenancy
* Soft delete

---

# Soft Delete Example

Instead of deleting:

```js
{
   isDeleted:true
}
```

Hide deleted records automatically.

```js
userSchema.pre(/^find/, function(next){

   this.where({
      isDeleted:false
   });

   next();

});
```

Now every find query automatically excludes deleted users.

---

# Aggregate Middleware

Runs before aggregation.

Example:

```js
schema.pre("aggregate", function(){

});
```

Useful for:

* Filtering archived records
* Multi-tenant systems

---

# Middleware Execution Order

```text
pre save
   ↓
MongoDB Operation
   ↓
post save
```

---

# Common Middleware Hooks

| Hook      | Trigger         |
| --------- | --------------- |
| save      | Save document   |
| validate  | Validation      |
| remove    | Remove document |
| find      | Find documents  |
| updateOne | Update          |
| deleteOne | Delete          |
| aggregate | Aggregation     |

---

# Virtuals

# What are Virtuals?

Virtual fields are fields that:

```text
Not Stored In Database
```

but are generated dynamically.

---

# Why Use Virtuals?

Suppose:

```json
{
  "firstName":"John",
  "lastName":"Doe"
}
```

Need:

```text
John Doe
```

No need to store:

```json
{
  "fullName":"John Doe"
}
```

Create virtual.

---

# Creating Virtual

```js
userSchema.virtual("fullName")
.get(function(){

   return `${this.firstName}
           ${this.lastName}`;

});
```

Document:

```json
{
   "firstName":"John",
   "lastName":"Doe"
}
```

Output:

```json
{
   "fullName":"John Doe"
}
```

Generated dynamically.

---

# Virtual with JSON Response

Enable virtuals:

```js
const userSchema = new mongoose.Schema(
 {},
 {
    toJSON:{
      virtuals:true
    }
 }
);
```

Without this:

```js
res.json(user);
```

Virtuals won't appear.

---

# Virtual Setter

Example:

```js
userSchema.virtual("fullName")
.set(function(name){

   const parts = name.split(" ");

   this.firstName = parts[0];
   this.lastName = parts[1];

});
```

Usage:

```js
user.fullName = "John Doe";
```

Automatically:

```js
firstName = "John";
lastName = "Doe";
```

---

# Real World Virtual Example

User Profile

Stored:

```json
{
   "profileImage":
   "profile.jpg"
}
```

Virtual:

```js
userSchema.virtual("profileUrl")
.get(function(){

 return `https://domain.com/uploads/${this.profileImage}`;

});
```

Output:

```json
{
   "profileUrl":
   "https://domain.com/uploads/profile.jpg"
}
```

---

# Population

# What is Population?

Population is Mongoose's version of:

```sql
JOIN
```

It replaces referenced ObjectIds with actual documents.

---

# Why Population?

Collections:

Users

```json
{
   "_id":"1",
   "name":"John"
}
```

Posts

```json
{
   "title":"MongoDB",
   "user":"1"
}
```

Without population:

```json
{
   "user":"1"
}
```

Not useful.

---

# Creating Relationship

User Schema

```js
const userSchema =
new mongoose.Schema({

   name:String

});
```

---

Post Schema

```js
const postSchema =
new mongoose.Schema({

   title:String,

   user:{
      type:mongoose.Schema.Types.ObjectId,
      ref:"User"
   }

});
```

---

# Using Populate

```js
Post.find()
.populate("user");
```

Output:

```json
{
  "title":"MongoDB",

  "user":{
     "_id":"1",
     "name":"John"
  }
}
```

---

# Select Specific Fields

```js
Post.find()
.populate(
   "user",
   "name email"
);
```

Output:

```json
{
   "user":{
      "name":"John",
      "email":"john@gmail.com"
   }
}
```

---

# Populate Multiple Paths

```js
Post.find()
.populate("user")
.populate("category");
```

---

# Nested Populate

Example:

```text
Post
 ↓
User
 ↓
Company
```

```js
Post.find()
.populate({
   path:"user",
   populate:{
      path:"company"
   }
});
```

---

# Population vs Lookup

Population:

```text
Mongoose Level
```

Lookup:

```text
MongoDB Level
```

---

# Interview Question

Which is faster?

```text
$lookup
```

because it executes directly in MongoDB.

Population requires additional processing by Mongoose.

---

# Transactions

# What is a Transaction?

Transaction ensures:

```text
All Operations Succeed
OR
All Operations Fail
```

Concept:

```text
Atomicity
```

---

# Why Transactions?

Bank Transfer

```text
Account A → Account B
```

Scenario:

```text
Deduct Money From A
Add Money To B
```

If server crashes between operations:

```text
Money Lost
```

Transactions prevent this.

---

# Without Transaction

```js
await Account.updateOne(
{
   _id:sender
},
{
   $inc:{balance:-100}
});

await Account.updateOne(
{
   _id:receiver
},
{
   $inc:{balance:100}
});
```

Problem:

Second query may fail.

---

# Transaction Flow

```text
Start Session
      ↓
Start Transaction
      ↓
Operation 1
      ↓
Operation 2
      ↓
Commit
```

or

```text
Abort
```

---

# Creating Transaction

```js
const session =
await mongoose.startSession();
```

---

# Start Transaction

```js
session.startTransaction();
```

---

# Example

```js
const session =
await mongoose.startSession();

try{

   session.startTransaction();

   await Account.updateOne(
   {
      _id:sender
   },
   {
      $inc:{
         balance:-100
      }
   },
   {
      session
   });

   await Account.updateOne(
   {
      _id:receiver
   },
   {
      $inc:{
         balance:100
      }
   },
   {
      session
   });

   await session.commitTransaction();

}
catch(err){

   await session.abortTransaction();

}
finally{

   session.endSession();

}
```

---

# Commit Transaction

```js
await session.commitTransaction();
```

Meaning:

```text
Save All Changes
```

---

# Abort Transaction

```js
await session.abortTransaction();
```

Meaning:

```text
Rollback Everything
```

---

# Real World Uses

Transactions are used in:

### Banking

Transfer money.

---

### E-Commerce

```text
Create Order
Reduce Inventory
Create Payment Record
```

All should succeed.

---

### Wallet Applications

```text
Debit Wallet
Credit Wallet
```

---

### Ticket Booking

```text
Reserve Seat
Create Booking
Process Payment
```

---

# Transaction Limitations

Requires:

```text
Replica Set
```

MongoDB standalone server doesn't support full transactions.

---

# Interview Questions

### What is Mongoose?

ODM library for MongoDB and Node.js.

### What is Middleware?

Functions executed before or after database operations.

### Difference Between Pre and Post Hook?

Pre → Before operation.

Post → After operation.

### What are Virtuals?

Fields not stored in database but generated dynamically.

### What is Population?

Mongoose feature used to replace ObjectId references with actual documents.

### Population vs Lookup?

Population → Mongoose side.

Lookup → MongoDB side.

### What is Transaction?

Group of operations that succeed or fail together.

### What is Commit?

Save transaction.

### What is Abort?

Rollback transaction.

### Real Use Case of Transactions?

Bank transfers, orders, payments, wallet systems.

---

# Summary

Mongoose Advanced Topics:

✔ Schemas

✔ Models

✔ Middleware (Hooks)

✔ Pre Hooks

✔ Post Hooks

✔ Virtuals

✔ Virtual Getters

✔ Virtual Setters

✔ Population

✔ Nested Population

✔ Transactions

✔ Sessions

✔ Commit & Abort

These concepts are frequently asked in MERN interviews for developers with 2–5 years of experience and are heavily used in real-world production applications.
