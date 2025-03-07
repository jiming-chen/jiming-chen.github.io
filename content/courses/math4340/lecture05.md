---
date: '2025-01-31T12:21:09-05:00'
title: 'Lecture 5: More Groups and Subgroups'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

An *action of $G$ on $X$* is a rule that assigns to each element $g \in G$ and each element $x \in X$ another element $g \cdot x \in X$ so that

- $e \cdot x = x$,
- and $(g_1g_2) \cdot x = g_1 \cdot (g_2 \cdot x)$.

This is equivalent to giving a *group homomorphism*

$$\phi : G_1 \to G_2$$

from $G_1$ to $G_2$. $\phi$ is an *isomorphism* if it is a bijection.

> **Fact**: Any group acts faithfully on some set. Essentially, you can view any group as a permutation group.

> **Fact**: Any two groups of order $2$ are isomorphic.

> **Theorem**: If $p$ is prime, then any two groups of order $p$ are isomorphic.

A *subgroup* $H \subset G$ is a subset which is closed under the operations. It follows that

- $e \in H$,
- if $h^\prime, h^{\prime\prime} \in H$, then $h^\prime h^{\prime\prime} \in H$,
- and if $h \in H$, then $h^{-1} \in H$.

> **Theorem**: If $H \subset G$ and $G$ is finite, then $\\# H \mid \\# G$.

**Proof**: Define an equivalence relation on $G$ where $g \sim g^\prime$ if and only if $g^{-1}g^\prime \in H$. then, we can make an equivalence class for each subgroup, and it works out.

> **Fact**: $\text{Sym}(5)$ has $5! = 120$ elements, but it does not have any subgroups with $40$ elements.

Given a group $G$ and element $g \in G$, we can define a subgroup $\langle g \rangle$ generated by $g$

which is the smallest subgroup containing $e, g, g^{-1}, g^2, g^{-2}, \ldots$. The order of $\langle g \rangle$ is also called the order of $g$.

> **Theorem**: If $p$ is prime, then any two subgroups of order $p$ are the same.

> **Corollary**: The subgroup generated by $g$ must be the whole group $G$.

If $G^\prime$ and $G^\prime^\prime$ are groups, then we can define a new group $G^\prime \times G^\prime^\prime$, which is their Cartesian product. This is also a group, and we call it a product of groups.