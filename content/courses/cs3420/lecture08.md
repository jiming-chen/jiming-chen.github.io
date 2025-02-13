---
date: '2025-02-13T13:33:09-05:00'
title: 'Lecture 8: Interrupts and Exceptions'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

Let's say your embedded device has some buttons, which are ways the user can interract with the program. We want the user to be able to respond appropriately in all situations. For example, if a user presses two buttons at once, the program should not crash.

One way to know if a button pressed is *polling*, where a loop continually reads the GPIO port.

Another option is to have an interrupt, in which the main code causes the program counter to move to another chunk of code called an *interrupt service routine* (ISR) and go back to the main code.

Polling and interrupts have their own benefits:

- Polling is simple and easy to implement.
- Polling can be slow and wasteful of CPU time.
- Polling scales poorly.
- Polling can be bad for precise timing. For example, polling if button 9 was pressed then button 2, the for loop could detect 2 first.
- Interrupts are efficient because code runs only when necessary.
- Interrupts are fast because they are in hardware.
- Interrupts scale well.
- Interrupts are more complicated to implement and can be very difficult to debug.
- Interrupts require processor support.

## Interrupt Handling

Interrupts may look like function calls, so what are the differences? Compared to function calls, the main code does not know that the ISR will be run, so it can't prepare for the PC change by doing things like preparing input arguments. As a result, inside the ISR, the processor should be in a high privilege mode so it can change whatever.

When you enter an interrupt handler, the hardware saves the PC (and registers) and enters a privileged mode. Then, it jumps to the PC specified in the interrupt vector table.

Inside the ISR, the ISR saves registers that will be used then might disrupt additional interrupts. The ISR finds out why the interrupt is happening and executes the code. Then, it returns.

When the interrupt is done, the hardware restores the registers saved from before the ISR, switches the privilege mode back, and jumps back to the original PC.

Usually, the interrupt will alow the current instruction to finish running before interrupting.

As opposed to interrupts, exceptions are synchronous. Exceptions happen for arithmetic overflow, floating point anomaly, page fault, etc.

## Exception Handling

The CPU is hardwired to handle exceptions.

1. First, it finishes the current instruction (unless it is lengthy, like multiplication).
2. Then, it pushes context onto the current stack (xPSR, return address, LR, R12, R3, R2, R1, R0). Recall that xPSR contains the status flags, which we need (for example, if the interrupt happened between a `CMP` and `BEQ`).
3. It switches to privileged mode.
4. It loads PC with address of exception handler.
5. It loads LR with a special EXC_RETURN code.
6. It loads IPSR with exception number.
9. Finally, it starts executing the exception code.

All of this takes about 16 cycles.

The vector table contains a lot of addresses which the PC can go to to execute the code. One example is the HardFault, which the PC goes to if you do an unaligned memory access.

`EXC_RETURN` tells the CPU what return mode to return in and what stack to return. There are three different combinations: Handler/MSP, Thread/MSP, and Thread/PSP.

When writing an ISR, there are a few things to keep in mind:

- Try to keep it short. Since it can go anywhere in code, if it is too long, it can change behavior.
- You might need to explicitly clear the source of the interrupt or else once the interrupt finishes, it can immediately trigger again.
- It should not have side effects since it can be run at any time.

It ISRs are written correctly, they can be very effective because you can write code for different scenarios in isolation.

## Volatile

The `volatile` keyword can be used to force a compiler to fetch a variable from memory each time it is accessed. Imagine you have a program with an ISR that updates a variable. The compiler may have that variable in a register and save it when it was updated in memory.

## ARM

On ARM specifically, you must enable interrupts on the peripheral. There is a configurable bit in the address space of the peripheral called the *interrupt enable*.

You also must configure the NVIC to accept interrupts. Finally, there is a global interrupt enable.