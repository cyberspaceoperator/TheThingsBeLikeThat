# Core Reversing Concepts

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Reevrsing Concepts</p></figcaption></figure>

## Ghidra

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

