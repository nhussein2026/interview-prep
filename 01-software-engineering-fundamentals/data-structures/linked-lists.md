# Linked Lists

## Overview

A linked list is a linear data structure where elements are stored in nodes, and each node points to the next node. Unlike arrays, elements are not stored in contiguous memory.

## Types of Linked Lists

| Type | Description | Use Case |
|------|-------------|----------|
| Singly | Each node points to next | Simple lists, stacks |
| Doubly | Nodes point to next and previous | Browser history, LRU cache |
| Circular | Last node points to first | Round-robin scheduling |

## Time Complexity

| Operation | Singly | Doubly |
|-----------|--------|--------|
| Access | O(n) | O(n) |
| Search | O(n) | O(n) |
| Insert at head | O(1) | O(1) |
| Insert at tail | O(n)* | O(1) |
| Delete at head | O(1) | O(1) |
| Delete at tail | O(n) | O(1) |

*O(1) if tail pointer is maintained

---

## Node Implementation

```javascript
// Singly Linked List Node
class ListNode {
    constructor(val) {
        this.val = val;
        this.next = null;
    }
}

// Doubly Linked List Node
class DoublyListNode {
    constructor(val) {
        this.val = val;
        this.next = null;
        this.prev = null;
    }
}
```

---

## Common Interview Questions

### 1. Reverse a Linked List
**The most common linked list question!**

```javascript
function reverseList(head) {
    let prev = null;
    let current = head;
    
    while (current !== null) {
        const next = current.next;
        current.next = prev;
        prev = current;
        current = next;
    }
    return prev;
}
// Time: O(n), Space: O(1)
```

### 2. Detect Cycle (Floyd's Algorithm)
**Problem**: Determine if a linked list has a cycle.

```javascript
function hasCycle(head) {
    let slow = head;
    let fast = head;
    
    while (fast !== null && fast.next !== null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow === fast) return true;
    }
    return false;
}
// Time: O(n), Space: O(1)
```

### 3. Find Middle Node
```javascript
function findMiddle(head) {
    let slow = head;
    let fast = head;
    
    while (fast !== null && fast.next !== null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
// Time: O(n), Space: O(1)
```

### 4. Merge Two Sorted Lists
```javascript
function mergeTwoLists(l1, l2) {
    const dummy = new ListNode(0);
    let current = dummy;
    
    while (l1 !== null && l2 !== null) {
        if (l1.val <= l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }
    current.next = l1 || l2;
    return dummy.next;
}
// Time: O(n + m), Space: O(1)
```

### 5. Remove Nth Node From End
```javascript
function removeNthFromEnd(head, n) {
    const dummy = new ListNode(0);
    dummy.next = head;
    let fast = dummy;
    let slow = dummy;
    
    // Move fast n+1 steps ahead
    for (let i = 0; i <= n; i++) {
        fast = fast.next;
    }
    
    // Move both until fast reaches end
    while (fast !== null) {
        slow = slow.next;
        fast = fast.next;
    }
    
    slow.next = slow.next.next;
    return dummy.next;
}
// Time: O(n), Space: O(1)
```

---

## Common Patterns

### Dummy Node
Use a dummy node when the head might change.

```javascript
const dummy = new ListNode(0);
dummy.next = head;
// ... operations
return dummy.next;
```

### Fast & Slow Pointers
- Find middle: slow moves 1, fast moves 2
- Detect cycle: if they meet, there's a cycle
- Find cycle start: reset one pointer to head

---

## Tips for Interviews

- Draw it out! Visualize the pointers
- Use dummy nodes to handle edge cases
- Be careful with null checks
- Consider: What if list is empty? Single node?
- Fast/slow pointer solves many problems

## Resources

- [LeetCode Linked List Problems](https://leetcode.com/tag/linked-list/)
