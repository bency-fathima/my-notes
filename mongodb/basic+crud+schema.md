# MongoDB Complete Notes

# 1. Introduction to MongoDB

MongoDB is a NoSQL database that stores data in JSON-like documents called BSON (Binary JSON).

## Features

- Document-oriented database
- Schema-less
- High performance
- Horizontal scaling
- Replication support
- Flexible data model

## Traditional SQL vs MongoDB

| SQL | MongoDB |
|-------|---------|
| Database | Database |
| Table | Collection |
| Row | Document |
| Column | Field |
| Primary Key | _id |
| Join | Embedding / Referencing |

---

# 2. MongoDB Architecture

```
Database
 ├── Collection
 │     ├── Document
 │     ├── Document
 │     └── Document
 └── Collection
```

Example:

```json
{
  "_id": "123",
  "name": "John",
  "age": 25
}
```

---

# 3. BSON (Binary JSON)

MongoDB stores data in BSON.

Advantages:

- Faster than JSON
- Supports Date
- Supports ObjectId
- Supports Binary Data

Example:

```json
{
   "_id": ObjectId("6654d34e5"),
   "createdAt": ISODate("2024-05-01")
}
```

---

# 4. MongoDB Data Types

## String

```js
{
   name: "John"
}
```

## Number

```js
{
   age: 25
}
```

## Boolean

```js
{
   isActive: true
}
```

## Array

```js
{
   skills: ["React", "Node"]
}
```

## Object

```js
{
   address: {
      city: "Delhi",
      pincode: 110001
   }
}
```

## Date

```js
{
   createdAt: new Date()
}
```

## ObjectId

```js
{
   _id: ObjectId("6654d34e5")
}
```

---

# 5. CRUD Operations

CRUD = Create, Read, Update, Delete

---

# CREATE

## Insert One Document

```js
db.users.insertOne({
   name: "John",
   age: 25
});
```

Output:

```js
{
   acknowledged: true,
   insertedId: ObjectId(...)
}
```

---

## Insert Multiple Documents

```js
db.users.insertMany([
  {
    name: "John",
    age: 25
  },
  {
    name: "David",
    age: 30
  }
]);
```

---

# READ

## Find All Documents

```js
db.users.find();
```

---

## Pretty Format

```js
db.users.find().pretty();
```

---

## Find One Document

```js
db.users.findOne({
   name: "John"
});
```

---

## Find Specific Fields

```js
db.users.find(
   {},
   {
      name: 1,
      age: 1
   }
);
```

---

## Query Operators

### Greater Than

```js
db.users.find({
   age: {
      $gt: 20
   }
});
```

### Less Than

```js
db.users.find({
   age: {
      $lt: 30
   }
});
```

### Greater Than Equal

```js
db.users.find({
   age: {
      $gte: 25
   }
});
```

### Less Than Equal

```js
db.users.find({
   age: {
      $lte: 30
   }
});
```

### Not Equal

```js
db.users.find({
   age: {
      $ne: 25
   }
});
```

---

# Logical Operators

## AND

```js
db.users.find({
   $and: [
      { age: 25 },
      { city: "Delhi" }
   ]
});
```

---

## OR

```js
db.users.find({
   $or: [
      { city: "Delhi" },
      { city: "Mumbai" }
   ]
});
```

---

## IN

```js
db.users.find({
   city: {
      $in: ["Delhi", "Mumbai"]
   }
});
```

---

# UPDATE

## Update One

```js
db.users.updateOne(
{
   name: "John"
},
{
   $set: {
      age: 30
   }
}
);
```

---

## Update Many

```js
db.users.updateMany(
{},
{
   $set: {
      isActive: true
   }
}
);
```

---

## Increment Value

```js
db.users.updateOne(
{
   name: "John"
},
{
   $inc: {
      age: 1
   }
}
);
```

---

## Push Into Array

```js
db.users.updateOne(
{
   name: "John"
},
{
   $push: {
      skills: "MongoDB"
   }
}
);
```

---

# DELETE

## Delete One

```js
db.users.deleteOne({
   name: "John"
});
```

---

## Delete Many

```js
db.users.deleteMany({
   age: {
      $lt: 18
   }
});
```

---

# 6. Indexing

Indexes improve query performance.

Without Index:

```
O(n)
```

With Index:

```
O(log n)
```

## Create Index

```js
db.users.createIndex({
   email: 1
});
```

1 = Ascending

---

## Descending Index

```js
db.users.createIndex({
   age: -1
});
```

---

## Compound Index

```js
db.users.createIndex({
   name: 1,
   age: -1
});
```

---

## Check Indexes

