# Definitions

### Wireless Sniffing Operating Modes

* **Master**
* **Managed** - Captures 802.3 traffic only when it is associated with the wireless network. Limited only to data packets, excludes all network management traffic and control packets.
* **Ad**-**Hoc**&#x20;
* **Monitor -** Captures 802.11 traffic from ANY network and embedded headers, data frames, management frames, control frames.



### WI-FI Physical Layer Variations

![](<../../.gitbook/assets/image (46) (1).png>)

HT40+

HT40-

HT40

Signal Strength / Outdoor Range Mapping\
&#x20;   \- Kismapping (preffered) - signal strength plotting on a google map \
&#x20;   \- Ekahau Heat Mapper (free trial)

### RF Propagation

**Antenna Types**

* Omni-directional (dipole), donut shaped propagation
* Semi-directional (Yagi, dish, etc)

### **Mac Layer** - Fragmentation support, supported data rates, encryption, separation of networks

* **IBSS** - (Ad-hoc) vs. Infrastructure networks, each peer implicitly trusts other nodes on the network when participating as the master node (each node participates as the master)
* **STA** - station (client)
* **SSID** (Service Set Identifier) - "name" of a Wi-Fi network
* **BSSID** (Basic Service Set Identification) - MAC address of AP
* **ESSID** (Extended Service Set Identifier) - Collection of APs with the same name
* **EAP** - Protocol for authentication clients via 802.1X
* **802.11 Frame Header**
  * Management - association/authentication, probing, beacons
  * Data - Data traffic
  * Control - Help with delivery of the other 2 frame types (RTS/CTS), ACK, Checksums, etc
  * Flags
    * TO DS Flag (if set) - Traffic going to wired network and AP from wireless
    * FROM DS Flag (if set) - traffic coming from wired network and AP, heading to wireless
    * Both bits (TO DS / FROM DS) cleared - Ad-Hoc
    * Both bits (TO DS/ FROM DS) set - WDS Network
    * Subtype flags
      * Authentication Request (subtype 11) - Authenticate to a WLAN
      * Association Request (subtype 0) - Join a WLAN
      * Beacon Frame (subtype 8) - advertisement of AP
      * Probe Request (subtype 4) - STA looking for known WLANs
      * Deauthentication \[_deauth_] Request (subtype 12) - Disconnect

![Frame control fields](<../../.gitbook/assets/image (56) (1).png>)

![](<../../.gitbook/assets/image (35).png>)

### **Rogue Threat Types / Rogue AP Analysis**

* Friendly - Implemented by users against company/org policy
* Malicious - Backdoor
* Unintended - Authorized but misconfigured

**Windows Bridging** - Allows users to extend their connectivity to others by bridging their connection and acting as an AP. Since Ad-hoc networks do not require security, this could be a potential way a hacker could have a backdoor into an otherwise legitimate network. Also extends DHCP, meaning somebody joining the Ad-hoc network will get their own IP.

**Windows Hosted Network** - Establishes WPA2-PSK AP using standard wireless card, automatically bridges to LAN, requires admin priv.

```
netsh wlan set hostednetwork mode=allow ssid=fuck key=pass0rd123
netsh wlan start hostednetwork
```

**Warwalking** - walk around your building, figure out if there's any rogue APs, keep a known list of MAC addresses or deploy Wireless LAN IDS

**Wireless LAN IDS** - Deploy monitoring agents around business / campus. Only effective if they are deployed everywhere

**Wireless LAN IPS** - If identifies rogue access point, it will spoof deauths continuously
