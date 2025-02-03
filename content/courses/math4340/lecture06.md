---
date: '2025-02-03T12:23:25-05:00'
title: 'Lecture 6: Groups Wrap-Up and Rings'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Groups Wrap_up

Given a homomorphism $\varphi : G \to H$, we can define the kernel of $\phi$ as

$$\text{Ker } \varphi = \{g \in G \mid \varphi(g) = e\},$$

where $e$ is the identity of $H$.

We often say $\text{Ker } \varphi \Delta G$, which means it is a normal subgroup of $G$.

> **Fact**: $\varphi$ is injective iff $\text{Ker } \varphi = \{e^\prime\}$.

> **Fact**: If $\gcd(m,n) = 1$, then
> $$\mathbb{Z}/{mn\mathbb{Z}} \cong \mathbb{Z} / {m\mathbb{Z}} \times \mathbb{Z} / {n\mathbb{Z}},$$

$\cong$ is the symbol for "is isomorphic to". You prove this statement by proving $\varphi$ is a group homomorphism and then that $\varphi$ is a bijection, which is only true because $\gcd(m,n) = 1$.

## Rings

A *ring* is a set $R$ with two operations, which we call $+$ and $\cdot$, with the following properties:
- $(R,+)$ is an abelian group.
- $(R,\cdot)$ is almost a group but doesn't have inverses.
- For $a,b,c \in \mathbb{R}$,
$$a\cdot(b+c) = a\cdot b + a\cdot c \quad\text{and}\quad (b+c)\cdot a = b\cdot a + b\cdot a.$$

$\text{End}(A)$, which is the set of all homomorphisms from $A$ to $A$, for an abelian group $A$, is a ring.