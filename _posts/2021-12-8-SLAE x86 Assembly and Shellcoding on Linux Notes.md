<p>Notes taken while going through the Pentester Academy x86 Assembly and Shellcoding on Linux Course.<br />Not done with it yet. Publishing the assignments as I complete them on my Github. <span></span></p><!--more--><br /><br /><br /><br />Setup up Debian lab environment, what is assembly<br />
Seeing CPU info on what we can do (architecture and stuff), and memory/cpu architecture basics<br />
Exploring registers with GDB, should get more familiar with GDB<br />
"set dissasembly flavor intel" within gdb<p></p><p>Modern normal computer use protected mode most of the time and that's what I should concern myself with.<br />
use assembler (NASM) and linker (LD) to link the assembly executable in ELF format<br />
Wrote hello world in intel 32bit assembly, getting used to syntax and nuances to write shellcode later on.<br />
syscalls or system calls to leverage operating system to avoid writing low level code that's been already been written<br />
int0x80 to invoke syscalls</p><p>Stepped through our hello world program with gdb to understand how registers were used throughout <br />
learned about data types such as bits, bytes, words, dwords, etc. (more of a refresher but it was useful none the less)</p><p>when moving data with the MOV instruction you will be moving it between registers<br />
memory to register (and vice versa)<br />
immediate data to register<br />
or immediate register to memory<br />
LEA stands for load effective address or loading pointer values for 
example LEA eax, [label]. label being the label of the name in the data 
section<br />
XCHG swaps values register to register, or also register to memory <br />&nbsp; <br /></p>
