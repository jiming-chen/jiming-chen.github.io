---
date: '2025-01-29T10:32:35-05:00'
title: "Lecture 4: Flux and Gauss' Law"
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

Flux can be imagined as the amount of flow going perpendicular to a surface. We can write
$$\Phi = \int \mathbf{E} \cdot d\mathbf{a}.$$

Gauss' Law states that

$$\int \mathbf{E} \cdot d \mathbf{a} = \frac{1}{\epsilon_0} \sum_i q_i = \frac{1}{\epsilon_0} \int \rho dV.$$

It turns out that because of the inverse square dependence, a charge outside of a Gaussian surface has zero net electric field flux through that surface.

What if we wanted to find the electric field at any point in space outside of a sphere of radius $r$ with surface charge density of $\rho$? We can use a larger sphere as a Gaussian surface, and Gauss' law tells us that

$$\mathbf{E} = \frac{\rho R^3}{3r^2\epsilon_0},$$

which tells us that we can treat the sphere like a point charge at its center.

Inside, the sphere, we have

$$\mathbf{E} = \frac{\rho r}{3\epsilon_0}.$$

We can also use Gauss' law to find the electric field due to a line charge. If we draw a cylinder whose axis is the line charge, we can see that by symmetry, the ends have no flux through them, so the net flux out of the cylinder is its curved surface area times the field at any point. Additionally, it also equals $\lambda L$, where $\lambda$ is the charge density at any point and $L$ is the length of the cylinder.