# Advanced NTFS Filesystem Tactics

{% embed url="https://social.technet.microsoft.com/wiki/contents/articles/5375.windows-file-systems.aspx" %}

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

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>mft unallocated or allocated</p></figcaption></figure>

### MFT Attributes

Most Common:

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>common mft attributes</p></figcaption></figure>

### Analyze Filesystem Metadata w/ Sleuth Kit istat

* Open source set of tools for performing disk-based forensic analysis
* istat tool displays statistics about a given metadata structure (aka 'inode'), including MFT entries
* Supports multiple image types RAW (dd), EWF (E01), & VMDK/VHD
* Supports multiple filsystem types, NTFS, FAT, EXT4, etc

```
# istat <image name> <record number>
# istat win7-c-drive.E01 60919
```

### $STANDARD\_INFORMATION

* Provides the 4 standard timestamps
  * M.A.C.B.

#### Time rules for $STANDARD\_INFORMATION

![](<../../.gitbook/assets/image (2).png>)

### Analyzing $FILE\_NAME (0x30) & Attributee Summary

$FILE\_NAME

* Provide file or directory attributes such as ReadOnly, Hidden, etc
* Provides name of file...duh.
* Includes 4 more timestamps

#### Time Rules for $FILE\_NAME

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption><p>Time rules for $FILE_NAME</p></figcaption></figure>

### Detecting Timestamp Manipulation

Timestomping

* Common attack to make their files hide in plain sight
* Artifacts from 'timestomping' vary based on the tool used
* Here are some anomolies to check for:
  1. **$STANDARD\_INFORMATION** "B" time prior to $FILE\_NAME "B" time
  2. Fractional second values are all zeros
  3. **$STANDARD\_INFORMATION** "M" time prior to ShimCache timestamp
  4. **$STANDARD\_INFORMATION** times prior to executable's compile time
  5. **$STANDARD\_INFORMATION** times prior to $I30 slack entries
  6. MFT entry number is significantly out of sequence from the expected range

