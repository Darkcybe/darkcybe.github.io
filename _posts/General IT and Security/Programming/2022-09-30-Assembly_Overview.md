---
title: Assembly Overview
categories: [General IT and Security, Programming]
tag: [assembly]
comments: true
---

# Overview

In previous posts about fundamental programming languages, such as Python and C#, topics were introduced explaining how high level languages are human readable and assist us in the creation of applications. However, a computer is unable to interrupt that high level language, requiring a translation into something that it can process. Compilation of written code into a binary format is how this translation is carried out, programming languages have different ways of conducting this also with some using a compiler (C#) and some using interpreters (Python).

CPU Architectures have differing ways of understanding and executing code also, several formats exist however arguably the most common and the one chosen for this post is the Intel x86 architecture.

# What is Assembly?

Source Code is written in a format that humans can understand in order to develop instructions for an underlying CPU to execute. However, CPUs cannot understand the high-level languages that are utilized when developing programs. The CPU understands one language only, and that is binary. A human is not capable of interpreting binary code at an appropriate fluency, therefore a low-level language is used to convert the binary values and structure into something that is easier to interrupt, Assembly Language. The example right displays a very basic overview of what assembly code looks like when a compiled program is de-compiled though the use of specialized tooling.

In the example below, there are some core sections that require a bit of explaining.

```plaintext
0x00401000      push    ebp
0x00401001      mov     ebp, esp
0x00401003      sub     esp, 8
0x00401006      lea     eax, [var_8h]
0x00401009      push    eax
0x0040100a      push    0xf003f    ; '?'
0x0040100f      push    0
0x00401011      push    str.SOFTWARE__Microsoft___XPS ; 0x40c040
0x00401016      push    0x80000002
0x0040101b      call    dword [RegOpenKeyExA] ; 0x40b020
0x00401021      test    eax, eax
0x00401023      je      0x401029
0x00401025      xor     eax, eax
0x00401027      jmp     0x401066
```

Memory Addresses
: The first column displays the location of the data stored in memory, a program's instructions must be put somewhere before they are executed. Memory addresses can be called by an application or function to execute the instructions contained within their memory space. Each instruction is allocated its own memory address and placed on an individual line.

Instructions
: The second and third columns display Assembly instructions which are generally described as being standardized mnemonics for the corresponding machine code. The corresponding machine code is written in binary and be expressed in Hexadecimal at times for easier interpretation. The example shown does not display the machine code, instead showing the corresponding assembly only.

Registers
: As with memory addresses that store data in memory, processors contain special variables called registers that store data while being processed. The x86 processor has several registers which are identify via a specific three letter designation, as seen in the third column of the example. Some common general purpose registers are:
- eax: Accumulator
- ecx: Counter
- edx: Data
- ebx: Base
- esp: Stack Pointer
- ebp: Base Pointer
- esi: Source Index
- edi: Destination Index
- eip: Instruction Pointer

# Interpreting Assembly

Assembly Language syntax generally follows a set style for displaying instructions. The below example displays a move operation that transfers the register esp to ebp. A destination and source can be either a register, a memory address or a value.

```plaintext
operation <destination>, <source>
mov ebp, esp
```

Assembly operations are quite intuitive, however some of the commonly encountered conditions are:
- mov: Move a value from source to destination
- sub: Subtract a value from source from destination
- inc: Increment a value by source from destination
- cmp: Compare the values of source and destination
- j*: Jump to an instruction at a memory address. Jumps have several iteration, all beginning with j and are generally used in correlation with comparisons, a few examples are:
    - jle: Jumps is the comparison is less than or equal to a value.
    - jnle: Jumps if not less or equal
    - jmp: Jump to a specified instruction (unconditional)
    - je: Jump if values are equal
    - jne: Jump if values are not equal
- nop: No operation
- ret: Return to specified address
- push: Publish a value to the stack
- pop: Restores the top of the stack into a register
- xor: Logical exclusive OR operation

# Sources
- [Coding Den - The Compilation Process](https://medium.com/coding-den/the-compilation-process-a1307824d40e)
- [Tutorials Point - Assembly Conditions](https://www.tutorialspoint.com/assembly_programming/assembly_conditions.htm)
- [Aldeid - x86 Assembly Instructions](https://www.aldeid.com/wiki/X86-assembly/Instructions)