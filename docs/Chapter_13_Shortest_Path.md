# Chapter 13 - Shortest Path Algorithms

## Core Concept: Cost Updating

### The Key Idea

All shortest path algorithms work on the same principle:

**"If I find a shorter path to a node than what I know, update it."**

```
Current best path to node C: distance 10
Find new path: A→B→C with distance 8
Update: distance to C = 8

Current best path to C: distance 8
Find new path: A→D→E→C with distance 7
Update: distance to C = 7
```

### Distance Array

Maintain a **distance array** where `dist[v]` = shortest distance from source to node v.

```
Initial:
dist[A] = 0 (source)
dist[B] = ∞ (unknown)
dist[C] = ∞ (unknown)
dist[D] = ∞ (unknown)

After relaxing edge A→B (weight 5):
dist[B] = min(∞, dist[A] + 5) = 5

After relaxing edge B→C (weight 3):
dist[C] = min(∞, dist[B] + 3) = 8
```

### Edge Relaxation

**Relaxing an edge (u, v, weight):**

```python
def relax(u, v, weight):
    if dist[u] + weight < dist[v]:
        dist[v] = dist[u] + weight
        # Optional: update parent for path reconstruction
        parent[v] = u
```

---

## Bellman-Ford Algorithm

### When to Use

- Works with **negative edge weights**
- Works with **negative cycles** (detects them)
- Slower than Dijkstra but more general
- Time: O(VE)

### Algorithm Steps

```
Input: Graph, source node s

1. Initialize:
   dist[s] = 0
   dist[v] = ∞ for all other v

2. Relax all edges (V-1) times:
   for i = 1 to V-1:
       for each edge (u, v, w):
           relax(u, v, w)

3. Check for negative cycle:
   for each edge (u, v, w):
       if dist[u] + w < dist[v]:
           return "Negative cycle exists"

4. Return dist array
```

### Example

```
Graph with weights:
    A
   /5 \2
  B     C
   \3  /
    \ / 4
     D

Find shortest path from A using Bellman-Ford:

Initial: dist = [0, ∞, ∞, ∞]

Iteration 1:
Edge A→B: dist[B] = min(∞, 0+5) = 5
Edge A→C: dist[C] = min(∞, 0+2) = 2
Edge B→D: dist[D] = min(∞, 5+3) = 8
Edge C→D: dist[D] = min(8, 2+4) = 6
After: dist = [0, 5, 2, 6]

Iteration 2:
No updates (already optimal)
dist = [0, 5, 2, 6]

No negative cycle detected
```

### Time Complexity

- **Relax phase:** (V-1) × |E| = O(VE)
- **Check phase:** O(E)
- **Total:** O(VE)

**Why (V-1) iterations?**
- Shortest path has at most V-1 edges (no cycles)
- Each iteration propagates distance one edge further
- After V-1 iterations, all shortest paths found

### Pseudocode

```python
def bellman_ford(graph, source):
    V = len(graph)
    dist = [float('inf')] * V
    dist[source] = 0
    parent = [-1] * V
    
    # Relax all edges V-1 times
    for _ in range(V - 1):
        for u in range(V):
            for v, weight in graph[u]:
                if dist[u] + weight < dist[v]:
                    dist[v] = dist[u] + weight
                    parent[v] = u
    
    # Check for negative cycle
    for u in range(V):
        for v, weight in graph[u]:
            if dist[u] + weight < dist[v]:
                return None  # Negative cycle
    
    return dist, parent
```

---

## Dijkstra's Algorithm

### When to Use

- **No negative edge weights** (required!)
- **Faster** than Bellman-Ford: O(E log V)
- Greedy algorithm
- Works great for GPS, networks, social graphs

### Key Idea

Always process the nearest unvisited node next.

**Intuition:** "If we always pick the nearest node, we'll never need to backtrack because negative edges don't exist."

### Algorithm Steps

```
Input: Graph, source node s

1. Initialize:
   dist[s] = 0, dist[v] = ∞ for others
   visited = empty set
   priority_queue = all nodes

2. While priority_queue not empty:
   u = node with smallest distance
   
   if u already visited:
       continue
   
   mark u as visited
   
   for each neighbor v of u:
       relax(u, v, weight)

3. Return dist array
```

### Example

