# Introduction

## Algorithm
Well-defined procedure for carrying out some computational task.
We want: **Correct** (Always halts + returns correct answer) & **Efficient** (Minimized consumption of computational resources like time, memory, etc.)

## Integer Arithmetic
How is input represented?
Indian-Arabic numeral system, representing numbers as sequence of digits in base 10

### Addition Problem
Given x,y in base 10, return x + y in base 10

**Algorithm 1:** repeated increments, start with x, and increment y times. Efficiency: increment takes less than or equal to n steps, and with y increments, => total number of stemps is less than or equal to $$10^{n} \cdot n$$, and can be simplified to 10^n steps considering carrying.

**Algorithm 2:** adding up using a lookup table for each digit, with an efficiency of $$O(n)$$

Cannot be better than Algorithm 2: Writing the answer down (n+1 digits) takes n steps, && Need to read at least all the input digits, exists inputs says the same answer for both.

### Multiplication Problem
Given x,y in base 10, return x times y in base 10

**Algorithm 1:** repeated additions, start with 0, add x over and over for y times. Efficiency: $$ \underset {(n)} {num_{steps}} \cdot \underset {(y)} {num_{add}} = O(n \cdot10^n )$$

**Algorithm 2:** grade school algorithm. Efficiency: $$O(n^2)$$

**Algorithm3:** Divide and Conquer (Recursion)
$$x = x_h \cdot 10^{\frac{n}{2}} + x_l$$
$$y = y_h \cdot 10^{\frac{n}{2}} + y_l$$
$$x \cdot y = x_h \cdot y_h \cdot 10^{\frac{n}{2}} + (x_h \cdot yl + x_l \cdot y_h) \cdot 10^{\frac{n}{2}}+ y_l \cdot x_l$$
Recurstion relation: $$T(n) \leq 4*T(\frac{n}{2}) + C\cdot n$$ if n >1, and  $$T(n) \leq C$$ if n = 1, where C is an additional addition effort constant.

**Analysis**: $$Total steps \leq C \cdot n \cdot(1 + 2 + ... + 2^k)$$, where k = the height of the tree, logn, so the efficiency is ¥$O(n^2)$$

**Algorithm 4:** Karatruba's Algorithm
$$x \cdot y = \underset{A}{x_h \cdot y_h} \cdot 10^{\frac{n}{2}} + \underset{C}{(x_h \cdot yl + x_l \cdot y_h)} \cdot 10^{\frac{n}{2}}+ \underset{B}{y_l \cdot x_l}$$


**Analysis**: Reduced 4 recursive calls into only 3. $$Total steps \leq C \cdot n \cdot(1 + \frac{3}{2} + ... + \frac{3}{2}^k)$$, where k = the height of the tree, logn, so $$< 3C \cdot 3^{log_2n}$$ $$ \lt O(n^{log_{2}3})$$

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