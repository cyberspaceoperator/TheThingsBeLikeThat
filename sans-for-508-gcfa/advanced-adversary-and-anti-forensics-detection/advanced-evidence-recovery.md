# Advanced Evidence Recovery

## Detecting and Overcoming Anti-Forensics



## Example of File Wiping w/ <mark style="color:blue;">SDelete</mark>

* Signed by Microsoft
* Very popular
* Wipes files, directories, and free space
* MFT Reuse via FileCreate
* <mark style="color:blue;">**Many artifacts remain, however**</mark>:
  * Uses "**DataOverwrite**" USNJrnl reason&#x20;
  * Renames file **26 times**
  * Windows Search Index "gather" logs
  * **$I30** slack
  * **Prefetch** of SDELETE.EXE may have mapped files associated with file wiping

## Other Data Wipers

### BCWipe

* Licensed product
* Effectively clears $I30 slack and MFT records
* Renames file once w/ a random name equal in size to original
* $UsnJrnl, $LogFile, and "Evidence of Execution" artifacts persist

### Eraser

* Open source
* Option to use legit filename prior to final deletion
* Erases files with random characters and symbols
* Overwrites 7 times
* $UsnJrnl, $LogFile, and "Evidence of Execution" artifacts persist

### Ciper

* Designed primarily for encryption via EFS, but also includes a feature to overwrite free space (not individual files)
* "LOLbin" sometimes used for cleanup (somewhat common with ransomware)
* Use ciper.exe /w:\<drive> to implement built-in-free-space wiping
* Creates a persistent directory named EFSTMPWP at the volume root and adds temp files  within it to fill free space

## File Recovery

### Registry Keys

* Deleted registry keys are marked as unallocated; possible to revoer
  * Keys
  * Values
  * Timestamps
* Use Eric Zimmerman's **Registry Explorer** to recover registry keys

### Metadata Method vs. Carving Method

#### Metadata method

* When a file is deleted, its metadata (MFT records) is marked unused but metadata remains
* Read metadata entries that are marked as deleted and extract the data from any clusters it points to.

#### Tool: **icat / tsk\_recover**

{% embed url="http://www.sleuthkit.org/sleuthkit/man/tsk_recover.html" %}
tsk\_recover
{% endembed %}

{% hint style="info" %}
**inode** is synonymous with MFT record, some of the tools work with both Windows and Linux
{% endhint %}

<pre class="language-bash"><code class="lang-bash"># Extract deleted files individually with icat
<strong>$ icat -r &#x3C;image> inode
</strong>
# Extract all deleted files with tsk_recover
# this is basically an icat wrapper that recursively runs icat
$ tsk_recover &#x3C;image> &#x3C;output-directory></code></pre>

#### Carving Method

* If metadata has been reused, we have to search for the file
* Use known file signatures to find the start, then extract the data to a known file footer or to a reasonable size limit
* **Files to target:**
  * Link files
  * Jumplists
  * Recycle bin
  * Prefetch
  * Binaries
  * Scripts
  * Archives
  * Offics Docs
  * CAD Drawings
  * Email stores
  * Images
  * Videos

#### Tool: PhotoRec

{% embed url="https://www.cgsecurity.org/wiki/PhotoRec_Step_By_Step" %}
photorec step-by-step
{% endembed %}

* Free file carver
* Runs on Linux, Windows, Mac
* Provides signatures for 300+ file types
* Leverages metadata from carved files
* Determines cluster size
* Determines which clusters are free and which are in use and only export the ones that aren't in use
  * We carves clusters not in use because we could just examine files in use on the system like normal, we are looking for suspicious activity

### Data Carving

#### Finding "Fileless" malware in the Registry

* Use Eric Zimmerman's **Registry Explorer** to recover registry keys
  * Helps bubble up suspicious payloads
  * Minimum / maximum value filters
  * Time-based filters
  * Regex + String searching
* Case study:
  * [https://az4n6.blogspot.com/2018/06/malicious-powershell-in-registry.html](https://az4n6.blogspot.com/2018/06/malicious-powershell-in-registry.html)

### Recovering Deleted Volume Shadow Copy Snapshots

#### Tool:  vss\_carver.py

{% embed url="http://www.kazamiya.net/en/DeletedSC" %}

**Step 1**: Use vss\_carver against the raw image

```bash
vss_carver -t RAW -i /mnt/ewf_mount/ewf1 -o 0 -c ~/vsscarve-basefile/catalog ~/vsscarve-basefile/store
```

**Step 2**: Review (and possibly reorder) recovered VSCs

```bash
vss_catalog_manipulator list ~/vsscarve_basefile/catalog
```

**Step 3**: Present recovered VSCs as raw disk images

```bash
vshadowmount -o 0 -c ~/vsscarve-basefile/catalog -s ~/vsscarve-basefile/store /mnt/ewf_file/ewf1 /mnt/vsscarve_basefile
```

**Step 4:** Mount all logical filesystems of snapshot

```bash
cd /mnt/vsscarve_basefile
for i in vss*; do mountwin $i /mnt/shadowcarve_basefile/$i; done
```

### Stream Carving for Event Log and File System Records

#### Tool: Bulk extractor
