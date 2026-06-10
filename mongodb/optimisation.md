# MongoDB Indexing - Complete Deep Dive

# What is Indexing?

An index is a special data structure that MongoDB uses to quickly locate documents without scanning the entire collection.

Think of an index like a book's table of contents.

Without Index:

```text
Book
 ↓
Read every page
 ↓
Find topic
```

With Index:

```text
Book
 ↓
Table of Contents
 ↓
Jump directly to page
```

MongoDB indexes work similarly.

---

# Why Do We Need Indexes?

Suppose a collection contains:

```text
1,000,000 Users
```

Query:

```js
db.users.find({
  email: "john@gmail.com"
})
```

Without an index:

```text
MongoDB checks every document
```

This is called:

```text
Collection Scan (COLLSCAN)
```

Time Complexity:

```text
O(n)
```

---

With an index:

```text
MongoDB directly jumps to matching record
```

Time Complexity:

```text
O(log n)
```

Much faster.

---

# Internal Working of Indexes

MongoDB primarily uses:

```text
B-Tree Data Structure
```

Example:

Users Collection

```json
{
  "name":"John"
}
```

```json
{
  "name":"David"
}
```

```json
{
  "name":"Alex"
}
```

Index:

```text
Alex
David
John
```

Stored in sorted order.

Searching becomes very efficient.

---

# Default Index

Every collection automatically gets:

```js
_id
```

index.

Example:

```json
{
   "_id": ObjectId("665abc"),
   "name":"John"
}
```

MongoDB automatically creates:

```js
{
   _id:1
}
```

index.

Check:

```js
db.users.getIndexes()
```

Output:

```json
[
 {
   "key": {
      "_id":1
   }
 }
]
```

---

# Single Field Index

Most common type.

Create index:

```js
db.users.createIndex({
   email:1
})
```

Output:

```json
{
  "createdCollectionAutomatically":false,
  "numIndexesBefore":1,
  "numIndexesAfter":2
}
```

---

# Ascending vs Descending Index

Ascending:

```js
db.users.createIndex({
   age:1
})
```

Descending:

```js
db.users.createIndex({
   age:-1
})
```

---

## What Does 1 and -1 Mean?

```js
1
```

Ascending order:

```text
10
20
30
40
```

---

```js
-1
```

Descending order:

```text
40
30
20
10
```

---

For equality searches:

```js
find({ age:25 })
```

both work equally.

Important mainly for sorting.

---

# Checking Existing Indexes

```js
db.users.getIndexes()
```

Output:

```json
[
 {
   "key":{
      "_id":1
   }
 },
 {
   "key":{
      "email":1
   }
 }
]
```

---

# Dropping Indexes

Delete one index:

```js
db.users.dropIndex("email_1")
```

Delete all indexes:

```js
db.users.dropIndexes()
```

---

# Compound Index

One of the most important interview topics.

A Compound Index contains multiple fields.

Example:

```js
{
   name:1,
   age:1
}
```

---

# Why Compound Index?

Suppose query:

```js
db.users.find({
   name:"John",
   age:25
})
```

Without compound index:

MongoDB may:

```text
Scan name index
+
Filter age
```

or

```text
Collection Scan
```

---

Create compound index:

```js
db.users.createIndex({
   name:1,
   age:1
})
```

Now both fields are indexed together.

---

# Compound Index Structure

Index:

```js
{
   name:1,
   age:1
}
```

Stored like:

```text
Alex   20
Alex   25
David  30
John   22
John   25
```

---

# Prefix Rule (Very Important)

Compound index:

```js
{
   name:1,
   age:1,
   city:1
}
```

MongoDB can use index for:

```js
{
   name:"John"
}
```

✅ Works

---

```js
{
   name:"John",
   age:25
}
```

✅ Works

---

```js
{
   name:"John",
   age:25,
   city:"Delhi"
}
```

✅ Works

---

```js
{
   age:25
}
```

❌ Cannot fully utilize index

---

```js
{
   city:"Delhi"
}
```

❌ Cannot fully utilize index

---

This is called:

```text
Compound Index Prefix Rule
```

Frequently asked in interviews.

---

# Compound Index with Sorting

Query:

```js
db.users.find({
   city:"Delhi"
})
.sort({
   age:-1
})
```

Create:

```js
db.users.createIndex({
   city:1,
   age:-1
})
```

Now:

```text
Filtering + Sorting
```

both use index.

---

# Unique Index

Prevents duplicate values.

Example:

```js
db.users.createIndex(
{
   email:1
},
{
   unique:true
}
)
```

Now:

```json
{
  "email":"john@gmail.com"
}
```

cannot be inserted twice.

---

# TTL Index (Time To Live)

Automatically removes documents after a specific time.

Common interview question.

---

# Why TTL Index?

Use cases:

* OTP Expiry
* Session Expiry
* Cache Data
* Login Tokens
* Verification Links

---

# Example

Collection:

```json
{
   "otp":"123456",
   "createdAt": ISODate("2025-01-01")
}
```

