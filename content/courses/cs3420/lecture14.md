---
date: '2025-03-11T13:32:29-04:00'
title: 'Lecture 14: Locks'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

Dekker's algorithm correctly implements mutual exclusion. However, this is a little bit convoluted. Additionally, each process must know about the other process, which violates modularity for programming. Therefore, we create a new abstraction: *locks*.

Say we have a lock called `l`. Then, we can acquire the lock (`lock(l)`) or release the lock (`unlock(l)`).

Then, a program consists of the `NCS` then a `lock` then `CS` then `unlock`.

Note that using a single lock for different resources that need to be protected with a lock can be inefficient. Therefore, if multiple resources can change `x`, we can assign a lock to `x`.

## Deadlock

It is possible to have nested locks without deadlock, but deadlock can happen, so it is best to avoid. For example, `P1` could acquire lock `a` then `b` and `P2` could acquire lock `b` then `a`.

## Implementation

How do we implement locks? We could disable interrupts since interrupts are used to switch between processes. However, this does not generalize to multi-core systems.