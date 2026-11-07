# 1. Graph Basics and Breadth-First Search

## Graphs

A graph G = (V, E) is a set of vertices joined by edges. Graphs model anything with items and connections between them: road maps, schedules, computer networks, hyperlinks, circuits.

- **Directed** graphs have ordered edges (u → v); **undirected** graphs do not.
- A graph is **connected** if there is a path between every pair of vertices.
- A **bipartite** graph splits V into V₁ and V₂ with edges only between the two groups.
- A **weighted** graph has a weight function w : E → ℝ.

## Representations

|                          | Adjacency list | Adjacency matrix |
|--------------------------|----------------|------------------|
| Memory                   | Θ(V + E)       | Θ(V²)            |
| List the neighbors of u  | Θ(deg u)       | Θ(V)             |
| Is (u, v) an edge?       | O(deg u)       | Θ(1)             |
| Best for                 | sparse graphs  | dense graphs     |

- Undirected graph: the list lengths sum to 2|E|, and the matrix is symmetric (A = Aᵀ).
- Directed graph: the list lengths sum to |E|.
- Weighted graphs store w(u, v) next to v in Adj[u], or in matrix cell (u, v).

## Breadth-First Search (BFS)

**Input:** a graph G and a source vertex s.
**Output:** d[v], the fewest edges from s to v, and π[v], the parent in a breadth-first tree.

BFS explores in waves. Every vertex at distance k is discovered before any vertex at distance k + 1. A FIFO queue keeps the waves in order.

- **WHITE:** undiscovered. **GRAY:** discovered and in the queue. **BLACK:** all neighbors scanned.
- A vertex turns GRAY when it is enqueued, so it can never enter the queue twice.
- Vertices that cannot be reached from s keep d = ∞.

```
BFS(G, s)
1  for each vertex u ∈ G.V − {s}
2      u.color = WHITE;  u.d = ∞;  u.π = NIL
3  s.color = GRAY;  s.d = 0;  s.π = NIL
4  Q = ∅;  ENQUEUE(Q, s)
5  while Q ≠ ∅
6      u = DEQUEUE(Q)
7      for each v in G.Adj[u]
8          if v.color == WHITE
9              v.color = GRAY
10             v.d = u.d + 1;  v.π = u
11             ENQUEUE(Q, v)
12     u.color = BLACK
```

**Running time:** O(V + E). Each vertex is enqueued and dequeued at most once, and each adjacency list is scanned once.

**Shortest-path property:** BFS computes δ(s, v), the minimum number of edges on any path from s to v. It counts edges, not weights.
