---
date: '2025-08-28T13:31:36-04:00'
title: 'Lecture 2: Semantic Security'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Perfect Indistinguishability

It can be proven that if you have an encryption scheme that is perfectly indistinguishable, then $|K| \geq |M|$.

To prove this, we can choose an $m \in M$ and $c \in C$ such that $c$ is valid, e.g.

$$\text{Pr}[\text{Enc}(k, m) = c] > 0.$$

We can define $M_c := \\{ m^\prime \in M : \exists k^\prime \in K \text{ s.t. } \text{Dec}(k^\prime, c) = m^\prime \\}$, which is the set of messages that can encrypt to $c$. Importantly, $|M_c| \leq |K|$ since each key for which there is a message $m^\prime \in M_c$ can correspond to at most one such message.

By perfect indistinguishability, $|M| = |M_c|$, so $|M| \leq |K|$, and we are done.

Because of this, perfect indistinguishability is too restrictive, so we want a looser definition of semantic security.

## Other Definitions

### One-Message Perfect Semantic Security

Now, we can define an encrytion scheme as being "one-message perfectly semantically secure" if for ANY adversary $\mathcal{A}$, $\text{Pr}[\mathcal{A} \text{ wins}] \leq 1/2$, where the game is that the adversary sends to plaintexts to the challenger, the challenger encrypts both and randomly chooses one, and the adversary tries to guess which plaintext was chosen. This criterion is equivalent to perfect indistinguishability but it has the adversary in play.

### Many-Message Perfect Semantic Security

What about "many-message perfect semantic security"? This time, we can allow the adversary to send two lists of plaintexts of length $\ell$ decided by the adversary. If the adversary can guess a bit from the correct list of ciphertexts with probability at most $1/2$, then it is many-message perfectly semantically secure. It turns out that this definition is vacuous, as no such encryption scheme exists, and we actually proved this above.

So what is a better definition for semantic security? First, we should be okay with $|M_c| \leq |M|$ because although an adversary can gain some information about what the plaintext could be for a cyphertext (e.g. win with probability slightly above $1/2$), we can assume that the adversary has a computational bound (probabilistic, polynomial-time).

We set a security parameter $n$ for the adversary to be consistent with efficiency in other areas of theoreticaly computer science. Basically, adversaries with higher security parameters are more powerful. Then, we can say that the adversary runs in polynomial time on $n$ if and only if the adversary is PPT.

We must also have the encryption scheme run in PPT.

Side note: at this point, we should say $k \leftarrow \text{Gen}(1^n)$.

### Super Many-Message Semantic Security

So how about making an encryption scheme "super many-message semantically secure" if for any PPT adversary $\mathcal{A}$ such that $\text{Pr}[\mathcal{A}(1^n) \text{ wins}] \leq 1/2$.

This turns out to be too strong (no encryption scheme exists) because an adversary can guess a key and compare the return from the challenger to one of the two plaintexts. Although it's not immediately obvious that this gives the adversary a slight advantage, it does.

### Many-Message Semantic Security

Now, we arrive at our last definition ("many-message semantic security"), whereby if for any PPT adversary $\mathcal{A}$, there exists a negligible $\epsilon(n)$ such that for all $n$,

$$\text{Pr}[\mathcal{A}(1^n) \text{ wins}] \leq 1/2 + \epsilon(n).$$

The motivation behind this is that negligible functions are eventually smaller than any (inverse) polynomial function, meaning PPT adversaries cannot take advantage of negligible advantages.

It will also be useful to us to define non-negligibility. A function $\epsilon : \mathbb{N} \to \mathbb{R}$ is *non-negligible* if there exists a positive integer $c$ such that $|\epsilon(n)| \geq n^{-c}$ for infinitely many values of $n$.

A rule of thumb that we can use is that a PPT algorithm will never see an event that happens with negligible probability.

At the end of the day, many-message semantic security is a working definition for semantic security of secret-key encryption. However, it is somewhat complex: three messages go back and forth between the adversarity and the challenger (two lists of plaintexts, a list of cyphertexts, and a bit), and we need PPT and negligibility. But we will develop the tools to be able to construct an encryption scheme such that no PPT adversary has non-negligible advantage in distinguishing $\text{Enc}(m_0)$ from $\text{Enc}(m_1)$.