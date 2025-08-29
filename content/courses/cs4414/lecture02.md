---
date: '2025-08-28T15:09:04-04:00'
title: 'Lecture 2: The Evolution and Architecture of Modern Computers'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

Modern CPU architecture has many cores on one chip, each of which has their own cache. There is also off-chip memory. Nvidia GPUs have many cores, and we can think about them virtually in 3D shapes (?? idk, Birman yaps).

Moore's Law says that because of transistor size decreases, we can get exponential speedups. But around 2006-2009, it seemed to be ending because faster chips require faster clock and can fry.

However, we started using parallelism (many CPUs with lower clock speeds).

A computer needs nearby memory, but applications need access to "all" the memory. This is why we get "non-uniform memory access" behavior (NUMA). With NUMA, we seem to have been able to keep up with Moore's Law.

Gene Ahmdahl researched parallelism and asked how fast we could do things with infinite parallelism. He realized that sequential bottlenecks become a huge issue, similar to how driving in a fast car doesn't matter if you're stuck behind a tractor. Ahmdal said that if $p$ is the percentage of the task that can be parallelized, then the maximum possible speed up is $1/(1-p)$ (this just says we cannot speed up sequential tasks).

When we write code with NUMA, nothing changes about what we actually write, but since each NUMA CPU (pair) has allocated DRAM, only a small portion of the memory is fast.

This results in a 4-tiered hierarchy for data access: registers, local DRAM, remote DRAM, and off-chip memory.

NUMA "pretends" to be old architecture by abstracting away architectural differences, but ML often does not know about this, resulting in unoptimized code. Similar behavior also occurs in GPUs such as Nvidia's H100.

Why are Python and Java expensive?

Python is interpreted and cpompiles down to a high-level representation. But this interpreter can make a lot of bad decisions for you.

Java compiles twice (to byte code then via JIT). This compiled code can be very fast, but sometimes, the compiler has to do a lot of type checking (at compile-time and runtime) because of dynamic types and polymorphism. Additionally, everything is an object, which hsa a lot of overhead.

In C++, all objects are compile-time, so there is no type-checking at runtime.