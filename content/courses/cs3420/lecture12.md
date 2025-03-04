---
date: '2025-03-04T13:50:07-05:00'
title: 'Lecture 12: Concurrency'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

Imagine a bitstream controlling some LEDs. If you have an interrupt in the middle of a bitstream (not aligned with the 24 bits that encode the color), then things will go wrong.

If we wanted blue light to travel down an LED strip faster than a red light, instead of using one loop, we should use two. But if we run two loops together, we can get weird bugs.

If we want two pieces of code to interact in the same memory space, we call them threads (as opposed to processes). This also means we have to be more careful.

We can deal with concurrency bugs in several manners, such as mutual exclusion, locks, condition variables, and semaphores.