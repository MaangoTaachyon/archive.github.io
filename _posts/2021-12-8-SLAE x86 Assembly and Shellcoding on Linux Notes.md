---
layout: single
title:  SLAE x86 Assembly and Shellcoding on Linux Notes 
date: 2021-12-8
classes: wide
header:
  teaser:
tags:
  - Certification
  - pwn
--- 
 
Notes taken while going through the Pentester Academy x86 Assembly and Shellcoding on Linux Course.
Not done with it yet. Publishing the assignments as I complete them on my Github.




Setup up Debian lab environment, what is assembly
Seeing CPU info on what we can do (architecture and stuff), and memory/cpu architecture basics
Exploring registers with GDB, should get more familiar with GDB
"set dissasembly flavor intel" within gdb

Modern normal computer use protected mode most of the time and that's what I should concern myself with.
use assembler (NASM) and linker (LD) to link the assembly executable in ELF format
Wrote hello world in intel 32bit assembly, getting used to syntax and nuances to write shellcode later on.
syscalls or system calls to leverage operating system to avoid writing low level code that's been already been written
int0x80 to invoke syscalls

Stepped through our hello world program with gdb to understand how registers were used throughout
learned about data types such as bits, bytes, words, dwords, etc. (more of a refresher but it was useful none the less)

when moving data with the MOV instruction you will be moving it between registers
memory to register (and vice versa)
immediate data to register
or immediate register to memory
LEA stands for load effective address or loading pointer values for example LEA eax, [label]. label being the label of the name in the data section
XCHG swaps values register to register, or also register to memory 
