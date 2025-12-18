# Sample Final Exam Solutions & Study Guide

## How to Use This Guide

This document provides strategies and examples for solving typical DSA final exam questions. Reference the chapter guides (02-13) for detailed explanations of each concept.

---

## Common Question Types & Approaches

### Type 1: Algorithm Analysis (Big-O Complexity)

**Typical Question:** "What is the time complexity of the following code?"

**Solution Strategy:**
1. Count nested loops: n loops = O(n), n×n = O(n²), etc.
2. Look for halving: O(log n)
3. Identify dominant term (ignore constants and lower-order terms)
4. Simplify to Big-O notation

**Example:**
```python
def mystery(arr):
    for i in range(len(arr)):           # O(n)
        for j in range(len(arr)):       # O(n)
            if arr[i] == arr[j]:        # O(1)
                return True
    return False
```
**Answer:** O(n²) - two nested loops

---

### Type 2: Recursion Problems

**Typical Question:** "Convert this loop to a recursive function" or "What does this recursive function return?"

**Solution Strategy:**
1. Identify the base case (stopping condition)
2. Identify the recursive case (how to break problem into smaller pieces)
3. Ensure progress toward base case
4. Trace through with small example

**Example Problem:** Write recursive function to find factorial
```python
def factorial(n):
    # BASE CASE
    if n == 0 or n == 1:
        return 1
    # RECURSIVE CASE
    else:
        return n * factorial(n - 1)
```

**Test:** factorial(5) = 5 × 4 × 3 × 2 × 1 = 120 ✓

---

### Type 3: Data Structure Questions

**Typical Question:** "Which data structure would you use for X?" or "What is the time complexity of operation Y?"

**Strategy:**
- **Stack:** LIFO, O(1) push/pop/peek
- **Queue:** FIFO, O(1) enqueue/dequeue with deque
- **Hash Table:** O(1) average insert/delete/lookup
- **BST:** O(log n) if balanced, O(n) if skewed
- **Heap:** O(log n) insert/delete, O(1) peek
- **Array:** O(1) access, O(n) insert/delete in middle

**Example:** "How would you implement a most-recently-used cache?"
**Answer:** Use a **Deque or Linked List with Hash Table** for O(1) operations

---

### Type 4: Sorting Algorithm Questions

**Typical Question:** "Which sort would you use if...?" or "Trace merge sort through this array"

**Quick Reference:**
- **Merge Sort:** O(n log n) guaranteed, stable, needs extra space
- **Quick Sort:** O(n log n) average, in-place, not stable
- **Heap Sort:** O(n log n) guaranteed, in-place
- **Counting Sort:** O(n + k) for integers in range [0, k]
- **Insertion Sort:** O(n²) but good for small/nearly-sorted
- **Bubble Sort:** O(n²), avoid unless very small

---

### Type 5: Graph Algorithm Questions

**Typical Question:** "Use DFS/BFS to..." or "What is the shortest path?"

**DFS (Depth-First Search):**
- Use stack or recursion
- Good for: Topological sort, cycle detection, backtracking
- Time: O(V + E)

**BFS (Breadth-First Search):**
- Use queue
- Good for: Shortest path (unweighted), level order
- Time: O(V + E)

**Shortest Path:**
- No negatives → Use **Dijkstra** O(E log V)
- Has negatives → Use **Bellman-Ford** O(VE)
- All pairs → Use **Floyd-Warshall** O(V³)

---

### Type 6: True/False Questions

**Strategy:** Think critically about edge cases and definitions.

**Example Questions & Answers:**

1. **"Merge sort is always faster than quick sort"**
   - **FALSE** - Quick sort is faster on average due to better constants and cache efficiency

2. **"A hash table with chaining has O(1) average-case insertion"**
   - **TRUE** - With good hash function and manageable load factor

3. **"Dijkstra's algorithm works with negative edge weights"**
   - **FALSE** - Requires non-negative weights; use Bellman-Ford instead

4. **"Binary search requires the array to be sorted"**
   - **TRUE** - This is a prerequisite

5. **"An in-order traversal of a BST gives sorted elements"**
   - **TRUE** - This is a key property of BSTs

---

### Type 7: Time Complexity Comparison

**Typical Question:** "Rank these algorithms by time complexity"

**Ordering (fastest to slowest):**
- O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ)

**Practical Example (n = 1,000,000):**
- O(1) = 1 operation
- O(log n) ≈ 20 operations
- O(n) = 1,000,000 operations
- O(n log n) ≈ 20,000,000 operations
- O(n²) = 10¹² operations (too slow!)

