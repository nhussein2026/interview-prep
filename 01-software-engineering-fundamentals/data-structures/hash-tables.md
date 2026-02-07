# Hash Tables

## Overview

A hash table (hash map) stores key-value pairs using a hash function to compute an index. It provides near-constant time lookups.

## How It Works

1. **Hash Function**: Converts key → index
2. **Array**: Stores values at computed indices
3. **Collision Handling**: Deals with same index for different keys

## Time Complexity

| Operation | Average | Worst* |
|-----------|---------|--------|
| Search | O(1) | O(n) |
| Insert | O(1) | O(n) |
| Delete | O(1) | O(n) |

*Worst case when many collisions occur

---

## Collision Resolution

### 1. Chaining
Store multiple elements at same index using linked list.

```javascript
class HashTable {
    constructor(size = 53) {
        this.keyMap = new Array(size);
    }
    
    _hash(key) {
        let total = 0;
        const PRIME = 31;
        for (let i = 0; i < Math.min(key.length, 100); i++) {
            const char = key[i];
            const value = char.charCodeAt(0) - 96;
            total = (total * PRIME + value) % this.keyMap.length;
        }
        return total;
    }
    
    set(key, value) {
        const index = this._hash(key);
        if (!this.keyMap[index]) {
            this.keyMap[index] = [];
        }
        // Update if key exists
        for (let pair of this.keyMap[index]) {
            if (pair[0] === key) {
                pair[1] = value;
                return;
            }
        }
        this.keyMap[index].push([key, value]);
    }
    
    get(key) {
        const index = this._hash(key);
        if (this.keyMap[index]) {
            for (let pair of this.keyMap[index]) {
                if (pair[0] === key) return pair[1];
            }
        }
        return undefined;
    }
}
```

### 2. Open Addressing
Find next available slot (linear probing, quadratic probing).

---

## JavaScript Maps & Objects

### Object vs Map

| Feature | Object | Map |
|---------|--------|-----|
| Key types | String/Symbol | Any type |
| Order | Not guaranteed | Insertion order |
| Size | Manual count | `.size` property |
| Iteration | `Object.keys()` | `.forEach()`, `for...of` |
| Performance | Good | Optimized for frequent add/remove |

```javascript
// Using Map
const map = new Map();
map.set('key', 'value');
map.get('key');        // 'value'
map.has('key');        // true
map.delete('key');
map.size;              // 0

// Using Object
const obj = {};
obj['key'] = 'value';
obj.key;               // 'value'
'key' in obj;          // true
delete obj.key;
Object.keys(obj).length; // 0
```

---

## Common Interview Questions

### 1. Two Sum (Hash Map)
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
```

### 2. Contains Duplicate
```javascript
function containsDuplicate(nums) {
    const set = new Set();
    for (const num of nums) {
        if (set.has(num)) return true;
        set.add(num);
    }
    return false;
}
// Or simply: return new Set(nums).size !== nums.length;
```

### 3. Group Anagrams
```javascript
function groupAnagrams(strs) {
    const map = new Map();
    for (const str of strs) {
        const key = str.split('').sort().join('');
        if (!map.has(key)) {
            map.set(key, []);
        }
        map.get(key).push(str);
    }
    return Array.from(map.values());
}
```

### 4. First Non-Repeating Character
```javascript
function firstUniqChar(s) {
    const count = new Map();
    for (const char of s) {
        count.set(char, (count.get(char) || 0) + 1);
    }
    for (let i = 0; i < s.length; i++) {
        if (count.get(s[i]) === 1) return i;
    }
    return -1;
}
```

### 5. Valid Anagram
```javascript
function isAnagram(s, t) {
    if (s.length !== t.length) return false;
    const count = {};
    for (const char of s) {
        count[char] = (count[char] || 0) + 1;
    }
    for (const char of t) {
        if (!count[char]) return false;
        count[char]--;
    }
    return true;
}
```

---

## Use Cases

- **Caching**: Store computed results
- **Counting**: Frequency of elements
- **Lookup tables**: O(1) search
- **Deduplication**: Remove duplicates
- **Indexing**: Map keys to data

---

## Tips for Interviews

- Hash maps are your best friend for O(n) solutions
- Use for: counting, lookup, grouping, finding pairs
- Consider using Set when you only need keys (no values)
- Watch for: custom hash functions, collision handling

## Resources

- [LeetCode Hash Table Problems](https://leetcode.com/tag/hash-table/)
