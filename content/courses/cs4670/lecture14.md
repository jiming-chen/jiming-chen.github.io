---
date: '2025-02-28T13:32:01-05:00'
title: 'Lecture 14: More 3D Geometry'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

How do we go from the film plane (projected image) to pixel array? Recall that for the latter, we place the origin in the top left. However, for the former, the origin is in the middle. Thus, to go from the former to the latter, we need to include an offset:

$$u = f\frac{X}{Z} + o_x, \quad v = f\frac{Y}{Z} +o_y.$$

This assumes that the focal length $f$ is the same in each direction. In practice, this may not be true, so we add scaling factors

$$u = \frac{1}{s_x}f\frac{X}{Z} + o_x, \quad v = \frac{1}{s_y}f\frac{Y}{Z} + o_y,$$

whre the aspect ratio is $s_y/s_x$.

To represent this as matrix multiplication, we can write

$$\begin{bmatrix}x^\prime \\\\ y^\prime \\\\ z^\prime\end{bmatrix} = \begin{bmatrix}
f/s_x & 0 & o_x & 0 \\\\
0 & f/s_y & o_y & 0 \\\\
0 & 0 & 1 & 0
\end{bmatrix} \begin{bmatrix}
X \\\\ Y \\\\ Z \\\\ 1
\end{bmatrix}.$$

Then, if $u = x^\prime/z^\prime$ and $v = y^\prime/z^\prime$, then we get the same equations. The large matrix above is called the *intrinsic matrix*.