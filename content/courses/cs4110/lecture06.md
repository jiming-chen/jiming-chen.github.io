---
date: '2025-09-08T10:15:30-04:00'
title: 'Lecture 6: IMP'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

This lecture is taught by Dexter Kozen and is about a simple imperative language called IMP. It has

- arithmetic expressions: $a \in \text{Aexp} \quad a ::= x | n | a_1 + a_2 | a_1 \times a_2$.
- Boolean expressions: $b \in \text{Bexp} \quad b::= \text{true} | \text{false} a_1 < a_2$.
- commands: $c \in \text{Com} \quad c::= \text{skip} | x := a | c_1 ; c_2 | \text{if } b \text{ then } c_1 \text{ else } c_2 | \text{while } b \text{ do } c$.

This is the syntax of the language. To describe the (small-step) semantics, we can define three relations, which tell us which store and arithmetic expression pairs can step to which store and arithmetic expression pair, and the same for boolean expression and commands.

We can do the same with big-step semantics.