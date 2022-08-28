# CS170 Lecture 1

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