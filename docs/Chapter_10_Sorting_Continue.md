# Chapter 10 - Advanced Sorting

## Comparison-Based Sorting Lower Bound

### The Lower Bound Theorem

**Theorem:** Any comparison-based sorting algorithm requires **at least Ω(n log n)** comparisons in the worst case.

**Meaning:**
- No comparison-based sort can be faster than O(n log n)
- Merge Sort and Heap Sort achieve this lower bound (optimal)
- Quick Sort achieves it on average
- Bubble Sort, Insertion Sort don't (they're O(n²))

### Why This Lower Bound?

**Proof idea:** Consider a sorting problem with n elements.
- Number of possible outputs (permutations): n!
- Each comparison gives 1 bit of information (< or ≥)
- To distinguish n! outcomes, need at least log₂(n!) comparisons

$$\log_2(n!) = \Theta(n \log n)$$

**Conclusion:** You need at least Θ(n log n) comparisons.

### Implication

Any algorithm that improves on O(n log n) must use **non-comparison** methods:
- Counting Sort
- Radix Sort
- Bucket Sort

---

## Stable vs Unstable Sorting

### What is Stability?

A sort is **stable** if elements with equal keys maintain their relative order after sorting.

**Example:** Sort by grade

```
Before: [(Alice, A), (Bob, A), (Charlie, B)]
           1st         2nd

After stable sort:
[(Alice, A), (Bob, A), (Charlie, B)]
  1st        2nd

After unstable sort (might be):
[(Bob, A), (Alice, A), (Charlie, B)]
  Alice and Bob switched!
```

### Stable vs Unstable Algorithms

| Stable | Unstable |
|--------|----------|
| Merge Sort | Quick Sort |
| Insertion Sort | Heap Sort |
| Counting Sort | Selection Sort |
| Radix Sort | — |

### When Does Stability Matter?

**Matters when:**
- You have multiple keys to sort by
- You need records grouped but in original order

**Doesn't matter when:**
- Sorting simple values (numbers, strings)
- No secondary key importance

---

## Counting Sort

### Idea

If you know all values are in range [0, k], count occurrences and reconstruct sorted array.

**Example:** Sort [3, 1, 4, 1, 5, 9, 2, 6] with all values ≤ 10

```
Value:  0  1  2  3  4  5  6  7  8  9 10
Count:  0  2  1  1  1  1  1  0  0  1  0
         (Two 1's, one 2, one 3, etc.)
```

### Steps

```
Input: [3, 1, 4, 1, 5]

Step 1: Count occurrences
count[0] = 0
count[1] = 2  (two 1's)
count[2] = 0
count[3] = 1  (one 3)
count[4] = 1
count[5] = 1

Step 2: Reconstruct in sorted order
Output: [1, 1, 3, 4, 5]
```

### Pseudocode (Stable Version)

```python
def counting_sort(arr, max_val):
    count = [0] * (max_val + 1)
    
    # Count occurrences
    for num in arr:
        count[num] += 1
    
    # Reconstruct sorted array
    result = []
    for i in range(max_val + 1):
        result.extend([i] * count[i])
    
    return result
```

### Time Complexity

- **Count phase:** O(n)
- **Reconstruct phase:** O(n + k) where k = max value
- **Total:** O(n + k)

**When k ≤ n (typical):** O(n)
**When k >> n:** Not efficient

**Space:** O(n + k)

### Advantages & Disadvantages

**Advantages:**
- Linear time O(n + k)
- Stable
- Simple to implement

**Disadvantages:**
- Only for non-negative integers (or can offset)
- Not efficient when k >> n
- Extra space needed

---

## Radix Sort

### Idea

Sort by individual digits, from least significant (rightmost) to most significant (leftmost).

**Real-world analogy:** Sort mail by ZIP code digits from right to left.

### Steps

**Example: Sort [329, 457, 657, 839, 436, 720, 355]**

```
Original: [329, 457, 657, 839, 436, 720, 355]

Pass 1 (Sort by ones place):
329 → 9
457 → 7
657 → 7
839 → 9
436 → 6
720 → 0
355 → 5

After: [720, 436, 457, 657, 355, 329, 839]

Pass 2 (Sort by tens place):
720 → 2
436 → 3
457 → 5
657 → 5
355 → 5
329 → 2
839 → 3

After: [720, 329, 436, 839, 457, 355, 657]

Pass 3 (Sort by hundreds place):
720 → 7
329 → 3
436 → 4
839 → 8
457 → 4
355 → 3
657 → 6

After: [329, 355, 436, 457, 657, 720, 839] ✓
```

