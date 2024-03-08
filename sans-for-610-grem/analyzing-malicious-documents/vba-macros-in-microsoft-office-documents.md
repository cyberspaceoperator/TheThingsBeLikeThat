# VBA Macros in Microsoft Office Documents

## Macros

* They can interact with the OS to perform risky actions, download or generate files and execute programs
* Even if the document or the VBA project is password-protected, the macros are not stored in an encrypted way
* Malicious VBA macros can be obfuscated to evade detection
* Microsoft Office supports 2 different formats
  * OLE2, legacy binary format (aka structured storage)
  * XML-based format "OOXML" incorporates multiple files that include the documents contents in a ZIP file
* VBA macro code is embedded inside streams as compiles code (p-code) and compressed source code.

## Tools

### Fingerprint / Analyze a file

{% hint style="info" %}
Viewing macros inside might not be the best idea, MS Office hides some information and will enforce 'password-protected' macros. Macro passwords aren't real outside of MS Office
{% endhint %}

* **file command**&#x20;

```bash
file <filename>
```

* **trid**

{% code fullWidth="true" %}
```bash
trid particulars.doc 

TrID/32 - File Identifier v2.24 - (C) 2003-16 By M.Pontello
Definitions found:  14064
Analyzing...

Collecting data from file: particulars.doc
 53.0% (.DOCM) Word Microsoft Office Open XML Format document (with Macro) (52000/1/9)
 23.9% (.DOCX) Word Microsoft Office Open XML Format document (23500/1/4)
 17.8% (.ZIP) Open Packaging Conventions container (17500/1/4)
  4.0% (.ZIP) ZIP compressed archive (4000/1)
  1.0% (.PG/BIN) PrintFox/Pagefox bitmap (640x800) (1000/1)
remnux@remnux:~/malware/day3$ 

```
{% endcode %}

Since OOXML documents are just zip files, you can use "zipdump.py" to look at what comprises a file

* zipdump.py \<filename>

```bash
remnux@remnux:~/malware/day3$ zipdump.py particulars.doc
Index Filename                       Encrypted Timestamp           
    1 [Content_Types].xml                    0 1980-01-01 00:00:00 
    2 _rels/.rels                            0 1980-01-01 00:00:00 
    3 word/_rels/document.xml.rels           0 1980-01-01 00:00:00 
    4 word/vbaProject.bin                    0 1980-01-01 00:00:00 
    5 word/media/image1.jpeg                 0 1980-01-01 00:00:00 
    6 word/theme/theme1.xml                  0 1980-01-01 00:00:00 
    7 word/_rels/vbaProject.bin.rels         0 1980-01-01 00:00:00 
    8 word/vbaData.xml                       0 1980-01-01 00:00:00 
    9 word/settings.xml                      0 1980-01-01 00:00:00 
   10 customXml/item1.xml                    0 1980-01-01 00:00:00 
   11 customXml/itemProps1.xml               0 1980-01-01 00:00:00 
   12 word/styles.xml                        0 1980-01-01 00:00:00 
   13 word/webSettings.xml                   0 1980-01-01 00:00:00 
   14 word/fontTable.xml                     0 1980-01-01 00:00:00 
   15 docProps/app.xml                       0 1980-01-01 00:00:00 
   16 customXml/_rels/item1.xml.rels         0 1980-01-01 00:00:00 
   17 word/document.xml                      0 2020-06-24 12:06:12 
   18 docProps/core.xml                      0 2020-06-24 12:07:48 


# Extract a file within a .doc file

$ zipdump.py <filename> -s <index number> -d > <output_filname>
```

* OLEVBA - olevba is a script to parse OLE and OpenXML files such as MS Office documents (e.g. Word, Excel), to **detect VBA Macros**, extract their **source code** in clear text, and detect security-related patterns such as **auto-executable macros**, **suspicious VBA keywords** used by malware, anti-sandboxing and anti-virtualization techniques, and potential **IOCs** (IP addresses, URLs, executable filenames, etc).  [https://github.com/decalage2/oletools/wiki/olevba](https://github.com/decalage2/oletools/wiki/olevba)
* OLEVBA output the code of of macros inside a doc file and outputs interesting keywords at the bottom of the analysis
*

    <figure><img src="../../.gitbook/assets/image (2) (4).png" alt=""><figcaption><p>olevba snippet</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1) (4).png" alt=""><figcaption><p>olevbd table output</p></figcaption></figure>

