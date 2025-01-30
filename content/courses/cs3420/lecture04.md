---
date: '2025-01-30T13:31:55-05:00'
title: 'Lecture 4: ARM Memory Organization'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## ARM Memory Map

All versions of ARM have the same memory encoding which makes compatibility easy. Each memory address is 32 bits, which means we can have 4 gigabytes of data (because each memory block holds 1 byte).

{{< newimgref src="/courses/cs3420/lecture04/map.png" alt="Cortex M0+ Memory Map" width="80%" >}}
<figcaption>Fig. 1. Memory map for Cortex M0+.</figcaption>

The bottom 500 megabytes of memory holds the code, some of which non-volatile (256 kB), meaning if the system reboots, the memory remains.

Our NXP ARM cores have a weird convention where they put one quarter of the 32 kB SRM in the code space and the rest in the SRAM code space. The reason they do this is because they want to prevent execution of some code. This is also done because the memory bus may be physically faster for the lower memory addresses.

## Memory Organization

A byte is 8 bits, and a word is 32 bits. Word and half words are aligned, so for a 4-byte word, the base address is divisible by 4, and its value is stored in the location as well as the next three bytes.

Endianness is the order of bytes in a larger data type. **Little endian** (used in our processors) says that the least significant byte has the smallest address. When using **big endian**, the smallest memory address has the most significant byte, so the first byte we see is the most significant byte. This is used a lot in networking and transmitting serial data.

Every word is connected to the memory bus, but when a specific word is addressed (by its starting byte), the whole word is activated in sent through the bus. However, we can also read one byte, but of the four bytes we receive in the bus, we have to choose the right one. The memory does this for us.

{{< newimgref src="/courses/cs3420/lecture04/lines.png" alt="Memory Lines" width="80%" >}}
<figcaption>Fig. 2. Diagram of memory addresses arranged in lines.</figcaption>

What if we want to read one word that is not word-aligned? In order to do this, we have to access the memory twice and extract the correct bytes to be assembled into the non-aligned word.

> **Question**: If we have four consecutive blocks that say `0xde`, `0xad`, `0xbe`, and `0xef` from highest address to lowest address, what data values are not possible between `0xdeadbeef`, `0xadbe`, `0xbeef`, and `0xefbe`?

> **Answer**: `0xadbe` is not half word aligned, and `0xefbe` is big endian, so they are not allowed.

## Load and Store Operations

ARM is a load-store architecture, meaning all the operations it does are on the registers. Therefore, if we want to interact with the memory, we need to move the memory to and from the registers. In some other systems, you can operate on memory without bringing the data to the CPU, but that architecture is way more difficult to operate. In this class, we do what is easy, but the tradeoff is that it can be slow to do simple things like check a value in memory.

- Load instructions look like `LDR <Rt>, <address>`.
- Store instructions look like `STR <Rt>, <address>`. If the data is not 32 bits, you will get an error.

You can also load half-word or byte data:

{{< newimgref src="/courses/cs3420/lecture04/load.png" alt="Loading Byte and Half-Word Instructions" width="90%" >}}
<figcaption>Fig. 3. Table showing instructions for loading bytes nad half words.</figcaption>

There are also versions of these instructions that sign extend the data.

### Addressing Modes

Sometimes we want to calculate the address of something. Then, we can write `[<Rn>, <offset>]` as an address in an instruction. We can also refer to a label as `=LABEL`, and the assembler will calculate the address.

Some examples of addressing modes in instructions are

- `LDR      R2, [R3, #8]`
- `LDR      R2, [R3, R4]`
- `LDR      R2, =SOME_LABEL`

## Timing

Different ARM instructions take different amounts of time to execute:

{{< newimgref src="/courses/cs3420/lecture04/timing.png" alt="Timing of Instructions" width="90%" >}}
<figcaption>Fig. 4. Table showing timing of instructions.</figcaption>

Notice that most instructions take only 1 cycle, but manipulating the program counter takes 2 cycles. This is because we have to wait for our 2-stage pipeline to finish and fetch and decode the next instruction.

## Pseudo-Instructions

Sometimes, we use an instruction that doesn't actually exist, but it allows functionality. For example, a `NOP` instruction (which does nothing but delays for one clock cycle) is implemented as a `MOV` where a register is moved to the same location.

Some other examples are below.

- `MOVS <Rt>, <Rn>` is implemented as `ADDS <Rt>, <Rn>, #0`.
- `LDR <Rt>, =LABEL` is implemented as `LDR <Rt>, [PC, #offset]`.

## Some C Data Structures

An **array** is a data structure consisting of a collection of elements which are contiguous in memory. We can declare an array of 3 integers by saying `int x[3]`. Then, since integers are 32 bits, we can have one word corresponding to each element of the array. Indexing based on 0 makes accessing addresses easily. We simply add that many words to the address of the array, which is the address of the first element.

**Strings** in C are stored as arrays of `char`s terminated by the nul character `\0`. A char is 1 byte long, so a 5 letter string is 6 bytes long. Since strings are just binary data instead of objects with fields like length, the nul character allows us to know where the string ends.