**Key:** Must use **stable sort** for each digit pass!

### Pseudocode

```python
def radix_sort(arr, base=10):
    max_num = max(arr)
    exp = 1  # Current digit place (1, 10, 100...)
    
    while max_num // exp > 0:
        # Use counting sort on current digit
        arr = counting_sort_by_digit(arr, exp, base)
        exp *= base
    
    return arr

def counting_sort_by_digit(arr, exp, base):
    count = [0] * base
    result = [0] * len(arr)
    
    # Count digits
    for num in arr:
        digit = (num // exp) % base
        count[digit] += 1
    
    # Convert to cumulative
    for i in range(1, base):
        count[i] += count[i - 1]
    
    # Build result (backward to maintain stability)
    for i in range(len(arr) - 1, -1, -1):
        digit = (arr[i] // exp) % base
        count[digit] -= 1
        result[count[digit]] = arr[i]
    
    return result
```

### Time Complexity

**Let:**
- n = number of elements
- d = number of digits
- k = base (usually 10)

**Time:** O(d × (n + k))

**For fixed range (like 32-bit integers):**
- d = constant (e.g., 32 bits)
- **Result:** O(n)

**For d proportional to log of max value:**
- d = log_k(max_value)
- **Time:** O(n log n) — not better than comparison sorts

### Comparison with Counting Sort

```
Counting sort: Works when max_value is small
Radix sort: Works for larger values by processing digits

Example: Sort integers 0 to 1,000,000
- Counting sort: O(n + 1,000,000) = O(n) but uses lots of space
- Radix sort: O(d × (n + 10)) where d=7 ≈ O(n) and uses less space
```

### Applications

- **Sorting by multiple keys:** (date, then name, then ID)
- **Lexicographic sorting:** Dictionary order
- **Network routing:** IP addresses

---

## Bucket Sort

### Idea

Distribute elements into buckets based on value range, sort each bucket, concatenate.

**Example: Sort 15 numbers in range [0, 1)**

```
Buckets: [0-0.1) [0.1-0.2) [0.2-0.3) ... [0.9-1)
```

### Steps

```
Input: [0.23, 0.51, 0.78, 0.12, 0.91, 0.45]

Create 10 buckets (for 0-1 range):

Bucket 0: [0.01, 0.09]
Bucket 1: [0.12]
Bucket 2: [0.23]
Bucket 3: []
Bucket 4: [0.45]
Bucket 5: [0.51]
...
Bucket 7: [0.78]
Bucket 9: [0.91]

Sort each bucket (small, so quick):

Bucket 1: [0.12]
Bucket 2: [0.23]
Bucket 4: [0.45]
Bucket 5: [0.51]
Bucket 7: [0.78]
Bucket 9: [0.91]

Concatenate: [0.12, 0.23, 0.45, 0.51, 0.78, 0.91] ✓
```

### Time Complexity

**Assumptions:**
- n elements uniformly distributed
- k buckets

**Time:**
- Distribution: O(n)
- Sorting each bucket: O(k × (n/k)²) average = O(n²/k)
- Concatenation: O(n)

**Average case:** O(n + n²/k)
- If k ≈ n, then O(n)

**Worst case:** O(n²) if all elements in one bucket

### Advantages & Disadvantages

**Advantages:**
- O(n) average case with right distribution
- Stable if sub-sort is stable
- External sorting (can use disk)

**Disadvantages:**
- Needs good distribution knowledge
- Extra space for buckets
- O(n²) worst case

---

## Summary of Non-Comparison Sorts

| Algorithm | Time | Space | Stable | When to Use |
|-----------|------|-------|--------|-------------|
| Counting Sort | O(n+k) | O(n+k) | Yes | Small integer range |
| Radix Sort | O(d×(n+k)) | O(n+k) | Yes | Integers, fixed digits |
| Bucket Sort | O(n) avg | O(n+k) | Yes | Uniformly distributed data |

---

## Key Takeaways

- **Lower bound:** Comparison sorts need Ω(n log n) minimum
- **Stable sort:** Maintains order of equal elements
- **Counting sort:** O(n) if range small, requires integer data
- **Radix sort:** Sort by digits, O(n) for fixed-size numbers
- **Bucket sort:** O(n) average if uniform distribution
- **Non-comparison:** Better than O(n log n) only with constraints

