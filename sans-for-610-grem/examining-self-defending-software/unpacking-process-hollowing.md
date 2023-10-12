# Unpacking Process Hollowing

What is process hollowing?

{% embed url="https://attack.mitre.org/techniques/T1055/012/" %}

1. Utilize capa -vv to find the memory address of the code that was flagged to begin reversing the behavior or a particular technique, in this case process hollowing

<figure><img src="../../.gitbook/assets/image (106).png" alt=""><figcaption><p>capa output</p></figcaption></figure>

2. Load .exe into ghidra and go to the memory address identified in step 1. Nearby, there may be a flag called dwCreationFlags marked 0x04, this indicates that the process should be created in a "suspended" state

<figure><img src="../../.gitbook/assets/image (107).png" alt=""><figcaption><p>suspended state process for process hollowing</p></figcaption></figure>

3. Find call near there for WriteProcessMemory
4. Set breakpoint
5. Run to breakpoint
6.  The grey process is the suspended process that has not been written into yet due to the breakpoint set above in step 5

    <figure><img src="../../.gitbook/assets/image (108).png" alt=""><figcaption><p>suspended process</p></figcaption></figure>
7. lpAddress of WriteProcessMemory contains the data that is going to be copied into the process to be injected.
8.  lpAddress is the third parameter (read the documentation of the API Call), select the parameter > follow in dump

    <figure><img src="../../.gitbook/assets/image (109).png" alt=""><figcaption><p>third parameter</p></figcaption></figure>

    <figure><img src="../../.gitbook/assets/image (110).png" alt=""><figcaption><p>Memory dump of 3rd parameter</p></figcaption></figure>
9. Right click in the memory dump > Follow in Memory Map > Right click memory map location and click Dump memoryto file
10. &#x20;Congrats, you've dumped an injected process

| Tool | Usage                   | Description                                                                             |
| ---- | ----------------------- | --------------------------------------------------------------------------------------- |
| capa | capa \<malware.exe>     | Outputs a list of techniques used by the exe                                            |
| capa | capa -vv \<malware.exe> | Gives more information about how the exe is performing a technique listed in the output |
|      |                         |                                                                                         |

