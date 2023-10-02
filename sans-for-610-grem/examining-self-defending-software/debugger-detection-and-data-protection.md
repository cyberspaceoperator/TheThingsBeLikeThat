# Debugger Detection and Data Protection

Some malware utilizes code that can detect the usage of a debugger so that it makes it more difficult to analyze



To analyze the file, there are a couple of methods than can be used:

{% hint style="danger" %}
Always remember to disable DynamicBase with  CFFExplorer or setdllcharacteristics so that ASLR is not enabled
{% endhint %}

## Detection Methods

### Method 1: Look for API call to IsDebuggerPresent

Some malware will utilize the windows API call "isDebuggerPresent" which returns 1 if there is a debugger being used. If so, the program will terminate itself. Search for this API call, set a breakpoint before the EAX/RAX register is checked and then modify EAX to be 0 instead of 1

### Method 2: Remove the "JNE/JNZ" instructions from the program completely

To remove the instructions that check the value of the EAX/RAX register, right-click the JNE/JNZ instruction and click "Assemble", fill in the box with "NOP" and then check the "Fill with NOPs" box as well to make sure all instructions related to JNE/JNZ are replaced with NOPs.

### Saving Patched version of code

File > Patch File

<figure><img src="../../.gitbook/assets/image (101).png" alt=""><figcaption><p>saving a patched file</p></figcaption></figure>

### Method 3: Look for API call to OutputDebugString

Some malware can detect a debugger by calling OutputDebugString which is designed to allow programs to print messages that a debugger can intercept and display to the developer who might be troubleshooting a problem with the code

If the process is being debigged, OutputDebugPresent will execute normally without registering an error that the process could check by making the GetLastError call. The the debugger is not present, an error code will be set as described [here](https://www.veracode.com/blog/2008/12/anti-debugging-series-part-ii). If OutputDebugString is called while the debugger is attached, it will return a valid memory address inside the process's memory space, however if there is no debugger, the API call with return a small integer such as 0 or 1

### Method 4: Malware checks the PEB (process environment block)

Inside the PEB, there is a 1-byte field called "BeingDebugged" which is set to 1 if the process is being debugged. The bit is located at offset 2 inside the PEB and can be checked using the assembly instructions:

```
mov eax,fs:"[30h]
mov eax,[eax+2]
test eax,eax
```

### Method 5: NtGlobalFlag PEB field

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

### Method 6: Timing

Malware can detect via "GetTickCount" that it's being debugged if it's executing too slowly due to single-stepping.

<figure><img src="../../.gitbook/assets/image (103).png" alt=""><figcaption><p>malware debugger detection psuedocode</p></figcaption></figure>

Other API calls include GetLocalTime, GetSystemTime

The assembly instruction RDTSC can be similarly used

### Method 7: String Stacking

This is where a malware author will use 1 byte long strings in a variable one by one and stack them on top of each other to build a string

1. You can use strdeob.pl to help look for strings like this

### Debugger Detection Mitigation Methods

1. Use ScyllaHide plugin for x64dbg/x32dbg to automatically conceal the debugger from common techniques
   * Select plugins > ScyllaHide > Options > Enable all options in left column under "Debugger Hiding" > Ok (no need to save or restart process)
     * Consider saving this as a profile

### Data Protection Mitigation Methods

Sometimes, malware authors obfuscate/hide/conceal strings inside of their code by using methods like XORing data with a 1-byte key

1. Utilize XORSearch to look for substrings that are commonly seen in malware or that you observed in an earlier analysis
   1.  You can see in the usage, searching the "http:" string it was found being hidden with the key "XOR 83"

       <figure><img src="../../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>



       <figure><img src="../../.gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>


2. You can also use brxor.py to deobfuscate XOR-encoded strings that include English words WITHOUT having to specify what you are looking for
3. bbcrack is another option
4. strdeob.pl searches for stacked strings in an executable
5. FLOSS automatically deobfuscates concealed strings using common and propriatary algorithms&#x20;
   1. Found fewer strings than strdeob.pl, just goes to show that not all tools are perfect, but FLOSS is really good
6.

## Tools

| Tool       | Usage                       | Description                                                                                       |
| ---------- | --------------------------- | ------------------------------------------------------------------------------------------------- |
| bbcrack    | bbcrack -l 1 \<filename>    | Ranks potentially deobfuscated patterns                                                           |
| brxor.py   | brxor \<filename>           |                                                                                                   |
| XORSearch  | xorsearch -i -s \<filename> | look for substrings that are commonly seen in malware or that you observed in an earlier analysis |
| FLOSS      |                             |                                                                                                   |
| strdeob.pl | strdeob.pl \<filename>      | Deobfuscates stacked strings                                                                      |

## References / More reading

{% embed url="https://osandamalith.com/2016/03/08/isdebuggerpresent-api/" %}

{% embed url="https://en.wikibooks.org/wiki/X86_Disassembly/Debugger_Detectors" %}

{% embed url="https://community.broadcom.com/symantecenterprise/communities/community-home/librarydocuments/viewdocument?DocumentKey=230d68b2-c80f-4436-9c09-ff84d049da33&CommunityKey=1ecf5f55-9545-44d6-b0f4-4e4a7f5f5e68&tab=librarydocuments" %}
