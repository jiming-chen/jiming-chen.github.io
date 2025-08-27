---
date: '2025-08-26T13:23:06-04:00'
title: 'Lecture 1: Course Overview and Introduction'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Logistics

This is a theoretical course. It is about math, definitions, and theorems, not real world practical security and programming.

There are 6 (plus one extra) problem sets, and the lowest score is dropped. They are due roughly every other Thursday.

## Cryptography Fun Examples

In Yao's millionaires problem, you have two millionaires, each with a net worth. They want to find out who is richer but "without revealing any other information" such as the actual net worths, as simply telling each other their net worths is boring.

Let's say you have a proof of the Riemann hypothesis. You're a loser undergrad, so you go to your professor and ask for advice, and the professor says, "no you don't." You bug the professor endlessly, and he finally agrees to look at it, at which point it (and the million dollar prize) is now the professor's. After all, who are they going to believe? It turns out that cryptography has a solution for you called a zero-knowledge proof which allows you to convey to your professor that you have proved it without actually showing him the proof.

Alice has a message which she would like to convey to Bob. Like in the millionaire's problem, she could simply send her message, but this time, there is a third party named Eve who is trying to read the message and who also has access to whatever Alice sends to Bob. Therefore, Alice wants to send some "encoding" of the message which Bob can decode but which Eve cannot. More specifically, we want a way to *encrypt* a *plaintext* $m$ into a *ciphertext* $c$, which Bob can *decrypt*, but "does not reveal $m$ to Eve".

The above problem is extremely old, and humans have come up with several ad hoc solutions:

- The Caesar cipher simply shifts all letters in the plaintext by a constant amount. This solution is pretty weak.
- Pig Latin is another solution. It is also not that secure.

For higher stakes, such as wars, we need better solutions. We need something secure against entire nation-states for decades or even mathematically secure.

## Encryption Schemes

When designing an encryption scheme, we have to prove security and correctness. Security is the interesting part, but correctness is still important (after all, sending 0 every time is secure but not correct).

Formally, an *encryption scheme* consists of a key space $K$, a plaintext space $M$, and a ciphertext space $C$. Additionally, we need the following:

- An algorithm $\text{Enc}$ such that if $k \in K, m \in M, c \leftarrow \text{Enc}(k, m)$, then $c \in C$. The $\leftarrow$ indicates that $c$ is sampled from the distribution induced by $\text{Enc}$ (this will be important later).
- An algorithm $\text{Dec}$ such that if $k \in K$, $c \in C$, and $m = \text{Dec}(k, c)$, then $m \in M$.
- **Correctness**: $\forall m \in M, k \in K, \text{Dec}(k, \text{Enc}(k, m)) = m$.
- An algorithm $\text{Gen}$ such that $k \leftarrow \text{Gen}()$ with $k \in K$. The purpose of the key is that it is something hard to know that Bob knows and Alice doesn't.

Ultimately, the encryption scheme is a 6-tuple. We will treat the adversary as fully knowing the encryption scheme (as opposed to the Caesar cipher, where we just hope the adversary doesn't know what's going on).

In 1949, Claude Shannon gave a definition (or rather three equivalent ones) for security, called perfect indistinguishability:

An encryption scheme is *perfectly indistinguishable* if for any $m_0, m_1 \in M$ and $c \in C$,

$$\text{Pr}\_{k \leftarrow \text{Gen}()} [\text{Enc}(k, m_0) = c] = \text{Pr}_{k \leftarrow \text{Gen}()} [\text{Enc}(k, m_1) = c].$$

We can show that there actually is an encryption scheme that satisfies this. One example of such an encryption scheme is Shannon's one-time pad. We can simply let $K = M = C = \\{0, 1\\}^n$, where $\text{Gen}() \sim \\{0, 1\\}^n$, $\text{Enc}(k, m) = k \oplus m$, and $\text{Dec}(k, c) = k \oplus c$.

It can be proved that this encryption scheme is correct, and it is also very easy and efficient in practice.

In order to see that the one-time pad is perfectly indistinguishable, we can use the fact that $a \oplus b = c \iff a = b \oplus c$, allowing us to see that the probability that any key of length $n$ encodes a message to $c \in C$ is $1/2^n$ for any message $m$.

The problem with the one-time pad is you can only use it once since if you use it twice, an adversary can learn information about the messages. This is because $(k \oplus m_1) \oplus (k \oplus m_2) = m_1 \oplus m_2$. One thing Eve can learn, for example, is whether two messages are the same.