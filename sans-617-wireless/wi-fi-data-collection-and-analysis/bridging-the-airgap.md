# Bridging the Airgap

netsh wlan show interface

netsh wlan show profiles (shows configured networks)

netsh wlan export profile \<profilename>

netsh wlan show networks - enumerate avilable networks in the area

netmon - Windows wireless captures (captures not compatible with Wireshark)

nmcap (netmon capture) /Network "Wireless network" /Capture Wifi /File \<filename output>

nm2lp - Converts netmon capture to .pcap

* Output works with ettercap, wireshark, asleap, aircrack-ng, etc



```
// Create new connection
netsh wlan connect name="profilename"
```

MacOS

airport -s (scans networks nearby)

airport -I (shows wifi interface details)

airport en0 sniff 1 (requires root)

### Keychain attack (macOS)

* macOS stores passwords in the user's keychain /library/keychains/\<keychainname>
  * Encrypted with user's passphrase
  * file accessible to any user
* take file, copy to kali
  * keychain2john (converts keychains into john compatible format)
  * dictionary attack
* networksetup -listpreferredwirelessnetworks en0
* sudo networksetup -addprefferedwirelessnetworkatindex en0 linksys 0 WPA2 \<password> (add network in position 0 (highest priority))
* sudo networksetup -removeprefferedwirelessnetwork en0 linksys
* networksetup -setairportnetwork en0 linksys password

![](<../../.gitbook/assets/image (83).png>)

