# JavaScript Local Storage - Complete Notes

# What is Local Storage?

Local Storage is a browser feature that allows JavaScript to store data permanently inside the user's browser.

Think of Local Storage as:

```text
Browser Memory
      ↓
Stores Data
      ↓
Data Remains Even After Refresh
```

Unlike variables:

```javascript
let name = "John";
```

which disappear when the page refreshes,

Local Storage keeps data even after:

* Page Refresh
* Browser Restart
* System Restart

---

# Why Do We Need Local Storage?

Imagine a user logs into a website.

Without Local Storage:

```text
Login
   ↓
Refresh Page
   ↓
Logged Out
```

With Local Storage:

```text
Login
   ↓
Save User Data
   ↓
Refresh Page
   ↓
Still Logged In
```

---

# Real World Uses

Local Storage is commonly used for:

✅ Authentication Tokens

✅ User Preferences

✅ Theme Settings

✅ Shopping Cart Data

✅ Form Data

✅ Recently Viewed Products

✅ Language Selection

---

# How Local Storage Works

Local Storage stores data as:

```text
Key : Value
```

Example:

```text
name : John
```

Similar to:

```javascript
{
   name: "John"
}
```

---

# Local Storage Methods

There are four main methods:

```javascript
localStorage.setItem()
localStorage.getItem()
localStorage.removeItem()
localStorage.clear()
```

---

# 1. setItem()

Used to store data.

Syntax:

```javascript
localStorage.setItem(
   key,
   value
);
```

Example:

```javascript
localStorage.setItem(
   "name",
   "John"
);
```

Stored as:

```text
name : John
```

---

# Visual Representation

```text
Local Storage
-------------------
name → John
-------------------
```

---

# 2. getItem()

Used to retrieve data.

Syntax:

```javascript
localStorage.getItem(key);
```

Example:

```javascript
const name =
localStorage.getItem("name");

console.log(name);
```

Output:

```text
John
```

---

# What if Key Doesn't Exist?

```javascript
console.log(
   localStorage.getItem(
      "city"
   )
);
```

Output:

```javascript
null
```

---

# 3. removeItem()

Removes a specific key.

Example:

```javascript
localStorage.removeItem(
   "name"
);
```

Before:

```text
name : John
```

After:

```text
Empty
```

---

# 4. clear()

Removes all local storage data.

```javascript
localStorage.clear();
```

Before:

```text
name : John
city : Kochi
theme : dark
```

After:

```text
Empty
```

---

# Important Limitation

Local Storage stores only strings.

Example:

```javascript
localStorage.setItem(
   "age",
   25
);
```

Actually stores:

```text
"25"
```

String, not number.

---

# Verify

```javascript
const age =
localStorage.getItem("age");

console.log(typeof age);
```

Output:

```text
string
```

---

# Storing Objects

This is a very important interview topic.

Suppose:

```javascript
const user = {

   name: "John",

   age: 25

};
```

---

# Wrong Way

```javascript
localStorage.setItem(
   "user",
   user
);
```

Output:

```text
[object Object]
```

Because Local Storage only stores strings.

---

# Correct Way - JSON.stringify()

Convert object into JSON string.

```javascript
localStorage.setItem(

   "user",

   JSON.stringify(user)

);
```

Stored as:

```json
{
   "name":"John",
   "age":25
}
```

---

# Reading Objects

Retrieve:

```javascript
const data =
localStorage.getItem(
   "user"
);

console.log(data);
```

Output:

```text
{"name":"John","age":25}
```

Still a string.

---

# Convert Back Using JSON.parse()

```javascript
const user =
JSON.parse(

   localStorage.getItem(
      "user"
   )

);

console.log(user);
```

Output:

```javascript
{
   name:"John",
   age:25
}
```

Now it's a real JavaScript object.

---

# Visual Flow

```text
Object
   ↓
JSON.stringify()
   ↓
String
   ↓
Local Storage
   ↓
getItem()
   ↓
String
   ↓
JSON.parse()
   ↓
Object
```

---

# Complete Example

```javascript
const user = {

   name: "John",

   age: 25

};

localStorage.setItem(

   "user",

   JSON.stringify(user)

);

const storedUser =
JSON.parse(

   localStorage.getItem(
      "user"
   )

);

console.log(storedUser);
```

