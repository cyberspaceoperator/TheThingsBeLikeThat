# RFID Tracking and Privacy Attacks

### Frequencies

* 868/900 MHz & 4.9/5 GHz variations most common worldwide
* 2.4 GHz, 5 GHz ISM 1 - 200M read range
* < 1GHz UHF, 1-12M or 300M read range
* 433 MHz UGH, 1 - 100 M read range
* 13.56 MHz (1/- 7kHz) HF, 10 cm - 1M read range
* 120 - 134.2, 140-148.5 kHz LF, 10cm read range

### Tag Types

* **Active RFID**
  * Both the transponder and receiver have active power sources like a batter or plugged in
    * Greater range and flexibility
      * Transponder or receiver can initiate an exchange
  * EZ-Pass (915 MHz)
  * AeroScout 2.4 GHz Wi-fi active tag
* **Passive RFID**
  * RFID receivers are powered through batter or persistent power
  * RFID transponders are passive devices
    * take power from the receiver
    * known as inductive or capacitive coupling

**PCD** (Proximity Coupling Device) - device that generates the electromagnetic field for the card

**PICC** ( Proximity integrated circuit Card ) - The card.



### Attack Surface

iBeacons

* Use BLE tools to observe iBeacons on the advertising channels
* ./ibeacon\_scan.sh
  * uses hcitool, hcidump, and sed
* Create an ibeacon

```
btmgmt le on
sudo hciconfig hci0 up
suo hciconfig hci0 leadv
// DON'T ACTUALLY INCLUDE BRACKETS BELOW
sudo hcitool -i hci0 cmd 0x08 1E 02 01 1A 1A FF 4C 00 02 15 [ 42 6C 75 65 43 48 41 72 6D 42 65 61 63 6F 6E 73 ] [ 38 38 ] [ 49 49 ] C5 00
```

