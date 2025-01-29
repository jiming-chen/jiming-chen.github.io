---
date: '2025-01-29T12:23:31-05:00'
title: 'Lecture 4: Groups and Homeomorphisms'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

One example of a group is $(\mathbb{Z}, +)$. It has identity $e = 0$ and inverse $-a$ for $a$. Another example of a group is $(\mathbb{R} \setminus \\{0\\}, *)$ which has $0$ excluded because $0$ does not have a multiplicative inverse.

Both of these are examples of Abelian groups because the order of the operands does not matter.

One example of a finite group is the symmetric group on (finite) $X$, which is the set of all permutations on $X$. This is usually non-Abelian.

Another example of a group is $GL_n(\mathbb{R}) = \\{\text{all invertible }n\times n\text{ matrices under matrix multiplication}\\}$.

For any group, the following properties hold.

- The identity unique. This is not obvious, as the identity is defined by a property, which doesn't inherently make it unique, but it is easily provable by contradiction.
- The inverse is unique.
- $(gh)^{-1} = h^{-1}g^{-1}$, and $(g^{-1})^{-1}=g$.

For any $g \in G$ and any integer $n$, we can define
$$g^n = \begin{cases}
    g \cdots g \quad n \text{ times} & n > 0 \\\\
    e & n = 0 \\\\
    g^{-1} \cdots g^{-1} \quad n \text{ times} & n < 0.
\end{cases}$$

One can prove $g^n \cdot g^m = g^{m+n}$ and $(g^m)^n = g^{mn}$.

The order of an element $g$ is defined as the smallest $n > 0$ such that $g^n = e$.

Given abstract groups $(G,\cdot) and $(H,*)$, a **homeomorphism** is a function $\phi$ from $G$ to $H$ where $\phi$ is compatible with the operations.

For a homeomorphism,
$$\begin{aligned}
    \phi(g\cdot g^\prime) &= \phi(g) * \phi(g') \\\\
    \phi(e) &= e \\\\
    \phi(g^{-1}) &= \phi(g)^{-1}.
\end{aligned}$$