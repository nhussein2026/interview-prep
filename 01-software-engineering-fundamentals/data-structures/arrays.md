# Arrays

## Overview

An array is a contiguous block of memory that stores elements of the same type. It's the most fundamental data structure and the building block for many other structures.

## Key Concepts

### Static vs Dynamic Arrays

| Type | Size | Memory | Example |
|------|------|--------|---------|
| Static | Fixed at creation | Stack/Heap | `int arr[5]` in C |
| Dynamic | Can grow/shrink | Heap | `ArrayList`, JavaScript arrays |

### Time Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| Access by index | O(1) | O(1) |
| Search (unsorted) | O(n) | O(n) |
| Search (sorted) | O(log n) | O(log n) |
| Insert at end | O(1)* | O(n) |
| Insert at beginning | O(n) | O(n) |
| Delete at end | O(1) | O(1) |
| Delete at beginning | O(n) | O(n) |

*Amortized O(1) for dynamic arrays

---

## Common Interview Questions

### 1. Two Sum
**Problem**: Find two numbers that add up to a target.

```javascript
function twoSum(nums, target) {
    const map = new Map();
    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        if (map.has(complement)) {
            return [map.get(complement), i];
        }
        map.set(nums[i], i);
    }
    return [];
}
// Time: O(n), Space: O(n)
```

### 2. Find Maximum Subarray (Kadane's Algorithm)
**Problem**: Find the contiguous subarray with the largest sum.

```javascript
function maxSubArray(nums) {
    let maxSum = nums[0];
    let currentSum = nums[0];
    
    for (let i = 1; i < nums.length; i++) {
        currentSum = Math.max(nums[i], currentSum + nums[i]);
        maxSum = Math.max(maxSum, currentSum);
    }
    return maxSum;
}
// Time: O(n), Space: O(1)
```

### 3. Rotate Array
**Problem**: Rotate array to the right by k steps.

```javascript
function rotate(nums, k) {
    k = k % nums.length;
    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
}

function reverse(arr, start, end) {
    while (start < end) {
        [arr[start], arr[end]] = [arr[end], arr[start]];
        start++;
        end--;
    }
}
// Time: O(n), Space: O(1)
```

### 4. Remove Duplicates from Sorted Array
**Problem**: Remove duplicates in-place, return new length.

```javascript
function removeDuplicates(nums) {
    if (nums.length === 0) return 0;
    let i = 0;
    for (let j = 1; j < nums.length; j++) {
        if (nums[j] !== nums[i]) {
            i++;
            nums[i] = nums[j];
        }
    }
    return i + 1;
}
// Time: O(n), Space: O(1)
```

---

## Common Patterns

### Two Pointers
Use when dealing with sorted arrays or when you need to find pairs.

```javascript
// Example: Find if pair exists with given sum (sorted array)
function hasPairWithSum(arr, target) {
    let left = 0, right = arr.length - 1;
    while (left < right) {
        const sum = arr[left] + arr[right];
        if (sum === target) return true;
        if (sum < target) left++;
        else right--;
    }
    return false;
}
```

### Sliding Window
Use for subarray/substring problems with contiguous elements.

```javascript
// Example: Maximum sum of k consecutive elements
function maxSumOfK(arr, k) {
    let windowSum = 0;
    for (let i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    let maxSum = windowSum;
    for (let i = k; i < arr.length; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```

---

## Tips for Interviews

- Always clarify: Is the array sorted? Are there duplicates? Can we modify it?
- Consider edge cases: empty array, single element, all same elements
- Think about space constraints before creating new arrays
- Two pointers and sliding window solve most array problems

## Resources

- [LeetCode Array Problems](https://leetcode.com/tag/array/)
- [NeetCode Array Playlist](https://neetcode.io/)
