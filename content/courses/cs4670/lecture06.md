---
date: '2025-02-05T13:31:49-05:00'
title: 'Lecture 6: Edge Detection and Clustering'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Edge Detection

Recall that we can use anisotropic filtering because Gaussian filtering should not be the same in every direction. One variant of the anisotropic Gaussian is the Sobel filter:

$$\begin{bmatrix} 1 \\\\ 2 \\\\ 1\end{bmatrix} \* \begin{bmatrix} 1 & 0 & -1 \end{bmatrix} = \begin{bmatrix}
    1 & 0 & -1 \\\\
    2 & 0 & -2 \\\\
    1 & 0 & -1
\end{bmatrix}.$$

This filter first highlights differences between dark and light areas but then does a convolution in the $x$ direction.

### Non-Maximum Suppression

Again, recall that in order to find edges, we want to find peaks in the derivative or gradient magnitude. In order to do this, we can pick a point $q$, and $g(q)$, the gradient, should point to the greatest change in intensity. Then, we can a linear approximation to find the intensities one step forward and backward from $q$. If $q$ is a local maximum (by a certain threshold), we include it in the edge.

### Hysteresis Thresholding

Sometimes, high thresholding can leave out some edge pixels while low thresholding can include noise. Therefore, we can start with a high threshold and lower the threshold in certain areas only to continue edges (which is a key property of images).

All of this is basically what "canny edge detection" does, which is available in many libraries. However, even with all of this, we still have two issues:

1. Texture is included as edges.
2. Some edges are low contrast.

## Clustering

Before, we viewed segmentation as representing regions, but we could also view it as gropuing together points. One way to do this is to use $k$-means.

$k$-means clustering assumes each group is a Gaussian with different means but the same standard deviation. Each Gaussian looks like

$$P(x_i \mid \mu_j) \propto e^{-\frac{1}{2\sigma^2} ||x_i-\mu_j||^2}.$$

Assuming we know all the means, we can assign each point to the nearest mean. If we know the clusters, we can find the mean by averaging the points in each cluster.

The problem, though, is that we don't know either, so instead we randomly pick $k$ centers. Then, we iterate by assigning all points and recomputing the means, doing so over and over.