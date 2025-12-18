# Chapter 7 - Binary Search Trees

## Binary Tree Basics

### What is a Binary Tree?

A binary tree is a tree where each node has **at most 2 children**: left child and right child.

```
        A (root)
       / \
      B   C
     / \
    D   E
```

**Terminology:**
- **Root:** Top node (A)
- **Parent:** Node above (A is parent of B and C)
- **Child:** Node below (B and C are children of A)
- **Leaf:** Node with no children (D, E, C)
- **Subtree:** Any node and its descendants

### Binary Tree Properties

**Height:** Longest path from root to leaf
- Tree above has height 2

**Balanced vs Unbalanced:**
- **Balanced:** Heights of left and right subtrees differ by ≤ 1
- **Unbalanced:** One side much taller than other (skewed tree)

### Tree Traversals

**Level-order (Breadth-First):**
```
A, B, C, D, E  (layer by layer)
```

**In-order (Left-Root-Right):**
```
D, B, E, A, C  (left subtree, node, right subtree)
```

**Pre-order (Root-Left-Right):**
```
A, B, D, E, C  (node, left subtree, right subtree)
```

**Post-order (Left-Right-Root):**
```
D, E, B, C, A  (left subtree, right subtree, node)
```

---

## Binary Search Tree (BST)

### BST Property

A binary search tree is a binary tree where:
- **Left subtree** contains all values **smaller** than node
- **Right subtree** contains all values **larger** than node
- This property holds for **every node**, not just root

```
        50
       /  \
      30   70
     / \   / \
    20 40 60 80
```

This is a valid BST:
- 50 > 30, 70 ✓
- 30 > 20, 40 ✓
- 70 > 60, 80 ✓

### BST Operations

#### Search (Lookup)

```python
def search(node, value):
    if node is None:
        return False  # Not found
    
    if value == node.value:
        return True   # Found!
    elif value < node.value:
        return search(node.left, value)   # Go left
    else:
        return search(node.right, value)  # Go right
```

**Time Complexity:**
- **Balanced tree:** O(log n)
- **Skewed tree (worst case):** O(n)
- **Average case:** O(log n)

#### Insert

```python
def insert(node, value):
    if node is None:
        return Node(value)  # Create new node
    
    if value < node.value:
        node.left = insert(node.left, value)
    elif value > node.value:
        node.right = insert(node.right, value)
    # If value == node.value, could handle duplicates or skip
    
    return node
```

**Time Complexity:** O(log n) balanced, O(n) worst case

#### Delete

**Three cases:**

**Case 1: Node is a leaf (no children)**
```
Simply remove it
```

**Case 2: Node has one child**
```
Replace node with its child
    30              40
   /  \    →       /  \
  20  40          20  50
      \
       50
```

**Case 3: Node has two children**
```
Find in-order successor (smallest in right subtree)
Replace node's value with successor's value
Delete the successor
```

Example:
```
    50                50
   /  \              /  \
  30  70    →       30  70
 / \ / \           / \ / \
20 40 60 80       20 40 60 80
              (delete 50, replace with 60)
    60
   /  \
  30  70
 / \ / \
20 40 60 80
```

**Pseudocode for delete:**

```python
def delete(node, value):
    if node is None:
        return None
    
    if value < node.value:
        node.left = delete(node.left, value)
    elif value > node.value:
        node.right = delete(node.right, value)
    else:  # Found the node to delete
        # Case 1: No children (leaf)
        if node.left is None and node.right is None:
            return None
        
        # Case 2: One child
        if node.left is None:
            return node.right
        if node.right is None:
            return node.left
        
        # Case 3: Two children
        # Find in-order successor
        successor = find_min(node.right)
        node.value = successor.value
        node.right = delete(node.right, successor.value)
    
    return node
```

**Time Complexity:** O(log n) balanced, O(n) worst case

### BST Properties

1. **In-order traversal produces sorted sequence**
   - Useful for outputting keys in order

2. **Finding min:** Follow left pointers until None
   - Min is leftmost node

3. **Finding max:** Follow right pointers until None
   - Max is rightmost node

4. **Searching range:** Easy to find all values in range [a, b]

---

## BST Time Complexity Summary

| Operation | Balanced | Skewed |
|-----------|----------|--------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Min/Max | O(log n) | O(n) |

**Why skewed?** When you insert sorted data:
```
Insert: 1, 2, 3, 4, 5

Result:
    1
     \
      2
       \
        3
         \
          4
           \
            5

This is O(n)!
```

---

## Self-Balancing BSTs (Fix Skewness)

### AVL Tree

Rebalances after each insertion/deletion to maintain height ≤ log n.

**Time Complexity:** O(log n) guaranteed for all operations

**Mechanism:** Uses rotations when tree becomes unbalanced

### Red-Black Tree

Another self-balancing BST with different balancing rules.

**Simpler rotations than AVL, good for real-world use**

**Time Complexity:** O(log n) guaranteed

---

## BST Applications

1. **Databases:** Primary index structure
2. **File systems:** Directory tree
3. **Expression parsing:** Operator precedence
4. **Sorting:** Convert to sorted order via in-order traversal
5. **Symbol tables:** Compilers store variables/functions

---

## Key Takeaways

- **BST Property:** Left < node < Right (holds for all nodes)
- **Search/Insert/Delete:** O(log n) if balanced, O(n) if skewed
- **In-order traversal:** Gives sorted order
- **Three delete cases:** Leaf, one child, two children
- **Skewed trees:** Avoid by using balanced variants (AVL, Red-Black)
- **Find min:** Go left; Find max: Go right

