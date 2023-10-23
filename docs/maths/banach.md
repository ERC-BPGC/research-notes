# Banach's Fixed Point Theorem

<strong>Theorem: </strong> The theorem also known as the _contraction_ _mapping_ _theorem_, states that if a function $\tau$ is a contraction mapping on a complete metric space $(X,d)$, then $\tau$ has a unique fixed point in $X$.

We approach the proof by first defining the contraction mapping, and then using an iterative method to show the existence of a fixed point.

### Some Preliminaries

<strong>Contraction: </strong> A function $\tau: X \mapsto X$ is a contraction mapping on a metric space $(X,d)$ if there exists a constant $0 < k < 1$ such that $\forall$ $x,y \in X$:

$$
d(\tau(x),\tau(y)) \leq kd(x,y)
$$

<strong>Complete Metric Space: </strong> A metric space $(X,d)$ is complete if every Cauchy sequence in $X$ converges to a point in $X$.

<strong>Cauchy Sequence: </strong> A sequence $(x_n)$ in a metric space $(X,d)$ is a Cauchy sequence if $\forall$ $\epsilon > 0$, $\exists$ $N \in \mathbb{N}$ such that $\forall$ $m,n \geq N$:

$$
d(x_m,x_n) < \epsilon
$$

### Proof

Let $(X,d)$ be a complete metric space and $\tau: X \mapsto X$ be a contraction mapping on $X$. We will show that $\tau$ has a unique fixed point in $X$ ie. $\exists$ $x^* \in X$ such that $\tau(x^*) = x^*$.

We can define an iterative process as follows:

$
x^{(0)} \in X \\
x^{(n+1)} = \tau(x^{(n)}) \quad \forall n \in \mathbb{N}\\
$

Hence we obtain  asequence {$x^{(n)}$} in $X$. We need to show that this sequence converges. Since the sequence is in a complete metric space, it is enough to show that this sequence is Cauchy.

$$
\begin{aligned}
d(x^{n}, x^{n+1}) &= d(\tau(x^{(n-1)}), \tau(x^{(n)}))\\
     &\leq kd(x^{(n-1)}, x^{(n)})\\
     &\leq k^2d(x^{(n-2)}, x^{(n-1)})\\
     &\vdots\\
     &\leq k^nd(x^{(0)}, x^{(1)})\\
\end{aligned}
$$

Now by the triangular inequality we can see that:

$$
\begin{aligned}
d(x^{(m)}, x^{(n)}) &\leq d(x^{(m)}, x^{(m+1)}) + d(x^{(m+1)}, x^{(m+2)}) + \dots + d(x^{(n-1)}, x^{(n)})\\
     &\leq k^md(x^{(0)}, x^{(1)}) + k^{m+1}d(x^{(0)}, x^{(1)}) + \dots + k^{n-1}d(x^{(0)}, x^{(1)})\\
     &= k^md(x^{(0)}, x^{(1)})\sum_{i=0}^{n-m-1}k^i\\
     &= k^md(x^{(0)}, x^{(1)})\frac{1-k^{n-m}}{1-k}\\
     &\leq \frac{k^md(x^{(0)}, x^{(1)})}{1-k}\\
\end{aligned}
$$

Since $0 < K < 1$ we can choose $m$ sufficiently large such that $d(x^{m}, x^{n})$ is small, and hence the sequence is Cauchy.

Finally, since the sequence is Cauchy, it converges to a point $x \in X$, ie. &nbsp; $x^{(n)} \xrightarrow{} x$

We now need to show that this point is a fixed point.

$$
\begin{aligned}
d(x,\tau(x)) &\leq d(x,x^{(n)}) + d(x^{(n)}, \tau(x))\\
&= d(x,x^{(n)}) + d(\tau(x^{(n-1)}), \tau(x))\\
&\leq d(x,x^{(n)}) + Kd(x^{(n-1)}, x)\\
\end{aligned}
$$

Since the sequence converges to $x$, &nbsp; $d(x,\tau(x)) \leq 0$ and hence $x = \tau(x)$.

Finally, we need to show that this fixed point is unique. Suppose there exists another fixed point $y \in X$ such that $y = \tau(y)$. Then:

$$
\begin{aligned}
d(x,y) &= d(\tau(x), \tau(y))\\
&\leq Kd(x,y)\\
\end{aligned}
$$

This implies $d(x,y) = 0$ and hence $x = y$ since $0 < K < 1$

Hence we have shown that the sequence of approximations converges to a point that is the fixed point, and that this fixed point is unique.

This completes the proof.




