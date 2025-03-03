---
date: '2025-03-03T13:29:10-05:00'
title: 'Lecture 15: Camera Calibration'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Recap of Aligning Coordinate Systems

Recall that $\mathbf{x}_c = R\mathbf{x}_w + \mathbf{t}$. We first find $R$:

$$R = \begin{bmatrix}X_c & Y_c & Z_c\end{bmatrix}^T.$$

Once we find this, if we let $\mathbf{c}$ be the location of the camera, then

$$R\mathbf{c} + \mathbf{t} = 0 \implies \mathbf{t} = -R\mathbf{c}.$$

Then, the extrinsic (world to camera) matrix is

$$\begin{bmatrix}
R & \mathbf{t} \\\\
\mathbf{0}^T & 1
\end{bmatrix}.$$

Overall, we have

$$
\begin{bmatrix} u \\\\ v \\\\ 1\end{bmatrix} \equiv \begin{bmatrix}
a_{11} & a_{12} & a_{13} \\\\
a_{21} & a_{22} & a_{23} \\\\
0 & 0 & 1
\end{bmatrix} \begin{bmatrix}
f & 0 & 0 & 0 \\\\
0 & f & 0 & 0 \\\\
0 & 0 & 1 & 0
\end{bmatrix} \begin{bmatrix}
R & \mathbf{t} \\\\
\mathbf{0}^T & 1
\end{bmatrix} \begin{bmatrix} U \\\\ V \\\\ W \\\\ 1\end{bmatrix},
$$

where the first two matrices on the RHS are $K$, the camera intrinsics. $K$ changes if you change your camera. The extrinsic matrix contains "camera extrinsics," meaning it changes if the position of the camera changes. These two matrices multiplied together are calleed $P$ (or $M$), which defines how the world is converted into a picture by a camera.

## Camera Calibration

In practice, finding acamera intrinsics and extrinsics can be difficult. Therefore, we want a process for estimating $P$, e.g. camera calibration.

$P$ is a $3\times4$ matrix, but the equation is equivalent (can scale) not equal, so we can set up a system with $11$ unknowns. For the same reason, we actually have

$$\begin{bmatrix}
\lambda x \\\\ \lambda y \\\\ \lambda\end{bmatrix} = P\begin{bmatrix}
X \\\\ Y \\\\ Z \\\\ 1\end{bmatrix}.$$

This gives us three equations. The third equation is

$$\lambda = P_{31}X + P_{32}Y + P_{33}Z + P_{34},$$

so we can substitute that into the other two equations, resulting in two equations with 12 variables:

$$XxP_{31} + YxP_{32} + ZxP_{33} + xP_{34} - XP_{11} - YP_{12} - ZP_{13} - P_{14} = 0,$$

We can write this as $A\mathbf{p} = 0$. Notice that we have not actually solved the scale equivalence problem, so we constrain $\mathbf{p}$ such that $\left\lVert\mathbf{p}\right\rVert = 1$.

Now, our problem is equivalent to minimizing $\mathbf{p}^TA^TA\mathbf{p}$ subject to $\left\lVert\mathbf{p}\right\rVert = 1$. We have a trick for solving this: if we want to minimize $\mathbf{x}^TM\mathbf{x}$ subject to $\left\lVert\mathbf{x}\right\rVert = 1$, then the solution for $\mathbf{x}$ is the eigenvector of $M$ with the smallest eigenvalue.

Thus, we want to find the eigenvector of $A^TA$ with the smallest eigenvalue. This method is called *direct linear transform*.

Now, every point gives us $2$ coordinates, so we need $6$ points, but not any $6$ points work. We want $A$ to be linearly independent, so we should not have coplanar or collinear points.

One way to find these points is to take a picture of a scene and measure it in real life.

Alternatively, we can hold a calibration object in the photo.