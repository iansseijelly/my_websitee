## LP Simplex Nuances

1. How to find a starting vertex?
   
   Solve a new LP with one more variabe T!
   max t such that Ax <= b*t
   x = 0, t = 0 is guaranteed to be a starting vertex

2. How to find a neighboring vertex?
    substitude only one of all the equations for the current vertex, we can get a neighbor.

# Network Flows
Finding the maimum flow of a network of water pipes!

Define: given a directed graph G(V,E) with edge capacities v(e) > 0, a flow vector is an m*1 vector with one value per edge

**constraint**: 0 <= f(e) <= v(e)
for all vertices except source and destination, the flow in is equal to the flow out.

define the flow f as the sum of all the flow coming out of the source.
Goal is to find the maximum flow f given the constraint.

## Algorithm: Ford Fulkerson

Greedily DFS to find path from s to t, modifying the graph to reflect the usage, until there is no more path left.

1). After each DFS, find the minimum flow z along that path
2). Subtract z along the path
3). If some edge is sending f, which is less than the original capacity, then we will create an edge in the opposite direction to relect as much as that amount of flow can potentially be pushed to the opposite direction.

### Runtime:

suppose all flow are positive integers, then each time the flow is at least 1. Thus the sum of all capacity is at most U, and U decreases by 1 every time at least. 
   
max cost <= DFS * U, which is O((M+N)*U) (not polynomial!)

# Zero-Sum Games

U(i,j) is how much Row wins, which is also how much Col loses

Some strategies:
1. Row pick R,P,S, tells Col, Col picks - Col always wins
2. Col pick R,P,S, tells Row, Row picks - Row always wins
3. Row&Col picks simultaneously
   a. if row (almost) always picks the same row, col notices this, then col can (almost) always win
   b. Ditto for Col?
4. Raaaandomly, what is the probability of picking U(i,j)? => xi*yj
Expected Utility:
$$EU = \sum_{i,j}x_i y_j U(i,j)$$

How should Row pick x_i to maximize the Expected Utility?

Define E(U) = "value of the game"