```
Same graph without negative weights:
    A
   /5 \2
  B     C
   \3  /
    \ / 4
     D

Start at A:

Step 1: Visit A (dist=0)
        Relax: B=5, C=2

Step 2: Visit C (dist=2, smallest unvisited)
        Relax: D=min(∞, 2+4)=6

Step 3: Visit B (dist=5)
        Relax: D=min(6, 5+3)=6

Step 4: Visit D (dist=6)
        Done

Result: [0, 5, 2, 6]
```

### Why No Negative Weights?

**Without this constraint:**
```
If negative weights exist:
- We process A
- Decide B is best with distance 10
- Later find path A→C→D→B with distance 8 (via D-C with weight -15)
- But D not processed yet!

We'd need to re-examine B (no greedy guarantee)
→ Falls back to Bellman-Ford complexity
```

### Pseudocode (with Priority Queue)

```python
import heapq

def dijkstra(graph, source):
    V = len(graph)
    dist = [float('inf')] * V
    dist[source] = 0
    parent = [-1] * V
    
    pq = [(0, source)]  # (distance, node)
    visited = set()
    
    while pq:
        d, u = heapq.heappop(pq)
        
        if u in visited:
            continue
        
        visited.add(u)
        
        for v, weight in graph[u]:
            if dist[u] + weight < dist[v]:
                dist[v] = dist[u] + weight
                parent[v] = u
                heapq.heappush(pq, (dist[v], v))
    
    return dist, parent
```

### Time Complexity

- **Priority queue operations:** O(E log V)
  - Each edge relaxed once: O(E) operations
  - Each operation is O(log V) heap operation
- **Total:** O(E log V)

**Better than Bellman-Ford's O(VE)** when E relatively small!

---

## Floyd-Warshall Algorithm

### When to Use

- Find **all-pairs shortest paths** (every pair of nodes)
- Works with **negative weights** but no negative cycles
- Time: O(V³)
- Space: O(V²)

### When NOT to Use

- Only need shortest path from one source → Use Dijkstra O(E log V)
- Only one path needed → Overkill
- Dense graphs → Compare O(V³) vs running Dijkstra V times O(VE log V)

### Algorithm Idea

**Dynamic Programming:**
- `dist[i][j][k]` = shortest path from i to j using first k nodes as intermediate

Optimize to 2D by updating in-place:
- `dist[i][j]` = shortest path from i to j

**Key:** Consider each node k as intermediate node

```python
def floyd_warshall(graph):
    V = len(graph)
    dist = [row[:] for row in graph]  # Copy graph
    
    # Try each node as intermediate
    for k in range(V):
        for i in range(V):
            for j in range(V):
                # Can we go i→k→j for shorter path?
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    
    return dist
```

### Example

```
Initial graph (adjacency matrix):
  A  B  C
A 0  5  2
B ∞  0  3
C ∞  ∞  0

Try A as intermediate (k=0):
No improvements (A used in paths already)

Try B as intermediate (k=1):
A to C: min(2, 5+3) = 2 (no change)

Try C as intermediate (k=2):
A to B: min(5, 2+∞) = 5 (no change)
B to A: min(∞, 3+∞) = ∞ (no change)

Result:
  A  B  C
A 0  5  2
B ∞  0  3
C ∞  ∞  0
```

### Time Complexity

- **Three nested loops:** O(V³)
- Best/average/worst all O(V³)

---

## Comparison of Shortest Path Algorithms

| Algorithm | Time | Space | Negative weights | Use case |
|-----------|------|-------|------------------|----------|
| Dijkstra | O(E log V) | O(V) | No | Single source, typical |
| Bellman-Ford | O(VE) | O(V) | Yes | Negative weights, detection |
| Floyd-Warshall | O(V³) | O(V²) | Yes | All pairs |
| DFS | O(V+E) | O(V) | N/A | Only unweighted |
| BFS | O(V+E) | O(V) | No (unweighted) | Unweighted shortest |

---

## Key Takeaways

- **Cost updating:** Core principle - update if shorter path found
- **Bellman-Ford:** O(VE), handles negatives, detects negative cycles
- **Dijkstra:** O(E log V), no negatives, greedy, practical
- **Floyd-Warshall:** O(V³), all-pairs, handles negatives
- **Choose:** Dijkstra for typical use, Bellman-Ford for negatives, Floyd-Warshall for all-pairs

