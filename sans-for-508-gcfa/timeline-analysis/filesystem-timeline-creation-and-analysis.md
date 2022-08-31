# Filesystem Timeline Creation and Analysis

{% hint style="warning" %}
When doing a timeline, stay in UTC for everything
{% endhint %}

## Windows NTFS Timestamps

* <mark style="color:blue;">M - Data content change time</mark>
  * Time the data content of a file was last modified
* <mark style="color:blue;">A - Data Last Access Time</mark>
  * Approx. Time when the file data was last accessed
* <mark style="color:blue;">C - Metadata Change Time</mark>
  * Time this MFT record was last modified
* <mark style="color:blue;">B - File Creation Time (most important, arguably) (Born Time)</mark>
  * Time file was created in the volume

![timestamps](<../../.gitbook/assets/image (9).png>)

{% embed url="https://docs.microsoft.com/en-us/windows/win32/sysinfo/file-times?redirectedfrom=MSDN" %}
filetimes
{% endembed %}

{% embed url="https://docs.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-filetime?redirectedfrom=MSDN" %}
filetime / minwinbase
{% endembed %}

{% hint style="warning" %}
When doing a timeline, stay in UTC for everything
{% endhint %}

{% hint style="danger" %}
Time rules for $STANDARD\_INFORMATION is different that __ $FILE\_NAME
{% endhint %}

![Windows Time Rules](<../../.gitbook/assets/image (10) (2).png>)

{% hint style="danger" %}
Modified timestamp before creation time!? That's because the file was probably copied from somewhere else, this can be an IOC, something interesting
{% endhint %}

What else can change the timestamps?

![time rule exceptions](<../../.gitbook/assets/image (13).png>)

## Understanding the Filesystem Timeline Format

To interpret a timeline you must understand the columns:

* Time: All entries with the same time are grouped
* <mark style="color:green;">!!! macb: Indication of timestamp type</mark> <mark style="color:red;">(important) !!!</mark>
  * ![](<../../.gitbook/assets/image (6) (2).png>)
* File Size
* Permissions (Unix only)
* User and Group (Unix only)
* Meta: Metadata address ($MFT record number for NTFS)
* File Name
  * Deleted files are appended with "(deleted)"

## Creating a Timeline

### Bodyfile Step 1: MFTECmd.exe (only for Windows)

```
C:\> MFTE.exe -f "E:\C\$MFT" --body "G:\timeline" --bodyf
```

### Bodyfile Step 1: (alternative) fls

```
// Some code
```

### Step 2: mactime

```
// Some code
```

