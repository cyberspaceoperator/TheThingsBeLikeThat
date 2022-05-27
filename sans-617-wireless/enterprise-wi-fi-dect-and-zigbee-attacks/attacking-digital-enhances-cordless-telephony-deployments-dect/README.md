# Attacking Digital Enhances Cordless Telephony Deployments (DECT)



### DECT

* **FP** (Fixed Part) - Broadcasts presence advertising RFPI
  * Up to 12 concurrent calls
* **RFPI** - Radio Fixed Part Indicator - Unique ID
* **PP** (Portable Part)
  * Scans for RFPI
  * Selects best RSSI, desired FP capabilities
  * Responsible for negotiating encryption support
* **EMEA**: 1.88-1.9 GHz, **North America**: 1.92.-1.93 GHZ
  * 5 channels in US
  * 10 channels in europe
    * Channels are divided into 24 slot pairs for full duplex. 24/2 = 12, 12 calls per FP
  * 523 kpbs data rate per slot
* **Authentication**
  * PIN auth between FP and PP
    * many manufacturers assign fixed PINs to devices (no way to change them)
    * ex: can't pair a samsung PP to a sony FP for instance
  * FP and PP initially pair to derive User Authentication Key (UAK)
    * used for subsequent auth and encryption key derivation
* Encryption
  * 128-bit key length
  * Key changes each time a PP connects
  * Uses UAK as input
