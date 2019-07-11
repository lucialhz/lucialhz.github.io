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

### Indirect

### Unintended

## Loops
