---
date: '2025-01-27T10:11:49-05:00'
title: 'Lecture 3: Electric Potential Energy and Electric Fields'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Electric Potential Energy

If you have a charges $q_1$ and $q_2$ infinitely far away, and you want to bring them together so that they are $r_{12}$ apart, work has to be done. Work is

$$\int \mathbf{F} \cdot d\mathbf{s} = \int_{\infty}^{r_{12}} -\frac{1}{4\pi\epsilon_0} \frac{q_1q_2}{r_{12}^2}dr = \frac{1}{4\pi\epsilon_0} \frac{q_1q_2}{r_{12}}.$$

Also, recall that work is path independent. Generalizing for multiple points, we have

$$U = \frac{1}{2} \sum_{j=1}^N \sum_{j\neq k}^N \frac{1}{4\pi\epsilon_0} \frac{q_1q_k}{r_{jk}}.$$

The $1/2$ is there because pairs of points are double-counted. The above statement is equivalent to Coulomb's law.

### Practice Question

Suppose we have two charges $q$ and a charge at their midpoint $Q$. How must they be so that the system is in equilibrium (force on each charge is zero)?

**Answer**: We can set the total electric potential energy of the system to $0$:

$$\frac{1}{4\pi\epsilon_0} \frac{q^2}{2d} + \frac{2}{4\pi\epsilon_0} \frac{qQ}{d} = 0.$$

This simplifies to $\boxed{q=-4Q}$.

## Electric Fields

Reecall that in a system,

$$\mathbf{F_0} = \frac{1}{4\pi\epsilon_0} \sum_{j=1}^N \frac{q_0q_j}{r_{0j}^2} \hat{\mathbf{r}_{0j}}.$$

However, this doesn't really mean anything unless we have a charge $q_0$, which we refer to as a **test charge**. Thus, we can place the test charge at any point to describe the system, and as a result, we really have a vector function (a.k.a. field), and it is conventional to draw field lines going out of positive points and into negative points (assuming a positive test charge).

We can let our test charge be a unit (positive) test charge, and the equation of the electric field becomes

$$E(x,y,z) = \frac{1}{4\pi\epsilon_0} \sum_{j=1}^N \frac{q_j\hat{\mathbf{r}}\_{0j}}{r_{0j}^2}.$$

In practice, we often have charge distributions that we have to integrate:

$$E(x,y,z) = \frac{1}{4\pi\epsilon_0} \int \frac{\rho(x^\prime,y^\prime,z^\prime)\hat{\mathbf{r}}}{r^2} dx^\prime dy^\prime dz^\prime.$$