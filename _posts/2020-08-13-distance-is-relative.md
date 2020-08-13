# Distance is relative: Introduction

Most people could probably calculate the distance between two points and even more have an intuition about what the distance represents. But what is the distance between two strings ? Can we choose another meaningful way to talk about the distance between points ? What exactly characterizes the idea of "distance" ? While those topics are often discussed in introductory college / university courses, I want to give a collection of the different concepts, as they appear in both mathematics and computer science.

## Examples

### Euclidean distance: $\mathbb{R}^n$

Let's begin with the most familiar example: what is the the distance between two points in 2 dimensions $\left(\mathbb{R}^2\right)$. Lets call the points $X=(x_1, x_2)$ and $Y=(y_1,y_2)$. Pythagoras tells us, that the length of the hypotenuse (and thereby the distance) equals $d(X,Y)=\sqrt{(x_1-y_1)^2+(x_2-y_2)^2}$. 

![Distance between two points](/images/Abstand1.png)

We could also write that as $d:\mathbb{R}^2\times \mathbb{R}^2 \to \mathbb{R}_{\geq 0}$ 

$$d(X,Y)=\sqrt{\sum_{k=1}^2 (x_k-y_k)^2}.$$

But how does this generalize to $\mathbb{R}^n$ ? While before we had 2 dimensions while summing over two indicies ($k=1,k=2$) we will now sum over n. We will now change the definition of our distance function : $d:\mathbb{R}^n\times \mathbb{R}^n \to \mathbb{R}_{\geq 0}$  to be

$$d(X,Y)=\sqrt{\sum_{k=1}^n (x_k-y_k)^2}.$$

Nice ! We found a way to characterize the distance between two points in $\mathbb{R}^n$. Is $\tilde{d}(X,Y)=2d(X,Y)$ a valid interpretation of distance ?

### Taxicab: $\mathbb{R}^n$ 

Let's say you have never seen a formal definition of distance and you have never heard of pythagoras' theorem. How would you define a formula for the distance between two points ? A first guess could be to just add the difference of each coordinate, but to avoid weird geometry (Why does this definition fail your intuitions ?) you need to take the absolute value of each difference.  

$$d(X,Y)=\sum_{k=1}^n \vert x_k-y_k\vert$$

### Maximum:  $\mathbb{R}^n$

Let's think about the following function:

$$d(X,Y)=\max\{|x_k-y_k| : 1\leq k \leq n\}$$

This function takes two points in space ($\mathbb{R}^n$) and returns the maximum absolute difference in one of the coordinates. What is $d((0,1),(1,1))$, what is $d((0.5,1), (1,1))$ ? Does this function satisfy your intuition ? 

### Does this describe some sort of distance ? 

Setting: Set $X$.

$$\delta:X\times X \to \{0,1\}, \delta(x,y)=\begin{cases}
    0&\text{ if } x=y\\
    1&\text{ otherwise}\\
\end{cases}$$

This seems weird right ? The distance between two points is either 0 or 1. There is no increase in distance if we pull our points farther away. But in some mathematical sense this does describe a distance !


### Levenshtein distance

While all of the previous examples measured "the" distance between two points in space, we will now consider the distance between two words (strings in particular). One way to think of the distance of two strings is to consider how we must change one of them to get the other. Then we can assign a "penalty" for each change we do and if we are lucky we get a good definition of distance. Let $\Omega$ be the set of characters we allow. With a penalty of 1 and some luck we will find the Levenshtein distance. $\text{d} : \Omega^n \times \Omega^n \to \mathbb{N}$. Let's compare two strings composed of (up to) n characters $a,b\in \Omega^n$ with $|a|,|b|$ non-empty characters respectively. We will define the Levenshtein distance in a recursive manner:

$$\text{lev}_{a,b}(i,j)=\begin{cases}
    \max(i,j) & \text{ if} \min(i,j)=0\\
    \min \begin{cases}
        lev_{a,b}(i-1,j)+1\\
        lev_{a,b}(i,j-1)+1\\
        lev_{a,b}(j-1,i-1) + 1 \cdot (a_j\neq b_j)
    \end{cases} & \text{otherwise}
\end{cases}$$

We will now define the distance of a and b : $d(a,b)=\text{lev}_{a,b}(|a|,|b|)$. I think it is a lot harder to judge the quality of this definition. The intuition behind the levenshtein distance is the following: Each case (2nd. and 3rd. case are the same, but for symmetry) represents one transformation of one of the strings

1. Insertion
2. Deletion
3. Substitution

While the initial idea of distance between two strings seems logical, this definition is quite convoluted. So let's think about properties any function describing distance should satisfy to have a consistent way to talk about distances.  

## Metrics

Given a set $\xi$ we want to find a function $d:\xi\times\xi \to$ ? What should the output be ? Let's choose $\mathbb{R}$ for now (this is not the most abstraction we can get, but a useful example). I would hope most of us would agree that a distance should always be $\geq 0$. It would also be great if the distance between $X$ and $Y$ is the same as the distance between $Y$ and $X$ right ? Another property, which should be perfectly fitting to your intuition, is that the distance between two points is 0 if and only if they are the same point. Well we are close to a useful definition, but there is one rule our new distance needs to satisfy for it to match our intuition. Let's consider three points $X,Y,Z\in\mathbb{R}^n$. What do the values of $d(X,Y)$ and $d(Y,Z)$ tell us ? Well these two numbers should represent the distance from $X$ to $Y$ and from $Y$ to $Z$. If I would ask: "What does this tell us about $d(X,Z)$ ?" Most would probably argue that the distance should be less than $d(X,Y)+d(Y,Z)$ right ? After all the shortest distance between two points should be a straight line, shouldn't it ? Well if we consider $X=(0,0),Y=(1,0),Z=(2,0)$ we get (using our normal definition of distance)
$$d(X,Y)+d(Y,Z)=1+1=d(X,Z).$$
Therefore $d(X,Z)\leq d(X,Y)+d(Y,Z)$ i.e. it is to strict to demand a strict inequality as opposed to $\leq$. But now we have every thing we need to define a mathematical abstraction of distance !

