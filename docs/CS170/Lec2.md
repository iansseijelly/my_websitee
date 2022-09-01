# CS170 Lecture 2

## Fibonacci Numbers

### Algorithm 1: Recursion

     def fib(n):
        if n <= 1:
            return n
        else: 
            return fib(n-1) + fib(n-2)

**flops** : calculate the # of "flops" -> the number of operation

**runtime**: n * exp(n)

In this model, there is a lot of redundant work. T(n) = 0 if n <= 1, else 

$$T(n) = T(n-1) + T(n-2) + 1$$

can prove T(n) == F(n+1) - 1, which is groing exponentially, making this algorithm very slow

### Algorithm 2: For Loop

    def fib(n):
        if n <= 1:
            return n
        else:
            a = 0
            b = 1
            for i in range (1, n-1):
                tmp = a + b
                a = b
                b = tmp
            return b

**flops**: This is basically dynamic programming, taking n-1 flops.

**runtime**: n^2

### Algorithm 3: Fast Matrix Powering

$$A = \begin{bmatrix}
1 & 1\\
1 & 0
\end{bmatrix}$$

$$A \times \begin{bmatrix}
F_1\\
F_0
\end{bmatrix} = \begin{bmatrix}
F_1+F_0\\
F_1
\end{bmatrix} = \begin{bmatrix}
F_2\\
F_1
\end{bmatrix}$$

Need to multiply A for n times, then multiply the base case.

By using the fast exponentiation algorithm, can reduce this to **logn** flops.

**runtime**: n^2 * logn
if we use the smart fast exponentiation, then n^2
if use a faster multiplication algorithm like karatsuba, can reduce it to n^1.5ish

### Algorithm 4: Closed Form Formula

Diagonalize A as 

$$A = Q \Lambda Q^T$$

$$A^n = Q \Lambda Q^T \times Q \Lambda Q^T ... \times Q \Lambda Q^T$$

$$\therefore A^n = Q \Lambda^n Q^T$$

$$\therefore F_n = \frac{1}{\sqrt{5}} \times [(\frac{1 + \sqrt{5}}{2}) ^ n - (\frac{1 - \sqrt{5}}{2}) ^ n]$$

flops: **logn** as well

## Asymptotic Notation

A way to compare the order of growth of functions.

* Big-O: less than or equal to 
* Little-0: strictly less than
* Big-Omega: greater than or equal to 
* Little-Omega: strictly greater than
* Theta: equal

$$Ffn) = O(g(n))$$
$$\implies \exists \ c> 0 \ s.t. \forall \ n \ f(n) \le c \times g(n)$$

---
$$f(n) = o(g(n))$$
$$\implies \lim_{n\to\inf}\frac{f(n)}{g(n)} = 0$$

---
$$f(n) = \Omega(g(n))$$
$$\implies g(n) = O(f(n))$$

---
$$f(n) = \omega(g(n))$$
$$\implies g(n) = o(f(n))$$

---
$$f(n) = \theta(g(n))$$
$$\implies g(n) = O(f(n)) \ \& \  f(n) = \Omega(g(n))$$