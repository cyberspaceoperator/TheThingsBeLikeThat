# Examining Malicious RTF Files

## What is RTF (Rich Text File)

* Supported by many systems
* Doesn't support macros, but allows attackers to embed other dangerous files as OLE1 objects and other binary contents
* RTF documents contain plaintext but also control words that start with '\\' which specify how the RTF-rendering application should format and display characters
* Groups are enclosed in { }, which can be nested
  * Groups specify the text affected by the group and its formatting
* Most often, the '\object' control word is where there will be evil/malicious things&#x20;



| Tool      | Description / Usage                                               |
| --------- | ----------------------------------------------------------------- |
| rtfdump   | rtfdump.py -O \<filename> \[-O shows object data]                 |
| xorsearch | search for a XOR, ROL, ROT, SHIFT or ADD encoded string in a file |
| scdbgc    | emulate shellcode execution                                       |
|           |                                                                   |
