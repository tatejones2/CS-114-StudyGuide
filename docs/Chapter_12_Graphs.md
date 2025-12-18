# Chapter 12 - Graphs

## Graph Basics & Terminology

### What is a Graph?

A graph is a collection of **nodes** (vertices) connected by **edges**. Used to represent relationships, networks, paths.

```
Nodes: {A, B, C, D}
Edges: {A-B, B-C, C-D, A-D}

Visual:
    A ─── B
    │     │
    D ─── C
```

### Key Terminology

**Node/Vertex:** Individual element in graph

**Edge:** Connection between two nodes
- **Directed edge:** A→B (one way)
- **Undirected edge:** A—B (two way)

**Graph type:**
- **Directed graph (Digraph):** Edges have direction
- **Undirected graph:** Edges are bidirectional
- **Weighted graph:** Edges have costs/weights
- **Unweighted graph:** All edges equal

**Degree:**
- **In-degree:** Number of edges pointing in
- **Out-degree:** Number of edges pointing out

**Path:** Sequence of nodes connected by edges

**Cycle:** Path that starts and ends at same node

**Connected:** Path exists between every pair of nodes

**Strongly connected:** (Directed) Path exists in both directions between every pair

**Sparse graph:** Few edges (≈ n edges)
**Dense graph:** Many edges (≈ n² edges)

---

## Graph Representations

### Representation 1: Adjacency Matrix

Use a 2D array where A[i][j] = 1 if edge exists from i to j.

```
Nodes: 0, 1, 2, 3

     0  1  2  3
0 [  0  1  0  1 ]
1 [  1  0  1  0 ]
2 [  0  1  0  1 ]
3 [  1  0  1  0 ]

Represents:
0 -- 1
|    |
3 -- 2
```

### Adjacency Matrix Properties

| Property | Value |
|----------|-------|
| Space | O(V²) where V = vertices |
| Check edge (i,j) | O(1) |
| Add/remove edge | O(1) |
| List neighbors | O(V) |
| Good for | Dense graphs, quick lookups |
| Bad for | Sparse graphs (wastes space) |

**For weighted graph:** Store weight instead of 1

```
     0   1   2   3
0 [  0   5   ∞   2 ]
1 [  5   0   3   ∞ ]
2 [  ∞   3   0   1 ]
3 [  2   ∞   1   0 ]
```

---

### Representation 2: Adjacency List

For each node, store list of neighbors.

```
Nodes: 0, 1, 2, 3

0: [1, 3]
1: [0, 2]
2: [1, 3]
3: [0, 2]

Represents same graph:
0 -- 1
|    |
3 -- 2
```

### Adjacency List Properties

| Property | Value |
|----------|-------|
| Space | O(V + E) where E = edges |
| Check edge (i,j) | O(degree of i) ≤ O(V) |
| Add/remove edge | O(1) or O(degree) |
| List neighbors | O(degree) |
| Good for | Sparse graphs, iteration |
| Bad for | Dense graphs, dense lookups |

---

### Comparison

| Task | Adjacency Matrix | Adjacency List |
|------|-----------------|----------------|
| Space (sparse) | O(V²) wastes | O(V+E) good |
| Space (dense) | O(V²) same | O(V²) same |
| Edge lookup | O(1) | O(degree) |
| Neighbor iteration | O(V) | O(degree) |
| Add edge | O(1) | O(1) |
| Remove edge | O(1) | O(degree) |
| Dense graphs | Better constants | Worse |
| Sparse graphs | Wastes space | Better |

**In practice:** Adjacency list more common

---

## Depth-First Search (DFS)

### What is DFS?

Explore a graph by going as deep as possible before backtracking. Uses a **stack** (explicit or via recursion).

**Analogy:** Explore a maze by always going forward until stuck, then backtrack.

### Algorithm

**Recursive version:**

```python
def dfs(node, visited):
    if node in visited:
        return
    
    visited.add(node)
    print(node)  # Process node
    
    for neighbor in graph[node]:
        dfs(neighbor, visited)
```

