---
date: '2025-09-04T13:28:03-04:00'
title: "Lecture 4: Levin's Universal One-Way Function"
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## One-Way Function Examples

### The Discrete Logarithm

There is an algorithm that runs in $O(\log^2q \cdot \log x)$ and computes $h = g^x \mod q$ for $g \in \mathbb{Z}/q\mathbb{Z}$.

We can define $x$ to be the *base-$g$ discrete logarithm of $h$ modulo $q$* if $g^x = h \mod q$.

We usually assume that DLOG is hard:

$$\Pr_{q \sim \mathbb{P}_n, g \sim \mathbb{Z}/q\mathbb{Z}, x \sim \mathbb{Z}/q\mathbb{Z}} [x^\prime \leftarrow \mathcal{A}(q, g, g^x \mod q) \ : \ g^{x^\prime} = g^x \mod q] \leq \varepsilon(n).$$

And we can also prove that if DLOG is hard, then OWFs exist.

### Ajtai's One-Way Function

Consider the family of functions $h(q, A, x) := (q, A, Ax \mod q)$, where $q \in \mathbb{N}, A \in \mathbb{Z}^{n\times m}, $x \in \{0, 1\}^m$. This is a one-way function and the start of lattice-based cryptography.

### Goldreich's PRG

We can have $n$ input bits and $m$ output bits and make all the output bits some function of $n$.

### Random Functions

If we sample $f$ uniformly at random, then it is provably a one-way function but it is not efficiently sampleable.

## Combining One-Way Functions

Let's say you think $f_N$ is a OWF but $f_T$ is not, but Tom thinks $f_T$ is a OWF but $f_N$ is not.

{{< newimgref src="/courses/cs4830/greenfn.png" alt="Green FN" width="50%" >}}
<figcaption>Fig. 1. Green FN.</figcaption>

Can we build a OWF $f_{N+T}$ that is guaranteed to be secure if $f_N$ or $f_T$ is secure?

Immediately, we might be tempted to just compose $f_N$ and $f_T$. But this actually doesn't work! For intuition imagine that the inner function always outputs a string of $0$s and the outer function behaves very "invertibly" when the first half of the input is all $0$s.

Instead we could define

$$f_{N+T}(x_1, x_2) := (f_N(x_1), f_T(x_2)).$$

We can prove that this is a OWF by proving the contrapositive.

Generalizing, given efficiently computable $f_1, \dots, f_k$, the function $g(x_1, \dots, x_k) = (f_1(x_1), \dots f_k(x_k))$ is one way if at least one of the $f_i$ is one way.

## Levin's One-Way Function

Instead of combining (potentially) one-way functions to produce a one-way function, we should want to produce a function that is one-way if *any* function is one-way.

We might think it is this, as a list of all computable functions:

$$f^*(x_1, x_2, \dots, x_n) := (T_1(x_1), T_2(x_2), \dots, T_n(x_n)).$$

But the above function, not only is not efficiently-computable, but it is not computable since there are infinitely many functions.

How about a list of all efficiently computable functions?

We define efficiently computable $T_I^\prime(x)$:

$$T_i^\prime(x) := \begin{cases}
T_i(x) & T_i \text{ halts in time } |x|^{10} \text{ on input }x \\\\
0 & \text{otherwise}
\end{cases}$$

Now, we can write

$$f_L(x_1, x_2, \dots, x_n) := (T_1^\prime(x_1), T_2^\prime(x_2), \dots T_n^\prime(x_n)).$$

Now, if there exists a OWF $f$ that can be computed (deterministically) in time $n^10$, then $f_L$ is a OWF.

To prove this, we can let $f$ be the OWF and notice that there is an $i^*$ such that $f = T_{i^*}^\prime$ since all programs are enumerable.

At this point, the professor's proof started becoming incomprehensible, but I guess there's a contradiction somewhere.