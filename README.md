# Graph Theory

Course materials for the graph-algorithms unit of **CSC 325: Algorithms and Advanced Data Structures** (Missouri State University): study notes and **Graph Algorithms Lab**, an interactive 3D web app that walks through each algorithm step by step.

## Graph Algorithms Lab

`index.html` is a single, self-contained page. Open it in any modern browser, or publish it with GitHub Pages (see below).

Each module shows a graph in 3D that you can rotate and zoom, and steps through the algorithm with:

- a plain-language explanation of each step, plus why it works
- the CLRS pseudocode, with the current line highlighted
- the live data structures: queues, call stacks, timestamps, disjoint sets, priority queues, and distance tables
- **Quiz me** checkpoints that ask you to predict the next step, with a running score

| # | Module | Example | Running time |
|---|--------|---------|--------------|
| 1 | Graph Representation | undirected and directed graphs | lists Θ(V + E), matrix Θ(V²) |
| 2 | Breadth-First Search | friend network | O(V + E) |
| 3 | Depth-First Search | all four edge types | Θ(V + E) |
| 4 | Topological Sort | CS course prerequisites | Θ(V + E) |
| 5 | Strongly Connected Components | four components | Θ(V + E) |
| 6 | Kruskal's Algorithm | campus fiber network | O(E lg V) |
| 7 | Prim's Algorithm | same network as Kruskal | O(E lg V) |
| 8 | Bellman-Ford | negative edges; negative cycle | O(VE) |
| 9 | DAG Shortest Paths | project steps with negative weights | Θ(V + E) |
| 10 | Dijkstra's Algorithm | delivery routes; a negative edge that breaks it | O(E lg V) |

**Controls:** drag to orbit, scroll or use +/− to zoom, click a vertex to highlight it in the tables. Use ← and → to step, Space to play or pause. Each module has its own link, such as `index.html#dij` for Dijkstra. The module ids are `rep`, `bfs`, `dfs`, `topo`, `scc`, `kruskal`, `prim`, `bf`, `dag` and `dij`.

The page loads [three.js r128](https://threejs.org/) from cdnjs and fonts from Google Fonts, so it needs an internet connection.

### Publish with GitHub Pages

1. In the repository, open **Settings → Pages**.
2. Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/ (root)`, and save.
3. After a minute the app is live at `https://<your-username>.github.io/graph-theory/`.

## Notes

- [1. Graph basics and BFS](notes/01-graph-basics-and-bfs.md): representations, BFS, shortest edge-count distances
- [2. DFS and minimum spanning trees](notes/02-dfs-and-minimum-spanning-trees.md): DFS, edge types, parenthesis and white-path theorems, topological sort, SCCs, the cut property, Kruskal, Prim
- [3. Shortest paths](notes/03-shortest-paths.md): relaxation, Bellman-Ford, DAG shortest paths, Dijkstra

## Repository layout

```
graph-theory/
├── index.html      Graph Algorithms Lab (the web app)
├── notes/          study notes for each lecture
└── README.md
```

## Sources

Based on the CSC 325 graph-algorithms lectures by Dr. Yifan Zhang, which follow Cormen, Leiserson, Rivest and Stein, *Introduction to Algorithms* (CLRS). The examples in the app are original.
