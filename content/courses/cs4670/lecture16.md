---
date: '2025-03-07T13:26:08-05:00'
title: 'Lecture 16: Binoocular Stereo'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

It is hard to do 3D reconstruction with a single image because obviously we have lost information about depth. However, if we have two cameras, we can use parallax to triangulate.

Assume we have two calibrated cameras and we have already run correspondence to find a pair of matching points:

$$\begin{aligned}
    \begin{bmatrix}
        x_1 \\\\ y_1 \\\\ 1
    \end{bmatrix} &\equiv P^{(1)}\begin{bmatrix}
        X \\\\ Y \\\\ Z \\\\ 1
    \end{bmatrix} \\\\
    \begin{bmatrix}
        x_2 \\\\ y_2 \\\\ 2
    \end{bmatrix} &\equiv P^{(2)}\begin{bmatrix}
        X \\\\ Y \\\\ Z \\\\ 1
    \end{bmatrix}
\end{aligned}
$$

This gives us four equations, but we have three unknowns, so our system is undetermined. this makes sense because two rays in 3D may not always intersect. To solve this, we can use least squares: to solve $A\mathbf{x} = \mathbf{b}$, we want $\min\|A\mathbf{x} - \mathbf{b}\|^2$.

The closed form is

$$\mathbf{x} = (A^TA)^{-1}A^T\mathbf{b}.$$

In our case, we want to minimized the *reprojection error*, which is the squared distance between the projection of the predicted 3D point and the projection of the correct 3D point.

If we have two images, high disparity means the image moved a lot between the two images, so it was closer.

We can also used *normalized cross correlation* (NCC), which is a 3D tensor, to find disparity.