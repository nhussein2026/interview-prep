# Stacks & Queues

## Overview

Both are linear data structures with specific access patterns:
- **Stack**: LIFO (Last In, First Out) - like a stack of plates
- **Queue**: FIFO (First In, First Out) - like a line of people

---

## Stack

### Operations & Complexity

| Operation | Time | Description |
|-----------|------|-------------|
| push | O(1) | Add to top |
| pop | O(1) | Remove from top |
| peek/top | O(1) | View top element |
| isEmpty | O(1) | Check if empty |

### Implementation

```javascript
class Stack {
    constructor() {
        this.items = [];
    }
    
    push(item) {
        this.items.push(item);
    }
    
    pop() {
        if (this.isEmpty()) return null;
        return this.items.pop();
    }
    
    peek() {
        return this.items[this.items.length - 1];
    }
    
    isEmpty() {
        return this.items.length === 0;
    }
}
```

### Common Stack Problems

#### Valid Parentheses
```javascript
function isValid(s) {
    const stack = [];
    const map = { ')': '(', '}': '{', ']': '[' };
    
    for (const char of s) {
        if (char in map) {
            if (stack.pop() !== map[char]) return false;
        } else {
            stack.push(char);
        }
    }
    return stack.length === 0;
}
```

#### Min Stack
```javascript
class MinStack {
    constructor() {
        this.stack = [];
        this.minStack = [];
    }
    
    push(val) {
        this.stack.push(val);
        const min = this.minStack.length === 0 
            ? val 
            : Math.min(val, this.getMin());
        this.minStack.push(min);
    }
    
    pop() {
        this.stack.pop();
        this.minStack.pop();
    }
    
    top() {
        return this.stack[this.stack.length - 1];
    }
    
    getMin() {
        return this.minStack[this.minStack.length - 1];
    }
}
```

---

## Queue

### Operations & Complexity

| Operation | Time | Description |
|-----------|------|-------------|
| enqueue | O(1) | Add to back |
| dequeue | O(1)* | Remove from front |
| front | O(1) | View front element |
| isEmpty | O(1) | Check if empty |

*O(n) if using array without optimization

### Implementation with Linked List

```javascript
class Queue {
    constructor() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }
    
    enqueue(val) {
        const node = { val, next: null };
        if (this.tail) {
            this.tail.next = node;
        }
        this.tail = node;
        if (!this.head) {
            this.head = node;
        }
        this.size++;
    }
    
    dequeue() {
        if (!this.head) return null;
        const val = this.head.val;
        this.head = this.head.next;
        if (!this.head) this.tail = null;
        this.size--;
        return val;
    }
    
    isEmpty() {
        return this.size === 0;
    }
}
```

---

## Special Queues

### Priority Queue
Elements are dequeued based on priority, not arrival order.

```javascript
// Simple implementation using sorted array
class PriorityQueue {
    constructor() {
        this.items = [];
    }
    
    enqueue(val, priority) {
        const element = { val, priority };
        let added = false;
        
        for (let i = 0; i < this.items.length; i++) {
            if (element.priority < this.items[i].priority) {
                this.items.splice(i, 0, element);
                added = true;
                break;
            }
        }
        
        if (!added) this.items.push(element);
    }
    
    dequeue() {
        return this.items.shift()?.val;
    }
}
// Note: Use a Heap for O(log n) operations in production
```

### Circular Queue
```javascript
class CircularQueue {
    constructor(k) {
        this.queue = new Array(k);
        this.size = k;
        this.head = -1;
        this.tail = -1;
    }
    
    enqueue(val) {
        if (this.isFull()) return false;
        if (this.isEmpty()) this.head = 0;
        this.tail = (this.tail + 1) % this.size;
        this.queue[this.tail] = val;
        return true;
    }
    
    dequeue() {
        if (this.isEmpty()) return false;
        if (this.head === this.tail) {
            this.head = -1;
            this.tail = -1;
        } else {
            this.head = (this.head + 1) % this.size;
        }
        return true;
    }
    
    isEmpty() {
        return this.head === -1;
    }
    
    isFull() {
        return (this.tail + 1) % this.size === this.head;
    }
}
```

---

## Use Cases

| Stack | Queue |
|-------|-------|
| Undo/Redo | Task scheduling |
| Browser back button | BFS traversal |
| Function call stack | Print queue |
| Expression parsing | Message queues |
| DFS traversal | Request handling |

---

## Tips for Interviews

- Recognize stack problems: matching, nesting, history
- Recognize queue problems: order processing, BFS, scheduling
- Monotonic stack for "next greater element" type problems
- Consider using two stacks to implement a queue (and vice versa)

## Resources

- [LeetCode Stack Problems](https://leetcode.com/tag/stack/)
- [LeetCode Queue Problems](https://leetcode.com/tag/queue/)
