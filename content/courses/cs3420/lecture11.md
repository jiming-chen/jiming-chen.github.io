---
date: '2025-02-27T13:30:20-05:00'
title: 'Lecture 11: Time Sharing'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Context Switching

If you open task manager, you will see hundreds of processes running all running at the same time. There is the illusion that there is a CPU dedicated to each program, but in reality, all of them have to time share on a single CPU.

In order to do this, we need to stop programs and switch very quickly. We refer to running programs as *processes*.

What does the CPU need to execute a program the same way at two different times? We would need at least

- Registers
- Processor flags
- Configuration of peripheral devices
- Variables stored in memory

Context switches are different from interrupts because user code doesn't know it is inside an interrupt body, so it doesn't know to restore the previous state. Contexts are stored in the stack.

The OS/scheduler maintains a queue of processes, implemented using linked lists. A round-robin scheduler continuously runs the process at the front of the queue then moves it to the back.

The *process control block* (PCB) is the unit that contains the context. It should include scheduling state, stack pointer, and a pointer to the next PCB. To store variables, you can use structs.

In order to switch contexts from A to B, we save A's SP into A's context, then switch the SP to B's SP. To switch back, we can run `POP PC`.

## Virtual Memory

Every context has the same memory layout, but these translate to to different areas in storage. This also prevents processes from accessing memory that isn't theirs. However, this can be difficult to implement.

## Scheduling

There are several options we have for scheduling. One is *first-come first-server* (FCFS). It is non-preemptive, meaning each program runs until it voluntarily gives up the CPU. This is easy to implement because we don't have to force it to switch contexts, but at the same time, if there is a bug or infinite loop, one process can affect the other processes.

*Round-robin* (RR) scheduling cuts each process into chunks. This is an example of preemptive scheduling.