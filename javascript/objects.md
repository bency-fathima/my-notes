# JavaScript Objects - Complete Notes

# What is an Object?

An object is a collection of **key-value pairs** used to store related data and functionality together.

Think of an object as a real-world entity.

Example:

```text
Person
├── Name
├── Age
├── City
└── Phone
```

In JavaScript:

```javascript
const user = {
   name: "John",
   age: 25,
   city: "Kochi"
};
```

---

# Why Do We Need Objects?

Without Objects:

```javascript
const name = "John";
const age = 25;
const city = "Kochi";
```

Managing related information becomes difficult.

With Objects:

```javascript
const user = {
   name: "John",
   age: 25,
   city: "Kochi"
};
```

All related information is grouped together.

---

# Object Structure

```javascript
const user = {
   name: "John",
   age: 25
};
```

### Memory Representation

```text
user
│
├── name → John
│
└── age → 25
```

Here:

```text
Key     Value
----    -----
name    John
age     25
```

---

# Creating Objects

## Object Literal

Most common method.

```javascript
const user = {
   name: "John",
   age: 25
};
```

---

## Empty Object

```javascript
const user = {};
```

Later:

```javascript
user.name = "John";
user.age = 25;
```

---

# Accessing Object Properties

There are two ways.

---

## 1. Dot Notation

```javascript
const user = {
   name: "John",
   age: 25
};

console.log(user.name);
```

### Output

```text
John
```

---

## 2. Bracket Notation

```javascript
console.log(user["age"]);
```

### Output

```text
25
```

---

# Dot vs Bracket Notation

### Dot Notation

```javascript
user.name
```

Easy and commonly used.

---

### Bracket Notation

```javascript
user["name"]
```

Useful when key is dynamic.

Example:

```javascript
const key = "name";

console.log(user[key]);
```

Output:

```text
John
```

---

# Adding Properties

Objects are mutable.

Meaning properties can be added later.

```javascript
const user = {
   name: "John"
};

user.city = "Kochi";

console.log(user);
```

### Output

```javascript
{
   name: "John",
   city: "Kochi"
}
```

---

# Updating Properties

```javascript
const user = {
   name: "John"
};

user.name = "Alex";

console.log(user);
```

### Output

```javascript
{
   name: "Alex"
}
```

---

# Deleting Properties

```javascript
const user = {
   name: "John",
   age: 25
};

delete user.age;

console.log(user);
```

### Output

```javascript
{
   name: "John"
}
```

---

# Object Destructuring

Introduced in ES6.

Allows extracting properties easily.

---

## Without Destructuring

```javascript
const user = {
   name: "John",
   age: 25
};

const name = user.name;
const age = user.age;
```

---

## With Destructuring

```javascript
const user = {
   name: "John",
   age: 25
};

const { name, age } = user;

console.log(name);
```

### Output

```text
John
```

---

# Why Destructuring?

Makes code shorter and cleaner.

Especially useful in:

* React Components
* API Responses
* Node.js Projects

---

## Example

```javascript
const user = {
   name: "John",
   age: 25,
   city: "Kochi"
};

const { name, city } = user;

console.log(name);
console.log(city);
```

### Output

```text
John
Kochi
```

---

# Renaming Variables While Destructuring

```javascript
const user = {
   name: "John"
};

const { name: userName } = user;

console.log(userName);
```

### Output

```text
John
```

---

# Default Values in Destructuring

```javascript
const user = {
   name: "John"
};

const {
   name,
   city = "Calicut"
} = user;

console.log(city);
```

### Output

```text
Calicut
```

---

# Nested Objects

Objects can contain other objects.

```javascript
const user = {

   name: "John",

   address: {
      city: "Kochi",
      state: "Kerala"
   }
};
```

---

## Access Nested Properties

```javascript
console.log(
   user.address.city
);
```

### Output

```text
Kochi
```

---

# Optional Chaining

Prevents errors when nested data doesn't exist.

Without Optional Chaining:

```javascript
console.log(
   user.address.city
);
```

If address is missing:

```text
TypeError
```

---

With Optional Chaining:

```javascript
console.log(
   user?.address?.city
);
```

Output:

```text
undefined
```

No crash occurs.

---

# Object Methods

Objects can contain functions.

```javascript
const user = {

   name: "John",

   greet() {

      console.log(
         "Hello"
      );
   }
};

user.greet();
```

### Output

```text
Hello
```

---

# this Keyword in Objects

```javascript
const user = {

   name: "John",

   greet() {

      console.log(
         this.name
      );
   }
};

user.greet();
```

### Output

```text
John
```

`this` refers to the current object.

---

# Object.keys()

Returns all keys.

```javascript
const user = {
   name: "John",
   age: 25
};

console.log(
   Object.keys(user)
);
```

### Output

```javascript
["name","age"]
```

---

# Object.values()

Returns all values.

```javascript
console.log(
   Object.values(user)
);
```

### Output

```javascript
["John",25]
```

---

# Object.entries()

Returns key-value pairs.

```javascript
console.log(
   Object.entries(user)
);
```

### Output

```javascript
[
 ["name","John"],
 ["age",25]
]
```

---

# Spread Operator with Objects

Copy object.

```javascript
const user = {
   name: "John"
};

const updatedUser = {
   ...user,
   city: "Kochi"
};

console.log(updatedUser);
```

### Output

```javascript
{
   name:"John",
   city:"Kochi"
}
```

---

# Object Reference Behavior

Important Interview Question.

```javascript
const user1 = {
   name: "John"
};

const user2 = user1;

user2.name = "Alex";

console.log(user1.name);
```

### Output

```text
Alex
```

Why?

Both variables point to the same object in memory.

---

# Deep Copy

To avoid changing original object:

```javascript
const user2 = {
   ...user1
};
```

Now changes won't affect original.

---

# Real API Example

```javascript
const user = {

   id: 1,

   name: "John",

   email: "john@gmail.com"
};
```

Destructuring:

```javascript
const {
   name,
   email
} = user;

console.log(name);
console.log(email);
```

### Output

```text
John
john@gmail.com
```

---

# Real MERN Interview Questions

## Get Name

```javascript
const user = {
   name:"John"
};

console.log(user.name);
```

---

## Add Property

```javascript
user.city = "Kochi";
```

---

## Update Property

```javascript
user.name = "Alex";
```

---

## Delete Property

```javascript
delete user.city;
```

---

## Destructure Object

```javascript
const {
   name,
   age
} = user;
```

---

# Most Important Object Topics

✅ Object Creation

✅ Accessing Properties

✅ Dot vs Bracket Notation

✅ Adding Properties

✅ Updating Properties

✅ Deleting Properties

✅ Object Destructuring

✅ Nested Objects

✅ Optional Chaining

✅ Object Methods

✅ this Keyword

✅ Object.keys()

✅ Object.values()

✅ Object.entries()

✅ Spread Operator

✅ Object References

✅ Deep Copy

These concepts are heavily used in React, Node.js, Express.js, MongoDB documents, API responses, and MERN Stack applications.
