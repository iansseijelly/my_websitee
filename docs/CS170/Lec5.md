# CS170 Lecture 5

## Cross Correlation
given vectors:
x(x0, ... , xm-1)
y(y0, ... , yn-1)
we want:

$$\sum_{k=0}^{m-1}x_ky_{i+k} $$, where i = 0,1,...,n-m

let 
$$X(z) = \sum_{j=0}^{m-1}x_jz^j$$
$$Y(z) = \sum_{j=0}^{m-1}y_jz^j$$

then, we can reverse X(z) to get them increment at the same tendency, arriving at:

$$X(z) * Y(z) = (x_{m-1}y_0)z^0 + (x_{m-1}y_1 + x_{m-2}y_0)z^1 + ... + (x_0y_0+x_1y_1+ ... + x_{m-1}y_{m-1})z^m-1 + (x_0y_1+x_1y_2+ ... )z^m + ...$$

Reading off the coefficients will give the desired results.

## Graphs

Convenient way of representing data, specifically obejcts and the **pairwise relationship** between those objects.

**Directed graphs:** pair (**V** (vertex),**E**(edges)) where E is a subset of ordered E -> E.

If no self-loops exist, i.e some edge points to itself through a vertex, then the graph is **simple**.

**Undirected graphs:** similar concept but, E is a subset of unordered E x E

**Weighted graphs:** Every edge has an associated number/
