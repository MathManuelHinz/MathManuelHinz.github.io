# Random thoughts

THis blog post contains some fun mathematical thoughts (mostly) about computer science ideas / functions / types. I hope all the ideas are correct, but, in contrast to most of my other blogs, these are original thoughts and not the outcome of some course work.

## foldl/foldr/reduce and monoids

For this thought we will have a look at what a "nice" folding of some list looks like and what it tells us about the structure of the underlying types.

### Setting (Using Haskell syntax)

Consider a foldr/l which operates on lists in the following way:

```haskell
foldl :: (a->a->a)->a->[a]->a 
foldr :: (a->a->a)->a->[a]->a 
```

Mathematically we could write foldl: $(a\times a\to a)\times a\times a^n\to a$ for some set $a$. See the [Definition](https://wiki.haskell.org/Fold) of foldl / foldlr for more details.

### Definition

For some $\varphi:(a\times a\to a)$ I call $(a,\varphi)$ a proper folding if: 

1. $$\varphi_{\pi}(b,a)=\varphi(a,b)$$
2. ${\exists_{0_{\varphi}\in a}\forall_{k\in a}: \varphi(0_\varphi,k)=k}$
3. $$\forall_{r\in a^n}: \text{foldl}(\varphi,0_{\varphi},r)=\text{foldr}(\varphi_{\pi},0_{\varphi},r)$$

A proper folding is called perfect if $\varphi$ is symmetric.

### Claim

I claim that $(a,\varphi)$ is a monoid if and only if $(a,\varphi)$ is a perfect folding.

### Proof.

If $(a,\varphi)$ is a monoid, then 

$$\forall_{a,b,c\in a}:\varphi(a,\varphi(b,c))=\varphi(\varphi(a,b),c)$$

and 

$$\exists_{0\in a}\forall_{k\in a}: \varphi(0,k)=k$$

hold. We can now easily define $\varphi_\pi(b,a):=\varphi(a,b)$ to account for the first property.  We can now choose  $0_\varphi:=0$ to satisfy the second property. The third property follows from repeatedly applying associativity:

$$\forall_{r\in a^n}:\text{foldl}(\varphi,0_{\varphi},r)=\varphi(\dots\varphi(\varphi(\varphi(0_\varphi, r_1),r_2),r_3),\dots),r_n)=\varphi(\dots\varphi(r_1,r_2),\dots),r_n)$$

$$\stackrel{\text{asso.}}{=}\varphi(r_1,\dots\varphi(r_{n-2},\varphi(r_{n-1},r_n)))=\varphi(r_1,\dots\varphi(r_{n-2},\varphi(r_{n-1},\varphi(0_{\varphi},r_n))))$$

$$=\text{foldr}(\varphi,0_{\varphi},r).$$

If $(a,\varphi)$ is a perfect folding, we can choose ${0_{\varphi}}$ to be our identity element:
$$\forall_{k\in a}:\varphi(0_{\varphi},k)\stackrel{\text{perfect}}{=}\varphi(k,0_{\varphi})=k.$$

For $k_1,k_2,k_3\in a$ let $k=(k_1,k_2,k_3)\in a^3$.
$(a,\varphi)$ is a perfect folding, therefore 

$$\text{foldl}(\varphi,0_{\varphi},r)=\text{foldr}(\varphi,0_{\varphi},r)$$

or equivalently 

$$\varphi(\varphi(0_{\varphi},k_1),\varphi(k_2,k_3))=\varphi(k_1,\varphi(k_2,k_3))=\varphi(\varphi(k_1,k_2),k_3)=\varphi(\varphi(k_1,k_2),\varphi(k_3, 0_{\varphi}))$$


### Further comments

This equivalence does not hold for the weaker assumption "$(a,\varphi)$ is a proper folding". Consider the following counter example:

$\Omega=\{A,B,C\}$

$\varphi:\Omega\times\Omega\to\Omega$:

$\varphi$ | A | B | C |
|---      |---|---|---|
A         | A | A | A |
B         | A | B | C |
C         | A | A | A |


$\varphi$ is associative (thereby foldl=foldr), has left identity B, but no right identity (check $\varphi(C,\_)$). Therefore $(\Omega, \varphi)$ is no monoid, but $(\varphi,B,\Omega)$ is a proper folding!