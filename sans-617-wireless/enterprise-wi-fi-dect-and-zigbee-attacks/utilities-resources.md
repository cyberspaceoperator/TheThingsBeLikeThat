# Utilities / Resources

**Cowpatty** - Audits WPA2-PSK for weak PSK

* Slow/time consuming
* Dictionary based

![](<../../.gitbook/assets/image (36).png>)

**aircrack-ng** (wpa/wpa2 psk dictionary attack)

```
aircrack-ng -w <wordlist> <pcap filename>
```

**Hashcat - You don't know what hashcat is? Jeez.**

**Reaver** - online PIN guessing attack, exploiting WPS deployments.



Pre-shared key dump (WINDOWS)

```
netsh wlan show profiles name='Profile Name' key=clear
```

### Decrypt WPA2 traffic dump

* Capture needs to include 4-way handshake
* You must also know the:
  * pre-shared key
  * SSID
  * this is because the PTK must be generated since it is different for each client

```
airdecap-ng -p <pre-shared key> -e <SSID> <packetdump>.dump
```

### Wireshark Filters

WPS&#x20;

* Is the wifi access point configured to support WPS?&#x20;
  * **wps.wifi\_protected\_setup\_state eq 0x02** (doesn't necessarily mean WPS is on if this one returns results)

**beacongraph**

{% embed url="https://github.com/daddycocoaman/BeaconGraph" %}

**airograph-ng** (this tool is old, use beacongraph if able)

**sniffair** - Password spraying against PEAP

* [https://github.com/Tylous/SniffAir](https://github.com/Tylous/SniffAir)

#### HOSTAPD-WPE

![](<../../.gitbook/assets/image (86).png>)

{% embed url="https://github.com/aircrack-ng/aircrack-ng/tree/master/patches/wpe/hostapd-wpe" %}

### DECT

{% embed url="https://dedected.org" %}

{% embed url="https://github.com/znuh/re-DECTed" %}

{% embed url="https://github.com/pavelyazev/gr-dect2" %}

{% embed url="https://github.com/signalseverywhere/gr-dect2" %}

### Zigbee

KillerBee : [https://github.com/riverloopsec/killerbee](https://github.com/riverloopsec/killerbee)

