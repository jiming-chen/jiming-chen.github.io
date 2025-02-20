---
date: '2025-02-20T13:37:25-05:00'
title: 'Lecture 9: Communicatinos Protocols'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## ISR Recap

Recall that ISRs are like functions, and the processor is hardwired to execute ISR and return. This includes pushing the context onto the stack (everything needed to resume execution as if nothing happened).

There are two types of interrupts:

- Non-maskable interrupts cannot be disabled, such as resets or HardFaults.
- Maskable interrupts are the most common. These are controlled by the user and can be selectively activated.

Additionally, recall that in our case, we need to enable interrupts, which requires several things:

1. We need to enable interrupts on the peripheral, which is done by setting a configuration bit in the address space of the peripheral.
2. We need to configure the NVIC to accept interrupts, which is done through the NVIC_ISER (interrupt set enable register).
3. Finally, there is a global interrupt enable we need to set.

If you have multiple IO devices, there is an exception number and vector table for figuring out which device raised the interrupt. Then, we also priority, where a lower IRQ in the table goes first.

While mot exceptions are prioritized as they happen, some priorities are fixed. For example, reset has priority -3, which is the highest priority. Additionally, we may adjust the priorities of other peripheral exceptions.

If a new exception is requested while an exception is currently being executed, if the newer one has higher priotity, then that one gets executed while if it has lower priority, it gets executed after.

Recall that variables modified in the ISR should be declared as `volatile`.

## Communication Protocols

A *bus* is a shared wire that a lot of nodes share between them. But there are still questions we might have. Do we do serial or parallel, clock or no clock? How do we know who the data is to or from? What if there are multiple things to be sent at once? How to handle errors?

### CAN Bus

The *Controller Area Network** is a vehicle bu standard developed in 1983. It can be thought of industrial strenth noise resistant transmission. It is used in transmission, ABS, controls, etc. in cars, for example. It is common in "harsh" environments.

CAN is serial, meaning one bit is transmitted at a time. There is no clock line because the data line is "self-clocked." There is a multi-controller so that anyone can initiate transmission.  There is no "deveice address," but rather a "message ID." CAN uses differential signaling, meaning the ground depends on the two endpoints rather than an actual ground, meaning it uses two twisted wires.

If a line has high priority, then the signal is active-low. Otherwise, it is active-high. Then, a wired AND is used for bit-wise arbitration. Therefore, if anyone is transmitting 0, then the bus will show 0.

All message start with 0 after an "Inter-Frame-Space" (long period) of 1. Then, an 11-bit base message message ID. At the end, you can check whether the data was corrupted with a checksum.

How do noes know when to read the bus without an explicit clock signal? We need them to be synchronized by themselves. The way we do this is by introducing transitions every five bits into the message where everyone can synchronize. This is done by hardware, so when we write software don't have to worry about this.

## I$^2$C

The *inter-integrated circuit* is used in cheap embedded environments like robotics. Data (SDA) is serial, and there is a clock line (SCL). SCL is driven by the controller while SDA is driven by either the controller or the nodes. There is the same wired AND.