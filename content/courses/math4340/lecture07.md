---
date: '2025-02-05T12:23:25-05:00'
title: 'Lecture 7: Rings'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Rings

Recall that rings are sets along with two operations, $+$ and $*$.

- Additive group: $(R,+)$ is an abelian group, which we call $R$.
- Multiplicative group: $(R^\*,\*)$ is a group.

The reason we use $R^\*$ is because $(R,*)$ doesn't necessarily have inverses, so we define

$$R^* = \\{r \in R \mid \exists r^\prime, rr^\prime = r^\prime r = 1\\}.$$

We say $R$ is called an *integral domain* if $R$ is commutative (meaning multiplication is commutative) and if $ab = 0$ implies $a = 0$ or $b = 0$. Thus, if you have any two nonzero elements, their product is nonzero.

$\mathbb{Z}$ is an integral domain. However, $\mathbb{Z}/6\mathbb{Z}$ is not because the class of $2$ and the class of $3$ are such that $[2][3] = [6] = [0]$.

## Fields

We call $R$ a *field* if

- it is commutative,
- non-zero elements have (multiplicative) inverses (technically stronger than ring requirement), and
- $0 \neq 1$.

## Subrings

We say $S \subset R$ is a *subring* if is a ring under the same operations. Importantly, this means $0$ and $1$ are in the subring. Apparently, some "subrings" can have different identities from the large ring, which is why we specify "same operations."

> **Example**: $\mathbb{Z}$ is a subring of $\mathbb{Q}$.

## Ring Homomorphisms

A *ring homomorphism* is a map which is compatible with the operations. In other words,

$$\phi : R \to R^\prime$$

is such that

- $\phi(0) = 0$,
- $\phi(a + b) = \phi(a) + \phi(b)$,
- $\phi(1) = 1$, and
- $\phi(a\*b) = \phi(a)\*\phi(b)$.

Some people drop the third condition, but we prefer to keep it. Additionally, condition $4$ implies condition $1$. Remember that the elements and operations on the right side are different from the left.

> **Fact**: $S$ is a subring of $R$ is equivalent to saying $S \xhookrightarrow{} R$ is a ring homomorphism, where $\xhookrightarrow{}$ is the inclusion map.

> **Fact**: If $\phi : R \to R^\prime$ is a homomorphism, then
> $$\text{im}(\phi) = \\{\phi(r) \mid r \in R\\} \subseteq R^\prime$$
> is a subring or $R^\prime$, and
> $$\text{ker}(\phi) = \{r \in R \mid \phi(r) = 0\} \subseteq R$$
> is not a subring or $R$.

### Group Homomorphisms vs. Ring Homomorphisms

If $G$ is any group, and $S$ is the trivial group with $1$ element, then there is only one homomorphism from $G$ to $S$ because every element in $G$ must map to the identity element in $S$.

Additionally, there is only one homomorphism from $S$ to $G$ because the identity in $S$ must map to the identity in $G$.

For rings, let $S$ be the trivial ring with $1$ element, where $0 = 1$. Then, there is a unique map from $R \to S$.

There is always a unique homomorphism from $\mathbb{Z} \to R$. We can do this by setting $\phi(0) = 0_R$ and $\phi(1) = 1_R$ and setting $\phi(2) = \phi(1) + \phi(1) = 1_R + 1_R$ and so on.

## Characteristic of Rings

Let $\phi$ be a homomorphism from $\mathbb{Z} \to R$. Then,

$$\text{char} R = \begin{cases}
0 & \phi \text{ is injective}, \phi(n) \neq 0 \text{ for } n \neq 0 \\\\
n & \phi(n) = 0, n \text{ is the smallest such } n.
\end{cases}$$

For integral domains, the characteristic is either $0$ or a prime number.

## Products of Groups vs. Products of Rings

If $G_1$ and $G_2$ are groups, then we are forced to make $G_1\times G_2$ such that

$$(g,h)\*(g^\prime,h^\prime) = (gg^\prime,hh^\prime).$$

This also allows us to make projections like $p_1((g,h)) = g$. Importantly, there are also two maps, $\iota_1$ and $\iota_2$ which go in the opposite direction. We are forced to let the other element in the output be the identity of the other group:

$$\begin{aligned}
    \iota_1(g) &= (g,e_2), \\\\
    \iota_2(h) &= (e_1,h).
\end{aligned}$$

In the case of rings, if we let $R_1$ and $R_2$ be rings, $R_1\times R_2$ is a ring, and $0$ in the ring $R_1\times R_2$ is $(0,0)$ and the $1$ is $(1,1)$. This time, we have

$$(g,h) + (g^\prime,h^\prime) = (g + g^\prime,h + h^\prime) \text{ and } (g,h)\*(g^\prime,h^\prime) = (gg^\prime,hh^\prime).$$

This is pretty expected though one would still have to verify the ring properties.

Additionally, we have the same projection mappings. However, $\iota_1 : R_1 \to R_1 \times R_2$ is not a ring homomorphism because while the image of the function is closed uner addition and multiplication, it is not a subring.