# Core Reversing Concepts

<figure><img src="../../.gitbook/assets/image (7) (2) (1).png" alt=""><figcaption><p>Reevrsing Concepts</p></figcaption></figure>

## Ghidra

Sometimes Ghidra might not have all the information for function type calls (API Calls)

* To remedy this:
  * Right-click in the "**Data type manager**" window on the type of sample you are analyzing > **Apply Function Data Types**
    * It won't really look like Ghidra does anything
  * Then click Analysis > One Shot > Windows x86 Propagate External Parameters
  * You should now be able to see comments on the right side of common API calls so that you can more easily identify their functio

Sometimes Ghidra might not interpret data correctly (it may mistake a string for something else, like a register or memory address)

*   To remedy this (if you know what the correct type is):

    * Right click on the label of the data you're trying to interpret, Data > \[choose data type]

    Before:\
    ![](<../../.gitbook/assets/image (7) (2).png>)

&#x20;     After:\
&#x20;    &#x20;



## Registers

* Can be used for any purpose but are most often used for specific purposes
  * EAX - Addition, multiplication, **return values**
  * ECX - Counter
  * EBP - Reference arguments and local variables (base pointer)
  * ESP - Points to the last item on a stack
  * ESI/EDI - Memory transfer instructions
  * **EIP** - Points to the next instruction to execute
  * EFLAGS - represent the outcome of computations and they control CPU operation
  * Segment Registers (not all inclusive) - describe different segments of memory:
    * CS: Code Segment
    * DS: Data segment
    * SS: Stack Segment

{% hint style="info" %}
If you remember any pointer, it would be **EAX** and **EIP**
{% endhint %}

## Assembly

Brackets means fetch data at the specified address (dereference)

Some tools do not include the brackets

<figure><img src="../../.gitbook/assets/image (8) (2).png" alt=""><figcaption><p>dereferencing</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption><p>indirect referencing</p></figcaption></figure>

Above: Indirect referencing may pose challenges because registers are not populated until runtime

### Assembly Instructions

{% hint style="warning" %}
The result of instructions are stored in the flags registers
{% endhint %}

#### Unconditional Jumps

JMP (unconditional) - JMP tells the CPU to start executing code from a new location in memory

CALL

RET

#### Conditional Jumps

Conditional jumps are formatted "Jcc"

* **Jcc**
  * A = ABOVE, B = BELOW, E = EQUAL, N = NOT EQUAL, G = GREATER THAN, L = LESS THAN, Z = ZERO
  * ABOVE/BELOW ARE USED FOR AN UNSIGNED COMPARISON
  * GREATER THAN/LESS THAN ARE USED FOR A SIGNED COMPARISON
* Example:
  * JA
  * JNE
  * JNA
  * JLE
  * etc

**TEST** - Tests if value is zero; TEST "ANDs" the values together to see if they are zero

* TEST EAX, EAX

**CMP** - implied SUB, tests if one value is larger than another

## Data Structures

Data structures group simple variables into more complex types

* Strings (most common)
* Linked lists
* Sockets
* File Handles
* etc



## Ghidra

* Reverse engineering suite of tools
* Its decompiler produces C representation of the code to speed up analysis
* Ghidra includes support for writing Java and Python scripts to automate analysis

