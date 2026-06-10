# MongoDB Aggregation Pipeline - Complete Guide

# What is Aggregation?

Aggregation is a framework in MongoDB used to:

* Process documents
* Transform data
* Filter data
* Group data
* Perform calculations
* Join collections
* Generate reports

Think of Aggregation as MongoDB's equivalent of:

```sql
SELECT
GROUP BY
HAVING
SUM
COUNT
AVG
JOIN
```

in SQL.

---

# Why Aggregation?

Suppose we have Orders Collection:

```json
[
  {
    "customer": "John",
    "amount": 500
  },
  {
    "customer": "John",
    "amount": 1000
  },
  {
    "customer": "David",
    "amount": 700
  }
]
```

Questions:

* Total sales?
* Average order value?
* Customer-wise sales?
* Monthly revenue?
* Top customers?

Aggregation solves all these.

---

# What is Aggregation Pipeline?

A Pipeline is a series of stages.

Output of one stage becomes input for next stage.

```text
Documents
    |
 $match
    |
 $group
    |
 $project
    |
 Result
```

Syntax:

```js
db.collection.aggregate([
   { Stage1 },
   { Stage2 },
   { Stage3 }
])
```

Example:

```js
db.orders.aggregate([
  {
    $match: {
      amount: {
        $gt: 500
      }
    }
  }
])
```

---

# Sample Dataset

Orders Collection

```json
[
  {
    "_id": 1,
    "customer": "John",
    "city": "Delhi",
    "amount": 500,
    "status": "completed"
  },
  {
    "_id": 2,
    "customer": "David",
    "city": "Mumbai",
    "amount": 1000,
    "status": "completed"
  },
  {
    "_id": 3,
    "customer": "John",
    "city": "Delhi",
    "amount": 700,
    "status": "pending"
  }
]
```

---

# 1. $match

Used to filter documents.

Equivalent SQL:

```sql
WHERE
```

Syntax:

```js
{
  $match: {
    field: value
  }
}
```

Example:

```js
db.orders.aggregate([
  {
    $match: {
      city: "Delhi"
    }
  }
])
```

Output:

```json
[
  {
    "_id": 1,
    "customer": "John"
  },
  {
    "_id": 3,
    "customer": "John"
  }
]
```

---

## Multiple Conditions

```js
db.orders.aggregate([
  {
    $match: {
      city: "Delhi",
      amount: {
        $gt: 600
      }
    }
  }
])
```

Output:

```json
[
  {
    "_id": 3,
    "amount": 700
  }
]
```

---

## Why Use $match First?

Bad:

```text
Group → Filter
```

Good:

```text
Filter → Group
```

Because fewer documents move through pipeline.

Improves performance.

---

# 2. $project

Used to:

* Select fields
* Remove fields
* Rename fields
* Create new fields

Equivalent SQL:

```sql
SELECT
```

---

## Select Fields

```js
db.orders.aggregate([
  {
    $project: {
      customer: 1,
      amount: 1
    }
  }
])
```

Output:

```json
{
  "customer": "John",
  "amount": 500
}
```

---

## Hide _id

```js
db.orders.aggregate([
  {
    $project: {
      _id: 0,
      customer: 1
    }
  }
])
```

Output:

```json
{
  "customer": "John"
}
```

---

## Rename Fields

```js
db.orders.aggregate([
  {
    $project: {
      customerName: "$customer",
      orderAmount: "$amount"
    }
  }
])
```

Output:

```json
{
  "customerName": "John",
  "orderAmount": 500
}
```

---

## Create New Fields

```js
db.orders.aggregate([
  {
    $project: {
      customer: 1,
      tax: {
        $multiply: ["$amount", 0.18]
      }
    }
  }
])
```

Output:

```json
{
  "customer": "John",
  "tax": 90
}
```

---

# 3. $group

Most important aggregation stage.

Used to:

* Count
* Sum
* Average
* Minimum
* Maximum

Equivalent SQL:

```sql
GROUP BY
```

---

## Count Documents

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$city",
      totalOrders: {
        $sum: 1
      }
    }
  }
])
```

Output:

```json
[
  {
    "_id": "Delhi",
    "totalOrders": 2
  },
  {
    "_id": "Mumbai",
    "totalOrders": 1
  }
]
```

---

## Total Sales Per City

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$city",
      totalSales: {
        $sum: "$amount"
      }
    }
  }
])
```

Output:

```json
[
  {
    "_id": "Delhi",
    "totalSales": 1200
  },
  {
    "_id": "Mumbai",
    "totalSales": 1000
  }
]
```

---

## Average Sales

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$city",
      avgSales: {
        $avg: "$amount"
      }
    }
  }
])
```

---

## Maximum Sales

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$city",
      maxSale: {
        $max: "$amount"
      }
    }
  }
])
```

