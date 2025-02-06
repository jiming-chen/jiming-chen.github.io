---
date: '2025-02-06T13:27:42-05:00'
title: 'Lecture 6: Function Calls and Program Memory Layout'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## Function Calls

If we want to implement `POP {R4}`, we first store the value in the stack pointer to `R4` then increment the stack pointer by 4 because the stack grows down.

We can also do `POP {PC}`, which is equivalent to `POP {Rx}` followed by `BX Rx`. We can't do `POP LR` because `LR` is not a lower register.

### PUSH Encoding

With the PUSH encoding, we encode a list of registers using a one-hot encoding to save instructions.

{{< newimgref src="/courses/cs3420/lecture06/push.png" alt="PUSH T1 Encoding" width="80%" >}}
<figcaption>Fig. 1. T1 Encoding for PUSH Instruction.</figcaption>

In one-hot encoding, bit `n` being 1 means `Rn` should be pushed.

### Saving LR in Stack

Once we have stacks, we can save the value of `LR` in a stack for each function call. Then, when it's time to return, we pop the correct value of `LR` into a register then branch to that value.

However, functions need to manipulate registers, so they may override the registers that are used for function calling.

### Saving and Restoring Registers

We can do two things

1. The caller can save its registers by using `PUSH` and `POP` to save the contents of the registers into memory and retrieve after the `BL func`. The order of the registers has to be switched though (we don't have to worry about this if using one instruction because of one-hot encoding), and also, pushing and popping each take 2 cycles per register.
2. The callee normally begins with `PUSH LR` and ends with `POP PC` but instead, we can put `PUSH R4` after the initial push and do `POP {R4}` before the final pop.

### Inline

Another thing we can do is use the `inline` keyword in a C function header which tells the compiler to copy and paste the function code into wherever it is called. This results in a larger executable, but if a function is called a lot, this can be beneficial, as there is no branching

### ARM Calling Convention

The ARM convention is that the caller has the responsibility to save registers R0 through R3. If there are more than four arguments, the rest get pushed to the stack (in reverse order). It is the callee's (function body's) responsibility to save R4 through R8 and higher registers if needed because the caller expects them to be preserved. You don't need to save them if you don't use them as long as they are preserved for the caller. It is important to follow this convention because later we will write assembly code to be called by a compiled C function, and the C compiler follows this convention.

{{< newimgref src="/courses/cs3420/lecture06/registers.png" alt="ARM Calling Convention Register Table" width="80%" >}}
<figcaption>Fig. 2. AAPCS Core Register Use.</figcaption>

Sometimes functions call other functions, in which case, a block of code can act as a callee and a caller. Then, the callee would need to still store `LR`.

In C, the calling convention says that the return value goes in R0. For larger return types, R0 could hold the address to the return value.

## Memory Layout

### Local Variables

For local variables, local variables are allocated in the stack, and a fixed address and amount of memory is allocated by the C compiler (although they may be stored in registers for optimization).

When reading local variables, we can use a `LDR`, but if we do that, the offset we do later has to be a multiple of 4.

### Activation Record

One way to think of the stack is a record of everything happening in the program. The stuff related to the `main` function go to the bottom of the stack (highest address). Then, `foo1`'s stuff go lower in the address space in the following order: parameters, return address, callee-saved registers, then local variables. Since we have a lot of memory, we can afford a lot of function calls.

### Program Memory Use

The Flash ROM is layed out in the following order:

- Constant Data, e.g. `const char c=123;`
- Initialization Data, e.g. `int d=31;`
- Startup and Runtime Library Code
- Program .text

The RAM has the following layout:

- Zero-Initialized Data
- Initialized Data, e.g. `int d=31;`
- Stack
- Heap Data

The heap is written to from the bottom up while the stack is written to from the top down. Then, they can intrude into each other's boundaries until they collide so that memory use can be maximized.

When a program is run, initialization data and constant data are stored in the flash ROM, and once the compiler allocates memory for variables, the data is copied from ROM to RAM. Note that there is still difference between initialized data and variables (in stack) within the RAM.