---
date: '2025-08-25T10:08:42-04:00'
title: 'Lecture 1: Course Overview'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

This lecture serves as theater, e.g. to show why studying programming languages might be cool.

## Programming Language Quirk Examples

One cool example of programming languages being weird is how `[] + []`, `[] + {}`, `{} + []`, and `{} + {}` all evaluate to different things in JavaScript.

If we run `a = [1], 2` and then `a[0] += [3]`, you might think we would have `a = [1, 3], 2`, but in Python, tuples are not mutable, so we get an error. However, after the error, `a` still evaluates as `([1, 3], 2)`. Why does this happen? At the source level, it might look like if the `+=` line threw an error, then its side effects wouldn't happen. But, if we look at the bytecode representation (by using the `dis` module), we see that is actually not the case.

In Java, two researchers (Nada Amin and Ross Tate) realized that Java's type system had gotten so complex that it no longer enforced type constraints as it was initially designed. They played with classes and methods in such a way so that any type could be cast to any other type (by taking advantage of the fact that subtypes can be cast up to their supertypes and by using wildcards without actually checking fot the existence of such the wildcard type). Ultimately, this allows `0` to be coerced to be a string and beat the compile-time type check but not the run-time type check.

## Programming Language Design

What makes a good programming language? One answer might be that it is one people use. But many people disagree that JavaScript is the best language. Some good features include the following:

- Simplicity
    - Implementing things should not be confusing (some languages like Perl and Python have their primitives non-orthogonal meaning there are many ways to do things).
- Readability
    - Code should be elegant and have intuitive syntax.
- Safety
    - Rust doesn't have an intermediate virtual machine like Java, but it still allows safe programming.
- Modularity
    - This allows composition and collaboration.
- Efficiency
    - It should be possible to write a good compiler so that code can actually run fast.

Almost always, these criteria conflict.

- Types usually restrict what types of programs you can write.
- Runtime checks eliminate errors, e.g. Python and Java can crash a program whereas C enters undefined behavior. But this has a cost and can affect efficiency.
- Some languages are nice for quick prototyping (Python) but may not be the best for long-term development (like OCaml might be better for).

Therefore, a lot of work in programming language design goes into maximizing these benefits while minimizing those conflicts, and people have come up with fairly creative solutions to these problems.

## Formal Semantics

What do programs mean, precisely? There are three classic approaches to this question.

Operational semantics treats programs like mathematical objects, which we can model and mathematically prove things about on an abstract machine. This is useful for implementing compilers and interpreters. Think finite automata and Turing machines. Start diggin in yo program twin.

Others like Dijkstra and David Gries model programs by the logical formulas they obey. This is called axiomatic semantics and is similar to preconditions, postconditions, and invariants that are often taught in introductory programming classes. Separation logic, which uses axiomatic semantics, is relatively new and is being adopted by big tech companies who write large verifiers.

Denotational semantics models programs literally as mathematical objects, like functions. This is helpful especially in functional programming, as it can help us think about composition as well as when things are expressable or equivalent. Professor Foster works in domain-specific languages, where denotational semantics is useful.

While these are all separate approaches, they generally agree. Additionally, these approaches come up in linguistics and NLP as well.

Interestingly, few languages have a formal semantics. JavaScript had a formal semantics imposed on it decades after it became mainstream. WebAssembly had it from the set. At Cambridge, they're trying to create formal semantics for instruction set architectures.

This is important because we want to know what our code means, especially if code controls our planes, money, etc. Also related is that we want verification of code, and to verify code, it helps to know what the code should even do to begin with.

But why don't many languages have formal semantics? In reality, programming languages can get very complex and dense, and this may not be feasible, but formal semantics is still important. There is often pressure to "add things in" when designing languages, and we need to be sure that new features don't break old features (which is what happened when Java added generics and wildcards).

One interest in AI/program synthesis is constraining code that LLMs generate to be safe since they are trained on code that may not necessarily be safe.