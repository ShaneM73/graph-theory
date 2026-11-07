# 2. Depth-First Search and Minimum Spanning Trees

## Depth-First Search (DFS)

DFS goes as deep as possible, then backtracks. It has no single source: the main loop starts a new tree from any vertex that is still WHITE, producing a **depth-first forest**.

Each vertex gets two timestamps: d[v] when it is discovered and f[v] when it finishes, with 1 ≤ d[v] < f[v] ≤ 2|V|.

```
DFS(G)
1  for each vertex u ∈ G.V
2      u.color = WHITE;  u.π = NIL
3  time = 0
4  for each vertex u ∈ G.V
5      if u.color == WHITE
6          DFS-VISIT(G, u)

DFS-VISIT(G, u)
1  time = time + 1;  u.d = time
2  u.color = GRAY
3  for each v in G.Adj[u]
4      if v.color == WHITE
5          v.π = u;  DFS-VISIT(G, v)
6  time = time + 1;  u.f = time
7  u.color = BLACK
```

**Running time:** Θ(V + E).

### Edge classification

When edge (u, v) is explored, the color of v decides its type:

| v's color | Edge type | Meaning |
|-----------|-----------|---------|
| WHITE | Tree | v is discovered through (u, v) |
| GRAY | Back | v is an ancestor of u (self-loops count) |
| BLACK, d[u] < d[v] | Forward | v is a descendant of u |
| BLACK, d[u] > d[v] | Cross | no ancestor relationship |

A directed graph has a cycle if and only if DFS finds a back edge. Undirected graphs only have tree and back edges.

### Key theorems

- **Parenthesis theorem:** for any u and v, the intervals [d[u], f[u]] and [d[v], f[v]] are either disjoint or one is nested inside the other. Nested means descendant.
- **White-path theorem:** v is a descendant of u if and only if, at time d[u], there is a path from u to v made only of WHITE vertices.

## Topological Sort

A topological sort of a DAG is a linear order where every edge (u, v) has u before v.

```
TOPOLOGICAL-SORT(G)
1  call DFS(G) to compute finish times v.f
2  as each vertex finishes, insert it onto the front of a linked list
3  return the linked list of vertices
```

**Why it works:** for every edge (u, v) in a DAG, f[v] < f[u]. When the edge is explored, v is either WHITE (it becomes a descendant and finishes first) or BLACK (it has already finished). It cannot be GRAY, since that would be a back edge, which means a cycle.

## Strongly Connected Components (SCCs)

An SCC is a maximal set of vertices where every pair can reach each other.

```
STRONGLY-CONNECTED-COMPONENTS(G)
1  call DFS(G) to compute finish times u.f
2  compute Gᵀ (reverse every edge)
3  call DFS(Gᵀ), taking vertices in decreasing order of u.f
4  output each tree of the second depth-first forest as one SCC
```

- G and Gᵀ have the same SCCs.
- The **component graph** (one vertex per SCC) is always a DAG.
- **Lemma:** if there is a path u ⇝ u′ from SCC C to SCC C′, there is no path back from C′ to C.

## Minimum Spanning Trees (MST)

Given a connected, undirected, weighted graph, find a tree T that connects every vertex with minimum total weight w(T).

- An MST has exactly |V| − 1 edges and no cycles.
- MSTs are not always unique.

### Cuts and safe edges

- A **cut** (S, V − S) partitions the vertices.
- An edge **crosses** the cut if one endpoint is on each side.
- A cut **respects** a set A if no edge of A crosses it.
- A **light edge** is a minimum-weight edge crossing the cut.

**Cut property:** if A is part of some MST and the cut respects A, any light edge crossing the cut is **safe**: A ∪ {(u, v)} is still part of some MST. The proof swaps (u, v) into an MST in place of another crossing edge, which cannot increase the weight.

### Kruskal's algorithm

Sort the edges by weight. Add each edge unless both endpoints are already in the same tree, which a disjoint-set structure (MAKE-SET, FIND-SET, UNION) checks.

```
MST-KRUSKAL(G, w)
1  A = ∅
2  for each vertex v ∈ G.V
3      MAKE-SET(v)
4  sort the edges of G.E into nondecreasing order by weight w
5  for each edge (u, v) taken in sorted order
6      if FIND-SET(u) ≠ FIND-SET(v)
7          A = A ∪ {(u, v)}
8          UNION(u, v)
9  return A
```

**Running time:** O(E lg V). A is a forest of trees that merge.

### Prim's algorithm

Grow one tree from a root r. A min-priority queue holds every vertex not yet in the tree, keyed by the weight of its cheapest edge to the tree.

```
MST-PRIM(G, w, r)
1  for each u ∈ G.V
2      u.key = ∞;  u.π = NIL
3  r.key = 0
4  Q = G.V
5  while Q ≠ ∅
6      u = EXTRACT-MIN(Q)
7      for each v in G.Adj[u]
8          if v ∈ Q and w(u, v) < v.key
9              v.π = u
10             v.key = w(u, v)        // DECREASE-KEY
```

**Running time:** O(E lg V) with a binary heap. Note that key[v] is a single edge weight, not a distance from the root.
