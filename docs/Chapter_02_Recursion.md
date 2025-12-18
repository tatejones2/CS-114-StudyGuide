# Chapter 2 - Recursion

## What is Recursion?

Recursion is when a function calls **itself** to solve a problem. Instead of using loops, you break a big problem into smaller versions of the same problem until you reach a simple case you can solve directly.

Think of it like Russian nesting dolls: each doll contains a smaller doll inside, until you reach the tiniest doll that can't open anymore.

## Key Components of Recursion

Every recursive function must have:

1. **Base Case**: The simplest version of the problem that you can solve directly without recursion. This is the "stopping point" - without it, your function would call itself forever and crash.

2. **Recursive Case**: The function calls itself with a simpler/smaller version of the original problem, moving toward the base case.

### Example: Factorial

**Problem**: Calculate n! (factorial)
- 5! = 5 × 4 × 3 × 2 × 1 = 120

**Non-recursive (iterative) approach:**
```python
def factorial_loop(n):
    result = 1
    for i in range(1, n + 1):
        result *= i
    return result
```

**Recursive approach:**
```python
def factorial(n):
    # BASE CASE: the simplest version we can solve directly
    if n == 0 or n == 1:
        return 1
    
    # RECURSIVE CASE: call the function with a smaller problem
    else:
        return n * factorial(n - 1)
```

**How it works:**
```
factorial(5)
= 5 * factorial(4)
= 5 * (4 * factorial(3))
= 5 * (4 * (3 * factorial(2)))
= 5 * (4 * (3 * (2 * factorial(1))))
= 5 * (4 * (3 * (2 * 1)))  ← base case reached
= 5 * (4 * (3 * 2))
= 5 * (4 * 6)
= 5 * 24
= 120
```

The function keeps calling itself with smaller numbers (5 → 4 → 3 → 2 → 1) until it hits the base case (1), then returns back up the chain, multiplying as it goes.

## Another Example: Sum of Array

**Recursive problem**: Find the sum of all numbers in an array

```python
def sum_array(arr):
    # BASE CASE: if array is empty or has one element
    if len(arr) == 0:
        return 0
    if len(arr) == 1:
        return arr[0]
    
    # RECURSIVE CASE: take first element + sum of the rest
    else:
        return arr[0] + sum_array(arr[1:])
```

**How it works:**
```
sum_array([1, 2, 3, 4])
= 1 + sum_array([2, 3, 4])
= 1 + (2 + sum_array([3, 4]))
= 1 + (2 + (3 + sum_array([4])))
= 1 + (2 + (3 + 4))  ← base case reached
= 1 + (2 + 7)
= 1 + 9
= 10
```

## Algorithm: Converting Iterative to Recursive

### Step 1: Identify the Base Case
Ask yourself: "What is the simplest version of this problem?"
- For factorial: 0! = 1 or 1! = 1
- For summing an array: empty array = 0

### Step 2: Identify the Recursive Case
Ask yourself: "How can I break this problem into a smaller version of itself?"
- For factorial: n! = n × (n-1)!
- For summing an array: sum(arr) = arr[0] + sum(arr[1:])

### Step 3: Make Sure You're Making Progress
The recursive call must move toward the base case:
- Factorial: n gets smaller each time
- Array sum: array gets shorter each time

### Step 4: Write the Function
```python
def recursive_function(problem_parameter):
    # BASE CASE(S)
    if simple_condition:
        return simple_solution
    
    # RECURSIVE CASE
    else:
        return some_operation(recursive_function(smaller_problem))
```

## Example Conversion: Fibonacci Sequence

**Problem**: Find the nth Fibonacci number
- Sequence: 0, 1, 1, 2, 3, 5, 8, 13...
- Each number = sum of previous two numbers

**Iterative version:**
```python
def fibonacci_loop(n):
    if n <= 1:
        return n
    
    prev, curr = 0, 1
    for i in range(2, n + 1):
        prev, curr = curr, prev + curr
    return curr
```

**Recursive version:**
```python
def fibonacci(n):
    # BASE CASES
    if n == 0:
        return 0
    if n == 1:
        return 1
    
    # RECURSIVE CASE: fib(n) = fib(n-1) + fib(n-2)
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)
```

## Why Use Recursion?

- **Pros:**
  - More natural and readable for problems that are inherently recursive (tree traversal, divide-and-conquer)
  - Elegant and concise code

- **Cons:**
  - Uses more memory (stores each function call in the call stack)
  - Can be slower than iterative solutions
  - Risk of stack overflow if recursion is too deep

## Common Mistakes

1. **Forgetting the base case** → Infinite recursion (crash!)
2. **Base case never reached** → Infinite recursion
3. **Not making progress toward base case** → Infinite recursion
4. **Wrong base case condition** → Incorrect results

## Key Takeaway

**Recursion = A function solving a problem by calling itself with a simpler version, until reaching a base case it can solve directly.**