---

## Minimum Sales

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$city",
      minSale: {
        $min: "$amount"
      }
    }
  }
])
```

---

# Common Group Operators

| Operator  | Purpose               |
| --------- | --------------------- |
| $sum      | Total                 |
| $avg      | Average               |
| $min      | Minimum               |
| $max      | Maximum               |
| $first    | First value           |
| $last     | Last value            |
| $push     | Store values in array |
| $addToSet | Unique values         |

---

# $push Example

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$city",
      customers: {
        $push: "$customer"
      }
    }
  }
])
```

Output:

```json
{
  "_id": "Delhi",
  "customers": [
    "John",
    "John"
  ]
}
```

---

# $addToSet Example

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$city",
      customers: {
        $addToSet: "$customer"
      }
    }
  }
])
```

Output:

```json
{
  "_id": "Delhi",
  "customers": [
    "John"
  ]
}
```

Duplicates removed.

---

# 4. $unwind

Used to break array elements into separate documents.

Before:

```json
{
  "name": "John",
  "skills": [
    "React",
    "Node",
    "MongoDB"
  ]
}
```

---

## Unwind

```js
db.users.aggregate([
  {
    $unwind: "$skills"
  }
])
```

Output:

```json
{
  "name": "John",
  "skills": "React"
}
```

```json
{
  "name": "John",
  "skills": "Node"
}
```

```json
{
  "name": "John",
  "skills": "MongoDB"
}
```

---

# Why Use $unwind?

Suppose:

```json
{
  "skills": [
    "React",
    "Node"
  ]
}
```

Need:

* Count each skill
* Group by skill
* Generate reports

Must unwind first.

---

# 5. $lookup

Used to join collections.

Equivalent SQL:

```sql
JOIN
```

---

# Collections

Users

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
  "userId": 1,
  "amount": 500
}
```

---

# Lookup Example

```js
db.users.aggregate([
  {
    $lookup: {
      from: "orders",
      localField: "_id",
      foreignField: "userId",
      as: "orders"
    }
  }
])
```

Output:

```json
{
  "_id": 1,
  "name": "John",
  "orders": [
    {
      "_id": 101,
      "amount": 500
    }
  ]
}
```

---

# Understanding Lookup

```js
{
  $lookup: {
     from: "orders",
     localField: "_id",
     foreignField: "userId",
     as: "orders"
  }
}
```

| Property     | Meaning                  |
| ------------ | ------------------------ |
| from         | Collection to join       |
| localField   | Current collection field |
| foreignField | Joined collection field  |
| as           | Result array name        |

---

# Lookup + Unwind

Without unwind:

```json
{
  "orders":[
    {...},
    {...}
  ]
}
```

After unwind:

```json
{
  "orders": {...}
}
```

Example:

```js
db.users.aggregate([
  {
    $lookup:{
      from:"orders",
      localField:"_id",
      foreignField:"userId",
      as:"orders"
    }
  },
  {
    $unwind:"$orders"
  }
])
```

Useful for reporting.

---

# Real World Example

E-Commerce Dashboard

Need:

* Customer Name
* Total Orders
* Total Revenue

Pipeline:

```js
db.orders.aggregate([
  {
    $group:{
      _id:"$customerId",
      totalOrders:{
        $sum:1
      },
      revenue:{
        $sum:"$amount"
      }
    }
  }
])
```

Output:

```json
{
  "_id":1,
  "totalOrders":10,
  "revenue":25000
}
```

---

# Complete Aggregation Example

```js
db.orders.aggregate([
  {
    $match:{
      status:"completed"
    }
  },
  {
    $group:{
      _id:"$city",
      totalRevenue:{
        $sum:"$amount"
      }
    }
  },
  {
    $project:{
      city:"$_id",
      revenue:"$totalRevenue",
      _id:0
    }
  }
])
```

---

# Interview Questions

### What is Aggregation Pipeline?

Framework used for processing and transforming documents through stages.

### What is $match?

Filters documents.

Equivalent to SQL WHERE.

### What is $group?

Groups documents and performs calculations.

Equivalent to SQL GROUP BY.

### What is $project?

Selects, removes, or creates fields.

Equivalent to SQL SELECT.

### What is $lookup?

Performs collection joins.

Equivalent to SQL JOIN.

### What is $unwind?

Converts array elements into separate documents.

### Difference between $push and $addToSet?

$push allows duplicates.

$addToSet removes duplicates.

### Why place $match first?

Improves performance by reducing documents early.

---

# Aggregation Learning Order

1. $match
2. $project
3. $group
4. $sort
5. $limit
6. $skip
7. $unwind
8. $lookup
9. $facet
10. $bucket
11. $graphLookup
12. Aggregation Optimization

These stages cover almost 90% of aggregation questions asked in MERN Stack interviews.