Output:

```javascript
{
   name:"John",
   age:25
}
```

---

# Storing Arrays

Arrays must also be converted.

Example:

```javascript
const users = [

   "John",

   "Alex",

   "Bob"

];
```

Store:

```javascript
localStorage.setItem(

   "users",

   JSON.stringify(users)

);
```

Read:

```javascript
const users =
JSON.parse(

   localStorage.getItem(
      "users"
   )

);
```

Output:

```javascript
[
   "John",
   "Alex",
   "Bob"
]
```

---

# Real Login Example

After Login:

```javascript
localStorage.setItem(

   "token",

   "abc123xyz"

);
```

Later:

```javascript
const token =
localStorage.getItem(
   "token"
);

console.log(token);
```

Output:

```text
abc123xyz
```

---

# Dark Mode Example

Save Theme:

```javascript
localStorage.setItem(
   "theme",
   "dark"
);
```

Load Theme:

```javascript
const theme =
localStorage.getItem(
   "theme"
);

if(theme === "dark"){

   document.body.classList
   .add("dark");

}
```

Even after refresh:

```text
Dark Mode Remains
```

---

# Shopping Cart Example

Store Cart:

```javascript
const cart = [

   {
      name:"Phone",
      price:20000
   }

];

localStorage.setItem(

   "cart",

   JSON.stringify(cart)

);
```

Retrieve Cart:

```javascript
const cartItems =
JSON.parse(

   localStorage.getItem(
      "cart"
   )

);
```

---

# Session Storage vs Local Storage

| Feature                      | Local Storage | Session Storage |
| ---------------------------- | ------------- | --------------- |
| Capacity                     | ~5MB          | ~5MB            |
| Persists After Refresh       | Yes           | Yes             |
| Persists After Browser Close | Yes           | No              |
| Accessible Across Tabs       | Yes           | No              |

---

# Local Storage vs Cookies

| Feature        | Local Storage | Cookies    |
| -------------- | ------------- | ---------- |
| Size           | ~5MB          | ~4KB       |
| Sent to Server | No            | Yes        |
| Expiration     | Manual        | Can Expire |
| Storage        | Browser       | Browser    |

---

# Security Considerations

Never store:

❌ Passwords

❌ Credit Card Information

❌ Sensitive Personal Data

Because:

```text
Anyone can inspect Local Storage
using browser developer tools.
```

---

# Common Interview Questions

## What is Local Storage?

A browser storage mechanism used to store key-value data permanently.

---

## Does Local Storage Persist After Refresh?

Yes.

---

## Does Local Storage Persist After Browser Close?

Yes.

---

## What Type of Data Can Local Storage Store?

Strings only.

---

## Why Use JSON.stringify()?

To convert objects into strings.

---

## Why Use JSON.parse()?

To convert stored strings back into JavaScript objects.

---

## Difference Between Local Storage and Session Storage?

Session Storage is cleared when browser tab closes.

Local Storage remains.

---

# Real MERN Stack Usage

Local Storage is used for:

### JWT Token Storage

```javascript
localStorage.setItem(
   "token",
   token
);
```

### User Information

```javascript
localStorage.setItem(
   "user",
   JSON.stringify(user)
);
```

### Theme Preferences

```javascript
localStorage.setItem(
   "theme",
   "dark"
);
```

### Shopping Cart

```javascript
localStorage.setItem(
   "cart",
   JSON.stringify(cart)
);
```

---

# Most Important Topics

✅ setItem()

✅ getItem()

✅ removeItem()

✅ clear()

✅ JSON.stringify()

✅ JSON.parse()

✅ Object Storage

✅ Array Storage

✅ Authentication Tokens

✅ Theme Persistence

✅ Local Storage vs Session Storage

✅ Security Best Practices

---

# Local Storage Formula

```text
JavaScript Object
        ↓
JSON.stringify()
        ↓
String
        ↓
Local Storage
        ↓
getItem()
        ↓
JSON.parse()
        ↓
JavaScript Object
```

or

```text
Save Data
      ↓
localStorage.setItem()
      ↓
Retrieve Data
      ↓
localStorage.getItem()
```

Local Storage is one of the most commonly used browser APIs and is essential for authentication, user preferences, shopping carts, React applications, and MERN Stack development.
