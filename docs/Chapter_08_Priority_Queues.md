# Chapter 8 - Priority Queues & Heaps

## Priority Queue

### What is a Priority Queue?

A priority queue is a data structure where elements are served based on **priority**, not order of arrival.

**Real-world analogy:** Hospital emergency room. Critical patients are seen first, not the person who arrived first.

```
Normal Queue:           Priority Queue:
1 → 2 → 3 → 4          URGENT → Medium → Low
Remove: 1              Remove: URGENT
```

### Operations

- **Insert(x, priority):** Add element with priority - O(log n)
- **DeleteMax/DeleteMin:** Remove highest/lowest priority - O(log n)
- **PeekMax/PeekMin:** Look at highest/lowest priority - O(1)

**Note:** Min vs Max depends on whether low numbers = high priority

---

## Heap Data Structure

### What is a Heap?

A heap is a **complete binary tree** that satisfies the **heap property**:
- Every parent is ≥ its children (Max-heap)
- Every parent is ≤ its children (Min-heap)

```
Max-Heap:               Min-Heap:
      10                    1
     /  \                 /   \
    9    7               2     3
   / \                  / \
  8   6                4   5
```

### Complete Binary Tree

Every level is filled except possibly the last, which is filled left-to-right.

```
Valid:                  Invalid:
    1                       1
   / \                      / \
  2   3                    2   3
 / \  /                       / \
4   5 6                       4   5
```

---

## Heap Implementation: Array

Heaps are efficiently stored in an **array** because complete binary trees map perfectly to arrays:

```
        1 (index 0)
       / \
      2   3 (indices 1, 2)
     / \
    4   5 (indices 3, 4)

Array: [1, 2, 3, 4, 5]
```

**Index relationships:**
- Left child of index i: `2*i + 1`
- Right child of index i: `2*i + 2`
- Parent of index i: `(i - 1) / 2`

### Max-Heap Properties

1. **Heap property:** Parent ≥ children
2. **Complete binary tree:** All levels filled except last
3. **Result:** Root is the maximum element

```
      50              ← Maximum is at root!
     /  \
    30   20
   / \   / \
  10  5  15  1
```

---

## Heap Operations

### Insert (Bubble Up)

1. Add element to end of array (maintains complete tree property)
2. Compare with parent; if greater, swap
3. Repeat until heap property restored

**Example: Insert 35 into max-heap**

```
Before:           After insert:     After bubble up:
      50                50               50
     /  \              /  \             /  \
    30  20            30  20           35  20
   / \  /            / \ /  \          / \ / \
  10 5 15            10 5 15 35       30 5 15 10
```

Time: O(log n) - height of tree

### Delete (Bubble Down)

