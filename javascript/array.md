# JavaScript Arrays - Complete Notes

# What is an Array?

An array is a special object that stores multiple values in a single variable.

## Without Array

```javascript
const student1 = "John";
const student2 = "Alex";
const student3 = "Bob";
```

This becomes difficult to manage.

## With Array

```javascript
const students = ["John", "Alex", "Bob"];
```

Now all related data is stored together.

---

# Array Structure

```javascript
const fruits = ["Apple", "Orange", "Mango"];
```

## Memory Representation

```text
Index    Value
-----    -----
0        Apple
1        Orange
2        Mango
```

### Access Values

```javascript
console.log(fruits[0]);
```

### Output

```text
Apple
```

---

# Why Arrays are Important?

Most API responses look like:

```javascript
const users = [
   {
       id: 1,
       name: "John"
   },
   {
       id: 2,
       name: "Alex"
   }
];
```

## Example from Backend

```javascript
app.get("/users", async(req,res)=>{

   const users = await User.find();

   res.json(users);

});
```

Here `users` is an array.

---

# Common Array Methods

---

# 1. map()

One of the most important methods in React.

## Purpose

Transform every element and return a new array.

```text
Old Array
↓
Transformation
↓
New Array
```

---

## Example 1

```javascript
const nums = [1,2,3];

const result = nums.map(num => num * 2);

console.log(result);
```

### Output

```javascript
[2,4,6]
```

---

## How map Works

### Iteration 1

```javascript
1 * 2 = 2
```

### Iteration 2

```javascript
2 * 2 = 4
```

### Iteration 3

```javascript
3 * 2 = 6
```

### New Array

```javascript
[2,4,6]
```

---

## Original Array

```javascript
console.log(nums);
```

### Output

```javascript
[1,2,3]
```

`map()` never modifies the original array.

---

## Visual Representation

```text
[1,2,3]
   ↓
 num * 2
   ↓
[2,4,6]
```

---

## Example 2

```javascript
const users = [
   {name:"John"},
   {name:"Alex"}
];

const names = users.map(user => user.name);

console.log(names);
```

### Output

```javascript
["John","Alex"]
```

---

## Real React Example

```jsx
users.map(user => (
   <h1>{user.name}</h1>
))
```

Used for rendering lists.

---

## map() Syntax

```javascript
array.map((element,index,array)=>{
   return transformedValue;
});
```

### Example

```javascript
const nums = [10,20,30];

nums.map((value,index)=>{

   console.log(value,index);

});
```

### Output

```text
10 0
20 1
30 2
```

---

## When to Use map()

Use when:

✅ Need new array

✅ Need transformation

### Examples

* Add GST
* Convert names to uppercase
* Render React Components

---

# 2. filter()

## Purpose

Return only matching elements.

Think:

```text
Filter = Keep only what satisfies condition
```

---

## Example

```javascript
const users = [
   {name:"John", age:20},
   {name:"Alex", age:30},
   {name:"Bob", age:35}
];

const result = users.filter(
   user => user.age > 25
);

console.log(result);
```

### Output

```javascript
[
 {name:"Alex", age:30},
 {name:"Bob", age:35}
]
```

---

## How filter Works

### User 1

```javascript
20 > 25
```

False → Removed

---

### User 2

```javascript
30 > 25
```

True → Added

---

### User 3

```javascript
35 > 25
```

True → Added

---

## Result

```javascript
[
 Alex,
 Bob
]
```

---

## Visual Representation

```text
Users
↓

John 20 ❌

Alex 30 ✅

Bob 35 ✅

↓

Result
```

---

## Example - Even Numbers

```javascript
const nums = [1,2,3,4,5,6];

const even = nums.filter(
   num => num % 2 === 0
);

console.log(even);
```

### Output

```javascript
[2,4,6]
```

---

# map vs filter

## map()

Transforms

```javascript
[1,2,3]
```

↓

```javascript
[2,4,6]
```

---

## filter()

Removes items

```javascript
[1,2,3,4]
```

↓

```javascript
[2,4]
```

---

# 3. find()

## Purpose

Return first matching item.

---

## Example

```javascript
const users = [
   {name:"John", age:20},
   {name:"Alex", age:30},
   {name:"Bob", age:35}
];

const user = users.find(
   user => user.age > 25
);

console.log(user);
```

### Output

```javascript
{
 name:"Alex",
 age:30
}
```

---

## Why only Alex?

