---
date: '2025-09-02T13:28:31-04:00'
title: 'Lecture 3: One-way Functions'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Some Definitions

Our definition of many-message semantic security from last lecture was pretty complex, so we can study the definition within a more restricted context.

A *one-way function* is a function $f : \\{0, 1\\}^* \to \\{0, 1\\}^*$ which has the following two properties:

1. It is easy to compute: there exists PPT $\mathcal{B}$ such that for any $x \in \\{0, 1\\}^*$, $\mathcal{B}(x) = f(x)$.
2. It should be hard to invert: for any PPT $\mathcal{A}$, there exists negligible $\varepsilon$ such that

$$\text{Pr}_{x \sim \\{0, 1\\}^n} [x^\prime \leftarrow \mathcal{A}(1^n, f(x)), f(x^\prime) = f(x)] \leq \varepsilon(n)$$

for all $n$. Here, $1^n$ is the security parameter.

Our goal for the next month will be to show that one-way functions exist if and only if semantically secure secret key encryption schemes exist.

We might be tempted to define $\text{OWF}^\prime$:

$$\text{Pr}_{x \sim \\{0, 1\\}^n} [x^\prime \leftarrow \mathcal{A}(1^n, f(x)), x^\prime = x] \leq \varepsilon(n).$$

However, notice that constant functions satisfy $\text{OWF}^\prime$ but not $\text{OWF}$. They are only equivalent when $f$ is injective.

Additionally, the security parameter is necessary or at least a string with length $n$. Without the security parameter, the input length might be very small, not allowing the adversary to compute in polynomial time, meaning we might call some functions one-way when they are actually not.

In fact, we could have $\text{OWF}^{\prime\prime\prime}$:

$$\text{Pr}_{x \sim \\{0, 1\\}^n} [x^\prime \leftarrow \mathcal{A}(1^{n^2}, f(x)), f(x^\prime) = f(x)] \leq \varepsilon(n).$$

This is equivalent to the original definition because polynomial in $n$ is the same as polynomial in $n^2$.

What if we switch the order of quantifiers? Consider $\text{OWF}^{\prime\prime\prime\prime}$: there exists negligible $\varepsilon$ such that for any PPT $\mathcal{A}$,

$$\text{Pr}_{x \sim \\{0, 1\\}^n} [x^\prime \leftarrow \mathcal{A}(1^n, f(x)), f(x^\prime) = f(x)] \leq \varepsilon(n).$$

This is obviously a stronger definition, but we can prove that no functions satisfy it. To prove this, we can fix $\varepsilon$ then let $n_0$ be such that $\varepsilon(n_0) < 1$.

Then $\mathcal{A}$ takes $1^n$ and outputs $y = f(x)$, where $x \sim \\{0, 1\\}^n$. If $n\neq n_0$, then the probability is $0$. If $n = n_0$, we can enumerate all $x^\prime \in \\{0, 1\\}^{n_0}$, which runs in $2^n_0$, which is constant-time.

In practice $2^{1000}$ being constant doesn't mean much, but in theory, it means any adversary can beat $\text{OWF}^{\prime\prime\prime\prime}$.

What if we define $\text{OWF}^{\prime\prime\prime\prime\prime}$: for any PPT $\mathcal{A}$, there exists negligible $\varepsilon$ such that for any $x \in \\{0, 1\\}^n$,

$$\text{Pr} [x^\prime \leftarrow \mathcal{A}(1^n, f(x)), f(x^\prime) = f(x)] \leq \varepsilon(n).$$

No function meets this definition because an adversary could always guess the same thing every time and guess $x^\prime$ for some $x$.

Finally, we may define $\text{OWF}^{\prime\prime\prime\prime\prime\prime}$: for any PPT $\mathcal{A}$, there exists negligible $\varepsilon$ such that for every $n$ there exists an $x \in \\{0, 1\\}^n$ such that

$$\text{Pr} [x^\prime \leftarrow \mathcal{A}(1^n, f(x)), f(x^\prime) = f(x)] \leq \varepsilon(n).$$

This is used in complexity theory, but it is still a little weak. We don't want there to exist an encryption which the adversary can't break; we want the average encryption to be hard.

## Efficiently Sampleable OWFs

A function $f$ and PPT algorithm $\text{Samp}$ form an *efficiently sampleable one-way function* if it's easy to compute and hard to invert: for any PPT $\mathcal{A}$ there exists negligible $\varepsilon$ such that

$$\text{Pr}_{x \leftarrow \text{Samp}(1^n)} [x^\prime \leftarrow \mathcal{A}(1^n, f(x)), f(x^\prime) = f(x)] \leq \varepsilon(n)$$

for all $n \geq 1$.

We can prove that if factoring is hard, then $(f_{mult}, \text{Samp})$ is an esOWF, where $\Samp(1^n)$ is sampled from pairs of $n$-bit primes.

This of course assumes that factoring is hard: for any PPT $\mathcal{A}$, there exisits negligible $\varepsilon$ such that

$$\text{Pr}_{p,q\sim\mathbb{P}_n} [(p^\prime, q^\prime) \leftarrow \mathcal{A}(pq) \quad : \quad p^\prime q^\prime = pq, p^\prime, q^\prime > 1] \leq \varepsilon(n).$$

We can also prove that OWFs exist if and only if efficiently sampleable one-way functions exist.