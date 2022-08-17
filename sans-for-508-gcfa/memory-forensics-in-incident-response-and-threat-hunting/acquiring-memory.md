# Acquiring Memory



![](<../../.gitbook/assets/image (27) (2).png>)

### Hibernation file analysis

* Compressed copy of RAM at time of hibernation
* Win8/10 has new format and greater frequency

#### Tools to decompress to raw

* Volatility **imagecopy**
* Comae **hibr2bin.exe**
* Aersenal **Hibernation Recon**

#### Tools to analyze natively

* BulkExtractor
* Magnet AXIOM
* Volatility
* Passware (instructor recommended)

### Virtual Machine Memory Acquisition

* VMWare
  * .vmem(raw) , .vmss & .vmsn (memory image)
* Hyper-V
  * .bin (memory image), .vsv (save state)
* Parrallels
  * .mem = raw
* VirtualBox
  * .sav = partial memory image

