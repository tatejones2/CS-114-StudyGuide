# Chapter 4 - Algorithm Analysis

## Asymptotic Notation & Growth Rates

### What is Asymptotic Analysis?

Asymptotic analysis helps us understand how an algorithm's runtime or space grows as the input size gets very large. We focus on the **dominant term** - the part that matters most when n is huge.

**Example:** If an algorithm takes $3n^2 + 5n + 100$ operations:
- For small n (like n=10): The constant 100 matters
- For large n (like n=1,000,000): Only the $n^2$ term matters
- As n → ∞, we say it's O(n²)

We ignore:
- Constant coefficients (3n² vs 2n² are both O(n²))
- Lower-order terms (n² + 5n both have the same growth rate)
- Constant additions (n² + 100 is still O(n²))

### Big-O Notation: Upper Bound

**O(f(n))** means the algorithm takes **at most** $c \times f(n)$ time for some constant c, when n is sufficiently large.

**In plain English:** "The algorithm won't be worse than this."

**Common Big-O complexities (from fastest to slowest):**

| Notation | Name | Behavior |
|----------|------|----------|
| O(1) | Constant | Always the same time, no matter input size |
| O(log n) | Logarithmic | Doubles in time when input size is squared |
| O(n) | Linear | Time grows proportional to input size |
| O(n log n) | Linearithmic | Better than quadratic |
| O(n²) | Quadratic | Time quadruples when input size doubles |
| O(n³) | Cubic | Even worse than quadratic |
| O(2ⁿ) | Exponential | Doubles in time when input size increases by 1 |
| O(n!) | Factorial | Extremely slow |

### Big-Omega Notation: Lower Bound

**Ω(f(n))** means the algorithm takes **at least** $c \times f(n)$ time.

**In plain English:** "The algorithm will be at least this slow."

### Big-Theta Notation: Tight Bound

**Θ(f(n))** means the algorithm takes **exactly** (within constant factors) $c \times f(n)$ time.

**In plain English:** "The algorithm is bounded both above and below by this function."

### Visual Comparison

```
O(1)    ═════════════════  (flat line, fastest)
O(log n) \
          \___
               O(n) /
                   /  O(n²) /
                        /  O(2ⁿ) /
                            /  (shoots up, slowest)
```

### Simplification Rules

When you have a function like $5n^3 + 2n^2 + n + 100$:

1. **Drop constants:** $n^3 + 2n^2 + n + 100$
2. **Drop lower-order terms:** $n^3$
3. **Result:** O(n³)

**More examples:**
- $10n^2 + 5n$ → O(n²)
- $2^n + n^100$ → O(2ⁿ) (exponential dominates polynomial!)
- $100n^2 + 1000n + 5000$ → O(n²)

---

## Algorithm Analysis Method

### Step 1: Count the Operations

Count the number of **basic operations** (comparisons, assignments, arithmetic).

### Step 2: Identify the Loop Structure

**Single loop:**
```python
for i in range(n):
    do_something()  # O(1)
```
**Time: O(n)** - loop runs n times, each operation is O(1)

**Nested loops:**
```python
for i in range(n):
    for j in range(n):
        do_something()  # O(1)
```
**Time: O(n²)** - outer loop n times, inner loop n times = n × n operations

**Sequential loops (not nested):**
```python
for i in range(n):
    do_something()  # O(n)

for j in range(n):
    do_something()  # O(n)
```
**Time: O(n)** - O(n) + O(n) = O(n), we drop the constant coefficient

**Loop with half each iteration:**
```python
i = n
while i > 0:
    do_something()
    i = i // 2
```
**Time: O(log n)** - we halve i each time (log₂ n iterations)

### Step 3: Write the Final Time Complexity

Combine all the parts and simplify using the rules above.

### Example Analysis: Bubble Sort

```python
def bubble_sort(arr):
    n = len(arr)                    # O(1)
    for i in range(n):              # Runs n times
        for j in range(n - 1 - i):  # Runs n, n-1, n-2... times
            if arr[j] > arr[j + 1]: # O(1) comparison
                arr[j], arr[j + 1] = arr[j + 1], arr[j]  # O(1) swap
```

**Analysis:**
- Outer loop: n iterations
- Inner loop: average of n/2 iterations
- Total operations: $n \times \frac{n}{2} = \frac{n^2}{2}$
- **Time Complexity: O(n²)**

---

## Binary Search

### What is Binary Search?

Binary search is a fast way to find a specific element in a **sorted array** by repeatedly cutting the search space in half.

**Like a guessing game:** I'm thinking of a number 1-100. You guess 50. I say "higher." You guess 75. I say "lower." Etc.

### Steps

**Input:** Sorted array and target value

```
1. Set left = 0, right = length - 1
2. While left ≤ right:
   a. Calculate middle = (left + right) / 2
   b. If array[middle] == target:
      → Found it! Return middle
   c. If array[middle] < target:
      → Search right half: left = middle + 1
   d. If array[middle] > target:
      → Search left half: right = middle - 1
3. If we exit the loop:
   → Element not found, return -1
```

### Example

Find 7 in sorted array: [1, 3, 5, 7, 9, 11, 13]

```
Initial: left=0, right=6, target=7

Step 1: middle = 3, array[3] = 7
        → Found! Return index 3
```

Another example: Find 6 in [1, 3, 5, 7, 9, 11, 13]

```
Initial: left=0, right=6, target=6

Step 1: middle = 3, array[3] = 7
        7 > 6, so right = 2

Step 2: left=0, right=2, middle = 1
        array[1] = 3
        3 < 6, so left = 2

Step 3: left=2, right=2, middle = 2
        array[2] = 5
        5 < 6, so left = 3

Step 4: left=3, right=2
        left > right, exit loop
        → Not found, return -1
```

### Pseudocode

```
function binarySearch(arr, target):
    left = 0
    right = length(arr) - 1
    
    while left <= right:
        middle = (left + right) / 2
        
        if arr[middle] == target:
            return middle
        else if arr[middle] < target:
            left = middle + 1
        else:
            right = middle - 1
    
    return -1  // not found
```

### Time Complexity

**Key insight:** Each comparison eliminates half the remaining elements.

- Array size: n → n/2 → n/4 → n/8 → ... → 1
- Number of halvings needed: log₂(n)

**Time Complexity: O(log n)** - very fast!

**Space Complexity: O(1)** - only uses a few variables

### Comparison Method (Analysis Technique)

To compare algorithm complexities:

**Visually:** 
- O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ)

**Numerically** (for n = 1,000,000):
- O(1) = 1 operation
- O(log n) ≈ 20 operations
- O(n) = 1,000,000 operations
- O(n log n) ≈ 20,000,000 operations
- O(n²) = 10¹² operations (too slow!)
- O(2ⁿ) = 2^1,000,000 operations (impossible!)

---

## Key Takeaways

- **Asymptotic notation** focuses on growth rate, ignoring constants and lower-order terms
- **Big-O** is an upper bound (worst case usually)
- **Algorithm analysis**: Count operations, identify loop structure, simplify
- **Binary search**: O(log n), only works on sorted arrays, very efficient
- **Growth rate comparison**: Exponential > Polynomial > Logarithmic

