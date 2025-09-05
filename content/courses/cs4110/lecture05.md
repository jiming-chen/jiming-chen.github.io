---
date: '2025-09-05T10:18:47-04:00'
title: 'Lecture 5: Big-Step Semantics'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

We can write a grammar for OCaml-style lists:

$$\begin{aligned}
    e ::&= n \\\\
    |& \ [ \ ] \\\\
    |& \ e_1 :: e_2 \\\\
    |& \ e_1 @ e_2 \\\\
    |& \ \text{rev } e \\\\
    |& \ \text{len } e
\end{aligned}$$

along with $\ell ::= [ \ ] \ | \  n :: \ell$ and $v ::= n \ | \ \ell$.

Now, we can define the big-step semantics using the notation $e \Downarrow v$. We need a reflexive rule and a nil rule, but the cons rule looks like this:

$$\frac{e_1 \Downarrow n_1 \quad e_2 \Downarrow \ell_2}{e_1 :: e_2 \Downarrow n_1 :: \ell_2} \text{Cons}.$$

When writing the inference rule for append, we might be tempted to have $\ell_1 @ \ell_2 \Downarrow \ell^\prime$ in the premises, but that uses circular reasoning. Instead of having arbitrary lists in our premise, we should insist some shape. For example if $e_1$ had a head and a tail, we could cons the head onto the result of appending the tail with $e_2$. Of course, this requires having an append rule for if $e_1 \Downarrow [ \ ]$.

Reverse and length are also easy to write rules for.

One disadvantage of big-step semantics is that it captures the good cases, and anything else (non-termination, being unable to step) is just treated as undefined behavior, whereas in small-step semantics, we would know exactly what fails.

**Theorem**: If $\text{len } \ell_0 \Downarrow n_0$, then $\text{len }(\text{rev }\ell) \Downarrow n_0$.

To prove this, we could induct on $\ell_0$, $n_0$, or even the inference rule for $\text{len}$.