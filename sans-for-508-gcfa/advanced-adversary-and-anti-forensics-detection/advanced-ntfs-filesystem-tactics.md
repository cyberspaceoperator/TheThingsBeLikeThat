# Advanced NTFS Filesystem Tactics

{% embed url="https://social.technet.microsoft.com/wiki/contents/articles/5375.windows-file-systems.aspx" %}

{% embed url="https://github.com/EricZimmerman/MFTECmd" %}
mftecmd
{% endembed %}

## MFT Tools

{% embed url="https://github.com/EricZimmerman/MFTECmd" %}
mftecmd
{% endembed %}

## NTFS Features

* Compression
* Journaling
* Hard / Soft Links
* Volume shadow copies
* Etc...

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption><p>ntfs features</p></figcaption></figure>

## Master File Table (MFT)

* Core **metadata** structure of the filesystem
* Stores MFT Entries (AKA MFT Records) for every file on the volume

### MFT Entries

* Generally sequential, regardless of location
* Typically 1024 bytes long (1MB)
* Every object gets an entry w/ attributes



<figure><img src="../../.gitbook/assets/image (99).png" alt=""><figcaption><p>mft record</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3) (4).png" alt=""><figcaption><p>mft unallocated or allocated</p></figcaption></figure>

### MFT Attributes

Most Common:

<figure><img src="../../.gitbook/assets/image (1) (3) (3) (1).png" alt=""><figcaption><p>common mft attributes</p></figcaption></figure>

### Analyze Filesystem Metadata w/ Sleuth Kit istat

* Open source set of tools for performing disk-based forensic analysis
* istat tool displays statistics about a given metadata structure (aka 'inode'), including MFT entries
* Supports multiple image types RAW (dd), EWF (E01), & VMDK/VHD
* Supports multiple filsystem types, NTFS, FAT, EXT4, etc

```
# istat <image name> <record number>
# istat win7-c-drive.E01 60919
```

### Analyzing $STANDARD\_INFORMATION (easily modified)

* Provides the 4 standard timestamps
  * M.A.C.B.
  * Easily modified

#### Time rules for $STANDARD\_INFORMATION

![](<../../.gitbook/assets/image (2) (1) (1).png>)

### Analyzing $FILE\_NAME (0x30) & Attribute Summary

$FILE\_NAME

* Provide file or directory attributes such as ReadOnly, Hidden, etc
* Provides name of file...duh.
* Includes 4 more timestamps

#### Time Rules for $FILE\_NAME (cannot be \[more difficult] to modify)

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption><p>Time rules for $FILE_NAME</p></figcaption></figure>

### Detecting Timestamp Manipulation

Timestomping

* Common attack to make their files hide in plain sight
* Artifacts from 'timestomping' vary based on the tool used
* Here are some anomolies to check for (use **mftecmd**):
  1. **$STANDARD\_INFORMATION** "B" time prior to $FILE\_NAME "B" time (these should match)
  2. Fractional second values are all zeros
  3. **$STANDARD\_INFORMATION** "M" time prior to ShimCache timestamp (check with AppCompatCacheParser.exe)
  4. **$STANDARD\_INFORMATION** times prior to executable's compile time
  5. **$STANDARD\_INFORMATION** times prior to $I30 slack entries
  6. MFT entry number is significantly out of sequence from the expected range

<figure><img src="../../.gitbook/assets/image (8) (3).png" alt=""><figcaption></figcaption></figure>

### Analyzing $DATA

* File data is maintained by the $DATA attribute
  * Resident: If data is \~600 bytes or less, it gets stored inside the $DATA attribute
  * Non-resident: $DATA attribute lists the clusters on disk where the data resides
* Files can have multiple $DATA streams
  * The extra, or "Alternate Data Streams" (ADS), must be named

#### Extracting Data with The Slueth Kit's "istat / icat"

<pre><code><strong># istat &#x3C;path_to_file> &#x3C;MFT Record ID>
</strong><strong># icat &#x3C;path_to_file> &#x3C;attribute ID (number for the Alternate data stream>
</strong></code></pre>

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>icat / istat output</p></figcaption></figure>

