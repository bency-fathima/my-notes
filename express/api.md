# REST API Design, API Versioning & Swagger/OpenAPI - Complete Deep Dive

# Introduction

When companies hire Node.js/MERN developers, they don't just check if you can create APIs.

They check whether you can design APIs professionally.

A professional API should be:

```text
Easy To Understand

Easy To Maintain

Scalable

Secure

Consistent
```

This is where:

```text
REST API Design

API Versioning

API Documentation
```

become important.

---

# PART 1 - REST API DESIGN

# What is an API?

API stands for:

```text
Application Programming Interface
```

An API allows two applications to communicate.

Example:

```text
Frontend
    ↓
API
    ↓
Database
```

User clicks:

```text
Login
```

Frontend sends:

```text
POST /login
```

Backend responds:

```json
{
  "token":"abc123"
}
```

---

# What is REST?

REST stands for:

```text
Representational State Transfer
```

REST is not a technology.

REST is an architectural style.

It defines rules for building APIs.

---

# Why REST?

Without standards:

```text
/projectGetUsers

/getProductsData

/fetchCustomerDetails
```

Different developers use different patterns.

Confusing.

REST provides consistency.

---

# REST Architecture

```text
Client
   ↓
HTTP Request
   ↓
REST API
   ↓
Database
   ↓
HTTP Response
```

---

# REST Principles

A proper REST API follows:

```text
1. Client Server Architecture

2. Stateless Communication

3. Uniform Interface

4. Resource Based URLs

5. Cacheable Responses
```

---

# 1. Client Server Architecture

Frontend and backend remain separate.

```text
React
    ↓
REST API
    ↓
MongoDB
```

Frontend doesn't directly access database.

---

# 2. Statelessness

Most important REST principle.

Every request must contain everything needed.

Bad:

```text
Server remembers previous request.
```

Good:

```text
Every request is independent.
```

Example:

```http
GET /profile
Authorization: Bearer token
```

Token sent every time.

---

# Why Stateless?

Server can handle:

```text
100 Users
1000 Users
10000 Users
```

easily.

---

# Resource Based URLs

Resources are nouns.

Good:

```text
/users

/products

/orders
```

Bad:

```text
/getUsers

/createProduct

/deleteOrder
```

URLs should represent resources.

---

# REST URL Design

# Users Resource

Get all users:

```http
GET /users
```

---

Get single user:

```http
GET /users/10
```

---

Create user:

```http
POST /users
```

---

Update user:

```http
PUT /users/10
```

---

Delete user:

```http
DELETE /users/10
```

---

# HTTP Methods

REST heavily relies on HTTP methods.

---

# GET

Used to fetch data.

```http
GET /products
```

Response:

```json
[
 {
   "id":1,
   "name":"Laptop"
 }
]
```

---

# POST

Create resource.

```http
POST /products
```

Body:

```json
{
 "name":"Laptop"
}
```

---

# PUT

Replace resource.

```http
PUT /products/1
```

---

# PATCH

Partial update.

```http
PATCH /products/1
```

Update only specific fields.

---

# DELETE

Remove resource.

```http
DELETE /products/1
```

---

# REST Naming Conventions

Always use:

```text
Plural Resources
```

Good:

```text
/users

/products

/orders
```

Bad:

```text
/user

/product

/order
```

---

# Nested Resources

Example:

Orders belonging to user.

```http
GET /users/5/orders
```

Meaning:

```text
Orders Of User 5
```

---

# Filtering

Products:

```http
GET /products?category=laptop
```

---

# Sorting

```http
GET /products?sort=price
```

---

# Pagination

```http
GET /products?page=2&limit=10
```

---

# Standard Response Structure

Bad:

```json
[
 {
  "name":"Laptop"
 }
]
```

---

Better:

```json
{
  "success":true,
  "message":"Products fetched successfully",
  "data":[
    {
      "name":"Laptop"
    }
  ]
}
```

Consistent responses improve maintainability.

---

# Status Codes

Very important interview topic.

---

# 200 OK

```http
GET /products
```

Success.

---

# 201 Created

```http
POST /products
```

New resource created.

---

# 400 Bad Request

Client sent invalid data.

---

# 401 Unauthorized

Authentication required.

---

# 403 Forbidden

User authenticated but not allowed.

---

# 404 Not Found

Resource doesn't exist.

---

# 500 Internal Server Error

Unexpected server failure.

---

# Real REST API Example

```text
GET    /users

GET    /users/1

POST   /users

PUT    /users/1

DELETE /users/1
```

Professional and predictable.

---

# PART 2 - API VERSIONING

# Why API Versioning Exists

Suppose version 1 API:

```json
{
 "name":"John"
}
```

Later requirement changes:

```json
{
 "firstName":"John",
 "lastName":"Doe"
}
```

Frontend using old API breaks.

Problem.

---

# Solution

Versioning.

---

# What is API Versioning?

Versioning allows multiple API versions to exist.

```text
Version 1

Version 2

Version 3
```

simultaneously.

---

# Benefits

