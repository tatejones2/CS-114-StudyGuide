# Chapter 5 - Lists, Stacks, Queues

## Lists

### What is a List?

A list is a collection of elements in a specific order where you can:
- Access elements by position (index)
- Insert elements anywhere
- Remove elements
- Add elements to the end

### List Implementation 1: Dynamic Array (Growable Array)

**Idea:** Store elements in a regular array, but double the size when you run out of space.

**How it works:**
```
Start with array of size 4: [1, 2, 3, 4]
Add 5th element → Array full → Create new array of size 8
Copy all elements: [1, 2, 3, 4, 5, _, _, _]
Add more elements...
```

**Operations and Time Complexity:**

| Operation | Time | Why |
|-----------|------|-----|
| Access by index (get) | O(1) | Direct array access |
| Add to end | O(1) amortized | Usually just increment, rarely resize |
| Add/remove at position | O(n) | Must shift all elements after position |
| Remove from end | O(1) | Just decrement counter |
| Search for element | O(n) | Must check each element |

**Amortized O(1) for add:**
- Most adds are O(1)
- Resizing happens rarely (every n operations)
- Average cost: O(1)

**Advantages:**
- Fast random access
- Memory efficient
- Fast appending

**Disadvantages:**
- Slow insertion/deletion in middle
- Occasionally expensive resize operation

---

### List Implementation 2: Linked List

**Idea:** Each element contains a value AND a pointer to the next element.

```
┌─────┐     ┌─────┐     ┌─────┐     ┌─────┐
│ 1 →└────→ │ 2 →└────→ │ 3 →└────→ │ 4 →null
└─────┘     └─────┘     └─────┘     └─────┘
 head
```

Each node contains:
```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None
```

**Operations and Time Complexity:**

| Operation | Time | Why |
|-----------|------|-----|
| Access by index (get) | O(n) | Must follow pointers from head |
| Add to beginning | O(1) | Just update head pointer |
| Add/remove at position | O(n) | Must find the position first |
| Remove from beginning | O(1) | Update head pointer |
| Search for element | O(n) | Must follow all pointers |

**Advantages:**
- Fast insertion/deletion (once you find position)
- No wasted space from resizing
- Grows naturally without copying

**Disadvantages:**
- Slow random access
- Extra memory for pointers
- Cache unfriendly

---

### List Implementation 3: Doubly Linked List

Like linked list, but each node has pointers to both next AND previous:

```
null ← ┌─────┐ ← → ┌─────┐ ← → ┌─────┐ → null
       │ 1  │       │ 2  │       │ 3  │
       └─────┘ ← → └─────┘ ← → └─────┘
       head                     tail
```

**Advantages:**
- Can traverse backwards
- Easier to remove node (know previous node)

**Time Complexity:** Same as linked list

---

## Stacks

### What is a Stack?

A stack is a **LIFO** (Last-In-First-Out) data structure.

**Real-world analogy:** A stack of plates. You add plates to the top and remove plates from the top. The last plate you put on is the first one you take off.

```
Add 1: [1]
Add 2: [1, 2]
Add 3: [1, 2, 3]
Remove: [1, 2]        ← removes 3 (most recent)
Remove: [1]           ← removes 2
```

### Stack Operations

- **Push(x):** Add element x to the top - O(1)
- **Pop():** Remove and return top element - O(1)
- **Peek():** Look at top element without removing - O(1)
- **isEmpty():** Check if stack is empty - O(1)

### Stack Implementation

**Using array:**
```python
class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, x):
        self.items.append(x)        # O(1) amortized
    
    def pop(self):
        return self.items.pop()     # O(1)
    
    def peek(self):
        return self.items[-1]       # O(1)
    
    def isEmpty(self):
        return len(self.items) == 0 # O(1)
```

### Stack Use Cases

1. **Function call stack:** Recursion, function returns
2. **Undo/Redo:** Store previous states
3. **Bracket matching:** Check if {[()]} is balanced
4. **Depth-first search:** Graph traversal
5. **Expression evaluation:** Convert infix to postfix

### Example: Bracket Matching

Check if string like "{[()]}" has matching brackets:

```python
def is_balanced(s):
    stack = Stack()
    pairs = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in '({[':
            stack.push(char)
        else:  # closing bracket
            if stack.isEmpty() or stack.pop() != pairs[char]:
                return False
    
    return stack.isEmpty()
```

---

## Queues

### What is a Queue?

A queue is a **FIFO** (First-In-First-Out) data structure.

**Real-world analogy:** A line at the grocery store. The first person in line is the first person served. New people join at the back.

```
Add 1: [1]
Add 2: [1, 2]
Add 3: [1, 2, 3]
Remove: [2, 3]        ← removes 1 (first in)
Remove: [3]           ← removes 2
```

### Queue Operations

- **Enqueue(x):** Add element x to the back - O(1)
- **Dequeue():** Remove and return front element - O(1)
- **Peek():** Look at front element - O(1)
- **isEmpty():** Check if queue is empty - O(1)

### Queue Implementation

**Using array (with waste):**
```python
class Queue:
    def __init__(self):
        self.items = []
    
    def enqueue(self, x):
        self.items.append(x)        # O(1) amortized
    
    def dequeue(self):
        return self.items.pop(0)    # O(n) - shifts all elements!
```

**Problem:** Dequeue is O(n) because removing from front requires shifting all elements.

**Better: Using deque (double-ended queue):**
```python
from collections import deque

class Queue:
    def __init__(self):
        self.items = deque()
    
    def enqueue(self, x):
        self.items.append(x)        # O(1)
    
    def dequeue(self):
        return self.items.popleft() # O(1) - no shift needed!
    
    def peek(self):
        return self.items[0]        # O(1)
```

### Queue Use Cases

1. **Breadth-first search:** Graph traversal
2. **Print queue:** Jobs waiting to print
3. **Task scheduling:** Process tasks in order received
4. **Message passing:** Async communication between threads
5. **Resource allocation:** Fair distribution

---

## Summary Comparison

| Feature | Dynamic Array | Linked List | Stack | Queue |
|---------|---------------|-------------|-------|-------|
| Access by index | O(1) | O(n) | N/A | N/A |
| Add to end | O(1) | O(1) | O(1) | O(1) |
| Add to beginning | O(n) | O(1) | O(1) | N/A |
| Remove from end | O(1) | O(n) | O(1) | N/A |
| Remove from beginning | O(n) | O(1) | N/A | O(1) |
| Add/remove at position | O(n) | O(n) | N/A | N/A |
| Best for | Random access | Frequent insertions | LIFO pattern | FIFO pattern |

---

## Key Takeaways

- **Dynamic Array**: Fast access, medium insertion/deletion
- **Linked List**: Slow access, fast insertion/deletion
- **Stack (LIFO)**: Last in, first out - perfect for recursion, undo/redo
- **Queue (FIFO)**: First in, first out - perfect for scheduling, BFS
- **All basic operations** on stacks and queues are O(1)

