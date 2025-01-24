---
date: '2025-01-23T13:23:22-05:00'
title: 'Lecture 2: Assembly Language Overview and Intro to ARM ISA'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Assembly Intro

Assembly code is a slightly more readable version of machine code. Machine code is a list of binary instructions that are executed by the processor. In programming languages, keywords, braces, etc. make programs understandable by humans and easy to use. Compilers take that code and convert it into assembly code. Then, each line of assembly is converted in machine code.

{{< newimgref src="/courses/cs3420/lecture02/layers.png" alt="Code translation" width="80%" >}}
<figcaption>Fig. 1. One line of C code often corresponds to three or four lines of assembly code. Assembly to machine is just decoding, which is a very thin layer of abstraction.</figcaption>

Why are keywords in assembly very short (two or three letters)? Back when programs were written on punched cards, there was a margin on the cards, and people wrote short descriptions for what the holes (binary representation) meant.

One can construct a programmable processor similar to one featured in ECE 2300 in years past:

{{< newimgref src="/courses/cs3420/lecture02/cpu.png" alt="Simple CPU" width="80%" >}}
<figcaption>Fig. 2. Simple CPU.</figcaption>

The above processor has an ALU, program counter, and registers which the ALU works with. In most assembly languages, the destination register usually goes first:
> `ADD rd, rs, rt` means add the value in `rs` and `rt` and put the result in `rd`.

{{< newimgref src="/courses/cs3420/lecture02/decode.png" alt="Decoding" width="80%" >}}
<figcaption>Fig. 3. Example of decoding assembly into machine code. In this case, the register is 8 bits large.</figcaption>

The processor uses the program counter to fetch instructions and runs the program. In our case, since we have a 4-bit wide opcode, we can have 16 different instructions. Additionally, as stated above, we have 8 registers to stoer data (not including PC and Z).

We can think of `1101 010 100` as `R[dst] <- R[src1] - R[src2]; Z <- R[dst] == 0; PC <- PC + 1`, where `Z` is the zero flag. Instructions can also include offsets, so `1001 111111` could mean `PC <- Z ? PC + 1 : PC + offset`.

So who programs in assembly? Nowadays, not many. Back when assembly was first created, it was considered high-level and big step up. In the 80's it was big, but it has steadily declined in favor of higher level languages. Most of the time, C works fine, but sometimes, people need to do very specific things and use small amounts of assembly.

Today, compilers are pretty good at turning high-level languages into machine code that is very optimized and error-free, which explains why people have stopped using assembly. However, sometimes, the compiler can add instructions that are not strictly necessary. This is not always bad, but if some code needs to be high-performance e.g. needs to be run a bazillion times a day in a data center, then we want less bloated code.

Why do we study assembly? When we program, ultimately we are manipulating the processor, so if we want a high degree of control over how we program, we should be aware of the hardware-software interface. For example, in Java we initialize variables and magically get memory, but we should understand how that happens.

Assembly also allows us to understand how operating systems manage multiple services and interaction with I/O devices. However, one can program without operating systems, which is known as "bare-metal" embedded systems. This is good for minimizing bloat and speeding up critical code blcoks.

When we talk about a specific assembly language, we are talking about an **instruction set architecture (ISA)**. The ISA describes what the hardware can actually do; it is a contract/specification between the client of the hardware and the implementer of the hardware. Therefore, hardware engineers can change the architecture or transistor technology, resulting in different implementations, but as long as the functionality is the same, the contract is upheld.

With one ISA, there can be many different design choices across product families; some people might want high-performance but some might want high-efficiency. Additionally, software can be pretty difficult to implement efficiently, so hardware engineers do a lot of trickery, e.g. out-or-order execution.

## Quiz questions

1. For `SUB R2 R4`, what is `SA`, `SB`, and `DR`?

Answer: `SA = 2`, `SB = 4`, `DR = 2`.

2. For `BNZ NEXT`, what is the value of `BS`?

Answer: `BS = 3`.

## Assembly Key Points
- Assembly is decoded into binary instructions.
- There are limitations because of the fact that it is decoded into binary.
    - There are limited registers.
    - You cannot branch too far away.
    - You cannot have large literals.

## ARM Based Processors

Many people are moving toward the ARM ISA. For example Apple produces their own chips with Apple Silicon, and many machine learning companies are hiring hardware engineers to make chips specifically for optimizing machine learning. In this class, we will use **ARMv6-M**.

The ARM Cortex-M architecture has a 32-bit datapath and 32-bit addressing space. Most instructions are 16-bit for compactness, but some are 32 bits. Having instructions of different sizes can be difficult to work with because the CPU has to fetch twice for the longer instructions.

There are M0, M0+, M3, M4, and M7 versions, but they have the same core functionality and just have additional features.

The M architecture is geared toward power efficiency, so it has fewer instructions and gates on the hardware level.

{{< newimgref src="/courses/cs3420/lecture02/arm.png" alt="Simple CPU" width="80%" >}}
<figcaption>Fig. 4. Instructions supported by different M-series ISAs. Notice that M0 does not have division, so division has to be written in software.</figcaption>

In this class, we will use the {{< newtabref href="https://www.nxp.com/part/MKL46Z256VLL4" title="MKL46Z256VLL4" >}} processor, which includes a capacitative touch interface.

All ARM processors have a similar register layout. There are 16 registers, 13 of which are general purpose, labeled from R0 to R15. These are registers we can use with operations like add or subtract. The highest registers (R13 to R15) are used in a specific way to implement software. For example, R15 acts as the program counter. R14 is the link register (LR), which is pretty helpful for software. R13 is the stack pointer (SP), which is also used in software, but we can also manipulate it in hardware.

{{< newimgref src="/courses/cs3420/lecture02/registers.png" alt="Register layout" width="80%" >}}
<figcaption>Fig. 5. Register layout for ARM.</figcaption>

Since we have a limited number of instructions, we usually only let instructions access the lower registers.

When we execute software, there are two different privileges, one for user code and one for kernel code. Each of these modes has its own copy of the stack. This is what it means to have **banked stack ponters**. These pointers are used to store the executation contexts of the software. When we switch contexts, we assume nothing about the general purpose registers.

The PSR, PRIMASK, and CONTROL registers tell us about the execution modes.
- PSR is 32 bits and has three sub-registers: Application (flags), Interrupt (exception number), and Execution (thumb state, which is always 1 for us)
    - Condition codes are what the APSR can hold. These codes are flags that say what the result of the last instruction was (N, C, Z, and V). We can suffix an instruction with "S", e.g. `SUBS` if we want to update the APSR.

The general instruction format for ARM is `op <dst> <src1> <src2>`. There may be fewer source operands or no destination register. Operands can be registers e.g. `R1` or a literal e.g. `#3`.

Sometimes we want to branch, and we call those labels, so we can write `BNE <label>`.

R0 to R7 are "low registers" and accessible by all instructions, and R8-R12 are "high registers" and are sometimes not accessible by 16-bit instructions.