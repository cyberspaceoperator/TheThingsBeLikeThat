# Advanced Evidence of Execution

## Windows Prefetch

Prefetch - Operating system loads key pieces of data and code from disk into memory before it is needed. Windows 7 and before, limited to 127 files. Post Windows 8 = 1024 files

Windows 8+ embeds the **last eight** execution times in .pf file

Last time of execution store _inside_ the .pf file as well

Modification date of .pf file (\~ -10 seconds)

Hex at the end of prefetch name is a hash of the file's path and any command-line arguments

#### To audit or disable prefetch

**To disable prefetch**

*   Update the **EnablePrefetcher** registry key in your run-time image:

    Key: **HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PrefetchParameters**\
    Name: **EnablePrefetcher**\
    Type: **REG\_DWORD**\
    Value: **0**

The **EnablePrefetcher** key has the following values:

0 = Disabled

1 = Application launch prefetching enabled

2 = Boot prefetching enabled

3 = Application launch and boot enabled

To disable Prefetch, set the value to 0.

### Prefetch file analysis

\-When parsing an entire directory of prefect files, PEcmd will output 2 files, one will contain embedded info such as run count, last run times, etc The second folder is a timeline view of the output (see screenshot)

{% embed url="https://github.com/EricZimmerman/PECmd" %}
PECmd.exe
{% endembed %}

```
 PECmd version 1.4.0.0

 Author: Eric Zimmerman (saericzimmerman@gmail.com)
 https://github.com/EricZimmerman/PECmd
 
         d               Directory to recursively process. Either this or -f is required
         f               File to process. Either this or -d is required
         k               Comma separated list of keywords to highlight in output. By default, 'temp' and 'tmp' are highlighted. Any additional keywords will be added to these.
         o               When specified, save prefetch file bytes to the given path. Useful to look at decompressed Win10 files
         q               Do not dump full details about each file processed. Speeds up processing when using --json or --csv. Default is FALSE
 
         json            Directory to save json representation to.
         jsonf           File name to save JSON formatted results to. When present, overrides default name
         csv             Directory to save CSV results to. Be sure to include the full path in double quotes
         csvf            File name to save CSV formatted results to. When present, overrides default name
         html            Directory to save xhtml formatted results to. Be sure to include the full path in double quotes
         dt              The custom date/time format to use when displaying timestamps. See https://goo.gl/CNVq0k for options. Default is: yyyy-MM-dd HH:mm:ss
         mp              When true, display higher precision for timestamps. Default is FALSE
 
         vss             Process all Volume Shadow Copies that exist on drive specified by -f or -d . Default is FALSE
         dedupe          Deduplicate -f or -d & VSCs based on SHA-1. First file found wins. Default is TRUE

         debug           Show debug information during processing
         trace           Show trace information during processing
 
 Examples: PECmd.exe -f "C:\Temp\CALC.EXE-3FBEF7FD.pf"
           PECmd.exe -f "C:\Temp\CALC.EXE-3FBEF7FD.pf" --json "D:\jsonOutput" --jsonpretty
           PECmd.exe -d "C:\Temp" -k "system32, fonts"
           PECmd.exe -d "C:\Temp" --csv "c:\temp" --csvf foo.csv --json c:\temp\json
           PECmd.exe -d "C:\Windows\Prefetch"

           Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
```

![PECmd screenshot](<../../../.gitbook/assets/image (65).png>)

## Application Compatibility&#x20;

### ShimCache

Shimcache - Designed to detect and remediate program compatibility issues when a program launches. A program may have been built to be used on an older version of windows, so Microsoft employs a subsystem allowing a program to invoke properties of different operating system versions. The different compatibility modes are called 'shims'

* The Last Modified Time reported in ShimCache is a file system time and date stamp. It is the LAST MODIFICATION time of the executable file.
* No execution time
* Shimcache does not track execution time for Win7+ systems.

