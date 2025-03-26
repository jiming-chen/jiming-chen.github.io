---
date: '2025-03-25T13:31:35-04:00'
title: 'Lecture 20: Scheduling'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

A *scheduling algorithm* is a strategy used to pick a ready task for execution. A round robin scheduler has the ability to stop a process in the middle of execution, so it is a *non-preemptive* scheduler.

One way we can judge a scheduler is by its average response time, in which we want to minimize the average tmie that a job waits in the ready queue before it gets scheduled.

First-come-first-serve does not achieve this goal (counterexample is a large first process).

Other scheduling algorithms are just in CS 4820.