---
layout: single
title:  Learning to Solve basic Crackme's with Pwnable.kr 
date: 2021-12-8
classes: wide
header:
  teaser: 
tags:
  - pwn
  - Crackme
  - ctf
--- 



Throughout the journey I will reference multiple sources and people to gain a better understanding of binary exploitation and reverse engineering at large.

To begin my journey I will go through

https://research.checkpoint.com/wp-content/uploads/2020/03/pwnable_writeup.pdf

-----------------------------------------------------------

I attempted http://microcorruption.com/  but with my current knowledge I can't even begin to understand what's going on. I would have to continue on the SLAE course and work more with disassemblers to read the assembly instructions and work with their debugger.

Onto pwnable.kr challenges, challenge 0x02 Bof, a buffer overflow challenge with the source code included to look at.

![](/assets/images/learning-crackme/crackme.png)

They initialize key as a variable, initialize a buffer of 32 bits for "overflow me", prints that statement to console. 

They then ask for inputs with gets 

(which is a dangerous for memory error if implemented incorrectly, it just seeks input without verifying how much data will fit in the buffer)

Work in progress, continuing tomorrow

