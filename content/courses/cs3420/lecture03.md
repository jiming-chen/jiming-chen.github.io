---
date: '2025-01-28T13:27:56-05:00'
title: 'Lecture 3: Some ARM Instructions'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

## ARM ISA Recap

Recall that the ARM ISA has most instructions as 16 bits; since we have resource constraints and everything costs money, we want to fit as many instructions on the chip as possible.

Some examples of instructions are

- `MOV 47, #4`
- `SUB R2, R2, R4` which is the same as `SUB R2, R4`
- `ADDS R2, R2, #3` which has the `S` suffix and updates the status flag

> **Question**: Which instruction(s) are not allowed to be compiled?
> - `SUB R2, R3`
> - `SUB R5, R9`
> - `SUB SP, #4`

> **Answer**: The second instruction is not legal because it accesses a higher register. The third option is legal because soetimes we want to add or subtract literals to the stack pointer.

## More ARM

In this ISA, a prefix of 0x1D to 0x1F means a 32-bit instruction. Otherwise, the instruction has 16 bits.

### SUB (immediate)

There are two encodings for immediate subtraction. Both have zero-extended immediate values.

- T1 has a 3-bit immediate value.
- T2 assumes `src1` and `dst` are the same, which frees up 3 bits. This allows an 8-bit literal (there is other space freed up).

{{< newimgref src="/courses/cs3420/lecture03/sub_imm.png" alt="SUB (immediate) encodings" width="80%" >}}
<figcaption>Fig. 1. SUB (immediate) encodings.</figcaption>

> **Question**: What is the largest number `#N` you can use in the instruction `SUBS R5, R3, #N`?

> **Answer**: 7. The destination is distinct from the source, so we know this is the T2 encoding, so we have 3 bits for the literal.

> **Question**: What is the largest number `#N` you can use in the instruction `SUBS R5, #N`?

> **Answer**: 255. The destination is distinct from the source, so we know this is the T2 encoding, so we have 8 bits for the literal.

> **Question**: Can you substract negative literals? E.g. `SUBS R3, #-3`.

> **Answer**: No. The immediate value is zero-extended, not sign-extended.

### SUB (register)

- T1 has two source registers and destination register. Always runs `SUBS`. Normally, the assembler changes `SUB` to `SUBS` automatically, but you can also allow it to be nitpicky and throw an error.

{{< newimgref src="/courses/cs3420/lecture03/sub_reg.png" alt="SUB (register) encodings" width="80%" >}}
<figcaption>Fig. 2. SUB (register) encodings.</figcaption>

### SUB (SP = R13)

- T1 has the stack pointer as a destination of a literal subtraction. The stack pointer is not encoded in the instruction because it is hard-coded into the opcode. This allows a 7-bit immediate value which is zero-extended 2 bits to the left (multiply by 4) since the number in the stack pointer must be a multiple of 4. The assembler ensures the immediate value is a multiple of 4 then divides it by 4 when it creates the instruction.

{{< newimgref src="/courses/cs3420/lecture03/sub_sp.png" alt="SUB (SP=13) encodings" width="80%" >}}
<figcaption>Fig. 3. SUB (SP=13) encodings.</figcaption>

### MOV (immediate)

- T1 moves an 8-bit immediate value into the destination register.

### MOV (register)
 
- T1 moves `Rm` (4 bits) into `Rd` (3 bits)
- T2 sets flags so can only allocate 3 bits to `Rm` (low registers).

Sometimes we just need to move stuff around, which is when we would use the T1 encoding.

## More Instruction Stuff

You can assume that add and move instructions always set status flags (`S` suffix).

Instructions are expressed in hexadecimal, so a 16-bit instructino would look like `0x2202`. (the `0x` just means the number is hexadecimal)

Most instructions take 1 cycle, except branch, load, and register manipulation instructions, which take 2 cycles. Also, multiply instructions can take a weird number of cycles. This assumes we have a 2-stage pipeline, though, and branch instructions will take longer with larger pipelines.

### Branches

In some other assembly languages, branches are referred to as jumps.

We can use an unconditional branch (`B`) or a conditional branch (`BXX` where the `XX` suffix tells which condition causes the branch.)

{{< newimgref src="/courses/cs3420/lecture03/branch.png" alt="Branch conditions" width="80%" >}}
<figcaption>Fig. 4. Branch condition table.</figcaption>

Whereas conditional branches limit you to about 256 B of branch, unconditional allows you to branch about 2 kB.

The assembler often computes offset for us, so we can use labels instead, e.g. "Loop."

We use conditional branching much more often, and it is encoded as follows:

- The T1 encoding allows an additional condition in the instruction but has a smaller immediate value (8 bits).
- the T2 encoding has a larger immediate value (11 bits). This is important because we often want to branch far away.

For both encodings, the assembler divides the immediate value by 2 in the instruction, and this gets shifted left by 1 bit when executed.

{{< newimgref src="/courses/cs3420/lecture03/branch_encoding.png" alt="Branch encodings" width="80%" >}}
<figcaption>Fig. 4. Conditional branch encodings.</figcaption>

Sometimes, we want to move really really far away. This is when a 32-bit instruction would be useful. The branch & link does exactly this.

{{< newimgref src="/courses/cs3420/lecture03/bnl.png" alt="Branch & link encodings" width="80%" >}}
<figcaption>Fig. 4. Branch & link encodings.</figcaption>

This 32-bit instruction is actually two instructions, but bits 15 to 11 of the first instruction are `11110`, which is `0x1E`. Recall that this tells the processor that this instruction is 32 bits.