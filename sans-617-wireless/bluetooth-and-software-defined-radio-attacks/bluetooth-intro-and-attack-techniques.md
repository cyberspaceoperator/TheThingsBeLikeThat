# Bluetooth Intro and Attack Techniques

Developed, tested, certified by Bluetooth Special Interest Group (SIG)

Developed as a 'cable replacement' technology



Bluetooth evolution: Classic -> AMP -> Low Energy

### Classic

* 2.1 Mbps (EDR \[enhanced data rate])
* 2.4 GHz (FHSS)
  * High degree of interference immunity
  * Adaptive Frequency Hopping intelligently eliminates noisy channels
* 79 channels
* Hops 1600x per second
* Hopping pattern based on Bluetooth device address (BD\_ADDR)
* Bluetooth address: 802-compliant, 48-bit address
  * Used as a secret in Bluetooth
  * LAP (Lower Address Portion) - last 3 bytes allocated to the device BY the vendor
  * UAP (Upper Address Portion) - Last byte of the OUI allocated to the vendor
  * NAP (Non-significant address portion)- First 2 bytes of the OUI allocated to the vendor
  * ![](<../../.gitbook/assets/image (78).png>)
* Security Modes
  * 1 - Node never initiates any security procedures (no security)
  * 2 - No link encryption, application-level security options
  * 3 - Link encryption/authentication before any data is exchanged
    * Authentication
      * Completed when devices first pair
      * User security: PIN Selection
      * PIN is mixed with BD-ADDR to generate 128-bit key (referred to as 'link key')
        * key is not transmitted in the air in the clear
        * protected with challenge/response protocol based on SAFER+ cipher
    * Encryption
      * E0

### AMP (Alternate MAC and PHY)

* Bluetooth 3.0
* Not widely used
* Leverages alternate wireless connection
* Observed 802.11 AP with the SSID AMP-BSSID
* Sets up alternate wireless connection for larger data transfers with temporary Access point
  * Continues to use Bluetooth Classic FHSS for other data

### Low Energy (BLE)

* Bluetooth 4.0
* Used for 'bursty' traffic like for watches, small devices, etc

### Protocol stack

![](<../../.gitbook/assets/image (30).png>)

#### LMP (Link Management Procol)

![](<../../.gitbook/assets/image (90).png>)

### Joining a piconet

&#x20;\- When 2 devices want to connect, a piconet is formed

![](<../../.gitbook/assets/image (26) (1).png>)

### Pairing authentication attack

* Evesfrop on pairing reaveals IN_RAND, COMB_KEYs, AU\_RAND, and SRES
* Attacker can mount offline PIN attack w/ **BTCRACK (**[**https://github.com/ifoundthetao/btcrack**](https://github.com/ifoundthetao/btcrack)**)**
  * Guess PIN, calculate LK\__RAND\_ab, K\_Init, link key, etc_
