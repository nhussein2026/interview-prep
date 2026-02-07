# MongoDB Queries & Operations

## Overview

MongoDB is a NoSQL document database. Data is stored in flexible, JSON-like documents called BSON.

---

## Basic CRUD Operations

### Create

```javascript
// Insert one document
db.users.insertOne({
    name: "John",
    email: "john@example.com",
    age: 30
});

// Insert multiple documents
db.users.insertMany([
    { name: "Jane", email: "jane@example.com" },
    { name: "Bob", email: "bob@example.com" }
]);
```

### Read

```javascript
// Find all
db.users.find();

// Find with condition
db.users.find({ age: { $gt: 25 } });

// Find one
db.users.findOne({ email: "john@example.com" });

// Projection (select specific fields)
db.users.find({}, { name: 1, email: 1, _id: 0 });
```

### Update

```javascript
// Update one
db.users.updateOne(
    { name: "John" },                    // filter
    { $set: { age: 31 } }                // update
);

// Update many
db.users.updateMany(
    { age: { $lt: 25 } },
    { $set: { status: "young" } }
);

// Replace entire document
db.users.replaceOne(
    { name: "John" },
    { name: "John", age: 32, city: "NYC" }
);
```

### Delete

```javascript
// Delete one
db.users.deleteOne({ name: "John" });

// Delete many
db.users.deleteMany({ status: "inactive" });

// Delete all
db.users.deleteMany({});
```

---

## Query Operators

### Comparison

| Operator | Description | Example |
|----------|-------------|---------|
| `$eq` | Equal | `{ age: { $eq: 25 } }` |
| `$ne` | Not equal | `{ age: { $ne: 25 } }` |
| `$gt` | Greater than | `{ age: { $gt: 25 } }` |
| `$gte` | Greater or equal | `{ age: { $gte: 25 } }` |
| `$lt` | Less than | `{ age: { $lt: 25 } }` |
| `$lte` | Less or equal | `{ age: { $lte: 25 } }` |
| `$in` | In array | `{ age: { $in: [25, 30] } }` |
| `$nin` | Not in array | `{ age: { $nin: [25, 30] } }` |

### Logical

```javascript
// AND (implicit)
db.users.find({ age: { $gt: 25 }, status: "active" });

// AND (explicit)
db.users.find({ $and: [{ age: { $gt: 25 } }, { status: "active" }] });

// OR
db.users.find({ $or: [{ age: { $lt: 20 } }, { age: { $gt: 60 } }] });

// NOT
db.users.find({ age: { $not: { $gt: 25 } } });
```

### Element

```javascript
// Field exists
db.users.find({ email: { $exists: true } });

// Field is of type
db.users.find({ age: { $type: "number" } });
```

### Array

```javascript
// Contains element
db.users.find({ tags: "developer" });

// Contains all
db.users.find({ tags: { $all: ["developer", "designer"] } });

// Array size
db.users.find({ tags: { $size: 3 } });

// Element match
db.users.find({ 
    scores: { $elemMatch: { $gt: 80, $lt: 90 } } 
});
```

---

## Update Operators

```javascript
// Set field value
{ $set: { name: "New Name" } }

// Remove field
{ $unset: { age: "" } }

// Increment
{ $inc: { age: 1 } }

// Push to array
{ $push: { tags: "newTag" } }

// Pull from array
{ $pull: { tags: "oldTag" } }

// Add to set (no duplicates)
{ $addToSet: { tags: "uniqueTag" } }
```

---

## Aggregation Pipeline

Process data through stages.

```javascript
db.orders.aggregate([
    // Stage 1: Filter
    { $match: { status: "completed" } },
    
    // Stage 2: Group
    { $group: {
        _id: "$customerId",
        totalSpent: { $sum: "$amount" },
        orderCount: { $count: {} }
    }},
    
    // Stage 3: Sort
    { $sort: { totalSpent: -1 } },
    
    // Stage 4: Limit
    { $limit: 10 },
    
    // Stage 5: Project
    { $project: {
        customerId: "$_id",
        totalSpent: 1,
        orderCount: 1,
        _id: 0
    }}
]);
```

### Common Aggregation Stages

| Stage | Description |
|-------|-------------|
| `$match` | Filter documents |
| `$group` | Group by field |
| `$sort` | Sort documents |
| `$limit` | Limit results |
| `$skip` | Skip documents |
| `$project` | Shape output |
| `$lookup` | Join collections |
| `$unwind` | Deconstruct array |

### $lookup (Join)

```javascript
db.orders.aggregate([
    {
        $lookup: {
            from: "users",           // Collection to join
            localField: "userId",    // Field in orders
            foreignField: "_id",     // Field in users
            as: "userInfo"           // Output array field
        }
    }
]);
```

---

## Indexing

```javascript
// Create index
db.users.createIndex({ email: 1 });  // 1 = ascending, -1 = descending

// Compound index
db.users.createIndex({ lastName: 1, firstName: 1 });

// Unique index
db.users.createIndex({ email: 1 }, { unique: true });

// Text index
db.articles.createIndex({ content: "text" });

// View indexes
db.users.getIndexes();

// Drop index
db.users.dropIndex("email_1");
```

---

## Mongoose (ODM)

```javascript
const mongoose = require('mongoose');

// Define Schema
const userSchema = new mongoose.Schema({
    name: { type: String, required: true },
    email: { type: String, required: true, unique: true },
    age: { type: Number, min: 0 },
    createdAt: { type: Date, default: Date.now }
});

// Create Model
const User = mongoose.model('User', userSchema);

// CRUD with Mongoose
const user = await User.create({ name: "John", email: "john@example.com" });
const users = await User.find({ age: { $gt: 25 } });
await User.findByIdAndUpdate(id, { name: "Jane" });
await User.findByIdAndDelete(id);
```

---

## Interview Questions

1. **SQL vs MongoDB: When to use which?**
   - SQL: Complex relationships, ACID transactions, structured data
   - MongoDB: Flexible schema, horizontal scaling, document-based

2. **What is an aggregation pipeline?**
   - Series of stages that process documents
   - Each stage transforms the data

3. **Embed vs Reference documents?**
   - Embed: Data accessed together, 1:1 or 1:few relationships
   - Reference: Large subdocuments, many:many, data accessed separately

4. **How do indexes improve performance?**
   - Create data structures for faster lookups
   - Trade-off: Faster reads, slower writes

---

## Resources

- [MongoDB Manual](https://docs.mongodb.com/manual/)
- [Mongoose Docs](https://mongoosejs.com/docs/)
