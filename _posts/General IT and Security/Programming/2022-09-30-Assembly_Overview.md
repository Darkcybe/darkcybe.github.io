---
title: Assembly Overview
categories: [General IT and Security, Programming]
tag: [assembly]
comments: true
---

## Overview

Assembly programming is a low-level computer programming language that is used to write software that runs directly on a computer's hardware. It is considered a low-level language because it is closer to the machine code that a computer can execute directly, as opposed to high-level languages like C, C++, and Python, which are closer to human-readable languages and require additional processing by a compiler or interpreter.

CPU Architectures have differing ways of understanding and executing code also, several formats exist however arguably the most common and the one chosen for this post is the Intel x86 architecture.

## Core Assembly Components

There are several fundamental components of assembly language, below is a list of some of the more commonly used components for malware analysis:

1. **Instructions:** These are the basic building blocks of an assembly program, and they tell the CPU what to do. Each instruction consists of an operation and one or more operands. Examples of instructions include "mov", "add", and "cmp".
2. **Registers:** These are special memory locations inside the CPU that are used to store data temporarily during the execution of a program. There are different types of registers for different purposes, such as storing values, addressing memory, and holding flags.
3. **Operands:** Operands are the values or addresses that are used by instructions. They can be registers, memory addresses, or constants. For example, in the instruction "mov eax, 5", the register "eax" is the destination operand and the constant "5" is the source operand.
4. **Memory:** This is where a program's data and instructions are stored. Programs can access memory using addresses, which are assigned to specific locations in memory.
5. **Labels:** These are used to give names to specific locations in a program's code. Labels can be used to make it easier to read and understand the code, and they can also be used as destinations for jumps and calls. For example, a label might be used to mark the start of a loop or the location of a specific function.

### Common Assembly 

Below is a table of common assembly language instructions:

| Operand |                                      Description                                      |
|:-------:|:-------------------------------------------------------------------------------------:|
|   MOV   | Move data from one location to another                                                |
|   ADD   | Add two values and store the result                                                   |
|   SUB   | Subtract one value from another and store the result                                  |
|   MUL   | Multiply two values and store the result                                              |
|   DIV   | Divide one value by another and store the result                                      |
|   INC   | Increment a value by 1                                                                |
|   DEC   | Decrement a value by 1                                                                |
|   AND   | Perform a bitwise AND operation on two values and store the result                    |
|    OR   | Perform a bitwise OR operation on two values and store the result                     |
|   XOR   | Perform a bitwise XOR operation on two values and store the result                    |
|   NOT   | Perform a bitwise NOT operation on a value (inverts all the bits)                     |
|   SHL   | Shift the bits of a value left by a specified number of positions                     |
|   SHR   | Shift the bits of a value right by a specified number of positions                    |
|   JMP   | Jump to a specified location in the program                                           |
|    JE   | Jump to a specified location if the last comparison resulted in equal                 |
|   JNE   | Jump to a specified location if the last comparison did not result in equal           |
|    JG   | Jump to a specified location if the last comparison resulted in greater than          |
|   JGE   | Jump to a specified location if the last comparison resulted in greater than or equal |
|    JL   | Jump to a specified location if the last comparison resulted in less than             |
|   JLE   | Jump to a specified location if the last comparison resulted in less than or equal    |
|   CMP   | Compare two values and set the status flags based on the result                       |
|   PUSH  | Push a value onto the stack                                                           |
|   POP   | Pop a value off the stack                                                             |
|   CALL  | Call a subroutine at a specified location                                             |
|   RET   | Return from a subroutine                                                              |
|   INT   | Generate a software interrupt                                                         |

Below is a table of common assembly language registers:

| Register |                                         Description                                        |
|:--------:|:------------------------------------------------------------------------------------------:|
|    EAX   | The accumulator register, used for arithmetic and data transfer operations                 |
|    EBX   | The base register, used to hold a memory address                                           |
|    ECX   | The count register, used as a loop counter and for shifts and rotates                      |
|    EDX   | The data register, used for arithmetic and data transfer operations                        |
|    ESI   | The source index register, used as a pointer to data in memory                             |
|    EDI   | The destination index register, used as a pointer to data in memory                        |
|    EBP   | The base pointer register, used to point to the base of the current stack frame            |
|    ESP   | The stack pointer register, used to point to the top of the stack                          |
|    EIP   | The instruction pointer register, holds the address of the next instruction to be executed |
|    CF    | The carry flag, set if the last arithmetic operation resulted in a carry or borrow         |
|    PF    | The parity flag, set if the last operation resulted in an even number of 1 bits            |
|    AF    | The auxiliary carry flag, used in binary-coded decimal (BCD) arithmetic                    |
|    ZF    | The zero flag, set if the last operation resulted in a zero value                          |
|    SF    | The sign flag, set if the last operation resulted in a negative value                      |
|    OF    | The overflow flag, set if the last operation resulted in an overflow or underflow          |

Here is a table of common assembly language labels:

|  Label  |                                                           Description                                                           |
|:-------:|:-------------------------------------------------------------------------------------------------------------------------------:|
|  START  | The label for the beginning of the program                                                                                      |
|   END   | The label for the end of the program                                                                                            |
|   LOOP  | A label used to mark the beginning of a loop                                                                                    |
| LOOPEND | A label used to mark the end of a loop                                                                                          |
|   PROC  | A label used to mark the beginning of a subroutine or procedure                                                                 |
| PROCEND | A label used to mark the end of a subroutine or procedure                                                                       |
|   DATA  | A label used to mark the beginning of a data section in the program                                                             |
|   BSS   | A label used to mark the beginning of a block of memory that will be used for data storage but does not have any initial values |

## Interpretation of Assembly Langague

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

In this example, there are several instructions that are being executed. The first instruction, `push ebp`, pushes the value of the EBP register onto the stack. The next instruction, `mov ebp, esp`, moves the value of the ESP register (the stack pointer) into the EBP register (the base pointer).

The `sub esp, 8` instruction subtracts 8 from the value of the ESP register, which is used to allocate space on the stack for local variables. The `lea eax, [var_8h]` instruction loads the address of the local variable `var_8h` into the EAX register.

Other instructions in this example include `push eax`, which pushes the value of the EAX register onto the stack, and `call dword [RegOpenKeyExA]`, which calls the RegOpenKeyExA function and passes it the values on the stack as arguments. The `test eax, eax` instruction tests the value of the EAX register, and the `je 0x401029` instruction jumps to the specified address if the test is equal to zero.

# References
- [Coding Den - The Compilation Process](https://medium.com/coding-den/the-compilation-process-a1307824d40e)
- [Tutorials Point - Assembly Conditions](https://www.tutorialspoint.com/assembly_programming/assembly_conditions.htm)
- [Aldeid - x86 Assembly Instructions](https://www.aldeid.com/wiki/X86-assembly/Instructions)