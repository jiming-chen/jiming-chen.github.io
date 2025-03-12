---
date: '2025-03-12T13:45:41-04:00'
title: 'Lecture 18: Epipolar Geometry'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

Given two camera centers and a point in 3D space, we can define the plane containing these points. We call this the *epipolar plane*. The *epipolar line* for a camera is the intersection of the epipolar plane and the camera plane.

We can also define the *epipolar pencil*, which is the family of planes formed by changing the point in 3D space.

For the math, we assume the intrinsic parameters are the identity matrix, and we assume the world coordinate system is centered at the 1st camera pinhole with the $Z$ axis along the viewing direction. Then,

$$\begin{aligned}
\mathbf{x}\_{img}^{(1)} &\equiv \mathbf{x}\_w \\\\
\mathbf{x}_{img}^{(2)} &\equiv R\mathbf{x}_w + \mathbf{t}
\end{aligned}$$

Then,

$$\mathbf{x}_2 \cdot (\mathbf{t} \times R\mathbf{x}_1) = 0.$$