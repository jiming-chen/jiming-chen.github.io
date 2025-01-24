---
date: '2025-01-24T10:12:56-05:00'
title: "Lecture 2: Charge and Coulomb's Law"
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Vector Calculus Recap

A **field** is a physical quantity that is defined at each point in time and space. It could be a scalar, vector, or tensor. Temperatures are an example of a scalar field and wind an example of vector fields.

Gradient (vector showing direction of steepest ascent), divergence (scalar), and curl (vector showing how much rotation) are vector field operations.

## Charge

Charge has some fundamental properties:
1. There are 2 types of charge.
2. Charges are quantized.
3. Charges are conserved.

## Coulomb's Law

We can write Coulomb's Law:

$$\mathbf{F}\_1 = k\frac{q_1q_2}{r_{12}^2}\hat\mathbf{r}_{12}.$$

Alternatively, we could substitute \\(\frac{1}{4\pi\epsilon_0}\\) for \\(k\\). By comparing \\(Gm^2\\) and \\(kq^2\\) for a proton, we can see that for two protons, the electric force is much stronger than the gravitational force.

Rearranging Coulomb's Law for three total points gives us

$$\mathbf{F} = k\frac{q_1q_3}{r_{13}^2} + k\frac{q_1q_2}{r_{12}^2},$$

which implies that we can calculate separate forces on \\(q_1\\) and add them together (remember we are adding vectors). This is known as **superposition**, which holds true for classical mechanics but not necessarily quantum mechanics. More generally,

$$\mathbf{F} = \sum_n k\frac{q_1q_n}{r_{1n}^2}.$$

If there is no force on a particle, then it is stable. This can be annoying to calculate, so we could look at work/energy instead.