**Iterative version (with stack):**

```python
def dfs_iterative(start):
    visited = set()
    stack = [start]
    
    while stack:
        node = stack.pop()  # Remove from top
        
        if node in visited:
            continue
        
        visited.add(node)
        print(node)  # Process node
        
        for neighbor in graph[node]:
            if neighbor not in visited:
                stack.append(neighbor)
```

### Example

```
Graph:      DFS from node 0:
    0
   / \      Visit: 0
  1   2     Push: 1, 2
  |   |     Pop: 2
  3   4     Visit: 2
            Push: 4
            Pop: 4
            Visit: 4
            Pop: 1
            Visit: 1
            Push: 3
            Pop: 3
            Visit: 3

Order: 0, 2, 4, 1, 3
```

### Time Complexity

- **Visit each node:** O(V)
- **Explore each edge:** O(E)
- **Total:** O(V + E)

**Space:** O(V) for recursion stack (worst case: O(V) deep tree)

---

## Breadth-First Search (BFS)

### What is BFS?

Explore a graph level by level. Uses a **queue**.

**Analogy:** Explore a maze by exploring all adjacent rooms before going deeper.

### Algorithm

```python
from collections import deque

def bfs(start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    
    while queue:
        node = queue.popleft()  # Remove from front
        print(node)  # Process node
        
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

### Example

```
Graph:      BFS from node 0:
    0
   / \      Queue: [0]
  1   2     Process 0, Queue: [1, 2]
  |   |     Process 1, Queue: [2, 3]
  3   4     Process 2, Queue: [3, 4]
            Process 3, Queue: [4]
            Process 4, Queue: []

Order: 0, 1, 2, 3, 4 (level by level)
```

### Time Complexity

- **Visit each node:** O(V)
- **Explore each edge:** O(E)
- **Total:** O(V + E)

**Space:** O(V) for queue (worst case: all nodes in queue)

---

## DFS vs BFS

| Property | DFS | BFS |
|----------|-----|-----|
| Data structure | Stack | Queue |
| Order | Deep-first | Level-first |
| Best for | Backtracking, cycles | Shortest path, levels |
| Space (sparse tree) | O(h) height | O(w) width |
| Implementation | Recursive easier | Iterative common |

### Use Cases

**DFS:**
- Cycle detection
- Topological sorting
- Connected components
- Backtracking problems

**BFS:**
- Shortest path (unweighted)
- Level-order traversal
- Connected components
- Social network distance

---

## Topological Ordering

### What is Topological Order?

A linear ordering of vertices such that for every edge u→v, u comes before v.

**Example:** Task dependencies
```
Task order: A must before B, B before C, B before D

Valid topological orders:
A, B, C, D
A, B, D, C
```

### When Applicable

**Only for DAG** (Directed Acyclic Graph) - no cycles!

If graph has cycle, no topological order exists.

### Algorithm (DFS-based)

1. Perform DFS on all nodes
2. When node is done (all neighbors explored), add to result
3. Result in reverse = topological order

**Pseudocode:**

```python
def topological_sort(graph):
    visited = set()
    result = []
    
    def dfs(node):
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                dfs(neighbor)
        result.append(node)  # Add after exploring neighbors
    
    for node in graph:
        if node not in visited:
            dfs(node)
    
    return result[::-1]  # Reverse for correct order
```

### Time Complexity

- **DFS:** O(V + E)
- **Total:** O(V + E)

---

## Key Takeaways

- **Graph:** Nodes and edges representing connections
- **Representations:** Adjacency matrix O(V²), adjacency list O(V+E)
- **DFS:** Deep-first using stack, O(V+E)
- **BFS:** Level-first using queue, O(V+E)
- **Topological sort:** Linear ordering for DAG, O(V+E)
- **Choose:** Matrix for dense/quick lookups; List for sparse/general
- **DFS vs BFS:** DFS for backtracking, BFS for shortest path

