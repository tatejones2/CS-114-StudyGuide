# Chapter 9 - Sorting Algorithms

## Merge Sort

### How Merge Sort Works

Merge sort uses **divide-and-conquer**:
1. **Divide:** Split array in half repeatedly until single elements
2. **Conquer:** Merge pairs back together in sorted order

**Visual:**

```
[5, 2, 8, 1, 9, 3]
     /          \
  [5, 2, 8]    [1, 9, 3]
   /    \       /    \
[5, 2]  [8]   [1, 9]  [3]
 /  \         /  \
[5] [2]     [1] [9]

Merging back:
[2, 5]  [8]      [1, 9]  [3]
  \    /            \   /
  [2, 5, 8]        [1, 3, 9]
       \              /
      [1, 2, 3, 5, 8, 9]
```

### Steps in Detail

```
Input: [5, 2, 8, 1]

DIVIDE:
[5, 2, 8, 1]
↓
[5, 2]  [8, 1]
↓
[5]  [2]  [8]  [1]

MERGE:
[5]  [2]  →  [2, 5]
[8]  [1]  →  [1, 8]

[2, 5]  [1, 8]  →  [1, 2, 5, 8]
```

### Merge Algorithm

```python
def merge(left, right):
    result = []
    i = j = 0
    
    # Compare elements from left and right, add smaller
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    # Add remaining elements
    result.extend(left[i:])
    result.extend(right[j:])
    
    return result

def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    return merge(left, right)
```

### Time Complexity Analysis

**Recurrence relation:**
$$T(n) = 2 \times T(n/2) + n$$
- 2 recursive calls on half the array: 2 × T(n/2)
- Merge takes O(n) time

**Solving:**
$$T(n) = O(n \log n)$$

**For all cases:**

| Case | Time |
|------|------|
| Best | O(n log n) |
| Average | O(n log n) |
| Worst | O(n log n) |

**Space:** O(n) - needs extra array for merging

### Advantages & Disadvantages

**Advantages:**
- Guaranteed O(n log n)
- Stable (preserves order of equal elements)
- Predictable performance

**Disadvantages:**
- Needs O(n) extra space
- Slower than Quick Sort on average due to copying

---

## Quick Sort

### How Quick Sort Works

Quick sort uses **divide-and-conquer** with **in-place** partitioning:

1. **Pick pivot:** Choose an element as pivot
2. **Partition:** Rearrange so all smaller elements left, larger right
3. **Recursively sort:** Left and right parts

**Visual:**

```
[5, 2, 8, 1, 9, 3]

Pick pivot = 5:
[2, 1, 3] [5] [8, 9]
All < 5        All ≥ 5

Recursively sort left and right:
[1, 2, 3] [5] [8, 9]

Result: [1, 2, 3, 5, 8, 9]
```

### Partition Algorithm (Key Step!)

**Goal:** Rearrange array so pivot ends in correct position

```python
def partition(arr, low, high):
    pivot = arr[high]           # Choose last element as pivot
    i = low - 1                 # Index of smaller elements
    
    # Scan through array
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]  # Swap
    
    # Place pivot in correct position
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1                # Return pivot index
```

**Example: Partition [5, 2, 8, 1, 9, 3] with pivot=3**

```
Initial: [5, 2, 8, 1, 9, 3]
                             pivot
i = -1

j=0: 5 > 3, no swap
j=1: 2 < 3, swap → [2, 5, 8, 1, 9, 3]
j=2: 8 > 3, no swap
j=3: 1 < 3, swap → [2, 1, 8, 5, 9, 3]
j=4: 9 > 3, no swap

Place pivot: [2, 1, 3, 5, 9, 8]
             return i+1 = 2
```

### Quick Sort Algorithm

```python
def quick_sort(arr, low, high):
    if low < high:
        # Partition and get pivot index
        pi = partition(arr, low, high)
        
        # Recursively sort left and right
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)
    
    return arr
```

### Pivot Selection Strategies

**1. First Element:** Simple but bad if already sorted

**2. Last Element (Hoare-Lomuto):** Used above, reasonable

**3. Median-of-three:** Better average case
```python
pivot = median(arr[low], arr[mid], arr[high])
```

**4. Random:** Avoids worst case on sorted data
```python
random_index = random.randint(low, high)
arr[random_index], arr[high] = arr[high], arr[random_index]
```

### Time Complexity

**Depends on pivot choice:**

| Case | Time | Why |
|------|------|-----|
| Best | O(n log n) | Pivot always at middle |
| Average | O(n log n) | Random pivot balanced |
| Worst | O(n²) | Pivot always at edge (sorted data) |

**Space:** O(log n) - recursion depth

### Different Split Results

**Balanced split (good):**
```
Pivot at middle:
[elements < pivot] [pivot] [elements > pivot]
     size n/2               size n/2

Result: O(n log n)
```

**Unbalanced split (bad):**
```
Pivot at edge:
[] [pivot] [all other elements]
            size n-1

Result: O(n²) - like Bubble Sort!
```

### Quick Sort vs Merge Sort

| Property | Quick Sort | Merge Sort |
|----------|-----------|-----------|
| Average time | O(n log n) | O(n log n) |
| Worst time | O(n²) | O(n log n) |
| Space | O(log n) | O(n) |
| In-place | Yes | No |
| Stable | No | Yes |
| Cache friendly | Better | Worse |
| Practical use | More common | Databases, stability needed |

---

## Summary Comparison

| Algorithm | Best | Average | Worst | Space | Stable | Comments |
|-----------|------|---------|-------|-------|--------|----------|
| Merge Sort | n log n | n log n | n log n | O(n) | Yes | Guaranteed, needs extra space |
| Quick Sort | n log n | n log n | n² | O(log n) | No | Faster average, worse space |
| Insertion Sort | n | n² | n² | O(1) | Yes | Good for small arrays |
| Heap Sort | n log n | n log n | n log n | O(1) | No | Guaranteed, less cache friendly |
| Bubble Sort | n² | n² | n² | O(1) | Yes | Worst, avoid |

---

## Key Takeaways

- **Merge Sort:** O(n log n) guaranteed, stable, needs extra space
- **Quick Sort:** O(n log n) average, O(n²) worst case, in-place, cache friendly
- **Pivot selection:** Affects performance; use random for robustness
- **Partition:** Rearranges array around pivot, O(n) operation
- **Balanced split:** Good pivots produce balanced recursive tree
- **Choose:** Merge Sort for guarantees/stability; Quick Sort for speed/space

