# Random thoughts[1]: The relation between proper folds and monoids

## What is a folding ?

A lot of programming languages have a builtin function similar to Haskells [fold(l/r)](https://wiki.haskell.org/Fold). While the specifics vary between different languages, the general idea is to apply a binary operation to the elements of some iterable / recursive data structure, thereby "combining" the elements to a new object.
A typical signature of such a function looks similar to the type of foldl in Haskell:

```haskell
foldl :: Foldable t => (b -> a -> b) -> b -> t a -> b
```

A common use case of foldl would lead to the following:

- t = List
- a = some numeric type:  float,double,int...
- b = some other datatype

and thereby:

```haskell
foldl :: (b -> a -> b) -> b -> [a] -> b
```

### Definition

For the latter case: 
```haskell
foldl :: (b -> a -> b) -> b -> [a] -> b
```
For $x_0 \in b,n\in\mathbb{N},r\in a^n$
 and $f: b\times a\to b$:

$$\text{foldl}(f,x_0,r)=\begin{cases}
\text{foldl}(f,(f(x_0,r_1),r_{2:}) & \text{if } r=[r_1,\dots,r_n] \text{ where } n\geq 1, \\
x_0 & \text{if } r=[]
\end{cases}$$

And 

$$\text{foldr}(f,x_0,r)=\begin{cases}f(r_1, \text{foldr}(f,x_0,r_{2:})) & \text{if } r=[r_1,\dots,r_n] \text{ where } n\geq 1, \\
x_0 & \text{if } r=[]
\end{cases}$$

Notation: For $r=[r_1,\dots,r_n]$ let $r_{k:}:=[r_k,\dots,r_n]$

### Other / similar implementations:

- python: functools.reduce, itertools.accumulate
- f#: fold
- C++: std:accumulate 

## A small proof

## Setting 

I will be exploring folds of a very restricted type, because this post is just trying to formalize an intuition.

Consider a foldr/l which operates on lists in the following way:

(Haskell syntax)

```haskell
foldl :: (a->a->a)->a->[a]->a 
foldr :: (a->a->a)->a->[a]->a 
```

### Definition

For some $\varphi:(a\times a\to a)$ I call $(a,\varphi)$ a proper folding if: 

1. $$\exists_{0_{\varphi}\in a}\forall_{k\in a}: \varphi(0_{\varphi},k)=k$$
2. $$\forall_{r\in a^n}: \text{foldl}(\varphi,0_{\varphi},r)=\text{foldr}(\varphi,0_{\varphi},r)$$

### Claim

I claim that $(a,\varphi)$ is a monoid if and only if $(a,\varphi)$ is a proper folding.

### Proof.

If $(a,\varphi)$ is a monoid, then 

$$\forall_{a,b,c\in a}:\varphi(a,\varphi(b,c))=\varphi(\varphi(a,b),c)$$

and 

$$\exists_{0\in a}\forall_{k\in a}: \varphi(0,k)=k=\varphi(k,0)$$

hold. We can now choose  $0_\varphi:=0$. The second property follows by repeatedly applying associativity:

$$\forall_{r\in a^n}:\text{foldl}(\varphi,0_{\varphi},r)=\varphi(\dots\varphi(\varphi(\varphi(0_\varphi, r_1),r_2),r_3),\dots),r_n)=\varphi(\dots\varphi(r_1,r_2),\dots),r_n)$$

$$\stackrel{\text{asso.}}{=}\varphi(r_1,\dots\varphi(r_{n-2},\varphi(r_{n-1},r_n)))=\varphi(r_1,\dots\varphi(r_{n-2},\varphi(r_{n-1},\varphi(r_n,0_{\varphi}))))$$

$$=\text{foldr}(\varphi,0_{\varphi},r).$$ 


If $(a,\varphi)$ is a proper folding, we can choose ${0_{\varphi}}$ to be our identity element:
$$\forall_{k\in a}:\varphi(0_{\varphi},k)\stackrel{\text{proper}}{=}\varphi(k,0_{\varphi})=k.$$ 

Because: 
$$\forall_{k\in a^1}:\text{foldr}(\varphi,0_{\varphi},k)=\text{foldr}(\varphi,0_{\varphi},k)$$

$$k\stackrel{1.}{=}\varphi (0_{\varphi}, k)\stackrel{2.}{=}\varphi (k, 0_{\varphi})$$

For $k_1,k_2,k_3\in a$ let $k=(k_1,k_2,k_3)\in a^3$.
$(a,\varphi)$ is a proper folding, therefore 

$$\text{foldl}(\varphi,0_{\varphi},r)=\text{foldr}(\varphi,0_{\varphi},r)$$

or equivalently 

$$\varphi(\varphi(0_{\varphi},k_1),\varphi(k_2,k_3))=\varphi(k_1,\varphi(k_2,k_3))=\varphi(\varphi(k_1,k_2),k_3)=\varphi(\varphi(k_1,k_2),\varphi(k_3, 0_{\varphi}))$$

We can conclude that $\varphi$ is associative and $(a,\varphi)$ is a monoid.