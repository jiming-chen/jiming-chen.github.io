---
date: '2025-03-18T13:30:42-04:00'
title: 'Lecture 16: Readers and Writers'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Concurrency Recap

In the past, we have been discussing concurrency, whether that be multiple programs on the same CPU or multiple processors accessing the same memory. We studied mutual exclusion, specifically using locks (which ensure that no two programs enter their critical sections at the same time).

We studied how atomicity ensures only one programming is running at once, but that is a very strong requirement. Concurrency means only one critical section is running at any given time though we can still have other interleaving.

Locks get you 90% of the way there although they are not efficient.

Recall that we assume that critical code does not crash; otherwise our guarantees about progress no longer hold.

## Readers and Writers

A *reader* is a process that reads a shared resource, and a *writer* is a process that modifies a shared resource. For any resource, we don't it to be read and written to simultaneously, and we don't want it written to by multiple processes simultaneously.

Therefore, if a process tries to read or write while a resource is already being written to, then we don't allow them to continue until the writing is done.

## Implementation

A reader consists of an `enter_r()` followed by code that reads the file followed by `exit_r()` while a writer calls `enter_w()` then writes then does `exit_w()`.

We use two shared variables: `nw` and `nr`.