# Chapter 3 - Math Background

## Arithmetic Sums

### What is an Arithmetic Sequence?

An arithmetic sequence is a list of numbers where the difference between consecutive terms is constant.

**Example:** 2, 5, 8, 11, 14...
- Common difference (d) = 3
- Each number = previous number + 3

### Arithmetic Sum Formula

**The sum of an arithmetic sequence:**

$$S = \frac{n(a_1 + a_n)}{2}$$

Where:
- $n$ = number of terms
- $a_1$ = first term
- $a_n$ = last term

**Alternative formula:**

$$S = \frac{n(2a_1 + (n-1)d)}{2}$$

Where:
- $d$ = common difference

### Example

Sum of 2 + 5 + 8 + 11 + 14:
- First term (a₁) = 2
- Last term (aₙ) = 14
- Number of terms (n) = 5

$$S = \frac{5(2 + 14)}{2} = \frac{5 \times 16}{2} = \frac{80}{2} = 40$$

## Geometric Sums

### What is a Geometric Sequence?

A geometric sequence is where each term is multiplied by a constant factor to get the next term.

**Example:** 2, 6, 18, 54...
- Common ratio (r) = 3
- Each number = previous number × 3

### Geometric Sum Formula

**The sum of a geometric sequence:**

$$S = a_1 \times \frac{1 - r^n}{1 - r}$$ (when r ≠ 1)

Or equivalently:

$$S = a_1 \times \frac{r^n - 1}{r - 1}$$

Where:
- $a_1$ = first term
- $r$ = common ratio
- $n$ = number of terms

**Special case (r = 1):** $S = n \times a_1$ (all terms are the same)

### Example

Sum of 2 + 6 + 18 + 54:
- First term (a₁) = 2
- Common ratio (r) = 3
- Number of terms (n) = 4

$$S = 2 \times \frac{3^4 - 1}{3 - 1} = 2 \times \frac{81 - 1}{2} = 2 \times \frac{80}{2} = 2 \times 40 = 80$$

### Common Geometric Sum in Computer Science

$$S = 1 + 2 + 4 + 8 + ... + 2^{n-1} = 2^n - 1$$

(This appears frequently in algorithm analysis!)

---

## Exponent Properties

### Basic Rules

| Rule | Example |
|------|---------|
| $a^m \times a^n = a^{m+n}$ | $2^3 \times 2^2 = 2^5 = 32$ |
| $\frac{a^m}{a^n} = a^{m-n}$ | $2^5 / 2^2 = 2^3 = 8$ |
| $(a^m)^n = a^{m \times n}$ | $(2^3)^2 = 2^6 = 64$ |
| $a^{-n} = \frac{1}{a^n}$ | $2^{-3} = \frac{1}{8}$ |
| $a^0 = 1$ | $5^0 = 1$ |
| $a^1 = a$ | $7^1 = 7$ |

### Important Powers of 2 (memorize these!)

| Power | Value |
|-------|-------|
| $2^0$ | 1 |
| $2^5$ | 32 |
| $2^{10}$ | 1,024 |
| $2^{20}$ | 1,048,576 |

---

## Logarithm Properties

### What is a Logarithm?

A logarithm answers: "What power do I raise the base to in order to get this number?"

$$\log_b(x) = y \text{ means } b^y = x$$

**Example:** $\log_2(8) = 3$ because $2^3 = 8$

In computer science, we almost always use **base 2** logarithms (binary logarithms).

### Basic Rules

| Rule | Meaning |
|------|---------|
| $\log_b(x \times y) = \log_b(x) + \log_b(y)$ | Log of product = sum of logs |
| $\log_b(\frac{x}{y}) = \log_b(x) - \log_b(y)$ | Log of quotient = difference of logs |
| $\log_b(x^n) = n \times \log_b(x)$ | Log of power = power times log |
| $\log_b(1) = 0$ | Log of 1 is always 0 |
| $\log_b(b) = 1$ | Log of the base is 1 |
| $\log_b(b^n) = n$ | Log of base^n is n |
| $\log_b(x) = \frac{\log_a(x)}{\log_a(b)}$ | Change of base formula |

### Examples

- $\log_2(16) = 4$ (because $2^4 = 16$)
- $\log_2(1) = 0$ (because $2^0 = 1$)
- $\log_2(32) + \log_2(4) = \log_2(128) = 7$
- $\log_2(128) - \log_2(4) = \log_2(32) = 5$

---

## Induction Proofs

### What is Mathematical Induction?

Induction is a proof technique that works like dominoes:
1. Show the first domino falls (base case)
2. Show that if one domino falls, the next one falls (inductive step)
3. Conclusion: all dominoes fall

It proves that a statement is true for ALL positive integers.

### Normal (Weak) Induction

**Structure:**

