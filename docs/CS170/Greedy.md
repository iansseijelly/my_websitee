# Greedy Algorithm

## Minimum Spanning Tree
Given an undirected graph G, find the subset T of edges with the smallest total weight that connects all vertices. 

**Fact**: optimal solution must not have a cycle, because if a cycle exists, it is possible to remove an edge to maintain the connectivity and yet have a cheaper total weight

**Greedy Algorithm**： 

    repeat:
        pick the shortest path remaining that does not create a cycle
    until all is connected
 
**Tree** is and undirected graph that is connected and has no cycle. (any vertex can be the root)
**Claim1**: Any 2 of these properties implies the third and implies that the graph is a tree
1. T is *connected*
2. T has *no* cycles
3. *e = v - 1*

**Cut in Graph**: is a partition of the vertices into some subsets and its complement, S and V - S, and possibly edges crossing it. 
**Claim2**: lightest edge in any cut also appears in some MST.

**Claim3**: Suppose x has no edges connecting some cut, and we want to add an edge, suppose e is the lightest edge connecting the cuts, then X + e is a MST

### Meta-Algorithm：

    repeat:
        pick a cut such that x does not cross the cut, add the edge e with smallest weight edge in cut and add it to x
    until n-1 edges are added

**Kruskal**: 

    //Naive：
    x = empty, sort all edges by their weight
    for all edges in increasing order of weight, if x + e has no cycle, add it in x. 

    //cost =  cost of DFS per edge = O(m * n)
---
    //1st optimization:
    x = empty, sort all edge by their weight
    for all vertices, make set (v).
    for all e = (u,v) increasing order, if find(u) != find(v) which finds their connected components, then we merge u and v's connected components

    //cost = O(m x logn) => can get to O(m x log*n)

**Union-find Implementation**:

    for each v in V, 
        pi(v) = a pointer of the parent of v in a tree defining a connected component
        rank(v) = height of the subtree rooted at V 
    
    def makeset(v): 
        v.pi = v, v.rank = 0
    
    def find(v)：
        while v.pi != v,
            go up the tree until you find the parent
    
    def union(u,v):
        make the root of the shorter subtree point to the root of the taller one, keeping the rank of the tree as small as possible <= log(vertices)
