---
layout: post
title: Rough Survey on CFI Monitoring
---

## Software-only

When CFI was first truly investigated by Adabi et. al in the seminar paper <a href="http://portal.acm.org/citation.cfm?doid=1102120.1102165">Control-Flow Integrity Principles, Implementations, and Applications</a> it mainly focused on implementing a software-based solution. I really need to know this paper back to front. Or perhaps it is the paper <a href="http://e-collection.library.ethz.ch/view/eth:46700">A Theory of Secure Control Flow</a> but the same author which I need, I think this will be the reading for tomorrow.

Signature modelling is the method I most understand, where each function call is accompanied with a signature. This signature can either be compared with an expected signature (this is often the case in solutions which provide integrity), or the signature can be combined with previous signatures to build up a hash of the path taken (this is the method usually used for attestation).

## Hardware-only

Often researchers will implement CFI through hardware (primarily FPGAs) in order to design what could, potentially, be incorporated into processor design. Methods usually involve intercepting fetched instructions and either swapping them for tailored/extended instructions which could add return addresses to shadow call stacks or insert records of executing basic blocks. By doing this, researchers have been able to allow application code to be compiled as standard, without needing to instrument their code (replacing assembly instructions with others for specific reasons). This has also been used in other examples where instructions are encrypted before being stored in memory and decryption when fetched in the processor pipeline.

Outside of FPGAs researchers have utilised debug ports to intercept instructions running over the bus (between memory and the CPU). These instructions can be treated in the same manner as when FPGAs are used, however, delays cannot be introduced. This method seems like a compromise, however it can be used as a good proof of concept.

## Hybrids of Hardware and Software

Some solutions require the use of modified software and hardware, an example of this is the latest work by R Lee (RHUL PhD student).

## Making use of hardware features

Trusted execution environments (TEEs) have been leveraged to provide security. In C-FLAT, ARM TrustZone is used to perform attestation duties, where the attestation engine lives in the secure world. The application being tested (and the majority of the associated logic) lives in normal world. "Trampolines" are heavily used but I forget what they do...  In these cases it is the normal world software which initiates CFI attestation, this is problematic as it requires trust that it is doing it properly. To solve this problem static attestation has been used to initially prove the application is in the expected state.
