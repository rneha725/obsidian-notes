

### Math
#### 1/2 + 1/3 + 1/4 ... 1/n = ~log n

Let

$$
S(n) = n + \frac{n}{2} + \frac{n}{3} + \cdots + \frac{n}{n}.
$$

Factor out \(n\):

$$
S(n) = n\left(1 + \frac{1}{2} + \frac{1}{3} + \cdots + \frac{1}{n}\right).
$$

Group the terms in parentheses by powers of two:

$$
1 + \frac{1}{2}
+ \left(\frac{1}{3} + \frac{1}{4}\right)
+ \left(\frac{1}{5} + \frac{1}{6} + \frac{1}{7} + \frac{1}{8}\right)
+ \cdots
$$

Each group after the first contributes at least \(\frac{1}{2}\). For example,

$$
\frac{1}{3} + \frac{1}{4}
\geq \frac{1}{4} + \frac{1}{4}
= \frac{1}{2},
$$

and

$$
\frac{1}{5} + \frac{1}{6} + \frac{1}{7} + \frac{1}{8}
\geq 4\left(\frac{1}{8}\right)
= \frac{1}{2}.
$$

The group sizes double each time, so there are \(\Theta(\log n)\) groups. Therefore,

$$
1 + \frac{1}{2} + \frac{1}{3} + \cdots + \frac{1}{n}
= \Theta(\log n).
$$

Thus,

$$
S(n)
= n \cdot \Theta(\log n)
= \boxed{\Theta(n\log n)}.
$$