* oledump - Provides even deeper visibility into VBA macros and related artifacts, examine the documents streams
  * Will automatically find and parse OLE2 files within OOXML files
  * **SRP** streams may contain cached previously executed VBA scripts that an attacker later modified

<figure><img src="../../.gitbook/assets/image (1) (4) (1).png" alt=""><figcaption><p>oledump.py</p></figcaption></figure>

* Use "-s a" paramted to oldedump.py to extra VBA macros from ALL streams in a particular document

```bash
oledump.py particulars.doc -s a -v | more
```

* re-search.py
* sets.py
* exiftool - Can look at metadata of a file (filename, directory, modified by, etc etc)

## Tool Summary

| [zipdump.py](https://videos.didierstevens.com/2014/08/14/zipdump-py/) _file.pptx_                                                                        | Examine contents of OOXML file _file.pptx_.                                                               |
| -------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| [zipdump.py](https://videos.didierstevens.com/2014/08/14/zipdump-py/) _file.pptx -s 3 -d_                                                                | Extract file with index _3_from _file.pptx_ to STDOUT.                                                    |
| [olevba](https://github.com/decalage2/oletools/wiki/olevba) _file.xlsm_                                                                                  | Locate and extract macros from _file.xlsm_.                                                               |
| [oledump.py](https://blog.didierstevens.com/programs/oledump-py/) _file.xls_ -i                                                                          | List all OLE2 streams present in _file.xls_.                                                              |
| oledump.py _file.xls_ -s _3_ -v                                                                                                                          | Extract VBA source code from stream _3_ in _file.xls_.                                                    |
| [xmldump.py](https://blog.didierstevens.com/2018/01/15/update-xmldump-py-version-0-0-2/) pretty                                                          | Format XML file supplied via STDIN for easier analysis.                                                   |
| oledump.py _file.xls_ -p plugin\_http\_heuristics                                                                                                        | Find obfuscated URLs in _file.xls_ macros.                                                                |
| [<mark style="background-color:green;">vmonkey</mark>](https://github.com/decalage2/ViperMonkey) _<mark style="background-color:green;">file.doc</mark>_ | <mark style="background-color:green;">Emulate the execution of macros in file.doc to analyze them.</mark> |
| [evilclippy](https://github.com/outflanknl/EvilClippy) -uu _file.ppt_                                                                                    | Remove the password prompt from macros in _file.ppt_.                                                     |
| [msoffcrypto-tool](https://github.com/nolze/msoffcrypto-tool) _infile.docm_ _outfile.docm -p_                                                            | Decrypt _outfile.docm_ using specified password to create _outfile.docm_.                                 |
| [pcodedmp](https://github.com/bontchev/pcodedmp) _file.doc_                                                                                              | <p>Disassemble VBA-stomped <br>p-code macro from <em>file.doc</em>.</p>                                   |
| [pcode2code](https://github.com/Big5-sec/pcode2code) file.doc                                                                                            | <p>Decompile VBA-stomped <br>p-code macro from <em>file.doc</em>.</p>                                     |
| [rtfobj.py](https://www.decalage.info/python/rtfobj) _file.rtf_                                                                                          | Extract objects embedded into RTF _file.rtf_.                                                             |
| [rtfdump.py](https://blog.didierstevens.com/2016/08/02/rtfdump-update-and-videos/) _file.rtf_                                                            | List groups and structure of RTF file _file.rtf_.                                                         |
| rtfdump.py _file.rtf_ -O                                                                                                                                 | Examine objects in RTF file _file.rtf_.                                                                   |
| rtfdump.py _file.rtf_ -s _5_ -H -d                                                                                                                       | Extract hex contents from group in RTF file _file.rtf_.                                                   |
| [xlmdeobfuscator](https://github.com/DissectMalware/XLMMacroDeobfuscator) --file _file.xlsm_                                                             | Deobfuscate XLM (Excel 4) macros in _file.xlsm_.                                                          |
| scdbgc /f \<executable> /s -1                                                                                                                            | Emulates running a file and shows the API calls being made                                                |
| 1768.py                                                                                                                                                  | Analyze Colbalt Strike beacons                                                                            |

## Examination

1. Identify the file type (OLE2 or OOXML)
   1. You may use **trid** for this
2. Identify if there are macros embedded in streams
   1. **oledump.py** should work for this
   2. olevba also works
3. If macros are present in streams, you need to extract the code from the streams
   1. using oledump.py \[filname] -s \[stream number] -v&#x20;
   2. some cleanup may be necessary

### VBA Stomping

VBA Stomping is when the source code of the VBA script is removed from the document/file, leaving only the compiled code in it's place

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

