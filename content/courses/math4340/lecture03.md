---
date: '2025-01-27T12:21:34-05:00'
title: 'Lecture 3: Permutation Groups and Abstract Groups'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

The **factorial** is defined as $n! = 1 \cdot 2 \cdot 3 \cdots n$ for $n \geq 1$ though we may define $0!=1$.

We also have binomials:

$$\binom{n}{i} = \frac{n!}{i!(n-i)!}.$$

> **Fact**: If $p$ is prime, then $p$ divides $\binom{p}{i}$ unless $i=0$ or $i=p$.

We can also write out the binomial expansion:

$$(a+b)^n = \sum_{i=0}^n \binom{n}{i} a^ib^{n-i}$$

where $a,b$ are "numbers." Importantly, the binomial coefficient is counting something, specifically the number of subsets of $1, \ldots, n$ with exactly $i$ elements.

If $S$ is a set, then a function $f : S \to S$ which is a bijection is called a **permutation** on $S$.

There are two notations commonly used to describe permutations:

1. $2$-$1$ notation shows how each element maps to its image in two rows, and it is what the textbook uses:

$$\sigma = \binom{1 \quad 2 \quad 3 \quad 4 \quad 5 \quad 6 \quad 7}{4 \quad 5 \quad 2 \quad 7 \quad 1 \quad 3 \quad 6}.$$

2. Cycle notation points each element to the next element in the list, e.g. $\sigma = (*, ?) (\cdot)$ (each set of parentheses is a cycle). Note that in this example $(\cdot)$ is a trivial cycle, so we may leave it out.

The **identity permutation** $e$ is the function $e(x) = x$.

We can also write the **inverse permutation** $\sigma^{-1}$ which maps each element to the element that points to it in $\sigma$.

Composition of permutation functions is referred to as **product of permutation**. It acts as function composition normally would and is notated the same way using $\circ$. Additionally, order of composition matters, and that a composition of bijections is also a bijection. We avoid composing permutations that act on different sets.

> **Fact**: The number of fixed points in $\sigma\circ\tau$ and $\tau\circ\sigma$ is the same.

Permutation composition has several properties:

1. Associativity: $(\sigma_1\circ\sigma_2) \circ \sigma_3 = \sigma_1 \circ (\sigma_2\circ\sigma_3)$.
2. Identity: $\sigma\circ e = \sigma = e\circ\sigma$.
3. Inverse: $\sigma\circ\sigma^{-1} = e = \sigma^{-1}\circ\sigma$.

These properties hold for function composition in general.

A **permutation group** is a set of permutations that satisfies three properties:

1. It contains the identity $e$.
2. It is closed under the inverse $^{-1}$, e.g. if $\sigma\in G$, then $\sigma^{-1} \in G$.
3. It is closed under product, e.g. if $\sigma,\tau\in G$, then $\sigma\circ\tau\in G$.

Some examples of permutation groups are
- the set of all permutations
- $\\{e\\}$
- the set of all motions that move all the points on a sponge and maintain its shape

> **Fact**: If a set $S$ is finite, it is enough to verify only closure under product to ensure that a set of permutations on $S$ is a permutation group.

The **order of a permutation group** is the number of elements in the group. This is according to our textbook, but some people say it is the number of elements in the set $S$.

We can also talk about **order of elements**. We say $\sigma$ is of order $n$ if $\sigma^n=e$.

The order of $e$ is $1$, and obviously, it is the only permutation with order $1$.

> **Fact**: If $S$ is finite, then any permutation has finite order, meaning repeatedly taking permutations results in a repeating cycle of sets.

An **abstract group** is a set $G$ with a binary operation $* : G \times G \to G$ with some axioms:

1. Associativity: $(a\*b)\*c = a*(b*c)$.
2. Existence of a special element (identity) $e$ or $1$: $(a\*e)=a=(e*a)$.
3. Existence, for any element $a$, of an element $a^{-1}$ (inverse): $a*a^{-1}=e=a^{-1}*a$.

> **Example** Any permutation group is an abstract group.