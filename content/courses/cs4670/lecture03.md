---
date: '2025-01-27T13:29:37-05:00'
title: 'Lecture 3: More Convolution'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Convolution

The below filter for a pixel if the pixels to the bottom left are light and the pixels to the top right are dark, so it detects edges.

{{< newimgref src="/courses/cs4670/lecture03/edge.png" alt="Edge filter" width="80%" >}}
<figcaption>Fig. 1. An edge-detecting filter.</figcaption>

Shift equivariance is an important property, and it is why convolution is so used in the real world (including in machine learning). If we have a filter that detects cats, it should not break if the cat moves.

Convolution has the following properties:
- Linearity.
- Shift equivariance.
- Commutativity: $w\*f=f*w$.
- Associativity.

We can prove commutativity:

$$(w*f)(m,n) = \sum_i\sum_j w(i,j)f(m-i,n-j).$$

If we let $i = m-i^\prime$, and $j = n-j^\prime$, then

$$\begin{aligned}
    (w\*f)(m,n) &= \sum_{i^\prime}\sum_{j^\prime} w(m-i^\prime,n-j^\prime)f(i^\prime,j^\prime) \\\\
    &= \sum_{i^\prime}\sum_{j^\prime}f(i^\prime,j^\prime) w(m-i^\prime,n-j^\prime) \\\\
    &= (f*w)(m,n).
\end{aligned}$$

One problem with the mean filter is that it assumes that all pixels in the neighborhood influence the pixel in question more. Therefore, we should weigh the nearer pixels to the pixel in question more. This motivates the Gaussian filter:

$$G_\sigma(x) = \frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{x^2}{2\sigma^2}},\quad G_\sigma(x,y) = \frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{x^2+y^2}{2\sigma^2}}.$$

We ignore the coefficient in front of the exponential and instead normalize the filter so that each square adds to $1$. Note that increasing $\sigma$ blurs the image more.

One thing we can do is take multiple Gaussians with different $\sigma$ values and subtract them, and this can help us detect various structures at different scales (e.g. the overall facial structure vs. the wrinkles).

What is the time complexity of convolution? If images are $w\times h$ and the filter is $k\times k$, then assuming "same" convolution, then there are $O(whk^2)$ operations. Assuming "full" convolution, then there are $O((w+k-1)(h+k-1)k^2)$ total operations. Regardless, the time complexity is $O(whk^2)$. This can be worrisome if $k$ is large.

Of course, we can make convolution faster. One optimization to make is to use separable filters (singular value decomposition). If $w = u*v$, then we can apply $v$ first then $u$. Since $v$ is $1\times k$ and $u$ is $k\times1$, each pixel only needs $O(k)$ operations, so the time complexity becomes $O(whk)$.

> **Quiz question**: Which operation is the result of a convolution? Rotation, rgb2gray, scaling, or blurring?

> **Answer**: Rgb2gray and blurring.

Rotation cannot be implemented using convolution because what the filter does must be different based on where the pixel is, which is not what convolution does.

Can we do convolution as explicit matrix multiplication? Yes. For the 1-dimensional convolution for the row of pixels $0,0,0,90,90,90,90,90,0,0$ with filter $1/3,1/3,1/3$, we can multiply the original row of pixels (as a vector) by

$$\begin{bmatrix}
    1/3 & 1/3 & 1/3 & 0 & 0 & \cdots \\\\
    0 & 1/3 & 1/3 & 1/3 & 0 & \cdots \\\\
    \vdots & & & & & \ddots
\end{bmatrix}$$

This is a sparse matrix, so we can perform this calculation efficiently.