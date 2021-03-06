# Distance is relative: Introduction

Most people could probably calculate the distance between two points and even more have an intuition about how distance works. But what is the distance between two strings ? Can we choose another meaningful way to talk about the distance between points ? What exactly characterizes the idea of "distance" ? While those topics are often discussed in introductory college / university courses, I want to give a collection of the different concepts, as they appear in both mathematics and computer science<sup id="a1">[1](#f1)</sup>.

## Examples

### Euclidean distance: $\mathbb{R}^n$

Let's begin with the most familiar example: what is the the distance between two points in 2 dimensions $\left(\mathbb{R}^2\right)$. Let's call the points $X=(x_1, x_2)$ and $Y=(y_1,y_2)$. Pythagoras tells us, that the length of the hypotenuse (and thereby the distance) equals $d(X,Y)=\sqrt{(x_1-y_1)^2+(x_2-y_2)^2}$. 

![Distance between two points](/images/Abstand1.png "Right triangle implicitly declared by two distinct points (unequal to the origin)")

We could also write $d:\mathbb{R}^2\times \mathbb{R}^2 \to \mathbb{R}_{\geq 0}$ 

$$d(X,Y)=\sqrt{\sum_{k=1}^2 (x_k-y_k)^2}.$$

But how does this generalize to $\mathbb{R}^n$(n dimensions)? Previously we had 2 dimensions while summing over two indicies ($k=1,k=2$), for n dimensions we will unsurprisingly sum over n. Generalization of the previous definition: $d:\mathbb{R}^n\times \mathbb{R}^n \to \mathbb{R}_{\geq 0}$

$$d(X,Y)=\sqrt{\sum_{k=1}^n (x_k-y_k)^2}.$$

Nice ! We found a way to characterize the distance between two points in $\mathbb{R}^n$. Is $\tilde{d}(X,Y)=2d(X,Y)$ a valid interpretation of distance?<sup id="a2">[2](#f2)</sup>

### Taxicab: $\mathbb{R}^n$ 

Let's say you have never seen a formal definition of distance and you have never heard of Pythagoras' theorem. How would you define a formula for the distance between two points ? A first guess could be to just add the difference of each coordinate, but to avoid weird geometry<sup id="a3">[3](#f3)</sup>  you need to take the absolute value of each difference.  

$$d(X,Y)=\sum_{k=1}^n \vert x_k-y_k\vert$$

### Maximum:  $\mathbb{R}^n$

Let's think about the following function:

$$d(X,Y)=\max\{\vert x_k-y_k\vert  : 1\leq k \leq n\}$$

This function takes two points in space ($\mathbb{R}^n$) and returns the maximum absolute difference in one of the coordinates. What is $d((1,0.5),(1,1))$, what is $d((0.8,0.5), (1,1))$ ? Does this function satisfy your intuition? 

### Does this describe some sort of distance ? 

Setting: Set $X$.

$$\delta:X\times X \to \{0,1\}, \delta(x,y)=\begin{cases}
    0&\text{ if } x=y\\
    1&\text{ otherwise}\\
\end{cases}$$

This seems weird right ? The distance between two points is either 0 or 1. There is no increase in distance if we pull our points farther away. Notice how we need no structure on X for this "distance"! There is no need to have any notion of addition or subtraction or any operation on our set X, but the test for equality! But in some mathematical sense this does describe a distance!

### Levenshtein distance

While all of the previous examples measured "the" distance between two points in space, we will now consider the distance between two words (strings in particular). One way to think of the distance of two strings is to consider how we must change one of them to get the other. Then we can assign a "penalty" for each change we do and if we are lucky we get a good definition of distance. 
**_Example:_**
![Distance between two points](/images/leven.png "Changing TRFT to TEST") 
Let $\Omega$ be the set of characters we allow (possibly including an "empty" character). With a penalty of 1 and some luck we will find the Levenshtein distance. $\text{d} : \Omega^n \times \Omega^n \to \mathbb{N}$. Let's compare two strings composed of (up to) n characters $a,b\in \Omega^n$ with $\vert a\vert,\vert b\vert$ non-empty characters respectively. We will define the Levenshtein distance using a recursive helper function:

$$\text{lev}_{a,b}(i,j)=\begin{cases}
    \max(i,j) & \text{ if} \min(i,j)=0\\
    \min \begin{cases}
        lev_{a,b}(i-1,j)+1\\
        lev_{a,b}(i,j-1)+1\\
        lev_{a,b}(j-1,i-1) + 1 \cdot (a_j\neq b_j)
    \end{cases} & \text{otherwise}
\end{cases}$$

We will now define the distance of a and b : $d(a,b)=\text{lev}_{a,b}(\vert a\vert,\vert b\vert)$. I think it is a lot harder to judge the quality of this definition. The intuition behind the Levenshtein distance is the following: Each case (2nd. and 3rd. case are the same, but for symmetry) represents one transformation of one of the strings

1. Deletion
2. Insertion
3. Substitution

While the initial idea of distance between two strings seems logical<sup id="a4">[4](#f4)</sup>, this definition is quite convoluted. So let's think about properties any function describing distance should satisfy to have a consistent way to talk about distances.  

## Metrics

Given a set $\xi$ we want to find a function $d:\xi\times\xi \to$ ? What should the output be? Let's choose $\mathbb{R}$ for now (this is not the most abstraction we can get, but a useful example). I would hope most of us would agree that a distance should always be $\geq 0$. It would also be great if the distance between $X$ and $Y$ is the same as the distance between $Y$ and $X$ right? Another property, which should be perfectly fitting to your intuition, is that the distance between two points is 0 if and only if they are the same point. Well we are close to a useful definition, but there is one rule our new distance needs to satisfy for it to match our intuition. Let's consider three points $X,Y,Z\in\mathbb{R}^n$. What do the values of $d(X,Y)$ and $d(Y,Z)$ tell us about $d(X,Z)$? Well these two numbers should represent the distance from $X$ to $Y$ and from $Y$ to $Z$. Most would probably argue that the distance should be less than $d(X,Y)+d(Y,Z)$ right? After all the distance between two points should be minimal in the sense that you can't find a path (think collection of points, where the first and the last point are X,Z respectively) which has a shorter distance, shouldn't it? Well if we consider $X=(0,0),Y=(1,0),Z=(2,0)$ we get (using our normal definition of distance)
$$d(X,Y)+d(Y,Z)=1+1=d(X,Z).$$
Therefore $d(X,Z)\leq d(X,Y)+d(Y,Z)$ i.e. it is to strict to demand a strict inequality as opposed to $\leq$. But now we have every thing we need to define a mathematical abstraction of distance!

### Definition

Given a set $\xi$, a function $d_{\xi}: \xi \times \xi \to [0,\infty)$ is called a metric (and $(\xi ,d_{\xi})$ a metric space) if
1. $\forall_{x,y\in\xi}: d(x,y)=d(y,x)$ (symmetry).
2. $\forall_{x,y\in\xi}: d(x,y)=0\iff x=y$
3. $\forall_{x,y,z\in\xi}: d(x,z)\leq d(x,y)+d(y,z)$ (triangle-inequality)

### Reconsidering the examples

Notice that every example above defines a metric !

To check if a function is a metric we have to check these three properties. Typically the first two are quit easy to check, while the triangle-inequality might take some work. I will show how to prove that our usual definition and $\delta$ are metrics. The triangle inequalities are proven at the end of this post. 

### The euclidean metric is a metric

Setting: $x,y,z\in\mathbb{R}^2$, $\vert x \vert_2=\sqrt{\sum_{k=1}^n (x_k)^2}$

1. $d(x,y)=\sqrt{(x_1-y_1)^2+(x_2-y_2)^2}=\sqrt{(-x_1+y_1)^2+(-x_2+y_2)^2}=d(y,x)$
2. $d(x,y)=0 \iff (x_1-y_1)=0 \land (x_2-y_2)=0 \iff x=y$.
3. One can proof that $\vert x+y\vert_2\leq\vert x\vert_2+\vert y\vert_2$. Substituting $x'=x-y$ and $y'=y-z$ we get $\vert x-z\vert_2 \leq \vert x-y\vert_2+\vert y-z\vert_2$.

### $\delta$ is a metric 

Setting: $x,y,z\in X$, $\delta$ as defined above.

1. $\delta(x,y)=\delta(y,x)$ by definition (if you want to be pedantic you could show this by evaluating all possible cases: $x=y,x\neq y$). You could also argue that $=$ is symmetric.
2. $\delta(x,y)=0 \iff x=y$ is literally the definition.
3. This is just a matter of evaluating all the possible cases (or using the transitivity of $=$ ).

## Norms

Let's talk about a concept closely related to distance: length. If we think about two points $\in \mathbb{R}^n$ surely the distance in the euclidean metric is the same as the length of the vector, which translates one of the points onto the other. Maybe we can find a way to mimic this relationship if we replace "distance" with "metric" ?

### Definition

For a $\mathbb{R}$-[vector space](https://en.wikipedia.org/wiki/Vector_space) V a function $f:V\to [0,\infty)$ is called a norm if 

1. $\forall_{\lambda\in \mathbb{R}, x\in V}: f(\lambda x)=\vert \lambda\vert f(x).$
2. $\forall_{x\in V}: f(x)=0\iff x=0$
3. $\forall_{x,y\in V}: f(x+y)\leq f(x)+f(y)$

This is not the most abstract definition we could use (read: $\mathbb{R}$-vector space for example), but it is a very intuitive definition.

$(V,f)$ is said to be a normed vector space.

### Connection to metric spaces

If $(V,f)$ is a normed vector space, $(V, (x,y)\mapsto f(x-y))$ is a metric space.

We can therefore think of normed vector spaces as a special case of metric spaces.

### Which metrics are induced by norms ?

Let's check our previous examples. The first three examples are norms (notice how $\mathbb{R}^n$ is a $\mathbb{R}$-vector space). Given that we already know that they are metrics, we only have to check the first property, which should be quite easy. But what about the last two examples ? Well as for the Levenshtein example, we don't even have the vector space structure, so there is little hope for us here :( How about the last example ? Well while we might operate on a vector space, $\delta(X^2)=${$0,1$} therefore we have no chance to satisfy the first property. We can conclude that only the first three norms induce a metric.

### Connection to dot products.

A euclidean vector space (vector space with **a** dot product<sup id="a5">[5](#f5)</sup> over the field $\mathbb{R}$) $(V,\langle\cdot\rangle)$ can be interpreted as a normed vector space: For $x\in V$ choose $f(x)=\sqrt{\langle x, x\rangle}$ and you have got yourself a normed vector space.

## Extra Proofs

### $\vert x+y\vert_2\leq\vert x\vert_2+\vert y\vert_2$

Proof (think about the details and notice how we are proving properties of the norm which induces the euclidean metric!):

Setting: $x,y\in\mathbb{R}^n$, $\langle x,y\rangle$ is the dot product of two vectors. 

Then $\vert x+y\vert_2\leq\vert x\vert_2+\vert y\vert_2$ follows by taking the square root after showing :

$$\vert x+y\vert_2^2=\vert x\vert_2^2+2 \langle x,y\rangle +\vert y\vert_2^2\leq \vert x\vert_2^2+2\vert x\vert_2\vert y\vert_2 +\vert y\vert_2^2 = (\vert x\vert_2+\vert y\vert_2)^2.$$

$\vert x\vert_2^2+2 \langle x,y\rangle +\vert y\vert_2^2\leq \vert x\vert_2^2+2\vert x\vert_2\vert y\vert_2 +\vert y\vert_2^2$ uses the following inequality : $\vert \langle x,y\rangle\vert_2^2\leq \vert x\vert_2\vert y\vert_2^2$.

### taxicab triangle inequality:

For $a,b,c\in\mathbb{R}$ :

$$\vert a-c\vert = \vert a-b+b-c\vert\leq \vert a-b\vert +\vert b-c\vert$$

For $a,b,c\in\mathbb{R}^n$ using the inequality above for each element of the sum:

$$\sum_{k=0}^n\vert a_k-c_k\vert\leq \sum_{k=0}^n \left(\vert a_k-b_k\vert +\vert b_k-c_k\vert\right)=\sum_{k=0}^n\vert a_k-b_k\vert+\sum_{k=0}^n\vert b_k-c_k\vert$$

### Maximum triangle inequality

For $a,b,c\in\mathbb{R}^n$:

$$d(a,c)=\max\{\vert a_k-c_k\vert  : 1\leq k \leq n\}$$

Because of the triangle inequality (taxicab $n=1$) we can proof the following inequality:

$$d(a,c)\leq \max\{\vert a_k-b_k\vert+\vert b_k-c_k\vert  : 1\leq k \leq n\}$$

$$\leq \max\{\vert a_k-b_k\vert  : 1\leq k \leq n\}+\max\{\vert b_k-c_k\vert  : 1\leq k \leq n\}$$

$$=d(a,b)+d(a,c).$$

### Triangle inequality: $\delta$

A priori there are $2^3$ possibilities, from which 5 are valid:

- $x=y$
  - $y=z$
    - $x=z$: $d(x,z)=0\leq d(x,y)+d(y,z)=0+0=0$
    - $x\neq z$ (Contradiction: Transitivity)
  - $y\neq z$
    - $x=z$ (Contradiction: Transitivity)
    - $x\neq z$: $d(x,z)=1\leq d(x,y)+d(y,z)=0+1$
- $x\neq y$
  - $y=z$
    - $x=z$ (Contradiction: Transitivity)
    - $x\neq z$:  $d(x,z)=1\leq d(x,y)+d(y,z)=1+0$
  - $y\neq z$
    - $x=z$: $d(x,z)=0\leq d(x,y)+d(y,z)=1+1$ 
    - $x\neq z$: $d(x,z)=1\leq d(x,y)+d(y,z)=1+1$

### Triangle inequality: Levenshtein (Intuition)

If you accept that the Levenshtein distance describes the minimal number of insertions,deletions and substitutions needed to go from one string to another, we can simply consider three strings $a,b,c\in\Omega^n$. If we have two sequences of such changes $\rho,\mu$, which transform $a$ to $b$ and $b$ to $c$ respectively, we can be sure that $\mu\circ\rho$ transforms $a$ to $c$. Therefore $d(a,c)$ is bounded by $d(a,b)+d(a,c)$. But it does not have to be equal, as the Levenshtein distance is the minimum length of such a sequence of operations. 

### Norms induce metrics

If $(V,f)$ is a normed vector space, $(V,(x,y\mapsto f(x-y)))$ is a metric space.

We have to show:
1. $\forall_{x,y\in V}: d(x,y)=d(y,x)$ (symmetry).
2. $\forall_{x,y\in V}: d(x,y)=0\iff x=y$
3. $\forall_{x,y,z\in V}: d(x,z)\leq d(x,y)+d(y,z)$ (triangle-inequality)

Proof:

1. $(x-y)=-1(y-x)$ implies (first property of norms) $f(x,y)=\vert-1\vert f(y,x)=f(y,x)$.
2. $x=y \iff x-y=0$ and the second property of norms $\implies$ the second property of metrics.
3. Apply the triangle inequality for norms for $\tilde{x}=x-y$ and $\tilde{z}=y-z$: $d(x,z)=f(x-y+y+-z)\leq f(x-y)+f(y-z)=d(x,y)+d(y,z)$.


### The euclidean metric is induced by a norm

The other three norms induced by metrics are proven in the exact same way. Because we already know that the euclidean metric is in fact a metric, we only have to check the first property(why?):

$$\forall_{\lambda\in \mathbb{R}, x\in V}: f(\lambda x)=\vert \lambda\vert f(x).$$

For any $\lambda \in \mathbb{R},x\in\mathbb{R}$:

$$f(\lambda x)=\sqrt{\sum_{k=1}^n (\lambda x_k)^2}=\sqrt{\lambda^2\sum_{k=1}^n x_k^2}=|\lambda|\sqrt{\sum_{k=1}^n x_k^2}=|\lambda|f(x).$$

## Further reading

- Paris metro metric: [wolfram](https://mathworld.wolfram.com/FrenchMetroMetric.html) or alternatively [scientificamerican](https://blogs.scientificamerican.com/roots-of-unity/a-few-of-my-favorite-spaces-the-sncf-metric/).
- The second part of Distance is relative (once it's finished).

## Footnotes

<b id="f1">1</b> Although there is a strong focus on mathematics. I will use edit distances (mainly Levenshtein) as an example, which does not stem from mathematics. 
[↩](#a1)

<b id="f2">2</b> Yes. After reading this blog post you should be able to confirm that $\tilde{d}$ is both a norm and a metric.
[↩](#a2)

<b id="f3">3</b> Consider the points $(0,1),(-1,0)$ and their distance.
[↩](#a3)

<b id="f4">4</b> You need to have a serious discussion about which operations you allow. One might argue that substitution is just a composition of deletion and insertion. That would be a perfectly valid argument.
Other metrics on strings (also called edit distances): [Damerau–Levenshtein distance](https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance) or the edit distance based on [LCS](https://en.wikipedia.org/wiki/Longest_common_subsequence_problem#Relation_to_other_problems).
[↩](#a4)

<b id="f5">5</b> I.e. a symmetric positive semidefinite bilinear form (In case of a  $\mathbb{R}$ vector space) or a hermitian positive semidefinite sesquilinear form. [↩](#a5)
