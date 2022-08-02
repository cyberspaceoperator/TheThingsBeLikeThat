# Code Injection, Rootkits, and Extraction

## Why Code Injection?

![](<../../.gitbook/assets/image (43).png>)

* Excellent Camoflauge
* Access to memory and permissions of target
* Process migration
* Evade A/V and application Control
* Assist with Complex Attacks (ie., Rootkits)

## Code Injection

* Very common, DLL Injection (Needs SEDebugPrivilege)  and Process Hollowing
* VirtualAllocEx(), CreateRemoteThread(), OpenProcess()
* SetWindowsHookEx()
* Reflective injection loads code without registering with host process



* Process Hollowing (Stuxnet and Dark Comet)  [https://www.trustwave.com/en-us/resources/blogs/spiderlabs-blog/analyzing-malware-hollow-processes/](https://www.trustwave.com/en-us/resources/blogs/spiderlabs-blog/analyzing-malware-hollow-processes/)
* Malware starts as a suspended instance of legit process
* Original process code deallocated and replaced
* Can retain DLLs, handles, data, etc from original process
* Process image EXE not backed with file on disk -> process hollowing

### Reflective Code Injection

{% embed url="https://www.dc414.org/wp-content/uploads/2011/01/242.pdf" %}

* Advanced technique to perform arbitrary code execution without explicitly calling LoadLibrary
  * Uses custom reflective loader instead of Windows loader
* Code is not registered at all in any way with the system, making it hard to detect

### Detecting Hidden and Reflective Code Injection

* Memory Section marked as <mark style="color:red;">**Page\_**</mark>_<mark style="color:red;">**Execute\_**</mark>_<mark style="color:red;">**ReadWrite**</mark>
* Memory section not backed with a file on disk
* Memory section contains code (PE File or shellcode)

## Volatility Code Injection Plugins

![volatility code injection plugins](<../../.gitbook/assets/image (39).png>)

### ldrmodules&#x20;

Queries each list and displays results for comparison

* DLLs are tracked in 3 different linked lists in the PEB for each process
  * The 3 lists are:
    * InLoad (tracks DLLs and EXE)
    * InInit (only tracks DLLs)
    * InMem (tracks DLLs and EXE)
* Stealthly malware can unlink loaded DLLs freom these lists
* This plugin queries each list and displays results for comparison

### malfind

* Scans process memory sections looking for indications of hidden code injection. Identified sections are extracted for further analysis

1. Does the first 2 steps, up to you to do the third
   1. Memory Section marked as <mark style="color:red;">**Page\_**</mark>_<mark style="color:red;">**Execute\_**</mark>_<mark style="color:red;">**ReadWrite**</mark>
   2. Memory section not backed with a file on disk
   3. Memory section contains code (PE File or shellcode)

### hollowfind

### threadmap

