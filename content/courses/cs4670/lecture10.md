---
date: '2025-02-19T13:27:22-05:00'
title: 'Lecture 10: Feature Descriptors'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

A feature descriptor is a way to describe pixels using vectors in order to do correspondence.

One example is the *Multiscale Oriented Patches* (MOPS) descriptor, which describes a corner by the patch around the pixel. The patch should have scale, rotation, and photometric invariance.

Automatic scale selection is done by taking the function at many scales and taking the scale which maximizes the function. This implemented using the Gaussian pre-filtering and blurring (Gaussian pyramid) that we used for zooming in on images and computing $f$ for a fixed window size.

For rotation invariance, we can again look at the second moment matrix:

$$\begin{bmatrix}
\sum I_x^2 & \sum I_xI_y \\\\
\sum I_xI_y & \sum I_y^2
\end{bmatrix}.$$

The eigenvectors of this matrix should tell us the orientation of the image.

Photometric invariance can be done very easily. If the image is brighter, then we can the mean from all values, and if the image has higher contrast, then we can divide by the standard deviation.

MOPS is invariant to additive and multiplicative changes to intensity, but sometimes lighting changes can be more sophisticated, such as day versus night. Therefore, we worry less about the precise color values and more about the edges.

Additionally, MOPS is only invariant to rotation and translation, but not shearing, which is a very common transformation because it results from any change in view angle.

The solution is to have an edge descriptor which groups objects into different buckets, which allows invariance to small deformations. This is called *quantized orientation* (quantization is also used in machine learning).

We can do this with histograms. Importantly, with histograms, you don't know which things go toward the count, just what the count is. Therefore, we can break an object into a grid and in each grid, we keep track of the number of pixels in each grid in each bucket. We can also use a weighted histogram.

For this descriptor (the *scale invariant feature transform*), we still want scale and rotation invariance as in the MOPS descriptor. SIFT is extremely well-engineered, so it is hard to find a deep learning solution which beats SIFT.

In SIFT, rather than using the second moment matrix, it uses the mode of the histogram (dominant orientation) to match orientations.

There are several things we have to watch out for when using SIFT.

- One trick that you can use is to only keep edges of high gradient magnitude, which we do because there are a lot of edge pixles with low gradient magnitudes, which are extremely noisy.

- Another issue we could encounter is a pixel switching bins after a transformation. We can fix this by allowing a pixel to vote for its two nearest neighbors (linear interpolation).

- We are using gradient magnitudes, but gradient magnitudes are still affected by multiplicative intensity changes, so we still have to normalize that.

- There could be multiple modes when finding the dominant orientation, in which case we have to check all possibilities.