# Definitions

Hashing Mechanisms

* **MD5** - One input, one output, cannot be reversed
* **HMAC** - Uses secret key/password combined with input data (RFC 2104)

WPA - Fix for WEP, relied on TKIP

RC4 - Steam cipher

AES - Symmetric Block Cipher

WPA2 - Introduced CCMP, retained TKIP

WPA/WPA2 PSK very similar authentication

TKIP and CCMP accomodate personal or enterprise-based authentication

* Personal: PSK
* Enterprise: 802.1X

CCMP - AES Counter Mode Cipher Block Chaining Message Authenticity Check Protocol

PSK - Pre-shared key

**PMK** - Pairwise Master Key, derived from PSK or EAP method

* 256 bits / 64 hex characters
* PMK is derived using passphrase, SSID, SSID Length information
* Hashed 4096 times using HMAC-SHA1

PMKID is the unique key identifier used by the AP to keep track of the PMK being used for the client. PMKID is a derivative of AP MAC, Client MAC, PMK and PMK Name.

You could express it as this code:

```
PMKID = HMAC-SHA1-128(PMK, "PMK Name" | MAC_AP | MAC_STA)
```

PMK caching is used to establish smooth roaming for time sensitive applications. Using PMKID caching the clients do not have to go through the entire authentication cycle and cuts down on the time needed for the client to authenticate to the new AP. This is especially important on WPA/WPA2-Enterprise networks that have authentication times of almost 1 second depending on authentication type being used.

**How is it different from the actual PMK**

The PMK itself is a hash that can be expressed as:

```
PMK = PBKDF2(Passphrase, SSID, 4096)
```

**PTK** - Pairwise Transient Key

* Combines MAC of STA and AP with STA nonce and AP nonce
* Updated nonces generate fresh key material
* Uses PMK as key and generates a 384/512 bit output PRF hash using SHA1
* Temporal Key
* Two MIC Keys (RX & TX)
* EAPOL Key Encryption Key
* EAPOL Key Confirmation Key



![WPA2 PTK Derivation](<../../.gitbook/assets/image (91) (1).png>)

### WPA3

* Simulltaneous authenthication of equials (SAE) + 4-way handshake
* Perfect Forward Secrecy (PFS)
* SPD replaced with Device Provisioning Protocol (DPP)
* Unauthenticated encryption w/ Opportunistic Wireless Encryption (OWE)
* Increased key sizes
  * AES-GCM 256-bit
  * ECC w/ 384 bit curves
  * SHA384
  * 3072-bit RSA
* **Attacks**
  * Dragonblood ([https://papers.mathyvanhoef.com/dragonblood.pdf](https://papers.mathyvanhoef.com/dragonblood.pdf))
  * OWE impersonation ([https://posts.specterops.io/war-never-changes-attacks-against-wpa3s-enhanced-open-part-1-how-we-got-here-71f5a80e3be7](https://posts.specterops.io/war-never-changes-attacks-against-wpa3s-enhanced-open-part-1-how-we-got-here-71f5a80e3be7))

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
