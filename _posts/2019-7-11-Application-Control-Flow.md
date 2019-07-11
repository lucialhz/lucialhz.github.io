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
## Control-flow graphs

## Types of branches

## Loops
