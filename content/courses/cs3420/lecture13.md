---
date: '2025-03-06T13:30:49-05:00'
title: 'Lecture 13: Mutual Exclusion (MUTEX)'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

There are two ways of thinking about concurrency control: atomic actions vs. mutual exclusion. The latter is more fine-grained.

Atomicity means that nothing else can happen at the same time. Imagine we had a one-lane bridge. If you were trying to cross the bridge, being atomic would mean stopping all cars in Ithaca so that you can safely cross. This is a very strong statement about the system and is overkill.

With mutual exclusion, we would simply stop cars on the other side of the bridge. In a computer, this means we don't want two processes to access the same resource at the same time.

Now, consider two programs, `P1` and `P2`. Each program has an `NCS` and `CS` section (noncritical and critical). We can interleave `NCS` with other code, but we can't interleave `CS1` and `CS2`. Also, we ensure that the two `CS` sections will execute quickly and not crash (since they don't interleave with each other).

## Mutual Exclusion Requirements

There are some requirements we have for mutual exclusion:

- At any moment, at most one process is inside its `CS`.
- At any moment, of all the programs trying to run their `CS`, at least one is guaranteed to get access in a finite amount of time. This ensures the programs make progress.
- At any moment, every process actively contenting to run their `CS` is guaranteed access in a finite amount of time.

Note that fairness implies progress.

If we have an intersection where all four directions are stuck in the middle of the intersection and the critical section is being in the intersection, then there is neither safety nor progress nor fairness.

## Dekker's Algorithm

Dekker's algorithm eliminates livelock and deadlock, as well as being safe. We first have to indicate that we want to get into the `CS` by setting `x = 0`, then we avoid deadlock and livelock inside the `while` loop using a `turn` flag. We avoid livelock because if multiple processes are waiting, the `turn` flag allows one to go through.

It is still possible (but unlikely) that Dekker does not ensure fairness. This could happen if `P1` was skipped in the scheduler and `P2` had a really short `NCS`.