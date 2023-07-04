# Control Flow In-Depth

## Loops

Looping is used to execute a single code block many times

There are two way to create a loop:

* Conditional jumps
* LOOPcc instructions in assembly
  * LOOPcc always uses the ECX register as the counter for the loop
  * loopnz: Loop if the zero flag is not set and ECX is not zero
  * loop: Loop if ECX is not zero
  * ECX is automatically decremented

### Loop Components (5)

1. **Control variable**: variable that are used to determine if a loop exists
2. **Loop initialization**: Starting value for a loop
3. **Loop body**: Code block
4. **Loop update**: Instruction that modifies the control variables during each loop iteration
5. **Stopping condition**: Conditions to determine if a loop should exit

### Loop Types

{% hint style="info" %}
For and While loops are functionally identical when it comes to the assembly opcodes
{% endhint %}

1. **For Loop** - Initialization, stopping, and updating conditions are specified at the beginning
   1.

       <figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
2. **While Loop** - Initialization is separate from the stopping condition, and the updating of the control variables is in the body of the loop
   1.

       <figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>
3. **Do while Loop** - Similar to while, except the code block gets executed at least once before the condition is evaluated
4. Switch Statement

{% hint style="info" %}
Switch statements have an embedded address table created by the compiler&#x20;
{% endhint %}

<figure><img src="../../.gitbook/assets/image (3) (3).png" alt=""><figcaption></figcaption></figure>

