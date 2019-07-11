---
layout: post
title: Application Control Flow
categories: [MScProject, CFI]
tags: [signatureModelling, BasicBlocks, ShadowStacks, CFG]
---

So lets go back to basics and talk about control flow within software.

## Control flow

Control flow is how instructions within software transition from one to another.

## Basic Blocks

Basic blocks is a useful way of breaking down the instructions of an application into manageable chunks. A basic block is a set of instructions which sequentially run from beginning to end. For example from the following pseudo-code:

```cs
void GreaterThanPlusOne(int firstNum, int secondNum)
    {
        string message;
        firstNum++;
        if (firstNum > secondNum)
        {
            message = "firstNum plus one is greater than secondNum";
        }
        else
        {
            message = "firstNum plus one is nto greater than secondNum"
        }
    }
```

The instructions from `string message;` to and including `if (firstNum > secondNum)` will sequentially (barring an exception being thrown). This is a basic block. The contents within the `if` and also the `else` parenthesis are also basic blocks.

So in this example the sequence of instructions can flow from the first basic block to either the second or the third basic block. An unauthorised flow would be from BB 2 to BB 3.

Called functions can also be a separator creating basic blocks, as they can be called from multiple places and return to multiple places.

## Control-flow graphs

Control-flow graphs (CFGs) are the graphical representation of the series of valid basic blocks within an application. It is logical that a CFG can become enourmous as the size of an application grows (**need to find reference for this**). So essentially a valid control flow is one which follows a CFG for a given application.

## Types of branches

An important aspect to consider is the difference between branch instruction types: direct, indirect and unintended.

### Direct

A direct branch is one where the jump to instruction is hard coded - the control can flow to a pre-defined location.

This can be violated by a hardware attack such as glitching to skip instructions or from an attack directly on memory where the destination address is swapped for another.

### Indirect

An indirect branch is one where the next instruction address is set at run-time, either through registers or pointers pointing to addresses in memory containing the jump destination. This could be the result of a compiled switch-case statement or dynamic libraries (e.g. reflection). In the case of the switch-case statement it is not so indirect in the source code, however when compiled has the possibility of indirectness.

In what common ways are these violated?

### Unintended

<a href="http://arxiv.org/abs/1706.07257">De Clerq</a> describes an indirect branch as one which arises due to variable length instruction encoding in architectures. An attacker alters control flow to the middle of an instruction. A high percentage (80%) of gadgets exploit this, according to <a href="https://ieeexplore.ieee.org/document/6355533">Kayaalp</a>. For more information reference section 4.2 of Kayaalp's work which describes unintended branches in detail. Does this affect embedded systems architectures?

### Function Return

When a function returns to its calling section of code it references the stack for its return address. The interesting part here is that it is not hard coded and according to a CFG a function could legally return to any locations in code which call that function. Tracking a path taken along the CFG allows the checking of calling location, to make sure the function has returned to the correct caller.

If the stack is attacked the return address can be re-written to point to existing code, or instructions placed in data (this is usually protected for by read XOR write). This is a well-explored area in computer security with various existing solutions including stack canaries.

## Loops

Loops are problematic in terms of CFG. Modelling a loop which could run indefinately is not feasible with CFGs. Various methods has been used to approach this, with some simply compacting loops (this presents issues where an attacker continually run a loop until they reach the desired outcome, such as PIN attempts. Although would this not be fixed by control flow checks within the loop?) while <a href="http://dx.doi.org/10.1145/3061639.3062276">others</a> track loop metadata such as iteration count.
