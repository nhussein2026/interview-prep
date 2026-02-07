# Big O Notation

## Overview

Big O describes how algorithm performance scales with input size. It focuses on the **worst case** and ignores constants.

---

## Common Complexities (Best to Worst)

| Notation | Name | Example |
|----------|------|---------|
| O(1) | Constant | Array access, hash lookup |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Loop through array |
| O(n log n) | Linearithmic | Merge sort, quick sort |
| O(n²) | Quadratic | Nested loops |
| O(2ⁿ) | Exponential | Recursive fibonacci |
| O(n!) | Factorial | Permutations |

---

## Visual Growth

```
n = 10:
O(1)       = 1
O(log n)   = 3
O(n)       = 10
O(n log n) = 30
O(n²)      = 100
O(2ⁿ)      = 1,024
O(n!)      = 3,628,800
```

---

## How to Calculate

### Rules

1. **Drop constants**: O(2n) → O(n)
2. **Drop lower terms**: O(n² + n) → O(n²)
3. **Different inputs = different variables**: O(a + b) not O(n)

### Common Patterns

```javascript
// O(1) - Constant
function getFirst(arr) {
    return arr[0];
}

// O(n) - Linear
function findMax(arr) {
    let max = arr[0];
    for (let num of arr) {      // n iterations
        if (num > max) max = num;
    }
    return max;
}

// O(n²) - Quadratic
function bubbleSort(arr) {
    for (let i = 0; i < arr.length; i++) {      // n times
        for (let j = 0; j < arr.length; j++) {  // × n times
            // ...
        }
    }
}

// O(log n) - Logarithmic
function binarySearch(arr, target) {
    let left = 0, right = arr.length - 1;
    while (left <= right) {     // Halving each time
        // ...
    }
}

// O(n log n)
function mergeSort(arr) {
    // log n levels of recursion
    // n work at each level
}
```

---

## Space Complexity

Same notation, but for **memory usage**.

```javascript
// O(1) Space - In-place
function reverse(arr) {
    let left = 0, right = arr.length - 1;
    while (left < right) {
        [arr[left], arr[right]] = [arr[right], arr[left]];
        left++; right--;
    }
}

// O(n) Space - Creating new array
function double(arr) {
    return arr.map(x => x * 2);
}

// O(n) Space - Recursion call stack
function factorial(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);  // n stack frames
}
```

---

## Amortized Analysis

Average time over a sequence of operations.

**Example**: Dynamic array `push()`
- Usually O(1)
- Occasionally O(n) when resizing
- Amortized: O(1)

---

## Quick Reference

### By Data Structure

| Structure | Access | Search | Insert | Delete |
|-----------|--------|--------|--------|--------|
| Array | O(1) | O(n) | O(n) | O(n) |
| Linked List | O(n) | O(n) | O(1) | O(1) |
| Stack/Queue | O(n) | O(n) | O(1) | O(1) |
| Hash Table | - | O(1)* | O(1)* | O(1)* |
| BST | O(log n)* | O(log n)* | O(log n)* | O(log n)* |
| Heap | - | O(n) | O(log n) | O(log n) |

*Average case, worst may differ

### By Algorithm

| Algorithm | Time | Space |
|-----------|------|-------|
| Binary Search | O(log n) | O(1) |
| Merge Sort | O(n log n) | O(n) |
| Quick Sort | O(n log n)* | O(log n) |
| BFS/DFS | O(V + E) | O(V) |
| Dijkstra | O(E log V) | O(V) |

---

## Interview Tips

- Always state time AND space complexity
- Use best/average/worst case when relevant
- Explain your reasoning, don't just state the answer
- Common follow-up: "Can you do better?"

## Resources

- [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
