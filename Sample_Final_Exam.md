# CS 114 – Sample Exam Walkthrough & Explanations

This document explains how to solve each question from the sample exam.  
Each section focuses on the reasoning and method needed to reach the correct answer.

---

## **Question 1 — Comparing Growth Rates**

To compare the functions:

- Simplify \(2^{\log n}\) → becomes \(n\).
- Use the standard growth hierarchy:
  \[
  (\log n)^2 \ll n \ll n^2 \log n \ll 2^n
  \]
- Convert this into Big‑O relationships.

**Correct chain:**  
\((\log n)^2 = O(n) = O(n^2 \log n) = O(2^n)\)

---

## **Question 2 — Nested Loops Runtime**

Two loops each run \(n\) times:

- Outer loop: \(n\) iterations  
- Inner loop: \(n\) iterations  
- Body: constant time  

Total work: \(n \cdot n = n^2\)

**Runtime:** \(O(n^2)\)

---

## **Question 3 — Doubling Loop**

The loop doubles a variable until it reaches \(n\):

- After \(t\) iterations: value is \(2^t\)
- Stop when \(2^t = n\)
- Solve: \(t = \log_2 n\)

**Runtime:** \(O(\log n)\)

---

## **Question 4 — BST Parent/Leaf Relationship**

If \(x\) is a leaf and child of \(y\), no value can lie strictly between them in a BST because:

- Anything between them must be in the subtree where \(x\) is the only node.
- That subtree cannot contain additional nodes.

**Answer:** True

---

## **Question 5 — Sorting by Total Digits**

The claim says sorting must take \(\Omega(n \log n)\) when \(n\) is the total number of digits.

But digit‑based algorithms (e.g., radix sort) run in **linear time** relative to digit count.

**Answer:** False

---

## **Question 6 — Finding the 5 Largest Numbers**

Scan the list once and maintain a fixed‑size structure of the top 5 values.

- Each update is \(O(1)\)
- One pass over \(n\) items → \(O(n)\)

**Answer:** True

---

## **Question 7 — Full Binary Tree Node Count**

In a full binary tree:

\[
\text{leaves} = \text{internal nodes} + 1
\]

Given 10 internal nodes:

- Leaves = 11  
- Total nodes = 21

**Total nodes:** 21

---

## **Question 8 — Sorting Algorithm Behavior**

Given:

- Random input: time slightly more than doubles → suggests \(O(n \log n)\)
- Sorted input: time quadruples → suggests worst‑case \(O(n^2)\)

Only **quicksort** behaves like this due to bad pivot choices on sorted input.

**Algorithm:** Quicksort

---

## **Question 9 — Priority Queue Lower Bound**

Insert all \(n\) items, then remove them in sorted order:

- This simulates comparison sorting.
- Must take \(\Omega(n \log n)\) in the worst case.

**Answer:** True

---

## **Question 10 — Binary Search Trace**

Midpoints when searching for 23:

1. mid = 4  
2. mid = 7  
3. mid = 5 (found)

**Mid indices:** 4, 7, 5

Binary search halves the search space each step → \(O(\log n)\)

**Runtime:** \(O(\log n)\)

---

## **Question 11 — Stack & Queue Palindrome Check**

The algorithm:

- Queue reads left‑to‑right
- Stack reads right‑to‑left
- Comparing them checks for a palindrome

Results:

- `"abccba"` → true  
- `"abcabc"` → false  

**Runtime:** \(O(n)\)  
**Space:** \(O(n)\)

---

## **Question 12 — BST Insertions & Traversal**

After inserting the keys, inorder traversal (sorted order):

**Inorder:** 1, 2, 3, 4, 8, 10, 12, 15, 18

Searching for 3 visits:

**Search path:** 10 → 4 → 2 → 3

Worst‑case search time depends on height \(h\):

**Runtime:** \(O(h)\)

---

## **Question 13 — Radix Sort on 3‑Digit Numbers**

LSD radix sort:

- 3 passes (ones, tens, hundreds)
- Each pass uses counting sort with digit range 0–9 → \(O(n)\)

**Total runtime:** \(O(n)\)

Comparison:

- Merge sort: \(O(n \log n)\)
- Radix sort: \(O(n)\) when digit count and range are constant

**Radix sort is faster for fixed‑digit numbers.**

---

## **Question 14 — Randomized Selection (Quickselect)**

### (a) Good split
\[
T(n) = T(n/2) + O(n) = O(n)
\]

### (b) Worst split (pivot always smallest)
\[
T(n) = T(n-1) + O(n) = O(n^2)
\]

### (c) Expected linear time
Random pivots avoid consistently bad splits.

### (d) Deterministic linear‑time selection
Use **median‑of‑medians**.

**Runtime:** \(O(n)\)

---

## **Question 15 — DFS Traversal Orders**

### (a) Preorder (print on visit)
**A, B, D, E, F, C**

### (b) Postorder (print on return)
**D, C, F, E, B, A**

### (c) Recursion stack before returning from D
**A → B → D**

---
