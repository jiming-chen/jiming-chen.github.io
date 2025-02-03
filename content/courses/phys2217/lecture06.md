---
date: '2025-02-03T10:24:52-05:00'
title: 'Lecture 6: Electric Potential, Conductors, and Electric Dipoles'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Electric Potential

If a sphere has an evenly distributed charge density $\rho$, then we know the sphere is not a conductor because in a conductor, the charge will evenly distribute on the surface because that is the minimum energy configuration.

Recall that electric potential over a path from $p_1$ to $p_2$ is

$$\phi = -\int_{p_1}^{p_2} \mathbf{E} \cdot ds$$

and it measures the potential energy difference per unit charge between any two points. When discussing the potential of a single point, we mean the amount of potential in relation to a point at infinity. The potential has units

$$1 \text{volt} = 1 \frac{\text{joule}}{\text{coulomb}}.$$

We can also write

$$\phi = \frac{1}{4\pi\epsilon_0} \int_v \frac{\rho}{r} dv.$$

> **Example**: What is the potential outside of a uniformly charged sphere with charge distribution $\rho$ and radius $R$?
> $$\phi_{out}(r) = -\int_\infty^r E(r^\prime)dr^\prime = -\int_\infty^r \frac{\rho R^3}{3\epsilon_0r^{\prime2}} dr^\prime = \frac{\rho R^3}{3\epsilon_0r}.$$
> We can find the potential inside the sphere too by breaking into two integrals.

Intuitively, it makes sense that equipotential surfaces or lines are perpendicular to electric fields.

## Conductors in Electric Fields

When a conductor is inside an electric field, the electrons move against the electric field, inducing an opposite electric field inside the conductor. This leads to an "axiom" of conductors, in which the net electric field inside them is always $0$.

## Electric Dipoles

Imagine we have two charges, $q$ and $-q$, a distance $\ell$ apart. Now, consider a point $P$ a large distance away. According to this approximation, the angle between $P$ and $q$ and the angle between $P$ and $-q$  as well as the two distances should be approximately equal. Then,

$$\phi(r,\theta) = \frac{q\ell\cos\theta}{4\pi\epsilon_0r^2}$$

which is easier to use than the exact expression

$$\phi_P = \frac{kq}{r_1} - \frac{kq}{r_2}.$$