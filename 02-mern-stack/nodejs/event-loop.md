# Node.js Event Loop

## Overview

The event loop is what allows Node.js to perform non-blocking I/O operations despite JavaScript being single-threaded. It offloads operations to the system kernel whenever possible.

---

## How It Works

```
   ┌───────────────────────────┐
┌─>│           timers          │ ← setTimeout, setInterval
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │ ← I/O callbacks deferred
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       idle, prepare       │ ← internal use only
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │           poll            │ ← retrieve new I/O events
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │           check           │ ← setImmediate
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
└──┤      close callbacks      │ ← socket.on('close')
   └───────────────────────────┘
```

---

## Event Loop Phases

### 1. Timers
Executes callbacks scheduled by `setTimeout()` and `setInterval()`.

### 2. Pending Callbacks
Executes I/O callbacks deferred to the next loop iteration.

### 3. Poll
- Retrieves new I/O events
- Executes I/O related callbacks
- Node will block here when appropriate

### 4. Check
`setImmediate()` callbacks are invoked here.

### 5. Close Callbacks
Close callbacks (e.g., `socket.on('close')`).

---

## Key Concepts

### Microtasks vs Macrotasks

| Microtasks (Higher Priority) | Macrotasks (Lower Priority) |
|------------------------------|---------------------------|
| process.nextTick() | setTimeout |
| Promise.then() | setInterval |
| queueMicrotask() | setImmediate |
| | I/O operations |

```javascript
console.log('1');

setTimeout(() => console.log('2'), 0);

Promise.resolve().then(() => console.log('3'));

process.nextTick(() => console.log('4'));

console.log('5');

// Output: 1, 5, 4, 3, 2
```

### process.nextTick() vs setImmediate()

```javascript
setImmediate(() => console.log('setImmediate'));
process.nextTick(() => console.log('nextTick'));

// Output: nextTick, setImmediate
// nextTick runs BEFORE the event loop continues
// setImmediate runs on the next iteration of event loop
```

---

## Common Interview Questions

### 1. What is the output?

```javascript
console.log('start');

setTimeout(() => {
    console.log('timeout');
}, 0);

Promise.resolve().then(() => {
    console.log('promise');
});

console.log('end');

// Answer: start, end, promise, timeout
```

### 2. Why is Node.js non-blocking?

- Uses event loop to handle async operations
- I/O operations are offloaded to system kernel
- Callbacks are executed when operations complete
- Single thread handles many concurrent connections

### 3. What's the difference between sync and async?

```javascript
// Synchronous - Blocks execution
const data = fs.readFileSync('file.txt');
console.log(data);
console.log('Next line'); // Waits for file read

// Asynchronous - Non-blocking
fs.readFile('file.txt', (err, data) => {
    console.log(data);
});
console.log('Next line'); // Runs immediately
```

---

## Blocking the Event Loop (Anti-pattern!)

```javascript
// ❌ BAD - Blocks the event loop
function heavyComputation() {
    let sum = 0;
    for (let i = 0; i < 1e9; i++) {
        sum += i;
    }
    return sum;
}

// ✅ GOOD - Break into chunks or use worker threads
function heavyComputationAsync(callback) {
    const { Worker } = require('worker_threads');
    const worker = new Worker('./heavy-worker.js');
    worker.on('message', callback);
}
```

---

## Best Practices

1. **Never block the event loop** with sync operations
2. **Use async/await** for cleaner async code
3. **Partition CPU-intensive work** into chunks
4. **Use Worker Threads** for CPU-bound tasks
5. **Use clustering** to utilize multiple cores

---

## Resources

- [Node.js Event Loop Docs](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick)
- [Philip Roberts: What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
