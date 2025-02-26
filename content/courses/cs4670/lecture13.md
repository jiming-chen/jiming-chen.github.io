---
date: '2025-02-26T13:39:12-05:00'
title: 'Lecture 13: Coordinate System Transformations'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

Recall our projection equations

$$x = \frac{X}{Z}, \quad y = \frac{Y}{Z}.$$

However, because we have division, this is not linear. How can we represent it like

$$\mathbf{x}_c = R\mathbf{x}_w + \mathbf{t}?$$



This brings us to homogeneous coordinates, whereby 2D points are actually 3D points and 3D points are actually 4D points. For example, we can write

$$(x,y) = (x,y,1) = (kx, ky, k).$$

This allows us to write the transformation above as

$$\begin{bmatrix}
R\mathbf{x}^w + \mathbf{t} \\\\ 1
\end{bmatrix} = \begin{bmatrix}
R & \mathbf{t} \\\\
0 & 1
\end{bmatrix} \begin{bmatrix}
\mathbf{x}^w \\\\ 1
\end{bmatrix},$$

which makes successive or inverse transformations easy.

As an example, if the camera coordinate system is the same as the world system but with the $y$ coordinate increased by $h$, then the extrinsic matrix of the camera is

$$\begin{bmatrix}
1 & 0 & 0 & 0 \\\\
0 & 1 & 0 & -h \\\\
0 & 0 & 1 & 0 \\\\
0 & 0 & 0 & 1
\end{bmatrix}.$$

Here, $M$ (or $R$) is the identity matrix because the linear transformation involved is the identity (nothing happens). However, $t$ is not the identity; instead the $y$ coordinate should be $-h$ because in the camera the same point would have to have a $y$ coordinate reduced by $h$. Finally, the bottom right element should be $1$.