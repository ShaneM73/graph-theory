# 3. Single-Source Shortest Paths

## Definitions

- The weight of a path p = ⟨v₀, v₁, …, vₖ⟩ is w(p) = Σ w(vᵢ₋₁, vᵢ).
- The shortest-path weight δ(u, v) is the minimum w(p) over all paths from u to v, or ∞ if there is no path.
- **Variants:** single-source, single-destination (reverse the edges), single-pair, and all-pairs.
- **Optimal substructure:** any subpath of a shortest path is itself a shortest path.

## Negative weights and cycles

- Negative edges are allowed as long as no negative-weight cycle is reachable from s.
- If a negative cycle is reachable, δ(s, v) = −∞ for vertices on or after the cycle, and no shortest path exists.
- Shortest paths never need cycles: positive and zero-weight cycles can simply be removed.

## Shared machinery

Every algorithm starts with INITIALIZE-SINGLE-SOURCE (d[v] = ∞, π[v] = NIL, d[s] = 0) and then repeatedly relaxes edges:

```
RELAX(u, v, w)
1  if v.d > u.d + w(u, v)
2      v.d = u.d + w(u, v)
3      v.π = u
```

The algorithms differ only in which edges they relax, in what order, and how many times.

## Bellman-Ford

```
BELLMAN-FORD(G, w, s)
1  INITIALIZE-SINGLE-SOURCE(G, s)
2  for i = 1 to |G.V| − 1
3      for each edge (u, v) ∈ G.E
4          RELAX(u, v, w)
5  for each edge (u, v) ∈ G.E
6      if v.d > u.d + w(u, v)
7          return FALSE
8  return TRUE
```

- Works with negative edges, and detects negative cycles (returns FALSE).
- |V| − 1 passes are enough because a shortest path has at most |V| − 1 edges. After pass k, every vertex whose shortest path uses at most k edges is correct.
- **Running time:** O(VE).

## Shortest paths in a DAG

```
DAG-SHORTEST-PATHS(G, w, s)
1  topologically sort the vertices of G
2  INITIALIZE-SINGLE-SOURCE(G, s)
3  for each vertex u, in topological order
4      for each vertex v in G.Adj[u]
5          RELAX(u, v, w)
```

- A DAG has no cycles, so shortest paths are always well defined, even with negative edges.
- Each edge is relaxed exactly once. **Running time:** Θ(V + E).

## Dijkstra's algorithm

```
DIJKSTRA(G, w, s)
1  INITIALIZE-SINGLE-SOURCE(G, s)
2  S = ∅
3  Q = G.V              // min-priority queue keyed by d
4  while Q ≠ ∅
5      u = EXTRACT-MIN(Q)
6      S = S ∪ {u}
7      for each vertex v in G.Adj[u]
8          RELAX(u, v, w)
```

- Requires **w(u, v) ≥ 0** for every edge. Zero-weight edges are fine.
- Greedy: once a vertex enters S, its distance is final.
- **Running time:** O(E lg V) with a binary heap.
- With a negative edge, Dijkstra can finalize a vertex too early and return wrong distances. The app's "Negative edge breaks it" example shows this.

## Comparison

| Algorithm | Negative edges | Needs | Running time |
|-----------|----------------|-------|--------------|
| Bellman-Ford | yes, detects negative cycles | — | O(VE) |
| DAG shortest paths | yes | a DAG | Θ(V + E) |
| Dijkstra | no | w ≥ 0 | O(E lg V) |
