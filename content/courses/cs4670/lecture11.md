---
date: '2025-02-21T13:27:21-05:00'
title: 'Lecture 11: Correspondences and Reconstruction'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Correspondences Wrap-Up

If we have a bunch of corners in two images, how we do match them to each other? Since each corner is described by a vector, we can compute Euclidean distance and correspond to each corner in image 1 the closest corner in image 2.

However, not every corner in image 1 is guaranteed to have a matching corner in image 2. Therefore, we devise a method of scoring correspondences.

For ambiguous matches, Euclidean distance does not differentiate well. A better approach is *ratio distance*:

$$\frac{||f_1-f_2||}{||f_1-f_2^\prime||}.$$

If the ratio is close to $1$, it means the correspondence is ambiguous and not good. If the ratio distance is high, the correspondence in the numerator is good.

## Reconstruction

One of the earliest ideas for a camera was the *camera obscura*. The core idea behind this is that if you are in a dark room and pierce a hole in a wall, an inverted image of the outside world will be prjoected on the other side of the room. This is how a pinhole camera works, and the same concept applies in modern cameras.

Therein lies the challenge in 3D reconstruction: we are given the projection of the 3D object but not the actual 3D structure, which is why we call this an "ill-posed problem."

In this class, we will put the pinhole at the origin and the opposite wall at $z = -1$. Then, any point $P = (X,Y,Z)$ will be projected to 

$$p = (x,y) = (-X/Z, -Y/Z, -1).$$

However, this is the inverted image, so we can invert it back:

$$p = (X/Z, Y/Z, -1).$$

Some consequences of this model:

1. Farther objects appear smaller.
2. Parallel lines converge at a point.