1. Remove root (it's the max)
2. Move last element to root position
3. Compare with children; swap with larger child if needed
4. Repeat until heap property restored

**Example: Delete max (50)**

```
Before:           After remove:     After bubble down:
      50                35              35
     /  \              /  \            /  \
    30  20            30  20          30  20
   / \  / \          / \             / \
  10 5 15 35        10  5           10  5  15
```

Time: O(log n)

### Pseudocode

```python
def insert(heap, value):
    heap.append(value)          # Add to end
    i = len(heap) - 1
    while i > 0:
        parent_i = (i - 1) // 2
        if heap[i] > heap[parent_i]:
            heap[i], heap[parent_i] = heap[parent_i], heap[i]
            i = parent_i
        else:
            break

def delete_max(heap):
    if not heap:
        return None
    
    max_val = heap[0]
    heap[0] = heap[-1]          # Move last to root
    heap.pop()
    
    i = 0
    while True:
        left = 2 * i + 1
        right = 2 * i + 2
        largest = i
        
        if left < len(heap) and heap[left] > heap[largest]:
            largest = left
        if right < len(heap) and heap[right] > heap[largest]:
            largest = right
        
        if largest != i:
            heap[i], heap[largest] = heap[largest], heap[i]
            i = largest
        else:
            break
    
    return max_val
```

### Heap Operation Time Complexity

| Operation | Time |
|-----------|------|
| Insert | O(log n) |
| DeleteMax | O(log n) |
| PeekMax | O(1) |
| BuildHeap (from array) | O(n) |

---

## Heap Sort

### How Heap Sort Works

1. **Build max-heap** from array: O(n)
2. **Repeatedly** delete max and place at end: n × O(log n)

**Example:**

```
Array: [3, 1, 4, 1, 5]

Step 1: Build heap
      5
     / \
    4   3
   / \
  1   1

Step 2: Delete 5, move to end
      4
     / \
    1   3
   /
  1
Array so far: [_, _, _, _, 5]

Step 3: Delete 4
      3
     / \
    1   1
Array so far: [_, _, _, 4, 5]

Step 4-5: Continue...
Final: [1, 1, 3, 4, 5]
```

### Time Complexity

- **Build heap:** O(n)
- **Delete n times:** n × O(log n) = O(n log n)
- **Total:** O(n log n)

**Space:** O(1) - sorts in place

### Heap Sort vs Other Sorts

- **Merge Sort:** O(n log n) but needs O(n) extra space
- **Quick Sort:** O(n log n) average, but O(n²) worst case
- **Heap Sort:** O(n log n) guaranteed, O(1) space

---

## Huffman Compression

### What is Huffman Coding?

Huffman coding assigns variable-length binary codes to characters based on frequency. More frequent characters get shorter codes.

**Example:**
- Character 'a' appears 10 times → code "0" (1 bit)
- Character 'b' appears 5 times → code "10" (2 bits)
- Character 'c' appears 2 times → code "110" (3 bits)
- Character 'd' appears 1 time → code "111" (3 bits)

**Savings:**
- Naive: 18 characters × 8 bits = 144 bits
- Huffman: 10×1 + 5×2 + 2×3 + 1×3 = 10 + 10 + 6 + 3 = 29 bits

### Huffman Algorithm Steps

1. **Count frequency** of each character
2. **Build min-heap** with each character as a node with its frequency
3. **While heap has > 1 node:**
   - Remove two nodes with smallest frequency
   - Create parent node with frequency = sum of children
   - Insert parent back into heap
4. **Build code tree** from heap
5. **Assign codes** using tree (left = 0, right = 1)

### Example

```
Frequencies: a:10, b:5, c:2, d:1

Initial heap: [1:d, 2:c, 5:b, 10:a]

Step 1: Remove d(1), c(2) → Create parent(3)
Heap: [3:dc, 5:b, 10:a]

Step 2: Remove dc(3), b(5) → Create parent(8)
Heap: [8:dcb, 10:a]

Step 3: Remove dcb(8), a(10) → Create parent(18)
Heap: [18:root]

Tree:
      18
     /  \
   8     10(a)
  / \
 3   5(b)
/ \
1   2
(d) (c)

Codes:
a: 1
b: 01
c: 001
d: 000
```

### Time Complexity

- **Count frequencies:** O(n)
- **Build initial heap:** O(k) where k = unique characters
- **Build tree:** k iterations of O(log k) = O(k log k)
- **Assign codes:** O(k)
- **Total:** O(n + k log k) ≈ O(n) for typical inputs

### Why Use Huffman?

- **Optimal prefix-free code:** No code is prefix of another
- **Minimizes average code length**
- **Used in:** ZIP files, PNG, JPEG compression

---

## Priority Queue Applications

1. **CPU scheduling:** Process most important jobs first
2. **Dijkstra's algorithm:** Find shortest paths
3. **Huffman coding:** Build optimal compression codes
4. **Heap sort:** Efficient sorting algorithm
5. **Load balancing:** Process highest priority requests

---

## Key Takeaways

- **Priority Queue:** Elements served by priority, not order
- **Heap:** Complete binary tree satisfying heap property
- **Max-heap:** Parent ≥ children; Root is maximum
- **Heap operations:** Insert/Delete O(log n), Peek O(1)
- **Heap sort:** O(n log n) time, O(1) space
- **Huffman coding:** Variable-length codes based on frequency, O(n + k log k)

