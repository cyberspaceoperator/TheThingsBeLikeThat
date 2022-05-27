# Resources / Utilities

### Bluetooth Tools

**BTCrack** - Offline pin guessing attack

* &#x20;[https://github.com/ifoundthetao/btcrack](https://github.com/ifoundthetao/btcrack)
* [https://blog.zoller.lu/2009/02/btcrack-11-final-version-fpga-support.html](https://blog.zoller.lu/2009/02/btcrack-11-final-version-fpga-support.html)

HCITOOL - interact with HCI layer to request information from BT devices

```
hcitool name <mac>
hcitool info <mac>
```

BLESuite - [https://github.com/nccgroup/BLESuite](https://github.com/nccgroup/BLESuite)

BLE-Replay (part of BLESuite)

HCITool -&#x20;

NRF - [https://www.adafruit.com/product/4076](https://www.adafruit.com/product/4076)

Ubertooth One

* [https://www.attify-store.com/products/ubertooth-one](https://www.attify-store.com/products/ubertooth-one)

Bettercap - [https://www.bettercap.org/](https://www.bettercap.org/)

Bleee (for apple devices \[[https://github.com/hexway/apple\_bleee](https://github.com/hexway/apple\_bleee)])&#x20;

* Look at iPhone state \[locked screen?]
* Wi-Fi password sharing between devices is available within the apple ecosystem
* Possible to precompute the SHA256 hashes of all phone #'s for a region
* Spoofing airpods?

Sweyntooth

* BLE Fuzzer, requires [nRF52840](https://www.nordicsemi.com/?sc\_itemid=%7BCDCCA013-FE4C-4655-B20C-1557AB6568C9%7D)
* [https://asset-group.github.io/disclosures/sweyntooth/](https://asset-group.github.io/disclosures/sweyntooth/)
* [https://asset-group.github.io/disclosures/sweyntooth/sweyntooth.pdf](https://asset-group.github.io/disclosures/sweyntooth/sweyntooth.pdf)
* [https://github.com/Matheus-Garbelini/sweyntooth\_bluetooth\_low\_energy\_attacks](https://github.com/Matheus-Garbelini/sweyntooth\_bluetooth\_low\_energy\_attacks)

BtleJuice - [https://github.com/DigitalSecurity/btlejuice](https://github.com/DigitalSecurity/btlejuice)

* BTLE man-in-the-middle

Bluetooth Stack Smasher (BSS) - stack fuzzer

```
// Some code
```

### SDR Tools

VaporTrail ([https://github.com/inguardians/VaporTrail](https://github.com/inguardians/VaporTrail))

* Data exfil via FM radio
* Uses Pi as a transmitter

HackRF

BladeRF

RTD-SDR

* 8+ varieties, **RX only**

Ettus SDR (**expensive as fuck**)

AirSpy

* RX only
* 24 MHz - 1.8 GHz, fast scanning

LimeSDR (and mini)

* RX/TX, $300, better specs than Ettus B210
* Full-Duplex, 10 kHz-3.6GHz
* 4 radios

![](<../../.gitbook/assets/image (79).png>)

### SDR Visualization Tools

**Retrogram-rtlsdr** (very simple tool)

* FFT display only
* Lo-fi
* no demodulation, plugins or audio output
* keybord only

**GR-FOSPHOR**

* Hi-Fi
* No demodulation
* Slider control
* standalone or GNURadio block

**GQRX**

* Multiplatform, open source
* linux: apt-get install gqrx
* windows: PothosSDR bundle ([https://github.com/pothosware/PothosSDR/wiki/GQRX](https://github.com/pothosware/PothosSDR/wiki/GQRX))
* extensible (plugins)
* recording of samples
* network remote control and UDP streaming

rtl\_power + heatmap.py

* excellent for scanning large portions of spectrum over time
* converts rtl-power CSV to image
* can be run for a very long time vs other tools (like GQRX)
*   linux install:

    * apt update
    * apt install rtl-sdr


* Usage:
  * rtlpower -f 88M:108M:125K -i 5m fm\_stations.csv \[example]

STINGRAY Detector via RTL-SDR

* Locate stingray devices
* Cell  towers are fixed structures with fixed transmission powers (normally), stingrays are not
* use custom **Kalibrate** tool&#x20;

### Resources

{% embed url="http://www.rtl-sdr.com" %}

LISTEN TO IRIDIUM satellite phones?! wtf?

rtl\_433 ([https://github.com/merbanan/rtl\_433](https://github.com/merbanan/rtl\_433))

* rtl\_433 (despite the name) is a generic data receiver, mainly for the 433.92 MHz, 868 MHz (SRD), 315 MHz, 345 MHz, and 915 MHz ISM bands.
* Fingerprints known transmissions
  * 100+ known devices
  * automatic, live demodulation and decode
* GNU Radio
  * create flows/blocks, etc