### Definition

Given a set $\xi$, a function $d_\xi:\xi\times\xi\to\mathbb{R}_{\geq0}$ is called a metric (and $(\xi,d_\xi)$ a metric space) if
1. $\forall_{x,y\in\xi}: d(x,y)=d(y,x)$ (symmetry).
2. $\forall_{x,y\in\xi}: d(x,y)=0\iff x=y$
3. $\forall_{x,y,z\in\xi}: d(x,z)\leq d(x,y)+d(y,z)$ (triangle-inequality)

### Reconsidering the examples

Notice that every example above defines a metric !

To check if a function is a metric we have to check these three properties. Typically the first two are quit easy to check, while the triangle-inequality might take some work. I will show how to prove that our usual definition is in fact a metric. The maximum- and the taxicab- metric can be proven in a similar fashion. 

### The euclidean metric is in fact a metric

Setting: $x,y,z\in\mathbb{R}^2$, $|\cdot|_2$ is the euclidean distance

1. $d(x,y)=\sqrt{(x_1-y_1)^2+(x_2-y_2)^2}=\sqrt{(-x_1+y_1)^2+(-x_2+y_2)^2}=d(y,x)$
2. $d(x,y)=0 \iff (x_1-y_1)=0 \land (x_2-y_2)=0 \iff x=y$.
3. One can proof that $|x+y|_2\leq|x|_2+|y|_2$. Substituting $x'=x-y$ and $y'=y-z$ we get $|x-z|_2 \leq |x-y|_2+|y-z|_2$.

### $\delta$ is a metric 

Setting: $x,y,z\in X$, $\delta$ as defined above.

1. $\delta(x,y)=\delta(y,x)$ definition (if you want to be pedantic you could show this by evaluating all the possible cases: $x=y,x\neq y$).
2. $\delta(x,y)=0 \iff x=y$ is literally the definition.
3. This is just a matter of evaluating the cases: $\{(x=y,y=z,y=z),(x=y,y\neq z,x\neq z),(x\neq y,y=z,x\neq z), (x\neq y,y\neq z,x= z),(x\neq y,y\neq z,x\neq z)\}$. Proof is left to the reader :)

## Norms

Let's talk about a concept closely related to distance: length. If we think about two points $\in \mathbb{R}^n$ surely the distance in the euclidean metric is the same as the length of the vector, which translates one of the points onto the other. Maybe we can find a way to mimic this relationship if we move from distance to some metric ?

### Definition

For a $\mathbb{R}$-[vector space](https://en.wikipedia.org/wiki/Vector_space) V a function $f:V\to [0,\infty)$ is called a norm if 

1. $\forall_{\lambda\in \mathbb{R}, x\in V}: f(\lambda x)=|\lambda|f(x).$
2. $\forall_{x,y\in V}: f(x+y)\leq f(x)+f(y)$
3. $\forall_{x\in V}: f(x)=0\iff x=0$

This is not the most abstract definition we could use (read: $\mathbb{R}$-vector space for example), but it is a very intuitive definition.

$(V,f)$ is said to be a normed vector space.

### Connection to metric spaces

If $(V,f)$ is a normed vector space, $(V, (x,y)\mapsto f(x-y))$ is a metric space.

We can therefore think of normed vector spaces as a special case of metric spaces.

### Which metrics are induced by norms ?

Let's check our previous examples first. The first three examples are norms (notice how $\mathbb{R}^n$ is a $\mathbb{R}$-vector space). Given that we already know that they are metrics, we only have to check the first property, which should be quite easy. But what about the last two examples ? Well as for the levenshtein example, we don't even have the vector space structure, so there is little hope for us here :( How about the last example ? Well while we do operate on a vector space, but $\delta(\mathbb{R}^2)=\{0,1\}$ therefore we can only satisfy the first property of a norm for $\lambda \in \{0,1\}$. We can conclude that only the first three norms induce a metric.

### Connection to dot products.

A euclidean vector space (vector space with **a** dot product<sup id="a1">[1](#f1)</sup> over the field $\mathbb{R}$) $(V,\langle\cdot\rangle)$ can be interpreted as a normed vector space: For $x\in V$ choose $\vert x\vert=\sqrt{\langle x, x\rangle}$ and you have got yourself a normed vector space.

## Extra Proofs

### $|x+y|_2\leq|x|_2+|y|_2$

Proof (think about the details !)

Setting: $x,y\in\mathbb{R}^n$, $\langle x,y\rangle$ is the dot product of two vectors. 

Then $|x+y|_2\leq|x|_2+|y|_2$ follows by taking the square root after showing :

$$|x+y|_2^2=|x|_2^2+2 \langle x,y\rangle +|y|_2^2\leq |x|_2^2+2|x|_2|y|_2 +|y|_2^2 = (|x|_2+|y|_2)^2.$$

$|x|_2^2+2 \langle x,y\rangle +|y|_2^2\leq |x|_2^2+2|x|_2|y|_2 +|y|_2^2$ uses the following inequality : $|\langle x,y\rangle|_2^2\leq |x|_2|y|_2^2$.

<b id="f1">1</b> I.e. a symmetric positive semidefinite bilinear form (In case of a  $\mathbb{R}$ vector space) or a hermitian positive definite sesquilinear form. [↩](#a1)