Create TTL index:

```js
db.otps.createIndex(
{
   createdAt:1
},
{
   expireAfterSeconds:300
}
)
```

Meaning:

```text
5 Minutes
```

After 5 minutes:

```text
Document automatically deleted
```

---

# Real World OTP Example

```json
{
   "email":"john@gmail.com",
   "otp":"5678",
   "createdAt":new Date()
}
```

Index:

```js
db.otps.createIndex(
{
   createdAt:1
},
{
   expireAfterSeconds:600
}
)
```

OTP automatically removed after:

```text
10 Minutes
```

---

# TTL with Exact Expiry Time

Document:

```json
{
   "expireAt":
   ISODate("2025-12-31T23:59:59")
}
```

Index:

```js
db.events.createIndex(
{
   expireAt:1
},
{
   expireAfterSeconds:0
}
)
```

Deleted exactly at:

```text
2025-12-31 23:59:59
```

---

# Limitations of TTL Index

TTL works only on:

```text
Date Fields
```

Not:

```text
String
Number
Boolean
```

---

Cannot be:

```text
Compound TTL Index
```

Only single-field.

---

# Explain Plans

One of the most important topics for performance tuning.

Used to understand:

```text
How MongoDB executes query
```

---

# Why Explain?

Suppose query is slow.

Need to know:

* Is index used?
* Is collection scan happening?
* How many documents scanned?
* How many documents returned?

Explain answers these questions.

---

# Syntax

```js
db.users.find({
   email:"john@gmail.com"
}).explain()
```

---

# Explain Modes

## queryPlanner

Shows plan only.

```js
db.users.find().explain("queryPlanner")
```

---

## executionStats

Most commonly used.

```js
db.users.find().explain("executionStats")
```

Shows:

* Documents scanned
* Documents returned
* Execution time

---

## allPlansExecution

Shows all possible plans.

```js
db.users.find().explain("allPlansExecution")
```

Used for deep debugging.

---

# Important Explain Terms

---

## COLLSCAN

Collection Scan

```text
Bad
```

MongoDB scans every document.

Example:

```json
"stage":"COLLSCAN"
```

Means:

```text
No index used
```

---

## IXSCAN

Index Scan

```text
Good
```

Example:

```json
"stage":"IXSCAN"
```

Means:

```text
Index used
```

---

# Example

Without Index

```js
db.users.find({
   email:"john@gmail.com"
}).explain("executionStats")
```

Output:

```json
{
  "stage":"COLLSCAN"
}
```

---

After Creating Index

```js
db.users.createIndex({
   email:1
})
```

Run again:

```json
{
  "stage":"IXSCAN"
}
```

Much faster.

---

# Key Metrics

## totalDocsExamined

How many documents MongoDB checked.

Example:

```json
{
  "totalDocsExamined":100000
}
```

Bad.

---

Better:

```json
{
  "totalDocsExamined":1
}
```

Excellent.

---

## nReturned

Documents returned.

Example:

```json
{
  "nReturned":1
}
```

---

## executionTimeMillis

Execution time.

Example:

```json
{
   "executionTimeMillis":2
}
```

Query took:

```text
2 milliseconds
```

---

# Real Interview Example

Query:

```js
db.orders.find({
   customerId:101
})
```

Slow.

Solution:

```js
db.orders.createIndex({
   customerId:1
})
```

Verify:

```js
db.orders.find({
   customerId:101
}).explain("executionStats")
```

Check:

```json
{
  "stage":"IXSCAN"
}
```

Performance improved.

---

# Best Practices

## Create Indexes On

Frequently searched fields.

```js
email
username
customerId
orderId
```

---

## Avoid Too Many Indexes

Bad:

```text
Every index consumes RAM
Every insert updates indexes
```

More indexes:

```text
Faster Reads
Slower Writes
```

Balance is important.

---

## Use Compound Indexes

Instead of:

```js
name
age
city
```

separate indexes.

Use:

```js
{
   name:1,
   age:1,
   city:1
}
```

when queries frequently use these fields together.

---

# Interview Questions

### What is Indexing?

Data structure that improves query performance.

### What Data Structure Does MongoDB Use?

B-Tree.

### What is Compound Index?

Index on multiple fields.

### What is Prefix Rule?

Compound index works from left to right.

### What is TTL Index?

Automatically deletes expired documents.

### Can TTL be Compound?

No.

### What is Explain?

Tool used to analyze query execution.

### Difference Between COLLSCAN and IXSCAN?

COLLSCAN → Full collection scan.

IXSCAN → Uses index.

### Why Not Create Indexes On Every Field?

Indexes consume memory and slow writes.

---

# Summary

MongoDB Performance Topics:

✔ Single Field Index

✔ Compound Index

✔ Unique Index

✔ TTL Index

✔ Explain Plans

✔ COLLSCAN

✔ IXSCAN

✔ Query Optimization

✔ Prefix Rule

✔ Execution Statistics

These topics are among the most frequently asked MongoDB interview questions for 2–4 year MERN Stack developers.
