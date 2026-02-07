# JavaScript ES6+ Features

## Overview

Essential modern JavaScript features you must know for interviews.

---

## let, const, var

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Hoisting | Yes (undefined) | Yes (TDZ) | Yes (TDZ) |
| Reassign | Yes | Yes | No |
| Redeclare | Yes | No | No |

```javascript
// var - function scoped, hoisted
function example() {
    console.log(x); // undefined (hoisted)
    var x = 5;
}

// let - block scoped
if (true) {
    let y = 10;
}
// console.log(y); // ReferenceError

// const - can't reassign, but objects can be mutated
const arr = [1, 2, 3];
arr.push(4); // ✅ OK
// arr = [5, 6]; // ❌ Error
```

---

## Arrow Functions

```javascript
// Traditional
function add(a, b) {
    return a + b;
}

// Arrow function
const add = (a, b) => a + b;

// With body
const multiply = (a, b) => {
    const result = a * b;
    return result;
};

// Key difference: 'this' binding
const obj = {
    name: 'John',
    regular: function() {
        console.log(this.name); // 'John'
    },
    arrow: () => {
        console.log(this.name); // undefined (inherits from parent)
    }
};
```

---

## Destructuring

```javascript
// Array destructuring
const [a, b, ...rest] = [1, 2, 3, 4, 5];
// a = 1, b = 2, rest = [3, 4, 5]

// Object destructuring
const { name, age, city = 'Unknown' } = { name: 'John', age: 30 };

// Rename
const { name: userName } = { name: 'John' };

// Nested
const { address: { city } } = { address: { city: 'NYC' } };

// Function parameters
function greet({ name, age }) {
    console.log(`${name} is ${age}`);
}
```

---

## Spread & Rest Operators

```javascript
// Spread - expand array/object
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]

const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // { a: 1, b: 2, c: 3 }

// Rest - collect remaining
function sum(...numbers) {
    return numbers.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3, 4); // 10
```

---

## Template Literals

```javascript
const name = 'John';
const age = 30;

// String interpolation
const message = `Hello, ${name}! You are ${age} years old.`;

// Multi-line strings
const html = `
    <div>
        <h1>${name}</h1>
    </div>
`;

// Tagged templates
function highlight(strings, ...values) {
    return strings.reduce((result, str, i) => 
        `${result}${str}<mark>${values[i] || ''}</mark>`, '');
}
```

---

## Promises & Async/Await

```javascript
// Promise
const fetchData = () => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('Data loaded');
        }, 1000);
    });
};

fetchData()
    .then(data => console.log(data))
    .catch(err => console.error(err));

// Async/Await
async function getData() {
    try {
        const data = await fetchData();
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}

// Promise methods
Promise.all([p1, p2, p3]);      // All must resolve
Promise.race([p1, p2, p3]);     // First to settle
Promise.allSettled([p1, p2]);   // Wait for all, regardless of status
Promise.any([p1, p2, p3]);      // First to resolve
```

---

## Classes

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        console.log(`${this.name} makes a sound`);
    }
    
    static isAnimal(obj) {
        return obj instanceof Animal;
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name);
        this.breed = breed;
    }
    
    speak() {
        console.log(`${this.name} barks`);
    }
}

const dog = new Dog('Rex', 'German Shepherd');
dog.speak(); // 'Rex barks'
```

---

## Modules

```javascript
// Named exports
export const PI = 3.14;
export function add(a, b) { return a + b; }

// Default export
export default class Calculator { }

// Import named
import { PI, add } from './math.js';

// Import default
import Calculator from './calculator.js';

// Import all
import * as math from './math.js';
```

---

## Array Methods

```javascript
const numbers = [1, 2, 3, 4, 5];

// map - transform each element
const doubled = numbers.map(n => n * 2); // [2, 4, 6, 8, 10]

// filter - keep elements that pass test
const evens = numbers.filter(n => n % 2 === 0); // [2, 4]

// reduce - accumulate to single value
const sum = numbers.reduce((acc, n) => acc + n, 0); // 15

// find - first element that passes test
const found = numbers.find(n => n > 3); // 4

// some - at least one passes
const hasEven = numbers.some(n => n % 2 === 0); // true

// every - all pass
const allPositive = numbers.every(n => n > 0); // true

// includes
numbers.includes(3); // true

// flat
[[1, 2], [3, 4]].flat(); // [1, 2, 3, 4]

// flatMap
[1, 2].flatMap(n => [n, n * 2]); // [1, 2, 2, 4]
```

---

## Optional Chaining & Nullish Coalescing

```javascript
// Optional chaining (?.)
const user = { address: { city: 'NYC' } };
const city = user?.address?.city; // 'NYC'
const zip = user?.address?.zip; // undefined (no error)

// Nullish coalescing (??)
const value = null ?? 'default'; // 'default'
const zero = 0 ?? 'default';     // 0 (only null/undefined trigger default)

// vs OR operator
const value2 = 0 || 'default';   // 'default' (0 is falsy)
```

---

## Interview Questions

1. **Difference between == and ===?**
   - `==` coerces types, `===` strict equality

2. **What is hoisting?**
   - Variables/functions moved to top of scope
   - var: initialized as undefined
   - let/const: temporal dead zone

3. **Explain closures**
   - Function that remembers its outer scope
   - Used for data privacy, callbacks

4. **What is event bubbling?**
   - Events propagate from target up to root
   - Can be stopped with `stopPropagation()`

---

## Resources

- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript.info](https://javascript.info/)
