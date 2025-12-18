# Chapter 6 - Dictionaries (Hash Tables)

## What is a Dictionary?

A dictionary is a data structure that stores **key-value pairs**. You look up values by their keys, similar to a real dictionary where you look up words to find definitions.

```python
dictionary = {"name": "Alice", "age": 25, "city": "NYC"}
print(dictionary["name"])  # Returns "Alice"
```

**Key operations:**
- **Insert/Update**: `dict[key] = value`
- **Lookup**: `dict[key]` returns value
- **Delete**: `del dict[key]`
- **Check existence**: `key in dict`

---

## Hash Table Implementation (Most Common)

### Core Idea

Use a **hash function** to convert keys into array indices. This allows O(1) average-case lookup!

```
Key → Hash Function → Index → Value
"Alice" → hash("Alice") → 3 → 25 (age)
```

### How It Works

1. **Hash Function:** Takes a key and returns a number (index)
   ```python
   def hash_function(key, table_size):
       return len(key) % table_size
   ```

2. **Store in array:** Put the value at the computed index
   ```python
   array[hash_function(key)] = value
   ```

3. **Lookup:** Compute the same hash and look at that index
   ```python
   value = array[hash_function(key)]
   ```

### Example

Hash table with 5 slots, hashing by first letter:

```
Key: "Alice" → Hash: 0 → Slot 0: ("Alice", 25)
Key: "Bob"   → Hash: 1 → Slot 1: ("Bob", 30)
Key: "Alice" → Hash: 0 → Slot 0: (update to new value)
```

### Time Complexity (Average Case)

| Operation | Time |
|-----------|------|
| Insert | O(1) |
| Lookup | O(1) |
| Delete | O(1) |

---

## Problem: Hash Collisions

**What if two different keys hash to the same index?**

```
Key: "Alice" → Hash: 3
Key: "Anna"  → Hash: 3  ← COLLISION!
```

### Solution 1: Chaining (Most Common)

Store a **linked list** at each index to handle multiple values:

```
Slot 0: Slot 1: Slot 2: Slot 3: ["Alice"→25, "Anna"→35]
Slot 4:
```

When looking up "Anna":
1. Compute hash = 3
2. Go to slot 3
3. Search through linked list for "Anna"

**Time complexity with chaining:**
- **Average case:** O(1 + α) where α = load factor
  - Load factor = number of keys / table size
  - α ≈ 1 means balanced table
- **Worst case:** O(n) if many collisions

### Solution 2: Open Addressing

If slot is occupied, try next slots: (i+1), (i+2), (i+3)...

**Methods:**
- **Linear probing:** Check i, i+1, i+2, ...
- **Quadratic probing:** Check i, i+1, i+4, i+9, ...
- **Double hashing:** Use a second hash function

**Problem:** Clustering - many collisions pile up in one area

---

## Balanced Search Tree Implementation (Alternative)

Instead of hashing, use a **balanced binary search tree** (like AVL tree or Red-Black tree).

**Time Complexity:**

| Operation | Time |
|-----------|------|
| Insert | O(log n) |
| Lookup | O(log n) |
| Delete | O(log n) |

**Advantages:**
- No collisions
- Ordered keys
- Worst-case O(log n) guaranteed

**Disadvantages:**
- Slower than hash table average case
- More complex to implement

---

## Other Implementations

### Skip List

A probabilistic data structure combining features of linked lists and search trees.

**Time Complexity:** O(log n) average case (simpler than BST)

### Bloom Filter

A space-efficient probabilistic structure for set membership (may have false positives but not false negatives).

**Use case:** Quick check if element might be in set (not definite)

---

## Choosing Dictionary Implementation

### Use Hash Table When:
- Average-case O(1) is important
- Order of keys doesn't matter
- Most operations are lookup/insert/delete
- Memory available for load factor < 1

### Use Balanced BST When:
- Guaranteed O(log n) worst-case is needed
- Need to iterate keys in sorted order
- Keys are very large (many collisions likely)
- Want simple collision handling

### Use Skip List When:
- Need ordered keys with simpler implementation than BST
- Space constraints (no extra pointers per level)

---

## Load Factor & Resizing

**Load Factor (α)** = Number of elements / Table size

**Why it matters:**
- Low α (α < 0.5): Fast lookup, wasted space
- High α (α > 0.75): Slow lookup due to collisions

**Resizing (Rehashing):**
When α exceeds threshold (usually 0.75):
1. Create new larger table (usually 2x size)
2. Rehash all keys into new table
3. Time: O(n) per resize

**Amortized:** O(1) per insertion despite occasional O(n) resize

---

## Hash Function Properties

A good hash function should:

1. **Deterministic:** Same key always produces same hash
2. **Uniform:** Keys distributed evenly across table
3. **Efficient:** Fast to compute (O(1) or close)
4. **Minimize collisions:** Different keys should hash differently

**Common hash functions:**
- **String:** Sum of ASCII values, then mod table_size
- **Integer:** Key mod table_size
- **Polynomial rolling hash:** Used in string matching

### Example Good Hash Function

```python
def hash_string(key, table_size):
    hash_value = 0
    for char in key:
        hash_value = (hash_value * 31 + ord(char)) % table_size
    return hash_value
```

(31 is a prime number, helps avoid collisions)

---

## Dictionary Applications

1. **Database indexing:** Quick lookup by ID
2. **Symbol tables:** Compilers store variable names
3. **Cache:** Store recently used data for fast retrieval
4. **Graphs:** Adjacency list representation
5. **Spell checker:** Set of valid words
6. **De-duplication:** Count occurrences (set or map)

---

## Key Takeaways

- **Dictionary** stores key-value pairs for fast lookup
- **Hash table** achieves O(1) average case using hash function
- **Collisions** handled by chaining (linked list) or open addressing
- **Load factor** (α) affects performance; resize when too high
- **Chaining** is simpler, open addressing uses less memory
- **Choose** hash table for speed, BST for ordering and guarantees

