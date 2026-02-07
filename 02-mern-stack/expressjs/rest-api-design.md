# REST API Design

## Overview

REST (Representational State Transfer) is an architectural style for designing networked applications.

---

## REST Principles

1. **Stateless**: Each request contains all info needed
2. **Client-Server**: Separation of concerns
3. **Cacheable**: Responses can be cached
4. **Uniform Interface**: Consistent resource identification
5. **Layered System**: Client doesn't know if connected directly to server

---

## HTTP Methods

| Method | Purpose | Idempotent | Safe |
|--------|---------|------------|------|
| GET | Read resource | Yes | Yes |
| POST | Create resource | No | No |
| PUT | Replace resource | Yes | No |
| PATCH | Partial update | No | No |
| DELETE | Remove resource | Yes | No |

---

## URL Structure Best Practices

```
✅ Good:
GET    /users              # Get all users
GET    /users/123          # Get user 123
POST   /users              # Create user
PUT    /users/123          # Update user 123
DELETE /users/123          # Delete user 123
GET    /users/123/posts    # Get posts by user 123

❌ Avoid:
GET    /getUsers
GET    /user/123/delete
POST   /createUser
GET    /users/123/getPosts
```

### Naming Conventions

- Use **nouns**, not verbs
- Use **plural** names (`/users`, not `/user`)
- Use **kebab-case** for multi-word (`/user-profiles`)
- Use **query parameters** for filtering

```
GET /users?status=active&sort=name&limit=10&page=2
```

---

## Status Codes

### Success (2xx)

| Code | Meaning | Use Case |
|------|---------|----------|
| 200 | OK | Successful GET, PUT, PATCH |
| 201 | Created | Successful POST (resource created) |
| 204 | No Content | Successful DELETE |

### Client Error (4xx)

| Code | Meaning | Use Case |
|------|---------|----------|
| 400 | Bad Request | Invalid request/validation error |
| 401 | Unauthorized | Not authenticated |
| 403 | Forbidden | Authenticated but not authorized |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Resource conflict (duplicate) |
| 422 | Unprocessable Entity | Validation failed |

### Server Error (5xx)

| Code | Meaning |
|------|---------|
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |

---

## Request/Response Format

### Request

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json
Authorization: Bearer <token>

{
    "name": "John Doe",
    "email": "john@example.com"
}
```

### Response

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com",
    "createdAt": "2024-01-15T10:30:00Z"
}
```

---

## Error Response Structure

```json
{
    "error": {
        "code": "VALIDATION_ERROR",
        "message": "Invalid input data",
        "details": [
            {
                "field": "email",
                "message": "Email is required"
            },
            {
                "field": "password",
                "message": "Password must be at least 8 characters"
            }
        ]
    }
}
```

---

## Pagination

### Offset-based

```
GET /users?limit=20&offset=40
```

Response:
```json
{
    "data": [...],
    "pagination": {
        "total": 100,
        "limit": 20,
        "offset": 40,
        "hasMore": true
    }
}
```

### Cursor-based (Better for large datasets)

```
GET /users?limit=20&cursor=abc123
```

---

## Filtering, Sorting, Field Selection

```
# Filtering
GET /users?status=active&role=admin

# Sorting
GET /users?sort=-createdAt,name  # - for descending

# Field selection
GET /users?fields=id,name,email

# Combined
GET /users?status=active&sort=-createdAt&fields=id,name&limit=10
```

---

## Versioning

```
# URL Path (Most common)
GET /api/v1/users
GET /api/v2/users

# Header
GET /api/users
Accept: application/vnd.api+json; version=1

# Query parameter
GET /api/users?version=1
```

---

## Authentication

### Bearer Token (JWT)

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

### API Key

```http
X-API-Key: your-api-key
```

---

## Express.js Implementation

```javascript
const express = require('express');
const router = express.Router();

// GET all users
router.get('/users', async (req, res) => {
    try {
        const { limit = 10, page = 1, status } = req.query;
        const users = await User.find(status ? { status } : {})
            .limit(Number(limit))
            .skip((page - 1) * limit);
        res.json({ data: users });
    } catch (err) {
        res.status(500).json({ error: err.message });
    }
});

// GET single user
router.get('/users/:id', async (req, res) => {
    try {
        const user = await User.findById(req.params.id);
        if (!user) {
            return res.status(404).json({ error: 'User not found' });
        }
        res.json(user);
    } catch (err) {
        res.status(500).json({ error: err.message });
    }
});

// POST create user
router.post('/users', async (req, res) => {
    try {
        const user = await User.create(req.body);
        res.status(201).json(user);
    } catch (err) {
        res.status(400).json({ error: err.message });
    }
});

// PUT update user
router.put('/users/:id', async (req, res) => {
    try {
        const user = await User.findByIdAndUpdate(
            req.params.id, 
            req.body, 
            { new: true }
        );
        if (!user) {
            return res.status(404).json({ error: 'User not found' });
        }
        res.json(user);
    } catch (err) {
        res.status(400).json({ error: err.message });
    }
});

// DELETE user
router.delete('/users/:id', async (req, res) => {
    try {
        const user = await User.findByIdAndDelete(req.params.id);
        if (!user) {
            return res.status(404).json({ error: 'User not found' });
        }
        res.status(204).send();
    } catch (err) {
        res.status(500).json({ error: err.message });
    }
});
```

---

## Interview Questions

1. **What makes an API RESTful?**
   - Stateless, uses HTTP methods, resource-based URLs

2. **PUT vs PATCH?**
   - PUT replaces entire resource
   - PATCH updates partial resource

3. **How do you handle errors?**
   - Appropriate status codes, consistent error format

4. **How do you version APIs?**
   - URL path (/v1/), headers, or query params

---

## Resources

- [REST API Best Practices](https://restfulapi.net/)
- [HTTP Status Codes](https://httpstatuses.com/)
