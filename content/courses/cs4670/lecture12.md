---
date: '2025-02-24T13:29:07-05:00'
title: 'Lecture 12: 3D Coordinates'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

Say we have a line passing through a point $A$ with direction $D$. Then, we can write

$$Q(\lambda) = A + \lambda D.$$

Then, we can have a parallel line

$$R(\lambda) = B + \lambda D.$$

Then, as $\lambda \to \infty$, the projections of both lines go to $(D_X/D_Z, D_Y/D_Z)$.

If $D_Z = 0$, it means the lines lie parallel to the $xy$-plane.

Additionally, we can take the limit of planes. Given

$$N_X X + N_Y Y + N_Z Z = d,$$

taking $Z \to \infty$ results in

$$N_X x + N_Y y + N_Z = 0,$$

which is independent of $d$.

## Changing Coordinate Systems

We change coordinate systems by using change of basis.

### Rotations

Rotations can be represented as matrix multiplication:

$$R(\theta) = \begin{bmatrix}
    \cos\theta & -\sin\theta \\\\
    \sin\theta & \cos\theta
\end{bmatrix}.$$

In order to represent 3D rotations, we can break into directions:

$$R = R_xR_yR_z.$$

For example,

$$R_x(\theta) = \begin{bmatrix}
1 & 0 & 0 \\\\
0 & \cos\theta & -\sin\theta \\\\
0 & \sin\theta & \cos\theta.
\end{bmatrix}$$

Note that for any rotation $R$,

$$R^TR = I.$$

If we take the determinant of the above equation, we get

$$\det(R) = \pm1,$$

If $\det(R) = 1$, then we have a rotation, and if $\det(R) = -1$, then we have a reflection.

### Aligning Coordinate Systems

If we have a point $\mathbf{p}$ in world coordinates, we can express it in camera coordinates with

$$\mathbf{q} = R\mathbf{p} + \mathbf{t}.$$