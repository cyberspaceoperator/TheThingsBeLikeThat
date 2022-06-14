# Attacking Zigbee Deployments

* Zigbee - Low cost, Low-Power, low-data rate wireless protocol
  * Built on IEEE 802.15.4
  * 250 Kbps
  * Small, lightweight stack (120 KB)
  * Star/mesh topology
  * 5-year batter goal
  * 10-100 meter range
  * 2.4GGz band, also at 900 MHz (US), 850MHz (europe)
  * DSSS Modulation
    * 16 channels at 5MHz, 11=2.405 GHz, 26=2.480 GHz
  * 127byte max frame size



### MAC Layer Frame types

* Beacon
* Data
* Command
* ACK
*

![](<../../.gitbook/assets/image (68).png>)

![](<../../.gitbook/assets/image (51) (1) (1).png>)

### Network Layer

* 16-bit / 64-bit addresses
* All zeroes address indicated coordinator
* Address given to devices when they join the network by coordinator

![](<../../.gitbook/assets/image (33) (1).png>)

### CMM Protocol

![](<../../.gitbook/assets/image (81) (1).png>)

### Security Modes

* Standard Security Model
  * Any FFD can authenticate
* High Security Model
  * (Commercial Mode)
  * Only TC (Trust center) can authenticate
  * Key center responsible for key negotiation and storage of all keys (TC)

### Key provisioning

* SKKE (Symmetric-Key Key Establishment)

### Authentication

* Zigbee 2006 - No mutual authentication of devices
* Zigbee 2007 - TC authenticates through challenge/response
* 802.15.4 includes ACL validation (MAC address filtering)

### Attacking

* **Killerbee** [https://github.com/riverloopsec/killerbee](https://github.com/riverloopsec/killerbee)
* Sniffing and injecting traffic, packet decoding / manipulation
* Pure python
* BSD License
* AVR RZ Raven USB
  * needs to be flashed with killerbee firmware
  * send to instructor to flash
* APIMote v4, available at Attify store & riverloop

![](<../../.gitbook/assets/image (67).png>)
