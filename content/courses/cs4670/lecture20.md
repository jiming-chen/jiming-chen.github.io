---
date: '2025-03-17T13:36:16-04:00'
title: 'Lecture 20 : Recovering Fundamental Matrix'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---


## Structure From Motion

If we have two cameras whose parameters we don't know, can we recover camera location and orientation without a calibration object in order to recover the 3D world?

Recall that $\mathbf{q}^TF\mathbf{p} = 0$. In order to find $F$, we can use an 8-point algorithm.

Suppose $\mathbf{p} = \begin{bmatrix}x_1 \\\\ y_1 \\\\ 1\end{bmatrix}$ and $\mathbf{q} = \begin{bmatrix}x_2 \\\\ y_2 \\\\ 1\end{bmatrix}$ correspond to the same 3D point. Then, we have a linear system of equations with the entries of $F$ as coefficients.

Since this gives us one equation, we can find eight points which give us eight equations represented as $A\mathbf{f} = \mathbf{0}$. To solve this we want to find $\mathbf{f}$ that minizes $\|A\mathbf{f}\|$. Additionally, we need $F$ to have rank less than $3$ (not full rank). In order to do this, we can just do singular value decomposition.

## Correspondence Outliers

Sometimes, with correspondence, there were matches which were wrong. They were not a few pixels off but completely wrong.

One way of finding a line of best fit is RANSAC. The algorithm consists of randomly sampling two points, fitting a line and counting how many points lie close to the line. This is repeated, and the line with the most inliers is chosen.