# Highlights of Analysis (WIP)

This post contains the particularly interesting definitions and results from the "Analysis 1" course at the University of Bonn (WS19/20). Some of the interesting, but rudimentary definitions will be omitted here, but can be found in the "mathematical basics" blog post.

## Sequences ($\mathbb{R}$ or [metric space](https://en.wikipedia.org/wiki/Metric_space) )

### Nested Intervals with exactly 1 shared point:

Let $(I_n)_{n\in\mathbb{N}}$ be a sequence of intervals $\subset \mathbb{R}$ s.t.

1. $I_n$ is none empty and compact ($I_n=[a_n,b_n]$ with $a_n\leq b_n$).
2. $\forall_{n\in\mathbb{N}}: I_{n+1}\subseteq I_n$.
3. $\forall_{\epsilon>0} \exists n\in\mathbb{N}: b_n-a_n <\epsilon$.

$$\left\vert\bigcap_{n\in\mathbb{N}} I_n\right\vert = 1$$

This can be used to define roots or to help proof Bolzano-Weierstraß.

### Convergence
Let $(X,d)$ be a metric space.
Let $\varphi:\mathbb{N}\to X$ be a sequence. The sequence converges to $x\in X$ if :
$$\forall_{\epsilon>0}\exists_{N_\epsilon\in\mathbb{N}}:\forall_{n\geq N_\epsilon} d(\varphi(n), x)<\epsilon.$$   

### Monotonic sequences:
Setting: metric space ($\mathbb{R},|\cdot|$).

A monotonic sequence converges if and only if it is bounded. The limit of a monotonically increasing/decreasing sequence equals the supremum/infimum of the sequence.

### Sequences on $\mathbb{R}$ have a monotonic subsequence

Setting: metric space ($\mathbb{R},|\cdot|$).
For each sequence $\varphi:\mathbb{N}\to\mathbb{R}$ $\exist_{\rho}$ with $\rho:\mathbb{N}\to\mathbb{N}$ monotonically increasing s.t. $\varphi\circ\rho$ is monotonic.

### Bolzano-Weierstraß:
Setting: metric space ($\mathbb{R},|\cdot|$).

Every bounded sequence has a convergent subsequence.

This follows directly from the previous two statements. A different proof uses a [sequence of nested intervals](https://www.youtube.com/watch?v=eM3S74kchoM).

### Cauchy sequence

Setting: metric space ($X,d$).

Let $\varphi:\mathbb{N}\to X$ be a sequence. This sequence is called cauchy if:
$$\forall_{\epsilon>0}\exists_{N_\epsilon\in \mathbb{N}}\forall_{n,m\geq N_\epsilon}: d(\varphi(n),\varphi(m))<\epsilon.$$

X is called complete or Cauchy space if every cauchy sequence converges. $(\mathbb{R}, \vert\cdot\vert)$ is complete.

### Compact subsets of metric spaces

Setting: metric space ($X,d$).

Let $K\subset X$ satisfy one of the following conditions:

1. For all sequences $\varphi:\mathbb{N}\to K$ $\exist$ a convergent subsequence.
2. For all $\{O_i\}_{i\in I}$ with 
 $K\subseteq \bigcup_{i\in I}O_i$ $\exists \text{ finite }J\subset I$ s.t. 
 $$K\subseteq \bigcup_{j\in J}O_j.$$

These two conditions are equivalent and we call such subsets compact.

## Riemann series theorem (Umordnungssatz)

$\varphi:\mathbb{N} \to \mathbb{R}$ $\sum_{n=0}^\infty \varphi(n)$ convergent, but not absolutely convergent. For any $x\in\mathbb{R}\exist_{\sigma: \text{ permutation}} \text{ s.t.:}$

$$\sum_{n=0}^\infty \varphi\circ \sigma(n)=x.$$

## Continuous functions

You can also define $f:A\to Y$, $A\subset X$. To keep things concise I will write $f:X\to Y$ in this chapter.

### Definition

Setting: metric spaces ($X,d_X$) and ($X,d_Y$), $x\in X$ 
A function $f:X\to Y$ is said to be continuous in $\bar{x}\in X$ if 

$$\forall_{\epsilon>0}\exists_{\delta>0}\forall_{x: d_X(x,\bar{x})<\delta}:d_Y(f(x),f(\bar{x}))<\epsilon.$$

We call $f$ continuous in $A\subset X$ if it is continuous for all $x\in A$.

### Equivalent definition

Setting: metric spaces ($X,d_X$) and ($X,d_Y$), $x\in X$ 

A function $f:X\to Y$ is continuous in $\bar{x}\in X$ iff for all  sequences $\varphi:\mathbb{N}\to X$ with $\lim_{n\to\infty}\varphi(n) = \bar{x}\implies \lim_{n\to\infty} f \circ \varphi(n)= f(\bar{x})$. 

### Another equivalent definition

Setting: metric spaces ($X,d_X$) and ($X,d_Y$), $A\subset X$ 

A function $f:X\to Y$ is continuous in $A$ iff $O\subset Y$ open $\implies f^{-1}(O)$ is open as well. 

### Uniform continuity

Setting: metric spaces ($X,d_X$) and ($X,d_Y$), $A\subset X$ 

A function $f: X\to Y$ is called uniformly continuous if 
$$\forall_{\epsilon>0}\exists_{\delta>0}\forall_{x,y: d_X(x,y)<\delta}:d_Y(f(x),f(y))<\epsilon.$$

Note that $\delta$ DOES NOT DEPEND on $x$ or $y$.

### Continuous functions on compact subsets of metric spaces