1. **Base Case**: Prove the statement is true for n = 1 (or the smallest case)
2. **Inductive Hypothesis**: Assume the statement is true for some arbitrary n = k
3. **Inductive Step**: Prove that if it's true for n = k, then it's also true for n = k + 1
4. **Conclusion**: By induction, it's true for all positive integers

### Example: Sum of First n Numbers

**Prove:** $1 + 2 + 3 + ... + n = \frac{n(n+1)}{2}$

**Base Case (n = 1):**
- Left side: 1
- Right side: $\frac{1(1+1)}{2} = \frac{2}{2} = 1$ ✓

**Inductive Hypothesis:**
Assume the formula is true for n = k:
$$1 + 2 + 3 + ... + k = \frac{k(k+1)}{2}$$

**Inductive Step:**
Prove it's true for n = k + 1:
$$1 + 2 + 3 + ... + k + (k+1) = \frac{(k+1)(k+2)}{2}$$

Left side:
$$\frac{k(k+1)}{2} + (k+1) = \frac{k(k+1) + 2(k+1)}{2} = \frac{(k+1)(k+2)}{2}$$ ✓

**Conclusion:** By mathematical induction, the formula is true for all positive integers n.

### Strong Induction

Strong induction is like normal induction, BUT you can assume the statement is true for **all values less than k**, not just k itself.

**Structure:**

1. **Base Case(s)**: Prove for n = 1, 2, 3... (whatever is needed)
2. **Inductive Hypothesis**: Assume true for all values from 1 to k
3. **Inductive Step**: Prove it's true for n = k + 1 using all previous cases
4. **Conclusion**: By strong induction, true for all positive integers

### When to Use Strong vs. Normal Induction?

- **Normal Induction**: When you only need the previous case to prove the next case
- **Strong Induction**: When you need multiple previous cases (like in recursion proof, Fibonacci, etc.)

---

## Sorting: Insertion Sort

### How Insertion Sort Works

**Idea:** Build a sorted list one element at a time by inserting elements into their correct position.

Like sorting playing cards in your hand - you pick up cards one at a time and put them in the right spot.

### Steps

```
Initial array: [5, 2, 8, 1, 9]

Pass 1: [2, 5, 8, 1, 9]     (insert 2 in correct position)
Pass 2: [2, 5, 8, 1, 9]     (8 already in position)
Pass 3: [1, 2, 5, 8, 9]     (insert 1 at beginning)
Pass 4: [1, 2, 5, 8, 9]     (9 already in position)

Result: [1, 2, 5, 8, 9]
```

### Pseudocode

```
for i = 1 to n-1:
    key = array[i]
    j = i - 1
    while j >= 0 and array[j] > key:
        array[j + 1] = array[j]
        j = j - 1
    array[j + 1] = key
```

### Time Complexity

- **Best case:** O(n) - when array is already sorted (1 comparison per pass)
- **Average case:** O(n²) - typically
- **Worst case:** O(n²) - when array is reverse sorted (many comparisons)

**Space complexity:** O(1) - sorts in place

---

## Sorting: Selection Sort

### How Selection Sort Works

**Idea:** Repeatedly find the smallest element and move it to the front of the unsorted part.

Like picking the smallest card from a pile repeatedly.

### Steps

```
Initial array: [5, 2, 8, 1, 9]

Pass 1: [1, 2, 8, 5, 9]     (find min=1, swap with position 0)
Pass 2: [1, 2, 8, 5, 9]     (find min=2, already in position)
Pass 3: [1, 2, 5, 8, 9]     (find min=5, swap with position 2)
Pass 4: [1, 2, 5, 8, 9]     (find min=8, already in position)

Result: [1, 2, 5, 8, 9]
```

### Pseudocode

```
for i = 0 to n-2:
    min_index = i
    for j = i + 1 to n-1:
        if array[j] < array[min_index]:
            min_index = j
    swap(array[i], array[min_index])
```

### Time Complexity

- **Best case:** O(n²)
- **Average case:** O(n²)
- **Worst case:** O(n²)

**Space complexity:** O(1) - sorts in place

**Important:** Selection sort always takes O(n²) time regardless of input!

---

## Key Takeaways

- **Arithmetic sum:** $\frac{n(a_1 + a_n)}{2}$
- **Geometric sum:** $a_1 \times \frac{r^n - 1}{r - 1}$
- **Exponent rules:** Multiply exponents, add exponents when multiplying bases
- **Logarithm rules:** Convert between multiplicative and additive operations
- **Normal induction:** Assume true for k, prove for k+1
- **Strong induction:** Assume true for all ≤ k, prove for k+1
- **Insertion sort:** O(n²) - good for small/nearly-sorted arrays
- **Selection sort:** Always O(n²) - good for minimizing writes

