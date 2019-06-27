---
layout: post
title: CFI - Some quick thoughts
---

Having reviewed a fair bit of literature of CFI it amazes me to see the huge variety in solutions.

## Signature Modelling

When I first learnt of CFI, it was described in relation to control-flow graphs (CFGs). Here if a series of executed instructions do not follow the trace of the program's CFG the control-flow integrity has been lost.
[The paper written by Yang et al in 2013](http://dx.doi.org/10.1016/j.cja.2013.02.019) did a very good job at describing this method as implemented in software. They also performed a throrough analysis of various existing solutions (page 2).
I first saw this methodology in use in hardware in the [paper written by Werner et al in 2016](http://link.springer.com/10.1007/978-3-319-31271-2_10) although I have to admit I got a bit scared off after seeing all of the algebra! I think I'll have to return to this paper at a later date.

A problem with signature modelling is that sequences of instructions (and their accompanying signatures) accumulate very quickly, requiring more storage. *Actually now I think about that is that really correct? A cryptographic signature (hash) is the same length for all inputs.* Another problem is the necessity to constantly compute signatures, this would add a fair amount of processing overhead. Although, as we will see in future methods, encryption is a common method of ensuring CFI so I suppose perhaps it is not too much to ask for. 

## Basic Blocks

As a result of investigating CFGs I learnt about basic blocks (BBs). These are sequences of instructions which will run all the way from the start to the end, i.e. there will be no branches within (for example if/else).
Various academics have leveraged the priniciple of BBs to simplify CFI, where source code / assembly is analysed to build basic blocks and determine how they link up. This leads on to the types of branches/jumps (e.g. direct or indirect) which I will focus on in a later blog post.
Many authors of papers leverage some sort of method to label which blocks can lead to others (and back again). The issue with this is that it can, again, accumulate for larger programs. Also keeping track of commonly called functions, and where they return to is a tricky problem (as discussed/attempted to address in various papers).

## Shadow Stacks

[Some](http://dl.acm.org/citation.cfm?doid=2857705.2857722) have argued that ensuring CFI without the use of shadow stacks (SSs) is an impossible task and that "the use of a a shadow stack is mandatory for any practical CFI deployment". A shadow stack is basically what is says on the tin. A seperate stack is stored which contains just the return addresses. This solves the problem of ROP, especially when working with commonly called or system functions as the implementation of the shadow stack ensures the calling function has to be the return function.

Of course, as there always seems to be in acedemia, there are some flaws with shadow stacks. These include:
* Storage of shadow stacks can be cumbersome - esecially if they are to be secure (as they would have to be if memory access is an assumed capability of an attacker).
* Maintiaining a shadow stack in multi-threaded systems will be troublesome - searching through the SS for a process ID will add additional time overhead to any processing.
* Recursive functions can add multiple entries of the same return address to the SS, this is an unnessasary addition so is reduced down to a single it flag (or  counter) by some solutions. This can raise the problem of manipulating the number of recursions by an attacker, however it is argued by some *reference pending* that this is a very limited attack.
* Instructions such as longjmp *(not sure about the spelling)* will skip back through multiple return addresses, so this needs to be taken into account - ever by removing the instruction from the ISA or another method such as iterating through the SS until the return address is identified (with the above addresses then removed from the SS to maintain a proper representation of the stack). I cannot work out if the second method is a good one or not, but it makes a lot of sense to me.

## What have a learnt today?

I think shadow stacks should be implemented in any solution I adopt - perhaps R. Lee's CEP from 2019. My idea would be to maintain a shadow stack in protected memory and page it out to main memory using encryption to secure the paged version. This shadow stack would make use of the recursive single-bit flag, a process ID is threading is used (although I have to admit that I haven't even considered threading yet - *project idea?*), and would simply pop entries off if a longjmp-like instruction is called (in the hope that this does not provide an attacker with an advantage, but I bet it would because they always find a workaround.).

I have also rediscovered my love for signature based CFI, which is what I planned on using for an attestation based CFI mechanism (much similar to <a href="http://arxiv.org/abs/1605.07763">C-FLAT</a>).

(C-FLAT)[http://arxiv.org/abs/1605.07763]

## Papers to re-discover

Yang et al and Werner et al.

This blog post has been brought to you by an excellent paper by de Clercq and Verbauwhede - (A survey of Hardware-based Control Flow Integrity (CFI))[http://arxiv.org/abs/1706.07257] which I have only reached section 6.13 so far.
