# Greedy Algorithm

## Task Scheduling
what order should you do your hw assignments in each with the deadline, to maximize the number of homeworks within their ddl?

* assignments: a1, a2, ... , an
* deadlines: d1, d2, ... , dn
\* each task takes 1hr to complete

### Algorithm

    repeat
        pick an unexpired ai with the nearest deadline
    until nothing to be done

**Correctness Proof**: 
Proof by contradiction: if A_sk != A_gk is the first task the greedy solution differs from the true optimal, changing A_sk to A_gk is still optimal, otherwise the greedy algorithm would have ended before the optimal one.

**case1:** if a_gk does not appear in S, then we can just change a_sk to a_gk
**case2:** if a_gk appears in S later, as a_sl = a_gk for some l > k, it is ok to swap a_sl to an earlier since it has not expired, as d_gk = d_sl <= d_sk, so we can do a_sk later and it is not expired yet.

## Huffman Coding
using the fewest number of bits to store a dataset losslessly

given a random string of chars of length n from alphabet {A,B,C,D}, how many bits needed to encode the string?

### Naively
2 bits/char => n chars takes 2n bits

### Better solution
Suppose we know the frequency for each characters appear. Example:

* A   B   C   D
* .4  .3  .2  .1

Use fewer bits for the more frequent characters.

* A   B   C    D
* 0   10  110  111

**Fact:** there's a 1-1 correspondence between prefix free codes and full binary tree -> each node has 0 or 2 children.

if f1 and f2 are the two least frequently visited chars, they should be at the very bottom of the tree.

Define that for any non-root node v in tree, $$cost(v) = \sum_i fi$$
$$cost(T) = \sum_i cost(v)$$

**Claim:** this prefix free algorithm results in an optimal tree with lowest cost

**Proof by Induction**: 
base case: 1 or 2 chars: just use 1 bit
induction: let T' be a tree for some f1+f2, f3, f4 ... optimal by induction
T = T' with f1, f2 as a tree added into the f1+f2 as a substitue.

cost (T) = cost (T') + f1 + f2

if T is not optimal, then T' is neither optimal, because we can just replace 

### Algorithm

    create a priority queue H of {1,2,...,n} sorted by frequency
    for k = n + 1 to 2n-1:
        i = deletemin(H), j = deletemin(H)
        create a new vertex k with i and j as children
        fk = fi + fj
        insert back into queue H(k, fk)

## SetCover
given a set v and m subsets that cover v, find fewest sets that still cover v. 

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
