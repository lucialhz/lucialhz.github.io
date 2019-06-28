---
layout: post
title: Academic Transparency
---

I have finally finished reading the fanstatstic paper by de Clercq and Verbauwhede - <a href="http://arxiv.org/abs/1706.07257">A survey of Hardware-based Control Flow Integrity (CFI)</a> and it is apparent that academia still his some way to go when it comes to transparency. Here a large number of promising CFI solutions are presented and compared, unfortunately one of their findings is the lack of conformity in results from paper to paper. For example, FPGAs are used in many solutions to present changes to ISAs and rapidly prototype design changes. Many solutions using FPGAs present the overhead of their solutions in terms of overheads of LUTs, registers, BRAM and gates compared to the original unmodified processor. This is all well and good on a project by project basis but in order to compare solutions this is not so helpful as different processors can have vast differences in terms of footprints. de Clerq and Verbauwhede note that even different configurations of the same processor can have significantly different area usage.

Another obstacle I am having trouble with is the availability of source code for these solutions. I understand that many of these solutions may be affected by patents, but many are simply left without any further usage once the project is written up. I suppose I find it hard to understand how results are verified, and how these contributions are supposed to be built upon. Perhaps I just need to email the authors of the papers!
