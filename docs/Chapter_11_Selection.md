# Chapter 11 - Selection Algorithms

## Pivot Trick Review

### What is the Pivot Trick?

The **pivot trick** (also called **partition**) is a technique that:
1. Picks one element as **pivot**
2. Rearranges array so all smaller elements are left, larger elements are right
3. **Guarantees** pivot ends up in its correct final position

**Example:**

```
Before:  [5, 2, 8, 1, 9, 3]
Pick pivot = 5

After partition:
[3, 2, 1, 5, 9, 8]
         ↑
    Pivot in correct position!
    (3 elements smaller, 2 elements larger)
```

### How Pivot Trick Works

**Pseudocode (Hoare-Lomuto partition):**

```python
def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1
```

### Key Insight

After partition, the pivot is in position k where:
- k = number of elements smaller than pivot
- Exactly k elements are to the left
- Exactly (n - k - 1) elements are to the right

This property is **crucial** for selection algorithms.

---

## Selection Problem

### What is Selection?

Given an unsorted array and a number k, find the **kth smallest element**.

**Examples:**
- Find median: k = n/2
- Find minimum: k = 1
- Find maximum: k = n
- Find 90th percentile: k = 0.9×n

### Naive Approach

Sort the array (O(n log n)) then return element at index k.

**Better approach:** Use pivot trick! We can find kth element without fully sorting.

---

## Quick Select (Randomized Selection)

### Algorithm Idea

Use pivot trick recursively, but only recurse on the side containing the kth element.

**Key insight:** After partition, we know exactly which side contains position k:
- If k < pivot position: Recurse left
- If k > pivot position: Recurse right
- If k == pivot position: Found it!

### Steps

```
Find 3rd smallest in [5, 2, 8, 1, 9, 3, 7]

Partition around pivot=5:
[3, 2, 1] [5] [8, 9, 7]
  k=3       k=4

Position of 5 is 3
Target k=3 equals position 3
→ Answer is 5
```

Another example: Find 5th smallest in [5, 2, 8, 1, 9, 3, 7]

```
Partition around pivot=5:
[3, 2, 1] [5] [8, 9, 7]
Positions: 0-2  3  4-6

Target k=5 is > 3, so search right
Recurse on [8, 9, 7] looking for (5-3-1)=1st smallest

Partition [8, 9, 7]:
[7] [8] [9]
 0   1   2

Target 1st smallest is at position 0
→ Answer is 7
```

### Pseudocode

```python
def quick_select(arr, k):
    # Find kth smallest (0-indexed)
    return quick_select_helper(arr, 0, len(arr) - 1, k - 1)

def quick_select_helper(arr, low, high, k):
    if low == high:
        return arr[low]
    
    # Partition
    pivot_index = partition(arr, low, high)
    
    if k == pivot_index:
        return arr[k]
    elif k < pivot_index:
        return quick_select_helper(arr, low, pivot_index - 1, k)
    else:
        return quick_select_helper(arr, pivot_index + 1, high, k)
```

### Time Complexity

**Analysis:**
- **Best case:** O(n) - pivot is always at position k, no recursion needed
- **Average case:** O(n) - each iteration roughly halves search space
  - T(n) = T(n/2) + O(n) = O(n)
- **Worst case:** O(n²) - pivot always at extreme position

**With random pivot selection:** Average case O(n) is guaranteed with high probability

### Example Time Analysis

**Average case:**
```
First iteration: n operations
Second iteration: n/2 operations
Third iteration: n/4 operations
...

Total: n + n/2 + n/4 + ... = 2n = O(n)
```

---

## Deterministic Selection

### Problem with Randomized

Worst case is O(n²) if unlucky with pivots. Can we guarantee O(n)?

### Median-of-Medians Algorithm

**Idea:** Choose a "good" pivot that guarantees reasonable split, not random.

**Steps:**
1. Divide array into groups of 5
2. Find median of each group (sort 5 elements)
3. Recursively find median-of-medians
4. Use that as pivot
5. Partition and recurse

**Time Complexity:** O(n) guaranteed

### Why O(n)?

With median-of-medians pivot:
- Guaranteed at least 30% of elements on each side (balanced split)
- Recursion tree is shallow
- T(n) = T(n/5) + T(7n/10) + O(n) = O(n)

### Trade-off

- **Quick Select:** Simpler, O(n) average, O(n²) worst
- **Median-of-Medians:** O(n) guaranteed, but larger constants

**In practice:** Quick Select is faster due to simpler pivot selection.

---

## Practical Considerations

### When to Use What

**Use Quick Select when:**
- Selecting from large unsorted data
- Average O(n) is acceptable
- Implementation simplicity matters

**Use Median-of-Medians when:**
- Hard guarantee needed
- Can't afford O(n²) worst case

**Use Sorting when:**
- Selecting multiple elements
- Already sorted
- Data very small

### Cache Efficiency

Quick Select has better cache locality than sorting because:
- Processes data in smaller chunks
- Better spatial locality
- Fewer memory accesses

---

## Applications

1. **Database queries:** Find kth percentile value
2. **Statistics:** Calculate median
3. **Load balancing:** Find threshold value splitting load evenly
4. **Top-k elements:** Find k largest/smallest efficiently
5. **Voting:** Find majority element

---

## Example: Find Median

```python
def find_median(arr):
    n = len(arr)
    if n % 2 == 1:
        # Odd: return middle
        k = n // 2 + 1
        return quick_select(arr, k)
    else:
        # Even: return average of two middle
        k1 = n // 2
        k2 = n // 2 + 1
        return (quick_select(arr, k1) + quick_select(arr, k2)) / 2
```

---

## Key Takeaways

- **Pivot trick:** Rearranges array, guarantees pivot position
- **Quick Select:** Find kth element in O(n) average time
- **Better than sorting:** Avoid O(n log n) when only need one element
- **Randomized:** Simple, practical, O(n) average
- **Deterministic:** Median-of-medians guarantees O(n) worst case
- **Applications:** Percentiles, medians, thresholds