```js
db.users.getIndexes();
```

---

# 7. Schema Design

MongoDB is schema-less but schema design is very important.

Bad schema design causes:

- Slow queries
- Duplicate data
- Difficult maintenance

---

# Example User Schema

```json
{
  "_id": "1",
  "name": "John",
  "email": "john@gmail.com",
  "phone": "999999999"
}
```

---

# Design Principles

## 1. Data Access Pattern First

Ask:

- How often is data read?
- How often is data updated?
- How frequently is data joined?

Design according to usage.

---

## 2. Avoid Excessive Joins

MongoDB prefers:

- Embedded documents
- Denormalization

Instead of SQL-style joins.

---

## 3. Keep Documents Small

MongoDB document limit:

```text
16 MB
```

Never store:

- Large videos
- Huge images

Use GridFS.

---

# 8. Relationships in MongoDB

MongoDB supports relationships using:

1. Embedding
2. Referencing

---

# One-to-One Relationship

Example:

User → Profile

---

## Embedding

```json
{
   "_id": 1,
   "name": "John",
   "profile": {
      "gender": "Male",
      "age": 25
   }
}
```

Advantages:

- Fast reads
- Single query

Disadvantages:

- Data duplication

---

## Referencing

Profile Collection

```json
{
   "_id": 10,
   "gender": "Male",
   "age": 25
}
```

User Collection

```json
{
   "_id": 1,
   "name": "John",
   "profileId": 10
}
```

Advantages:

- Separate collections
- Easy updates

Disadvantages:

- Multiple queries

---

# One-to-Many Relationship

Example:

Customer → Orders

One customer has many orders.

---

## Embedding

```json
{
   "_id": 1,
   "name": "John",
   "orders": [
      {
         "product": "Laptop",
         "price": 50000
      }
   ]
}
```

Suitable when:

- Limited number of orders

---

## Referencing

Customers

```json
{
   "_id": 1,
   "name": "John"
}
```

Orders

```json
{
   "_id": 101,
   "customerId": 1,
   "product": "Laptop"
}
```

Suitable when:

- Thousands of orders

---

# Many-to-Many Relationship

Example:

Students ↔ Courses

One student can enroll in many courses.

One course can contain many students.

---

Students

```json
{
   "_id": 1,
   "name": "John",
   "courseIds": [1, 2]
}
```

Courses

```json
{
   "_id": 1,
   "courseName": "React"
}
```

---

# Mongoose Relationship Example

User Schema

```js
const userSchema = new mongoose.Schema({
   name: String
});
```

Post Schema

```js
const postSchema = new mongoose.Schema({
   title: String,
   user: {
      type: mongoose.Schema.Types.ObjectId,
      ref: "User"
   }
});
```

---

# Populate

Equivalent of JOIN

```js
Post.find()
.populate("user");
```

Output:

```json
{
   "title": "MongoDB",
   "user": {
      "_id": "123",
      "name": "John"
   }
}
```

---

# Embedding vs Referencing

| Feature | Embedding | Referencing |
|----------|-----------|------------|
| Read Speed | Fast | Slower |
| Update | Hard | Easy |
| Duplication | High | Low |
| Joins | No | Required |
| Complexity | Low | Medium |

---

# Interview Questions

### What is MongoDB?

A NoSQL document database that stores BSON documents.

### Difference between MongoDB and MySQL?

MongoDB is document-based.
MySQL is table-based.

### What is BSON?

Binary representation of JSON.

### What is ObjectId?

Unique identifier for documents.

### What is Indexing?

Data structure that improves query performance.

### What is Aggregation?

Framework for data processing and transformations.

### What is Populate?

Mongoose feature used to fetch referenced documents.

### Embedding vs Referencing?

Embedding stores related data together.
Referencing stores relation through ObjectId.

### Maximum document size?

16 MB.

### What are Collections?

Equivalent to SQL tables.

---

# Real Project Recommendation

## User Collection

```js
{
   _id,
   name,
   email
}
```

## Product Collection

```js
{
   _id,
   title,
   price
}
```

## Order Collection

```js
{
   _id,
   userId,
   productIds,
   totalAmount
}
```

Use Referencing:

User → Orders

Product → Orders

Avoid embedding thousands of records.

---

# Summary

MongoDB Core Topics:

✓ BSON

✓ Collections

✓ Documents

✓ CRUD Operations

✓ Query Operators

✓ Indexing

✓ Schema Design

✓ Embedding

✓ Referencing

✓ Populate

✓ Relationships

✓ Aggregation (Must Learn Next)

✓ Transactions (Advanced)

✓ Replication & Sharding (Advanced)
