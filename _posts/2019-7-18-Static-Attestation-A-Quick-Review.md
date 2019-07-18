---
layout: post
title: Static Attestation - A Short Review
categories: [MScProject]
tags: [attestation]
---

I'm going to have a quick look at various mechanisms designed to provide static attestation for embedded system.

## SMART: Secure and Minimal Architecture for (Establishing a Dynamic) Root of Trust

<a href="https://www.ics.uci.edu/~gts/paps/smart.pdf">SMART</a> provides a simple method of attesting the state of a user application through slight hardware alterations. As an input from the verifier it takes a nonce, start memory address and end memory address (as well as additional parameters for an optional start position of user application). The prover replies with a SHA-1 HMAC of the requested code space.

Important properties required by SMART are:

* Secure key storage - this has not been directly addressed here however options are presented such as the key being hard-coded at production time (and never changed again) or by implementing a secure means of modifying by an authorized party, but not reading (as in the key can only be read by SMART). The second option was deferred to being future work.
* Secure key access - only SMART code on ROM can access the key. This is provided by registers only allowing access to the key while the program counter is located within SMART application space.
* It is not possible to enter or exit SMART instructions other than in the beginning or end. E.g. if the PC is outside of SMART its previous location must also be outside of SMART (or the last instruction), and if the PC is inside SMART its previous location must be at the start or also within SMART.
* Smart code is not editable, it is stored on ROM.

SMART was implemented on low-end MCUs such as MSP430 or AVR. After implementation it was realised that only the memory access controller needed to be modified, therefor making the solution possibly compatible with "black box" processors such as low-end ARM cores.

To critique the method - it uses SHA1 which is now outdated. It also could succumb to cold-boot attacks but states that due to the typical MCU design where the processor and memory are a single package meaning that memory could not be accessed directly.

## VIPER: Verifying the Integrity of PERipherals' Firmware

<a href="https://dl.acm.org/citation.cfm?id=2046711">VIPER</a> is a software only solution designed to provide attestation for peripherals' firmware, where a the host CPU queries peripherals. It uses combinations of checksums, hashes and time-frame based checks to ensure peripheral firmware is correct. I have not found this solution to be particularly gripping however I may return to it if it suddenly makes sense to me.  

## Pioneer: Verifying Code Integrity and Enforcing Untampered Code Execution on Legacy Systems

<a href="https://dl.acm.org/citation.cfm?id=1095812">Pioneer</a> is another software-only solution which works in a similar way to VIPER but is designed to attesting legacy systems. A checksum is through the verifying code, and is compared to an expected execution time, the checksum appears to also cover control flow of the verification code, which is interesting. This proves that the testing mechanism is correct. The testing mechanism then builds a hash of the target code.

## SWATT: SoftWare-based ATTestation for Embedded Devices

<a href="https://ieeexplore.ieee.org/document/1301329">SWATT</a> is another software-based solution which uses a random number generator to generate random memory addresses which are attested - this means an attacker cannot predict which regions they need to leave intact. They also use time measurement to ensure an attacker is not manipulating the results, although I have to comment that time-based checking is questionable especially when attestation is taking place remotely ,e.g. over a network.

This paper also references solutions which use secure coprocessors which are used during system initialisation to bootstrap trust, examples of these are TCG (Trusted Computing Group, formerly known as TCPA) and Next-Generation Secure Computing Base (NGSCB, formerly Palladium) which may be of interest at a later point as we will be able to see if they produce a useful output after startup has occurred.

## What have a learnt today?

To provide attestation one usually requires a nonce and an area of memory to attest. The problem of offline attestation (audit) is that a previous good report could be cloned. To overcome this I have considered sequence numbers - where they need to be unique each time the process occurs, these would be stored alongside the HMAC prior to being encrypted. Another option could be using a time-stamp, however the time value would have to be strongly protected and secure in order to prevent an attacker from changing the time to the desired time when the attack is planned in order to create a "good" report. Chaining reports together might also provide protection against this but I have not fully decided about this yet.

It seems that a large amount of the focus of the software-only mechanisms focus on proving the integrity of the verification software, which is a legitimate concern.

Apparently one must always state that attackers are assumed to be unable to break cryptographic primitives.

Perhaps the solution will need a certain amount of hardware assistance, could this be where ARM TrustZone comes in?
