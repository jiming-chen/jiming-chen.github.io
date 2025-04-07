---
date: '2025-03-26T13:36:50-04:00'
title: 'Lecture 21: Other Methods of Obtaining 3D Structure'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

So far, when talking about 3D structure from images, we have been talking about the depth of each pixel $D[\mathbf{p}] \in \mathbb{R}^+$. However, we can also look at the *surface normal* at any point because up close, any object approximates a plane. We can let this information be conveyed in another channel $N[\mathbf{p}] \in \mathbb{S}^2$.

Sometimes, when things in images are too far, the depth channel can have less information, but we can use surface normal to fill in missing information to complete the depth map.

Additionally, the surface normal affects shadows and shading in the image. For example, the sun is on the bright half of the moon. Therefore, we can extract information about an object by capturing an object under different lighting conditions.

*Irradiance* iss the power received by a surface patch of unit area on the camera. Irradiance is the radiance multiplied by the cosine of the angle at which the ray comes in. We also have an outgoing radiance at a different angle with a fraction of the incoming power.