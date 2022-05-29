# Low-Frequency RFID Attacks

Uses:

* Asset tracking, antitheft, point-of-sale
* proximity door lock systems
* Simple unique code mostly (unlike HF)

**Q5 tag** (LF RFID tag) \[ read/write ]&#x20;

* \~$10
* Can be used for cloning LF tags

**HID Proxcard II** (125 kHz) \[represents about 70-80% of the physicall access control deployments in the U.S.

* Popular
* Exterior doors to buildings
* Interior doors to secure facilities
* Parking garages, gates locations, etc
* Architecture
* &#x20;Not writable
  * Purchase t5557 cards on ebay/amazon, though or use a **ProxMarkIII**

![](<../../.gitbook/assets/image (25) (1).png>)

&#x20;Sequentially numbered LF cards

* Card #1 may be janitor, Card #2 admin, and so on...
* ProxBrute for **ProxMarkIII \[**customer firmware] [https://github.com/exploitagency/github-proxmark3-standalone-lf-emulator](https://github.com/exploitagency/github-proxmark3-standalone-lf-emulator)
  * Reads a HID card in standalone mode with battery power
  * Decrements card ID until stopped by user or card ID is zero
  * One card attempt per second
  * Hopefully this gets you in a door.
