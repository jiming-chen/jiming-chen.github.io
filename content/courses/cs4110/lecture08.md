---
date: '2025-09-12T10:18:40-04:00'
title: 'Lecture 8: More IMP'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

How would we add concurrent composition to IMP? For example, if $\sigma(x) = \sigma(y) = 0$ and

$$x := y + 1 ; y := y + 1 \ || \ y := y + 1,$$

then what is $\sigma^\prime$?

If we use big-step semantics, it kind of defeats the point of concurrency, where we want multiple computations to be interleaved. For small-step rules, we might say

$$\frac{\langle \sigma, c_1 \rangle \to \langle \sigma^\prime, c_1^\prime\rangle}{\sigma, c_1 \ || c_2 \rangle \to \langle \sigma^\prime, c_1^\prime \ || \ c_2 \rangle}$$

and

$$\frac{\langle \sigma, c_2 \rangle \to \langle \sigma^\prime, c_2^\prime \rangle}{\langle \sigma, c_1 \ || \ c_2 \rangle \to \langle \sigma^\prime, c_1 \ || \ c_2^\prime\rangle}.$$

Here, we have no scheduler, so we're basically allowing all schedulers.

Additionally, since programs terminate in `skip`, we should have

$$\frac{}{\langle \sigma, \text{skip} \ || \ \text{skip}\rangle \to \langle \sigma^\prime, \text{skip}\rangle}.$$