---

## Master Checklist for Exam

Before your exam, make sure you can:

### Recursion
- [ ] Identify base case vs recursive case
- [ ] Convert loop to recursion
- [ ] Trace recursive calls
- [ ] Understand tail recursion

### Math Background
- [ ] Use arithmetic sum formula
- [ ] Use geometric sum formula
- [ ] Simplify exponential expressions
- [ ] Apply logarithm properties
- [ ] Prove statement by induction

### Algorithm Analysis
- [ ] Determine Big-O from code
- [ ] Ignore constants and lower-order terms
- [ ] Understand amortized analysis
- [ ] Perform binary search mentally

### Lists, Stacks, Queues
- [ ] Compare array vs linked list performance
- [ ] Implement stack/queue operations
- [ ] Know when to use each structure
- [ ] Understand trade-offs

### Dictionaries & Hash Tables
- [ ] Explain collision handling (chaining vs open addressing)
- [ ] Calculate load factor
- [ ] Design hash function
- [ ] Understand rehashing

### Trees
- [ ] Verify BST property
- [ ] Perform tree traversals
- [ ] Do insertion/deletion on BST
- [ ] Calculate tree height

### Heaps & Priority Queues
- [ ] Bubble up/down operations
- [ ] Trace heap sort
- [ ] Understand heap properties
- [ ] Apply to priority queue

### Sorting
- [ ] Trace merge sort step-by-step
- [ ] Understand partition in quick sort
- [ ] Know comparison sort lower bound
- [ ] Choose appropriate sort for scenario

### Selection
- [ ] Use pivot trick for selection
- [ ] Find median with quick select
- [ ] Understand time complexity

### Graphs
- [ ] Distinguish adjacency matrix vs list
- [ ] Perform DFS/BFS by hand
- [ ] Find topological sort
- [ ] Identify connected components

### Shortest Path
- [ ] Apply Dijkstra's algorithm
- [ ] Handle negative weights with Bellman-Ford
- [ ] Understand when to use each algorithm
- [ ] Identify edge relaxation steps

---

## Quick Formulas & Facts

### Sums
- Arithmetic: $\sum_{i=1}^{n} i = \frac{n(n+1)}{2}$
- Geometric: $\sum_{i=0}^{n-1} r^i = \frac{r^n - 1}{r - 1}$
- Powers of 2: $\sum_{i=0}^{n-1} 2^i = 2^n - 1$

### Logarithms
- $\log_2(n)$ is how many times you divide by 2 to reach 1
- $\log(ab) = \log(a) + \log(b)$
- $\log(a^n) = n \log(a)$

### Complexity Classes
- O(1) = Constant
- O(log n) = Binary search, balanced tree ops
- O(n) = Linear scan, most sorting average
- O(n log n) = Good sorts (merge, heap, quick average)
- O(n²) = Bubble, selection, insertion
- O(2ⁿ) = Exponential, avoid if possible

---

## Final Exam Tips

1. **Read carefully** - Understand what the question is asking
2. **Start with known examples** - Work through small cases first
3. **Check edge cases** - Think about n=0, n=1, empty arrays
4. **Show your work** - Partial credit often available
5. **Verify answers** - Re-read question and double-check logic
6. **Time management** - Easier questions first, harder ones later
7. **Review tradeoffs** - O(n) time vs O(n) space, etc.

---

## Common Mistakes to Avoid

1. **Confusing O(n²) with O(2ⁿ)** - Polynomial vs exponential!
2. **Forgetting base case in recursion** - Leads to infinite recursion
3. **Using Dijkstra with negative weights** - Use Bellman-Ford instead
4. **Assuming BST is balanced** - Could be O(n) not O(log n)
5. **Sorting counted as O(n)** - Comparison-based sorts need O(n log n)
6. **Not including space complexity** - Asked for both time and space
7. **Skipping edge cases** - Empty arrays, single elements, duplicates

---

## Study Tips for Tonight

1. **Review chapter by chapter** - Don't jump around
2. **Work practice problems** - Don't just read
3. **Trace algorithms by hand** - Actually write out steps
4. **Explain concepts aloud** - Teaching reinforces learning
5. **Time yourself** - Get a feel for exam pacing
6. **Group related topics** - See how concepts connect
7. **Take breaks** - Every 45 minutes, refresh your mind

---

## Good Luck! 🎓

You've covered a lot of material. Trust your preparation and think carefully through each problem. You've got this!

