---
date: '2025-09-03T10:22:44-04:00'
title: 'Lecture 4: Semantics'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Progress

Recall the progress property that we discussed last lecture that helps us prove that any well-formed configuration eventually steps to some integer.

We want to prove that for all$e \in \text{Exp}$, $P(e)$ holds, where

$$P(e):= \forall \sigma \in \text{Store} \ . \ \text{wf}(\sigma, e) \implies C(\sigma, e),$$

where

$$C(\sigma, e) := e \in \text{Int} \lor \exists \sigma^\prime \in \text{Store}, e^\prime \in \text{Exp} \ . \ \langle \sigma, e\rangle \to \langle \sigma^\prime, e^\prime\rangle.$$

We prove by induction on the structure of $e$.

In the case where $e = x$, we let $\sigma \in \text{Store}$ and assume $\text{wf}(\sigma, x)$. By the definition of being well-formed, we have $\text{fvs}(x) \subseteq \text{dom}(\sigma)$. Then, since $\text{fvs}(x) = \\{x\\}$, we have $\exists n \ . \ \sigma(x) = n$.

By $\text{Var}$, $\langle \sigma, x\rangle \to \langle \sigma, n\rangle$, which finishes the case.

In the case where $e = e_1 + e_2$, we can let $\sigma \in \text{Store}$ and assume $\text{wf}(\sigma, e_1, e_2)$. By the definition of well-formedness, $\text{fvs}(e_1 + e_2) \text{dom}(\sigma)$. Then, since $\text{wf}(\sigma, e_1)$ and $\text{wf}(\sigma, e_2)$. By the induction hypothesis on $e_1$, we have $C(\sigma, e_1)$.

In the subcase where $e_1 \in \text{Int}$, e.g. $e_1 = n_1$. Integers don't step, so at this point, we may only invoke induction on $e_2$. By the inductive hypothesis on $e_2$, we have $C(\sigma, e_2)$.

In the subsubcase where $e_2 \in \text{Int}$, e.g. $e_2 = n_2$, then let $n = n_1 + n_2$. By $\text{Add}$, $\langle \sigma, n_1 + n_2 \rangle \to \langle \sigma, n \rangle$.

In the subsubcase where $\exists \sigma^\prime, e^\prime \ . \ \langle \sigma, e_2 \rangle \to \langle \sigma^\prime, e^\prime \rangle$. By $\text{RAdd}$, $\langle \sigma, n_1 + e_2 \rangle \to \langle \sigma^\prime, n_1 + e^\prime \rangle$, which finishes the subsubcase and the subcase.

In the subcase where $\exists \sigma^\prime, e^\prime \ . \ \langle \sigma, e_1\rangle \to \langle \sigma^\prime, e^\prime \rangle$, by $\text{LAdd}$, $\langle \sigma, e_1 + e_2 \rangle \to \langle \sigma^\prime, e^\prime + e_2\rangle$, which finishes the subcase and the proof.