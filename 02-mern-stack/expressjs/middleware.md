# Express.js Middleware

## Overview

Middleware functions have access to the request (`req`), response (`res`), and the `next` function. They can execute code, modify req/res, end the request-response cycle, or call next().

---

## How Middleware Works

```
Request → Middleware 1 → Middleware 2 → Route Handler → Response
              ↓              ↓               ↓
           next()         next()          res.send()
```

```javascript
const express = require('express');
const app = express();

// Middleware function
const logger = (req, res, next) => {
    console.log(`${req.method} ${req.url}`);
    next(); // Pass to next middleware
};

app.use(logger);

app.get('/', (req, res) => {
    res.send('Hello World');
});
```

---

## Types of Middleware

### 1. Application-Level Middleware

```javascript
// Applies to ALL routes
app.use((req, res, next) => {
    console.log('Time:', Date.now());
    next();
});

// Applies to specific path
app.use('/api', (req, res, next) => {
    console.log('API request');
    next();
});
```

### 2. Router-Level Middleware

```javascript
const router = express.Router();

router.use((req, res, next) => {
    console.log('Router middleware');
    next();
});

router.get('/users', (req, res) => {
    res.json({ users: [] });
});

app.use('/api', router);
```

### 3. Built-in Middleware

```javascript
// Parse JSON bodies
app.use(express.json());

// Parse URL-encoded bodies
app.use(express.urlencoded({ extended: true }));

// Serve static files
app.use(express.static('public'));
```

### 4. Third-Party Middleware

```javascript
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');

app.use(cors());          // Enable CORS
app.use(helmet());        // Security headers
app.use(morgan('dev'));   // Request logging
```

### 5. Error-Handling Middleware

Must have 4 parameters: `(err, req, res, next)`

```javascript
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ 
        error: 'Something went wrong!' 
    });
});
```

---

## Common Middleware Patterns

### Authentication Middleware

```javascript
const authenticate = (req, res, next) => {
    const token = req.headers.authorization?.split(' ')[1];
    
    if (!token) {
        return res.status(401).json({ error: 'No token provided' });
    }
    
    try {
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        req.user = decoded;
        next();
    } catch (err) {
        res.status(401).json({ error: 'Invalid token' });
    }
};

// Use on protected routes
app.get('/profile', authenticate, (req, res) => {
    res.json({ user: req.user });
});
```

### Validation Middleware

```javascript
const validateUser = (req, res, next) => {
    const { email, password } = req.body;
    
    if (!email || !password) {
        return res.status(400).json({ 
            error: 'Email and password required' 
        });
    }
    
    if (password.length < 6) {
        return res.status(400).json({ 
            error: 'Password must be at least 6 characters' 
        });
    }
    
    next();
};

app.post('/register', validateUser, (req, res) => {
    // Create user...
});
```

### Rate Limiting

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100 // limit each IP to 100 requests per window
});

app.use('/api', limiter);
```

---

## Middleware Order Matters!

```javascript
// ❌ Wrong order - route won't have parsed body
app.post('/users', (req, res) => {
    console.log(req.body); // undefined!
});
app.use(express.json());

// ✅ Correct order
app.use(express.json());
app.post('/users', (req, res) => {
    console.log(req.body); // works!
});
```

---

## Error Handling Best Practices

```javascript
// Async error handling wrapper
const asyncHandler = (fn) => (req, res, next) => {
    Promise.resolve(fn(req, res, next)).catch(next);
};

// Usage
app.get('/users', asyncHandler(async (req, res) => {
    const users = await User.find();
    res.json(users);
    // If error, automatically passed to error middleware
}));

// Custom error class
class AppError extends Error {
    constructor(message, statusCode) {
        super(message);
        this.statusCode = statusCode;
    }
}

// Error middleware
app.use((err, req, res, next) => {
    const status = err.statusCode || 500;
    res.status(status).json({
        error: err.message || 'Internal Server Error'
    });
});
```

---

## Interview Questions

1. **What is middleware in Express?**
   - Functions that have access to req, res, and next()
   - Can execute code, modify req/res, end cycle, or call next()

2. **What happens if you don't call next()?**
   - Request hangs, never reaches route handler

3. **How do you handle errors in Express?**
   - Error-handling middleware with 4 parameters
   - Async wrapper to catch promise rejections

4. **What's the difference between app.use() and app.get()?**
   - app.use() runs for all HTTP methods on a path
   - app.get() only runs for GET requests

---

## Resources

- [Express Middleware Docs](https://expressjs.com/en/guide/using-middleware.html)
