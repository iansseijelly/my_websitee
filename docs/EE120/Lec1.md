# EE120 Lecture 1


## Signals are function
Discrete Time (DT) Signals:

$$\mathbb{Z} => \mathbb{R} \ or \ \mathbb{C}$$

Continuous Time (CT) Sginals: 

$$\mathbb{R} => \mathbb{R} \ or \ \mathbb{C}$$

x represents a signal as a whole, and x(n) refers to a value in that signal, and in CT, use x(t)

## DT Impulse
Kreneker Delta:
$$\delta(n) = \begin{cases}
1&\text{n=0}\\
0&\text{otherwise} 
\end{cases}$$

Claim: Any DT signal can be decomposed into a linear combination of shifted impulses

## DT Unit-Step
$$ u(n) = \begin{cases}
0&\text{n < 0}\\
1& \text{n >= 0}\\ 
\end{cases}$$

$$u(n) = \sum_{k=0}^{\infty}\delta(n-k)$$

let m = n - k, we can rewrite it as...

$$u(n) = \sum_{m=-\infty}^{n}\delta(m)$$

This can be represented as capturing the delta 0 value (which is 1) and keep that for the rest of the journey to n.

You can also write the impulse back as a linear combination of unit steps like:

$$\delta(n) = \frac{u(n) - u(n-1)}{1} $$

Adding the derivation of 1 reminds of the **derivative**!

## Systems

Systems are also functions. They have an input x and an output y, which are both **scalar signals**. This is thus a single input single output system :O

Signal is the mapping that takes an x in the domain and maps it to the corresponding y, these xs and ys are NOT values but functions with its individual plots.

if $$x= [\mathbb{R} \rArr \mathbb{R}] \ and \ y= [\mathbb{R} \rArr \mathbb{R}]$$, then F is a CT system.

if $$x= [\mathbb{Z} \rArr \mathbb{R}] \ and \ y= [\mathbb{Z} \rArr \mathbb{R}]$$, then F is a DT system.

We might also see sampling functions and other forms that transforms discrete into continuous.

## The Linearity of System
Linearity is established if satisfying the following conditions:

**Scaling:**

$$x_1 \rArr y_1$$

$$\rArr \alpha x_1 \rArr \alpha y_1$$

**Additivity:**

$$x_1 \rArr y_1$$

$$x_2 \rArr y_2$$

$$\rArr x_1 +  x_2 \rArr y_1 + x_2$$

Thus, Linearity means:

$$\rArr \alpha x_1 + \beta x_2 \rArr \alpha y_1 + \beta x_2$$