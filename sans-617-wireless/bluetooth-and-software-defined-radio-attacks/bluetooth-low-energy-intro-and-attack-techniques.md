# Bluetooth Low Energy Intro and Attack Techniques

* Low-Power / Five-year coin cell support
* Increased range w/ GFSK (gaussian frequency shift keying)
* &#x20;Inexpensive radio
  * $1 to implement (goal)
* Uses GATT/ATT protocol to exchange data

### Channels

#### Channel-Hopping

* 37 channels for data
* 3 advertising channels
* 40 overall
* Next hop, mod 37, always 5-16 from previous

#### Pairing

* Each pairing derives keys, just like Classic
* Keys are not retained between connections, negotiated each time
* 'Just Works', most devices uses fixed-PINs of 000000

### Profiles

### Attacks

#### Man in the middle&#x20;

* [https://github.com/DigitalSecurity/btlejuice](https://github.com/DigitalSecurity/btlejuice)

#### Pin/Passcode Cracking

#### HCI Snooping

