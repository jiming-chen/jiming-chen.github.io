---
date: '2025-08-27T10:12:50-04:00'
title: 'Lecture 2: Introduction to Semantics'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

What is the meaning of a program? We could execute the program or consult a manual, but these are not satisfactory approaches because the program could execution could not work and manuals are not speciic.

A simple language we could look at is that of arithmetic expressions with assignment.

First we establish some metavariables. We can establish $x, y, z \in \text{Var}$ (variables), let $n, m \in \text{Int}$ (integers), and let $e \in \text{Exp}$ (expressions).

The syntax of this language is given by the following BNF grammar:

$$\begin{aligned}
e ::&= x \\\\
&|\ n \\\\
&|\ e_1 + e_2 \\\\
&|\ e_1 \* e_2 \\\\
&|\ x := e_1 \ \; \ e_2
\end{aligned}$$

This describes a set of expressions that includes assignment and sequences.

We can look at expressions as trees (the last three options in the grammar would have multiple children). There can be multiple possible trees for an expression, but we will not worry about that in this course (it is dealt with in CS 4120).

Such a tree is called an *abstract syntax tree* and is the result of parsing. We may assume that the AST has already been achieved.

In languages like OCaml, construction of algebraic data types look very similar to the BNF grammar. In object oriented languages, we can use types and subtypes to implement the same thing.

## Operational Semantics

Recall that operational semantics models programs by execution on an abstract machine and is useful for implementing interpreters and compilers.

A small-step operational semantics describes how such an execution proceeds from configuration to configuration: $\langle \sigma, e \rangle \to \langle \sigma^\prime, e^\prime \rangle$.

A *configuration* $\langle \sigma, e \rangle$ is a pair of a *store* $\sigma$ that records the values of the variables and the *expression* $e$ being evaluated. More precisely, a store is a partial function from variables to integers:

$$\begin{aligned}
\text{Store} &\triangleq \text{Var} \rightharpoonup \text{Int} \\\\
\text{Config} &\triangleq \text{Store} \times \text{Ex}.
\end{aligned}$$

The small-step operational semantics itself is a relation on configurations because we can relate different configurations to each other:

$$(\langle \sigma, e \rangle, \langle \sigma^\prime, e^\prime \rangle) \in "\rightarrow".$$

So how do we define this relation? It seems difficult because the set of configurations is infinite, but we can define if inductively using *inference rules*:

$$\frac{\text{premise}_1 \quad \text{premise}_2 \quad \cdots}{\text{conclusion}} \text{NAME}.$$

This is basically a big if statement; if all the *premises* hold, then the *conclusion* also holds.

Formally, "$\rightarrow$" is the smallest relation that is closed under all the inference rules.

Here is an example inference rule for our arithmetic case:

$$\frac{p = m + n}{\langle \sigma, n + m \rangle \rightarrow \langle \sigma, p\rangle} \text{ADD}.$$

Note that for addition, this is not sufficient; we need rules that transition between configurations that have sums of expressions and expressions as well as integers and expressions. Those are called *congruence rules*.

For assignments, the premise is $\sigma^\prime = \sigma[x \mapsto n]$.