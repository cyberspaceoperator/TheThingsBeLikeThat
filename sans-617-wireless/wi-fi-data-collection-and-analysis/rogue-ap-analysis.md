# Rogue AP Analysis

### Types of Rogue Threats

* Friendly
* Malicious
* Unintended

**Windows Bridging** - Allows users to extend their connectivity to others by bridging their connection and acting as an AP. Since Ad-hoc networks do not require security, this could be a potential way a hacker could have a backdoor into an otherwise legitimate network. Also extends DHCP, meaning somebody joining the Ad-hoc network will get their own IP.

**Windows Hosted Network** - Establishes WPA2-PSK AP using standard wireless card, automatically bridges to LAN, requires admin priv.

```
netsh wlan set hostednetwork mode=allow ssid=fuck key=pass0rd123
netsh wlan start hostednetwork
```

### AP Fingerprinting

```
nmap -sS -O --open --script=rogueap.nse x.x.x.x-x.x.x.x //wired-side analysis
```

Warwalk with Kismet or inSSIDer, filter out MAC addresses with a list of known MACs provided by customer

```
//Kismet Filters (exmaples)
filter_tracker=BSSID(!00:00:00:00:00:00, !00:00:00:00:00:01)
filter_tracker=SRC(!00:00:00:00:00:00, !00:00:00:00:00:01)
filter_tracker=ANY(!00:00:00:00:00:00, !00:00:00:00:00:01)
filter_tracker=ANY(00:00:00:00:00:00, 00:00:00:00:00:01)
```

### **Rogue countermeasures**

**Wireless LAN IDS** - Deploy monitoring agents around business / campus. Only effective if they are deployed everywhere

**Wireless LAN IPS** - If identifies rogue access point, it will spoof deauths continuously

### Locating Rogues&#x20;

* Manual locating with SNR (Signal-to-Noise Ratio)
* nmap rogueap.nse
* Kismet has dB meter, maybe use kismapping
* Netscout/Netally Aircheck G2 (\$$\$$)
* If CDP enabled (Cisco Proprietary)&#x20;
* triangulation
* signal analysis
* traffic analysis

