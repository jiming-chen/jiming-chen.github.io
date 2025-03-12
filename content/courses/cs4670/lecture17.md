---
date: '2025-03-10T13:30:23-04:00'
title: 'Lecture 17: Planar Homography'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

Since we can set the world coordinates ourselves, we can let the world $Z$ coordinate be $0$, discarding the third column of the matrix $P$, resulting in a matrix we call $H$.

$H$ has $9$ entries, but accounting for $\lambda$, there are only $8$ parameters. Thus, we need four points to determine $H$.

Homography works for image stitching because it helps us map different camera angles onto the same coordinate plane.