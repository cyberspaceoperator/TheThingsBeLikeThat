# Control Flow In-Depth

Looping is used to execute a single code block many times

There are two way to create a loop:

* Conditional jumps
* LOOPcc instructions in assembly
  * LOOPcc always uses the ECX register as the counter for the loop
  * loopnz: Loop if the zero flag is not set and ECX is not zero
  * loop: Loop if ECX is not zero
  * ECX is automatically decremented
