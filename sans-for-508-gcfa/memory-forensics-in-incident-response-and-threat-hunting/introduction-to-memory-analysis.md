# Introduction to Memory Analysis

## Windows Memory Analysis

1. <mark style="color:blue;">**Identify Context**</mark>
   1. Find the Kernel Processor Control Region (KPCR), Kernel Debugger Data Block (KDBG), and/or Directory Table Base (DTB)
2. <mark style="color:blue;">**Parse Memory Structures**</mark>
   1. Executive Process (EPROCESS) blocks
   2. Process Environment (PEB) blocks
      1. DLLs loaded
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

### 2. Analyze process DLLs and handles

### 3. Review network artifacts

### 4. Look for evidence of code injection

### 5. Check for signs of a rootkit

### 6. Dump suspicious processes and drivers
