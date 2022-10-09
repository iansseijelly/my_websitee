# CS170 Lecture 4

## Sorting
Assume that no two numbers are equal => no need to check for equality => only less than computation
* Depth of the tree is **D**
* n! possible orderings of inputs
* each possible input permutation needs a different output permutation to sort them the same
* we have D >= log(n!)

$$\frac{n}{2}log(\frac{n}{2}) \leq log(n!) \leq nlog(n)$$

## Selection
Given A[1,...,n], return the kth smallest element in A.

If using quick select, then it is a random algorithm on linear time

**Deterministic algorithm:**
Group the array A into groups of 5, calculate the median
Then calculate the median recursively, then use it as the pivot in quick sort
Then, partition the array into L, P, and R
* if k < |L|, return select(L,k)
* if k = |L|, return p
* if k > |L|, return select(R, k-|L|-1)

The median calculated this way is guaranteed to be greater and smaller than 
$$p > \frac{n}{5}* \frac{1}{2} * 3  = \frac{3n}{10}$$

$$T(n)\leq T(\frac{n}{5}) + T(\frac{7n}{10}) + C*n$$

It is linear and it can be proven by **induction**.

## Fast Fourier Transform 

Given two polynomials:
$$A(x) = \sum_{i=0}^{n-1}a_ix^i, B(x) = \sum_{i=0}^{n-1}b_ix^i$$

we want the coefficients of C(x) = (A*B)(x)

$$c_k = \sum_{i=0}^{n-1}a_ib_{k-i}$$

### Alg1: Straightforward
Loop over the two loops k and i, resulting in O(n^2) complexity

### Alg2: Karatsuba
A(x) = A_l + A_h * x ^{N/2}^
O(N^log2_3^)

### Alg3: Fast Fourier Transform
if we know C evaluated at N distinct points, we can uniquely determine C
$$X_i = \omega^i, \omega = e^{2\pi i/2}$$
$$F_{ij} = \omega^{ij}$$

FFT: given x, returns F*x

* a hat is FFT(a)
* b hat is FFT(b)
* for i = 0 to N-1, c_i hat gets a_i hat * b_i hat
* c = F^-1 * c hat

A(x) = Aeven(x^2) + x * Aodd(x^2)

T(N) = 2 * T(N/2) + O(N)

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

$$X(z) * Y(z) = (x_{m-1}y_0)z^0 + (x_{m-1}y_1 + x_{m-2}y_0)z^1 + ... + (x_0y_0+x_1y_1+ ... + x_{m-1}y_{m-1})z^{m-1} + (x_0y_1+x_1y_2+ ... )z^m + ...$$

Reading off the coefficients will give the desired results.