```text
Backward Compatibility

Safer Updates

Easy Migration
```

---

# URL Versioning

Most popular approach.

```http
/api/v1/users

/api/v2/users
```

---

# Example

Version 1:

```http
GET /api/v1/users
```

Response:

```json
{
 "name":"John"
}
```

---

Version 2:

```http
GET /api/v2/users
```

Response:

```json
{
 "firstName":"John",
 "lastName":"Doe"
}
```

Both work.

---

# Express Versioning Example

```js
app.use(
 "/api/v1",
 v1Routes
);

app.use(
 "/api/v2",
 v2Routes
);
```

---

# Header Versioning

Version passed in header.

```http
GET /users

API-Version: 2
```

---

# Query Versioning

```http
GET /users?version=2
```

Not recommended.

---

# Best Practice

Use:

```text
URL Versioning
```

Most common in industry.

---

# When To Create New Version?

Create new version when:

```text
Response Structure Changes

Request Structure Changes

Breaking Changes Introduced
```

---

# Don't Create New Version For

```text
Bug Fixes

Internal Improvements

Performance Enhancements
```

---

# Versioning Strategy

```text
v1 → Stable

v2 → New Features

v3 → Major Redesign
```

---

# PART 3 - API DOCUMENTATION

# Why Documentation Matters

Imagine frontend developer asks:

```text
What APIs exist?

What data should I send?

What response comes back?
```

Without documentation:

```text
Need To Ask Backend Developer Every Time
```

Productivity decreases.

---

# What is API Documentation?

Documentation describes:

```text
Endpoints

Request Body

Headers

Responses

Authentication

Errors
```

---

# Example

```http
POST /users
```

Documentation should explain:

```text
Purpose

Request Format

Response Format

Status Codes
```

---

# What is Swagger?

Swagger is the most popular API documentation tool.

Swagger automatically generates documentation UI.

---

# Swagger Flow

```text
Code
  ↓
Swagger Config
  ↓
Interactive Documentation
```

---

# Benefits

```text
Interactive

Easy To Test

Auto Generated

Professional
```

---

# OpenAPI

Swagger follows:

```text
OpenAPI Specification
```

(OAS)

OpenAPI is the standard.

Swagger is the tool.

---

# Install Swagger

```bash
npm install swagger-ui-express
npm install yamljs
```

---

# Basic Setup

```js
const swaggerUi =
require("swagger-ui-express");

app.use(
 "/api-docs",
 swaggerUi.serve,
 swaggerUi.setup(swaggerDocument)
);
```

---

# Access Documentation

```text
http://localhost:3000/api-docs
```

---

# Swagger UI Features

Shows:

```text
Available Endpoints

Request Parameters

Response Examples

Authentication

Try It Out Button
```

---

# Example Endpoint Documentation

```yaml
/users:
  get:
    summary: Get all users

    responses:
      200:
        description:
          Users retrieved successfully
```

---

# Swagger Response Example

```json
{
  "success":true,
  "data":[
    {
      "id":1,
      "name":"John"
    }
  ]
}
```

---

# Authentication Documentation

Swagger can document JWT APIs.

Example:

```http
Authorization:
Bearer <token>
```

Developers can test secured APIs directly.

---

# Real Project Structure

```text
project

├── routes

├── controllers

├── models

├── middleware

├── swagger

│   └── swagger.yaml

├── app.js
```

---

# REST API Best Practices

### Use Nouns

Good:

```http
/users
```

Bad:

```http
/getUsers
```

---

### Use Proper HTTP Methods

```http
GET

POST

PUT

PATCH

DELETE
```

---

### Use Status Codes Correctly

Never return:

```http
200
```

for every response.

---

### Version APIs

```http
/api/v1

/api/v2
```

---

### Document APIs

Use Swagger/OpenAPI.

---

### Consistent Responses

Always follow same structure.

---

# Real Interview Questions

### What is REST?

Architectural style for designing APIs.

---

### Difference Between PUT and PATCH?

PUT:

```text
Replace Entire Resource
```

PATCH:

```text
Update Specific Fields
```

---

### Why API Versioning?

Prevent breaking existing clients.

---

### Most Common Versioning Strategy?

```text
URL Versioning
```

Example:

```http
/api/v1/users
```

---

### What is Swagger?

Tool for generating API documentation.

---

### Swagger vs OpenAPI?

OpenAPI:

```text
Specification
```

Swagger:

```text
Implementation Tool
```

---

### Why API Documentation?

Makes APIs easier to understand, test, and maintain.

---

# Final Summary

Professional API Development Topics:

✔ REST API Design

✔ Resource-Based URLs

✔ HTTP Methods

✔ Status Codes

✔ Filtering

✔ Sorting

✔ Pagination

✔ API Versioning

✔ URL Versioning

✔ Backward Compatibility

✔ Swagger

✔ OpenAPI

✔ API Documentation

✔ JWT Documentation

✔ Interactive API Testing

These concepts are essential for building production-grade Express.js APIs and are frequently asked in Node.js/MERN interviews from 2–6 years experience level.
