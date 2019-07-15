---
layout: post
title: Attacks on Control-Flow
categories: [MScProject, CFI]
tags: [signatureModelling, BasicBlocks, ShadowStacks, CFG]
---

In the last post I looked into control flow and now we will see what the common attacks targeting control flow are.

## Hardware-based attacks

I have already covered these elsewhere.

## Buffer Overflow

Target of attack is manipulating control-flow information stored on the stack and heap of a program in order to achieve further objectives.

## Injected code

Where control flow is deviated to existing injected code (usually as data) - usually solved by using NX bit which marks data memory as non-executable. 

## Code re-use attack

Where control flow is deviated to existing code such as system functions or unintended code sequences (refactor last phrase). 

### Return Oriented Programming (ROP)

An attacker can string together (benign) existing code sequences (gadgets) to form malicious program actions.  

## jump-to-libc

## return-to-libc

## Pointer subterfuge

## Non-control data attacks

Corrupting data used to decide on control flow for example in a comparison in an `if` statement. These usually produce unintended yet valid program flow, however there are examples which do not induce unintended flows - see <a href="https://dl.acm.org/citation.cfm?id=1315313">The geometry of innocent flesh on the bone: return-into-libc without function calls (on the x86)</a> for more details.

## Summary

So this blog hasn't been spectacular. I think I will need to come back to this subject when I get a bit more time and inspiration!
