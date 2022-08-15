# Introduction to Memory Analysis

## Windows Memory Analysis

1. <mark style="color:blue;">**Identify Context**</mark>
   1. Find the Kernel Processor Control Region (KPCR), Kernel Debugger Data Block (KDBG), and/or Directory Table Base (DTB)
2. <mark style="color:blue;">**Parse Memory Structures**</mark>
   1. Executive Process (EPROCESS) blocks
   2. Process Environment (PEB) blocks
      1. List of DLLs loaded (3 lists)
         1. In order of Load
         2. In order of Initialized
         3. In order of Memory
   3. Virtual Address Descriptors (VAD) Tree
      1. List of memory sections belonging to the process
   4. Kernel Modules/drivers
3. <mark style="color:blue;">**Scan for Outliers**</mark>
   1. Unlinked processes, DLLs, sockets, and threads
   2. Unmapped memory pages w/ execute privs
   3. Hook detection
   4. Known heuristics and signatures
4. <mark style="color:blue;">**Scan for Outliers**</mark>
5. <mark style="color:blue;">**Analysis: Search for Anomalies**</mark>
6. <mark style="color:blue;">**Dump Suspicious Processes**</mark>

![](<../../.gitbook/assets/image (75).png>)

## Volatility

### How to use Volatility

```python
vol.py -f [image] --profile=[profile] [plugin]

# HELP
vol.py -h
# Plugin specific help
vol.py <plugin> -h
# Profiles and registered objects
vol.py --info
```

#### Image Identification

* Volatility profiles now match build number
* Document the version and build number during collection
  * If you haven't, **kdbgscan** plugin can identify the build string

#### Hibernation File Conversion: imagecopy

* Converts crash dumps and hibernation files to raw format

## Finding the First "Hit"

### 1. Identify Rogue Processes

#### Processes

{% hint style="info" %}
Know normal to find evil
{% endhint %}

{% hint style="info" %}
****[**https://www.echotrail.io/products/insights/**](https://www.echotrail.io/products/insights/)****
{% endhint %}

* <mark style="color:orange;">When reviewing/analyzing processes, focus on these 6 areas</mark>:
  * _**Image name** - Legit process? Spelled correctly? Matches system context? (windows process' running on linux, that shouldn't happen)_
  * &#x20;_ **Full Path** - Is this where you expect to see this process and/or is a process running out of a peculiar area (ie - C:\windows\temp)_
  * &#x20;_ **Parent Process** - Is the parent process what you'd expect?_
  * &#x20;_**Command Line** - Executable matches image name, do arguments make sense?_
  * &#x20;_ **Start Time** - Was the process started at boot? Were any processes started near the time of the known attack?_
  * &#x20;_ **Security IDs**_&#x20;

### Volatility Plugins

#### PsList **(this only shows stuff that's running, normally)**

![](<../../.gitbook/assets/image (80) (2).png>)

#### PsScan (carves unallocated parts of memory, which can show exited processes)

![](<../../.gitbook/assets/image (58).png>)

#### PsTree

![](<../../.gitbook/assets/image (46).png>)

### Suspicious WMI Processes

* Wmi event consumers can be used for persistence and code execution
  * 2 most common abused
    * **CommandLine** and **ActiveScript**

![CommandLineEventConsumer](<../../.gitbook/assets/image (57).png>)

![ActiveScriptEventConsumer](<../../.gitbook/assets/image (48) (2).png>)

### 2. Analyze process objects / DLLs and handles

| Object of Interest     | Definition / Use                                         | Volatility Plugin to Use                                                                                                                                                                                                                                                          |
| ---------------------- | -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| DLLS                   | Dynamic Linked Libraries                                 | vol.py -f \<image> dlllist -p \<process ID>                                                                                                                                                                                                                                       |
| Handles                | Pointer to a resource (including named pipes)            | <p>vol.py -f &#x3C;image> handles -p &#x3C;PID><br><br>[-s] suppress unnamed handles<br><br>[-t type] show only handles of<br><br>Handle types:<br>Process, Thread, Key, Port, Files, Mutant, Token, Timer, IOCompletion, WindowStation, Directory, WmiGUID, Event, Semaphore</p> |
|    Files               | Open Files or I/O devices                                |                                                                                                                                                                                                                                                                                   |
|     Directories        | Lists of names used for aaccess to kernel objects        |                                                                                                                                                                                                                                                                                   |
|     Registry           | Access to a key within the Windows Registry              |                                                                                                                                                                                                                                                                                   |
|     Mutexes/Semaphores | Control/Limit access to an object                        |                                                                                                                                                                                                                                                                                   |
|     Events             | Notifications that help threads communicate and organize |                                                                                                                                                                                                                                                                                   |
| SIDs                   | Unique ID on Windows for each account/group              | vol.py -f \<image> getsids -p \<processID>                                                                                                                                                                                                                                        |
| Threads                | Smallest unit of execution; the workhorse of a process   |                                                                                                                                                                                                                                                                                   |
| Memory Sections        | Shared memory areas used by a process                    |                                                                                                                                                                                                                                                                                   |
| Sockets                | Network port and connection information within a process | vol.py -f \<image> --profile=\<profile type> netscan                                                                                                                                                                                                                              |
|                        |                                                          | vol.py -f \<image> --profile=\<profile type> connscan                                                                                                                                                                                                                             |
|                        |                                                          |                                                                                                                                                                                                                                                                                   |

### 3. Review network artifacts

* Suspicious Ports
  * Abnormal ports, backdoor ports
* Suspicious Connections
  * External connections, known bad IPs, TCP/UDP connections, creation times
* Suspicious Processes
  * Processes with open sockets that are unusual

{% hint style="warning" %}
Keep an eye out for:\


1. Processes communicating over 80,443,8080 that aren't browsers
2. Browsers not communicating over 80,443,8080
3. Connections to internal/external IP addresses that are unexplained (connection to China? wtf?)
4. Web requests directly to IP addresses rather than a domain name
5. RDP (port 3389)
6. DNS to weird domains
7. Workstation to workstation connections\


Might wanna think about baselining this traffic


{% endhint %}

Use **netscan** plugin to identify network sockets and TCP structures resident in memory

### 4. Look for evidence of code injection

{% content-ref url="code-injection-rootkits-and-extraction.md" %}
[code-injection-rootkits-and-extraction.md](code-injection-rootkits-and-extraction.md)
{% endcontent-ref %}

### 5. Check for signs of a rootkit

{% content-ref url="code-injection-rootkits-and-extraction.md" %}
[code-injection-rootkits-and-extraction.md](code-injection-rootkits-and-extraction.md)
{% endcontent-ref %}

### 6. Dump suspicious processes and drivers

{% content-ref url="code-injection-rootkits-and-extraction.md" %}
[code-injection-rootkits-and-extraction.md](code-injection-rootkits-and-extraction.md)
{% endcontent-ref %}



### Automating Analysis

#### Baseline (volatility plugin)

{% embed url="https://github.com/csababarta/volatility_plugins/blob/master/baseline.py" %}
baseline.py
{% endembed %}

![](<../../.gitbook/assets/image (49).png>)