#### Zone.Identifier : Evidence of Download

{% hint style="info" %}
How does Windows recognize that some files were downloaded from the internet?

[https://textslashplain.com/2016/04/04/downloads-and-the-mark-of-the-web/](https://textslashplain.com/2016/04/04/downloads-and-the-mark-of-the-web/)
{% endhint %}

{% embed url="https://www.hecfblog.com/2018/06/daily-blog-402-solution-saturday-62318.html" %}

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption><p>zone.identifier / zones</p></figcaption></figure>

### NTFS Directory Attributes $I30

* Stored in an index named $I30
  * Contains $FILENAME and $STANDARD\_INFORMATION attributes
  * $INDEX\_ROOT - **required** (stored in MFT)
  * $INDEX\_ALLOCATION - required for larger directories (stored in clusters)
* Implemented as a B-Tree (binary tree) structure

<figure><img src="../../.gitbook/assets/image (5) (3).png" alt=""><figcaption></figcaption></figure>

### B-Tree Indexing

* Index trees need to be rebalanced periodically

{% embed url="https://en.wikipedia.org/wiki/B-tree#Origin" %}

#### Parsing $I30 Directory Indexes

* **Indx2Csv**
  * Parses out active and slack entries
  * Includes additional features for recovering partial entries

```
Indx2Csv /IndxFile:G::\cases\$I30 /outputpath:G:\cases
```

* **Velociraptor** for live file system and mounted images
* You can search 1 or more computers at a time

```
Velociraptior artifacts collect Windows.NTFS.I30 --args DirectoryGlobs="F:\\Windows\\Temp\\Perfmon\\" --format=csv
```

## File System Journaling

<figure><img src="../../.gitbook/assets/image (15) (2).png" alt=""><figcaption><p>journaling cheat sheet</p></figcaption></figure>

* Records file system **metadata changes**
* Two files track these changes
  * **$LogFile**
  * **$UsnJrnl**
* Goal: Return file system to a clean state
  * Secondary Goal: Providing hints to applications about file changes
* You can use journals to identify prior state of files, and when their state changed
  * Like VSS, they serve as a time machine, detailing past file system activities
  * Unlike VSS, the journals are rolling logs, rather than point-in-time snapshots

### $LogFile&#x20;

* Provides file-system resilience
* Stores **low-level** transactional logs for NTFS consistency
* Default size 64MB
* Typically only lasts a few hours on a live system
  * On secondary drives the journal may be there for days
* VSS can also possibly yield more journals

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>LogFile op codes</p></figcaption></figure>

### $UsnJrnl

* Provide file system change tracking service
* Stores high-level summary information about changes to files and directories
* <mark style="color:orange;">**Used by applications**</mark> to determine which files they should act upon
* Tends to last a few days
  * Secondary drives often last weeks
* Stored in large $J ADS (alternated data streams)
* VSS can also possibly yield more journals

<figure><img src="../../.gitbook/assets/image (1) (3) (3).png" alt=""><figcaption><p>USNJrnl op codes</p></figcaption></figure>

### Useful Filters and Searches in Journals

<figure><img src="../../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

### Tool: LogFilerParser.exe

{% embed url="https://github.com/jschicht/LogFileParser" %}
logfileparser link
{% endembed %}

* Outputs $LogFile into .csv format

### Tool: MFTECmd.exe

* Use this tool for paring $USnJrnl (among other things)

## What happens when a file is deleted?

### Data Layer

* Clusters will be marked as unallocated in $Bitmap, but data will be left intact until clusters are reused
* File data as well as slack space will still exist

### Metadata Layer

* A sing bit in the file's $MFT record is flipped, so all metadata will remain until record is reused
* $LogFile, $USNJrnl, and other system logs still reference file

### Filename Layer

* $FILE\_NAME attribute is preserved until MFT record is reused
* $I30 index entry in parent directory may be preserved
