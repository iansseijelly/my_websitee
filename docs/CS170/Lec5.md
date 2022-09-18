# Graphs
## Basics

Convenient way of representing data, specifically obejcts and the **pairwise relationship** between those objects.

**Directed graphs:** pair (**V** (vertex),**E**(edges)) where E is a subset of ordered E -> E.

If no self-loops exist, i.e some edge points to itself through a vertex, then the graph is **simple**.

**Undirected graphs:** similar concept but, E is a subset of unordered E x E

**Weighted graphs:** Every edge has an associated number E -> R

**N** represents the number of **vertices**, and **M** represents the number of **edges**

**Cycles**: where you can start from a vertex, traverse through the edges non repetitively, and go eventually go back to that vertex

**Strongly Connected Components**: a subset of a directed graph in which all vertices can go to all other vertices. Every graph is a DAG of its SCCs.


### Application

1. Road networks: weighted, directed
2. Social media (Facebook): undirected (instagram/twitter/tiktok): directed
3. Picture pixel: undirected
4. Chat groups: (Hypergraphs), where a hyperedge is some subset of Vertices -> not just size 2!

### Implementation

1. **Adjacency matrix**: 2D array, nxn booleans, where A[i][j] = 1 if and only if (i,j) is an edge
   Memory requirement: n^2 bits
   check adjacency: 1
   loop through all neighbors: n
2. **Adjacency list**: array of n pointer to linked lists, each contains the vertices that is an adjacent vertex (has an edge)
   Memory requirement: n + m bits
   check adjacency: degree of u + 1
   loop through all neighbors: du + 1

## Depth First Search:

    #global variables:
    clock: 1
    visited[1,n] <- initially all 0 falses
    preorder[1,n], postorder[1,n]

    for all vertex u in vertex set:
        if not visited[u]:
            visited[u] = True
            explore(u)
            
    #The workhorse of this function!
    def explore(u):
        preorder[u] = clock++
        for each (u,v) that belongs to Edges
            if not visited[v]:
                visited[v] = True
                explore(V)
        postorder[u] = clock++

**Claim:** DFS explore starting from v visits all vertices reachable from V. visited[u] = True if and only if V can reach U.

DFS runtime: Adjacency matrix: O(n^2)
Adjacency list: O(n + m)

Classification of edges e = (u,v):
1. Tree edge: if e belongs to the DFS tree
2. Back edge: u is descendant of v not in DFS tree
3. Forward edge: u is an ancestor to a descendant and uv is not in the tree
4. Cross edge: All other edges

for U -> V
if pre(U) < pre(V) < post(V) < post(U) => must be a tree edge or a forward edge
if pre(V) < pre(U) < post(U) < post (V) => must be a back edge
if pre(V) < post(V) < pre(U) < post(U) => must be a cross edge

**Claim**: A Graph is a DAG if and only if DFS finds no back edges
if back edge is found, then there exists a cycle
if there's a cycle, start with the lowest preorder vertex in cycle V, do DFS, pre(V) < pre(U), so UV is a backedge as there exists a cycle

### DAG Topological Soring 
Given a DAG, sort vertices in order of their dependencies

Theore: In a DAG, every pair of edge (U,V) has post post(V) < post(U)
Algorithm: run DFS, then order form highest to lowest post order number

## Strongly Connected Components

**Connected**: U and V are connected if path from u to v and from v to u
if A and B are connected and B and C are connected, then A, B and C are all connected

Parition all the vertices into disjoint sets where all the vertices in the set are connected to one another, each forming an SCC. => Every graph is a DAG of its SCCs.

A **Source SCC** has no incoming edges from other SCCs, and a **Sink SCC** has no outgoing edges to another SCC.

Fact: explore(V) stops when it has visted all vertices reachable from V
Fact: if V belonging to a sink SCC, explore(V) only visits the sink SCC

### Algroithm: 

run explore on sink SCCs, and then remove all that is found.

	label = 0
	repeat:
		if some V is not visited in a sink SCC:
			label++
			explore V, give all vertice a label
	untill all vertices are labeled

### Identify sink SCC?
if U has the highest post order #(visited last) => u is in a source SCC
Form a Reversed graph Grev, and then find the source SCC in the reversed graph, it is actually a sink!

## Finding Shortest Paths

Easiest: length of path from u to v is just the number of edges
Algorithm: Breadth First Search (BFS) cost = O(m+n)

Harder: Add **weights** to the edges, like distances/travel time/cost
Algorithms:
* if all the weights are postivie, use Dijkstra algorithm, at the cost of O((n+m)logn) if we use a binary heap
* if weights can be negative(like in finance), must use Bellman-Ford, at the cost of O(mn)

### Naive Algorithm:
	initial begin
		pick source S, use Q(ueue), dist(s) = 0, for all v not is s, set dist(v) to infinity, and put S in queue
		while Q not empty:
			u = pop(Q)
			for all (U,V) where v is connected to u
				if dist(v) == infinity, ... not visited
				dist(u) = dist(V) + 1
				add v to Q

### Social Network:
Top Down: Run BFS for a few steps to find all the famous people
Bottom up: for other vertices, ask if they are connected to any of the famous people. If found, can skip looking for a lot of other people