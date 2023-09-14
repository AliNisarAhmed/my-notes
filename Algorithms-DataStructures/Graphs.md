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


### Transitive Closure

Transitive Closure of a directed graph G is itself a directed Graph G* such that:
- the vertices of G* are the same as G
- G* has an edge (u, v), whenever G has a directed *path* from u to v
	- including the case when (u,v) is an edge in G (not just path)

(in other words, if there is a path from a -> c, add an edge from a -> c (if not already in G of course))

For a directed Graph that consists of a simple directed path of `n` vertices (str8 line), the transitive closure has `n(n - 1) / 2` edges


### Topological Ordering

For directed graph G with `n` vertices, a *topological ordering* is an ordering of its vertices (v1 .. vn) such that for every edge (vi, vj), it is the case that i < j.
- i.e. a topological ordering is an ordering such that any directed path in G traverses vertices in increasing order

Example application:
- Topological sorting can help us in identifying which courses to take given that some courses are a pre-req of others

**Proposition**: A directed graph has a topological ordering if and only if it is acyclic

![[Pasted image 20230913211123.png]]


![[Pasted image 20230913211147.png]]




---


## Graph Traversals

### DFS

In DFS, we start with a vertex, pick its neighbor, and do DFS on it.

### DFS Graph edges classification

- **Depth-first search tree**
	- The vertices visited during DFS identify a "depth-first search tree" rooted at starting vertex
- **Tree edge OR discovery edge**
	- whenever during DFS an edge is used to discover a new vertex, that edge is known as *
- **Non-tree Edge**
	- all edges which take us to a previously visited vertex
	- For undirected graphs
		- **Back Edge** all non-tree edges connect the current vertex to one that is an ancestor of it in the DFS tree
	- For directed graphs
		- There are 3 possibilities
		- **Back Edge**
			- which connect a vertex to an ancestor in DFS tree
		- **Forward Edge**
			- which connect a vertex to a descendant in DFS tree
		- **Cross Edge**
			- which connect a vertex to a vertex that is neither its ancestor not its descendant (possibly because one of those vertices is not part of DFS tree)

![[Pasted image 20230912210155.png]]



---

## Data Structures for Graphs


There are 4 ways to represent graphs
1. Edge List
	- maintain an unordered list of all edges
	- No efficient way to locate a particular edge
	- No efficient way to find the set of all edges incident to a vertex v
2. Adjacency List
	- For each vertex maintain a separate list containing those edges that are incident to that vertex
	- The complete set of edges can be determined by taking the union of the smaller sets
3. Adjacency map
	- very similar to Adjacency List, but the secondary container of all edges incident to a vertex is organized as a map, rather than as a list, with the adjacent vertex serving as a key
	- This allows access to a specific edge in O(1) expected time
4. Adjacency Matrix
	-  maintain an `n x n` matrix for a graph with `n` vertices
	- provides worst case O(1) access to a specific edge

![[Pasted image 20230912202631.png]]

### Edge List

![[Pasted image 20230912202747.png]]


### Adjacency List

![[Pasted image 20230912202804.png]]


### Adjacency Map

![[Pasted image 20230912202840.png]]


### Adjacency Matrix

![[Pasted image 20230912202904.png]]

