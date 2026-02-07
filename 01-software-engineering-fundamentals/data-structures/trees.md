# Trees

## Overview

A tree is a hierarchical data structure consisting of nodes connected by edges. Trees are used everywhere: file systems, DOM, databases, and more.

## Terminology

| Term | Definition |
|------|------------|
| Root | Top node with no parent |
| Leaf | Node with no children |
| Height | Longest path from root to leaf |
| Depth | Distance from root to node |
| Subtree | Tree formed by a node and descendants |

---

## Binary Tree

Each node has at most 2 children (left and right).

```javascript
class TreeNode {
    constructor(val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}
```

### Tree Traversals

```javascript
// Inorder: Left → Root → Right (gives sorted order for BST)
function inorder(root) {
    if (!root) return [];
    return [...inorder(root.left), root.val, ...inorder(root.right)];
}

// Preorder: Root → Left → Right (good for copying tree)
function preorder(root) {
    if (!root) return [];
    return [root.val, ...preorder(root.left), ...preorder(root.right)];
}

// Postorder: Left → Right → Root (good for deletion)
function postorder(root) {
    if (!root) return [];
    return [...postorder(root.left), ...postorder(root.right), root.val];
}

// Level Order (BFS)
function levelOrder(root) {
    if (!root) return [];
    const result = [];
    const queue = [root];
    
    while (queue.length) {
        const level = [];
        const size = queue.length;
        
        for (let i = 0; i < size; i++) {
            const node = queue.shift();
            level.push(node.val);
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
        result.push(level);
    }
    return result;
}
```

---

## Binary Search Tree (BST)

A binary tree where: `left child < parent < right child`

### Operations

| Operation | Average | Worst (unbalanced) |
|-----------|---------|-------------------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |

```javascript
// Search
function searchBST(root, val) {
    if (!root || root.val === val) return root;
    return val < root.val 
        ? searchBST(root.left, val)
        : searchBST(root.right, val);
}

// Insert
function insertBST(root, val) {
    if (!root) return new TreeNode(val);
    if (val < root.val) {
        root.left = insertBST(root.left, val);
    } else {
        root.right = insertBST(root.right, val);
    }
    return root;
}

// Validate BST
function isValidBST(root, min = -Infinity, max = Infinity) {
    if (!root) return true;
    if (root.val <= min || root.val >= max) return false;
    return isValidBST(root.left, min, root.val) 
        && isValidBST(root.right, root.val, max);
}
```

---

## Common Interview Questions

### 1. Maximum Depth
```javascript
function maxDepth(root) {
    if (!root) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

### 2. Invert Binary Tree
```javascript
function invertTree(root) {
    if (!root) return null;
    [root.left, root.right] = [invertTree(root.right), invertTree(root.left)];
    return root;
}
```

### 3. Same Tree
```javascript
function isSameTree(p, q) {
    if (!p && !q) return true;
    if (!p || !q || p.val !== q.val) return false;
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

### 4. Lowest Common Ancestor (BST)
```javascript
function lowestCommonAncestor(root, p, q) {
    if (p.val < root.val && q.val < root.val) {
        return lowestCommonAncestor(root.left, p, q);
    }
    if (p.val > root.val && q.val > root.val) {
        return lowestCommonAncestor(root.right, p, q);
    }
    return root;
}
```

### 5. Path Sum
```javascript
function hasPathSum(root, targetSum) {
    if (!root) return false;
    if (!root.left && !root.right) return root.val === targetSum;
    return hasPathSum(root.left, targetSum - root.val) 
        || hasPathSum(root.right, targetSum - root.val);
}
```

---

## Balanced Trees (Know the Concepts)

| Type | Property |
|------|----------|
| AVL | Height difference ≤ 1 for all nodes |
| Red-Black | Self-balancing with color properties |
| B-Tree | Multiple keys per node, used in databases |

---

## Tips for Interviews

- Most tree problems use recursion
- Base case: usually when `root === null`
- Return type matters: boolean, number, node, array
- Think about: traversal order, what to track at each node
- BFS for level-based problems, DFS for path-based problems

## Resources

- [LeetCode Tree Problems](https://leetcode.com/tag/tree/)
- [Visualgo - BST Visualization](https://visualgo.net/en/bst)