{% hint style="warning" %}
Shimcache does exhibit some interesting behavior: if the executable is modified (content changes) or renamed, it will be shimmed again, and a new entry will be created for the same file. Thus a rename pattern in this artifact looks like multiple entries with the exact same last modification date, but different names
{% endhint %}

{% embed url="https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/dd837644(v=ws.10)?redirectedfrom=MSDN" %}

{% embed url="https://web.archive.org/web/20191115211402/http://www.alex-ionescu.com/?p=39" %}

[https://web.archive.org/web/20190209113245/https://www.fireeye.com/content/dam/fireeye-www/services/freeware/shimcache-whitepaper.pdf](https://web.archive.org/web/20190209113245/https://www.fireeye.com/content/dam/fireeye-www/services/freeware/shimcache-whitepaper.pdf)

![](<../../../.gitbook/assets/image (44) (1).png>)

#### **AppCompatCache** - used to track application execution including name, full path, and last mod time.&#x20;

*   Properties:

    * Windows 7+ -SYSTEM\CurrentControlSet\Control\SessionManager\AppCompatCache\AppCompatCache
    * Windows XP - SYSTEM\CurrentControlSet\Control\SessionManager\AppCompatibility\AppCompatCache



### **AppCompatCacheParser**

{% embed url="https://github.com/EricZimmerman/AppCompatCacheParser" %}

```
AppCompatCache Parser version 1.4.4.0

Author: Eric Zimmerman (saericzimmerman@gmail.com)
https://github.com/EricZimmerman/AppCompatCacheParser

        c               The ControlSet to parse. Default is to extract all control sets.
        f               Full path to SYSTEM hive to process. If this option is not specified, the live Registry will be used
        t               Sorts last modified timestamps in descending order

        csv             Directory to save CSV formatted results to. Required
        csvf            File name to save CSV formatted results to. When present, overrides default name

        debug           Debug mode
        dt              The custom date/time format to use when displaying timestamps. See https://goo.gl/CNVq0k for options. Default is: yyyy-MM-dd HH:mm:ss
        nl              When true, ignore transaction log files for dirty hives. Default is FALSE

Examples: AppCompatCacheParser.exe --csv c:\temp -t -c 2
          AppCompatCacheParser.exe --csv c:\temp --csvf results.csv

          Short options (single letter) are prefixed with a single dash. Long commands are prefixed with two dashes
```

### Amcache.hve

* **Tracks installed applications, loaded drivers, and unassociated executables**
* Has its own hive C:\Windows\AppCompat\Programs\Amcache.hve
* Entries can be due to automated file discovery or program installation and do NOT always indicate program execution
* Full path, file size, file mod time, compilation time, publisher metadata
* **Provides SHA1 hash**

#### Auditing amcache.hve

* InventoryApplicationFile key
  * Contains subkeys named per application, providing easy means to ident executables of interest

![](<../../../.gitbook/assets/image (38) (1).png>)

![](<../../../.gitbook/assets/image (26) (1).png>)

#### Amcacheparser.exe

* Most important output files from AmcacheParser (see 2nd screenshot below)
  * Amcache\_UnassociatedEntries
  * Amcache\_DriverBinaries
  * Amcache\_ProgramEntries

{% embed url="https://ericzimmerman.github.io/#!index.md" %}

![](<../../../.gitbook/assets/image (51) (1) (1).png>)

![amcacheparser.exe output](<../../../.gitbook/assets/image (25) (2).png>)

## Automating Application Execution Analysis

* Seach across the enterprise for:
  * Malware 101: One/two letter executables, executions from temp of $Recycle.Bin folders
  * Tools: **psexesvc.exe, wmic.exe, scrons.exe, certutil.exe, rar.exe, wsmprochost.exe, whoami.exe**
  * Indicators of Compromise: Known malware, tool, staging directories

**appcompatprocessor.py** - Designed to perform scalable hunting of ShimCache and Amcache artifacts

* Capable of ingesting data in many different formats
* Easy merging of ShimCache and Amcache artifacts

{% embed url="https://github.com/mbevilacqua/appcompatprocessor" %}
appcompatprocessor.py link
{% endembed %}

