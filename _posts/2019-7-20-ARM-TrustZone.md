---
layout: post
title: ARM TrustZone
categories: [MScProject]
tags: [trustzone]
---

## Introduction

ARM TrustZone is a method used to separate "secure" applications from non-secure ones in two separate worlds, memory can be designated as secure or not secure. Execution is transferred from each world either through interrupts or direct function calls. [This page](https://developer.arm.com/docs/ddi0333/latest/programmers-model/secure-world-and-non-secure-world-operation-with-trustzone/how-the-secure-model-works) describes the memory aspect quite well. While [this stack overflow q and a](https://stackoverflow.com/questions/36288422/how-to-introspect-normal-world-from-secure-world-using-trustzone) explores it in a more real life example.

I believe secure world TrustZone applications should have access to normal world memory, this will enable static attestation.

Provisioning is an interesting aspect which I haven't quite worked out out.

A secure world application would need to supply the normal world application with a nonce to make sure the response is not a replay.
