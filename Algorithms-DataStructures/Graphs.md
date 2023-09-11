# Graphs

## Definitions

A graph is a way of representing relationships that exist between pairs of objects.


> a graph G is simply a set V of **vertices** and a collection E of pairs of vertices from V , called **edges**.


Edges can be **directed** or **undirected**
- an edge `(u, v)` is **directed** from `u` to `v` if the pair (u, v) is ordered, with u preceding v
	- such a relation between u and v is called **asymmetric** relation
- an edge (u, v) is **undirected** if the pair (u, v) is not ordered
	- such a relation between u and v is called **symmetric** relation


The two vertices joined by an edge are called the **end vertices** (or **endpoints**)
- if an edge is directed, its first endpoint is **origin** and the other is **destination**


Two vertices `u` and `v` are said to be **adjacent** if there is an edge whose vertices are `u` and `v`


An edge is said to be **incident** to a vertex if the vertex is one of the edge's endpoints
- The **outgoing edges** of a vertex are the directed edges whose origin is that vertex
- The **incoming edges** of a vertex are the directed edges whose destination is that vertex


The **degree** of a vertex `v`, denoted as `deg(v)` is the number of incident edges of v.
- **in-degree** = number of incoming edges
- **out-degree** = number of outgoing edges


**Parallel edges / Multiple edges**: two undirected edges to have the same vertices, OR, for 2 directed edges to have the same origin and destination

**Self-loop**: an edge (directed or undirected) if its 2 endpoints coincide

A graph is **simple** if it does not have parallel edges or self-loops


A **path** is a sequence of alternating vertices and edges that starts at a vertex and ends at a vertex such that each edge is incident to its predecessor and success vertex
- a path is **simple** if each vertex in the path is distinct
- a **directed path** is a path such that all edges are directed and are traversed along their direction

A **cycle** is a pth that starts and ends at the same vertex, and that includes at least 1 edge
- A cycle is simple, if each vertex in the cycle is distinct, except the first and last one
- A **directed cycle** is a cycle such that all edges are directed and traversed along their direction

A directed graph is **acyclic** if it has no directed cycles


Given vertices `u` and `v` of a directed graph, we say that `u` **reaches** `v` and `v` is **reachable** from `u`, if the graph has a directed path from `u` to `v`.
- In an undirected graph, notion of reachability is **symmetric**


A graph is **connected** if, for any 2 vertices, there is a path between them
- A directed graph is **strongly connected** if for any 2 vertices `u` and `v` , u reaches v and v reaches u


A **subgraph** of a graph G is a graph H whose vertices and edges are subsets of the vertices and edges of G.
- a **spanning subgraph** of G is a subgraph of G that contains all the vertices of G
- **connected components**: maximal connected subgraphs of a Graph G which is not connected
- A **forest** is a graph without cycles
- A **tree** is a connected forest, i.e, a connected graph without cycles
- A **spanning tree** of a graph is a spanning subgraph that is a tree


![[Pasted image 20230901171856.png]]