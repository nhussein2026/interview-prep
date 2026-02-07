# OOP & SOLID Principles

## Overview

Object-Oriented Programming organizes code around objects that combine data and behavior. SOLID principles guide maintainable OOP design.

---

## Four Pillars of OOP

### 1. Encapsulation

Bundling data and methods that operate on that data within a class, restricting direct access.

```javascript
class BankAccount {
    #balance = 0;  // Private field
    
    deposit(amount) {
        if (amount > 0) {
            this.#balance += amount;
        }
    }
    
    getBalance() {
        return this.#balance;
    }
}

const account = new BankAccount();
account.deposit(100);
console.log(account.getBalance()); // 100
// account.#balance // Error - can't access private
```

### 2. Abstraction

Hide complex implementation, expose simple interface.

```javascript
class EmailService {
    // User doesn't need to know how it works
    sendEmail(to, subject, body) {
        this.#connect();
        this.#authenticate();
        this.#formatMessage(to, subject, body);
        this.#transmit();
    }
    
    #connect() { /* complex logic */ }
    #authenticate() { /* complex logic */ }
    #formatMessage() { /* complex logic */ }
    #transmit() { /* complex logic */ }
}
```

### 3. Inheritance

Create new classes based on existing ones.

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        console.log(`${this.name} makes a sound`);
    }
}

class Dog extends Animal {
    speak() {
        console.log(`${this.name} barks`);
    }
    
    fetch() {
        console.log(`${this.name} fetches the ball`);
    }
}

const dog = new Dog('Rex');
dog.speak(); // 'Rex barks'
```

### 4. Polymorphism

Same interface, different implementations.

```javascript
class Shape {
    area() {
        throw new Error('Must implement area');
    }
}

class Rectangle extends Shape {
    constructor(width, height) {
        super();
        this.width = width;
        this.height = height;
    }
    
    area() {
        return this.width * this.height;
    }
}

class Circle extends Shape {
    constructor(radius) {
        super();
        this.radius = radius;
    }
    
    area() {
        return Math.PI * this.radius ** 2;
    }
}

// Same method, different behavior
const shapes = [new Rectangle(4, 5), new Circle(3)];
shapes.forEach(s => console.log(s.area()));
```

---

## SOLID Principles

### S - Single Responsibility Principle

A class should have only one reason to change.

```javascript
// ❌ Bad - multiple responsibilities
class User {
    saveToDatabase() { }
    sendEmail() { }
    generateReport() { }
}

// ✅ Good - single responsibility
class User { }
class UserRepository {
    save(user) { }
}
class EmailService {
    sendEmail(user) { }
}
class ReportGenerator {
    generate(user) { }
}
```

### O - Open/Closed Principle

Open for extension, closed for modification.

```javascript
// ❌ Bad - modifying existing code for new types
class AreaCalculator {
    calculate(shape) {
        if (shape.type === 'rectangle') {
            return shape.width * shape.height;
        } else if (shape.type === 'circle') {
            return Math.PI * shape.radius ** 2;
        }
        // Need to modify for each new shape
    }
}

// ✅ Good - extend without modifying
class Shape {
    area() { throw new Error('Implement me'); }
}

class Rectangle extends Shape {
    area() { return this.width * this.height; }
}

class Circle extends Shape {
    area() { return Math.PI * this.radius ** 2; }
}

// Just add new classes, no modification needed
```

### L - Liskov Substitution Principle

Subclasses should be substitutable for their base classes.

```javascript
// ❌ Bad - Square violates Rectangle behavior
class Rectangle {
    setWidth(w) { this.width = w; }
    setHeight(h) { this.height = h; }
    area() { return this.width * this.height; }
}

class Square extends Rectangle {
    setWidth(w) { this.width = w; this.height = w; }
    setHeight(h) { this.width = h; this.height = h; }
}

// This breaks when substituting
const rect = new Square();
rect.setWidth(5);
rect.setHeight(4);
rect.area(); // Expected 20, got 16!

// ✅ Good - separate abstractions
class Shape {
    area() { throw new Error('Implement me'); }
}
class Rectangle extends Shape { }
class Square extends Shape { }
```

### I - Interface Segregation Principle

Clients shouldn't depend on interfaces they don't use.

```javascript
// ❌ Bad - forcing unused methods
class Worker {
    work() { }
    eat() { }
    sleep() { }
}

class Robot extends Worker {
    work() { /* OK */ }
    eat() { /* Robots don't eat! */ }
    sleep() { /* Robots don't sleep! */ }
}

// ✅ Good - smaller, specific interfaces
class Workable {
    work() { }
}

class Eatable {
    eat() { }
}

class Human {
    work() { }
    eat() { }
}

class Robot {
    work() { }
}
```

### D - Dependency Inversion Principle

Depend on abstractions, not concrete implementations.

```javascript
// ❌ Bad - depending on concrete class
class MySQLDatabase {
    save(data) { }
}

class UserService {
    constructor() {
        this.db = new MySQLDatabase(); // Tight coupling!
    }
}

// ✅ Good - depend on abstraction
class Database {
    save(data) { throw new Error('Implement me'); }
}

class MySQLDatabase extends Database {
    save(data) { /* MySQL logic */ }
}

class MongoDatabase extends Database {
    save(data) { /* Mongo logic */ }
}

class UserService {
    constructor(database) {
        this.db = database; // Inject dependency
    }
}

// Easy to swap implementations
const service = new UserService(new MySQLDatabase());
// or
const service2 = new UserService(new MongoDatabase());
```

---

## Quick Reference

| Principle | Focus |
|-----------|-------|
| **S**ingle Responsibility | One reason to change |
| **O**pen/Closed | Extend, don't modify |
| **L**iskov Substitution | Subtypes substitutable |
| **I**nterface Segregation | Small, focused interfaces |
| **D**ependency Inversion | Depend on abstractions |

---

## Interview Tips

- Use real-world examples when explaining
- Discuss benefits: maintainability, testability, flexibility
- Know trade-offs (inheritance vs composition)
- Be ready to refactor violating code

## Resources

- [SOLID Principles in Pictures](https://medium.com/backticks-tildes/the-s-o-l-i-d-principles-in-pictures-b34ce2f1e898)
