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

### Malfind Countermeasures:

* Advanced adversaries can attack tools and process
* Malfind only shows a "preview" of the first 64 bytes, advanced adversaries do things like:
  * Overwrite the first 4 bytes (MZ header)
  * Clear entire PE header (or first 4096 bytes)
  * Jump or redirect to code placed later in the page
* \--dump-dir option outputs entire contents
  * Strings, scan with YARA sigs, antivirus scan
  * Have a reverse engineer validate the code

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

* Detects process hollowing

### threadmap

* Detects process hollowing

## Hooking and Rootkit Detection

![](<../../.gitbook/assets/image (25) (4).png>)

### Rootkit Hooking

### Volatility Rootkit Detection & Plugins

![](<../../.gitbook/assets/image (86).png>)

### Plugin: SSDT (System Service Descriptor Table)

Displays hooked functions within System Service Descriptor table (Windows Kernel Hooking)

SSDT holds pointers to the various kernel functions that power WIndows

The plugin will list all entries with the following information:

* Table Entry
* Function Offset
* Function
* Function Owner (the module that the SSDT entry is sending the request to)

{% hint style="warning" %}
Ignore entries related to ntoskrnl.exe and win32k.sys because they own the majority of legit functions that the SSDT table tracks
{% endhint %}

```
vol.py -f <image name> --profile=<profile> ssdt | egrep -v '(ntos|win32k)'
```

Example of real SSDT results

![](<../../.gitbook/assets/image (34) (1).png>)

{% embed url="https://www.welivesecurity.com/wp-content/uploads/200x/white-papers/Passing_Storm.pdf" %}

### Direct Kernel Object Manipulation

* Advanced process hiding technique
  * Unlink and EPROCESS from the doubly linked list
  * Makes changed to kernel objects directly within memory
  * Changes aren't written to disk
  * <mark style="color:orange;">You will not find process with</mark> <mark style="color:orange;"></mark><mark style="color:orange;">**pslist**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">when using psxview</mark>

### Plugin: psxview

* Performs a cross-view analysis using seven different process listing plugins to visually identify hidden processes
* Limit false positives by using "known good rules" (-R)

### Plugin: modscan

* Identify suspicious drivers in memory image

{% hint style="info" %}
**Compare to known-good (using volatility '**<mark style="color:blue;">**baseline**</mark>**' plugin to create the baseline on your own, and then use 'driverbl' plugin or Google drivers you don't recognize**
{% endhint %}

![driverbl plugin](<../../.gitbook/assets/image (32) (3).png>)

### Plugin: apihooks

* Detect inline and Import Address Table function hooks used by rootkits to modify and control information found

![apihooks](<../../.gitbook/assets/image (23) (3).png>)

## Extracting Processes, Drivers, and Objects in Volatility

### Plugin: dlldump

By default, dumps all DLLs in memory image

Try to limit the amount of DLLs to specific processes like with '-p' or '-r' options

![dlldump](<../../.gitbook/assets/image (93).png>)

### Plugin: moddump

Once you've identified **potentially malicious drivers** within a memory image, the next step is to extract them from the image for further review/analysis

![moddump](<../../.gitbook/assets/image (38).png>)

![moddump example](<../../.gitbook/assets/image (51).png>)

### Plugin: procdump

![procdump](<../../.gitbook/assets/image (48).png>)

### Plugin: memdump

![memdump](<../../.gitbook/assets/image (80).png>)

![strings usage](<../../.gitbook/assets/image (94).png>)

![grep usage](<../../.gitbook/assets/image (40).png>)

#### String Searching with memdump

```
vol.py -f <image> memdump -n conhost --dump-dir=.
**********
Writing conhost.exe [xxxx] to xxxx.dmp
**********
# strings -t d -e l *.dmp >> conhost.uni
# grep -i "command prompt" conhost.uni
```

{% hint style="info" %}
volatility also has it's own strings plugin\
[https://github.com/volatilityfoundation/volatility/wiki/Command-Reference#strings](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference#strings)
{% endhint %}

{% hint style="warning" %}
Before doing string searching, run 'winmem\_decompress.py' or 'win10memcompression.py' first to decompress the memory dumps. This is because windows compresses certain sections of memory and you might not get everything if searching through a compressed memory dump
{% endhint %}

{% embed url="https://www.itprotoday.com/windows-10/understand-windows-10-memory-compression" %}
Understanding windows 10 memory compression
{% endembed %}

{% embed url="https://www.mandiant.com/resources/finding-evil-in-windows-ten-compressed-memory-part-one" %}

### Plugin: cmdscan

![cmdscan](<../../.gitbook/assets/image (34).png>)

### Plugin: consoles

You see what the user typed in terminal AND what the output was of that command

![consoles/cmdscan difference](<../../.gitbook/assets/image (23).png>)

## Extracting files

### Plugin: dumpfiles

{% hint style="info" %}
Often used in combination with filescan
{% endhint %}

![](<../../.gitbook/assets/image (26).png>)

![dumpfiles](<../../.gitbook/assets/image (24).png>)

### Plugin: filescan