Because `find()` stops after the first match.

---

## Visual

```text
John ❌

Alex ✅
STOP
```

Bob is never checked after a match is found.

---

# find vs filter

## find()

Returns one object

```javascript
{
 name:"Alex"
}
```

---

## filter()

Returns an array

```javascript
[
 {name:"Alex"},
 {name:"Bob"}
]
```

---

## Example - Find Product

```javascript
const products = [
  {id:1,name:"Phone"},
  {id:2,name:"Laptop"}
];

const product =
products.find(
   p => p.id === 2
);

console.log(product);
```

### Output

```javascript
{
 id:2,
 name:"Laptop"
}
```

Very common in React applications.

---

# 4. reduce()

Most powerful array method.

---

## Purpose

Reduce array into a single value.

### Examples

* Sum
* Total Price
* Average
* Grouping
* Counting

---

## Example

```javascript
const nums = [1,2,3,4];

const total = nums.reduce(
   (acc,current)=>{
       return acc + current;
   },
   0
);

console.log(total);
```

### Output

```javascript
10
```

---

# Understanding reduce() Step-by-Step

### Array

```javascript
[1,2,3,4]
```

### Initial Value

```javascript
0
```

### acc

```text
Accumulator
```

### current

```text
Current Element
```

---

## Iteration 1

```javascript
acc = 0
current = 1

0 + 1 = 1
```

New acc:

```javascript
1
```

---

## Iteration 2

```javascript
1 + 2 = 3
```

New acc:

```javascript
3
```

---

## Iteration 3

```javascript
3 + 3 = 6
```

New acc:

```javascript
6
```

---

## Iteration 4

```javascript
6 + 4 = 10
```

New acc:

```javascript
10
```

---

## Final Result

```javascript
10
```

---

## Visual Representation

```text
0
↓
0+1 = 1
↓
1+2 = 3
↓
3+3 = 6
↓
6+4 = 10
```

---

# Real World Example

## Shopping Cart Total

```javascript
const cart = [

 {name:"Phone",price:1000},

 {name:"Laptop",price:50000},

 {name:"Watch",price:2000}

];
```

### Calculate Total

```javascript
const total = cart.reduce(

   (sum,item)=>{

       return sum + item.price;

   },

   0
);

console.log(total);
```

### Output

```javascript
53000
```

---

# Counting Occurrences

```javascript
const fruits = [
   "apple",
   "banana",
   "apple",
   "orange",
   "apple"
];
```

Using reduce:

```javascript
const count = fruits.reduce(

   (acc,fruit)=>{

       acc[fruit] = (acc[fruit] || 0) + 1;

       return acc;

   },

   {}
);

console.log(count);
```

### Output

```javascript
{
 apple:3,
 banana:1,
 orange:1
}
```

---

# Most Important Array Methods for Interviews

## forEach()

Loop through array.

```javascript
nums.forEach(num=>{
   console.log(num);
});
```

### Returns

```javascript
undefined
```

---

## map()

Transforms and returns a new array.

---

## filter()

Returns matching items.

---

## find()

Returns first matching item.

---

## reduce()

Returns a single value.

---

# Quick Comparison

| Method  | Returns        |
| ------- | -------------- |
| forEach | Nothing        |
| map     | New Array      |
| filter  | Filtered Array |
| find    | First Match    |
| reduce  | Single Value   |

---

# Real MERN Interview Question

### Given

```javascript
const users = [
 {name:"John",salary:1000},
 {name:"Alex",salary:2000},
 {name:"Bob",salary:3000}
];
```

---

## Get All Names

```javascript
const names =
users.map(user=>user.name);
```

### Output

```javascript
["John","Alex","Bob"]
```

---

## Salary Greater Than 1500

```javascript
const result =
users.filter(
   user=>user.salary > 1500
);
```

---

## Find Bob

```javascript
const bob =
users.find(
   user=>user.name === "Bob"
);
```

---

## Total Salary

```javascript
const total =
users.reduce(
   (sum,user)=>sum + user.salary,
   0
);
```

### Output

```javascript
6000
```

---

# Summary

The four most important array methods in modern JavaScript are:

* map()
* filter()
* find()
* reduce()

These methods are used extensively in:

* React Components
* API Data Processing
* Dashboards
* Search Functionality
* Tables
* Reports
* MERN Stack Applications

Mastering these methods is essential for every JavaScript and MERN Stack developer.
