---
description: >-
  This is to gather certificate information to be used in the "Impersonating
  PEAP networks" section
---

# Identifying Vulnerable Clients

### Exercise: Identifying Vulnerable Clients <a href="#exercise-identifying-vulnerable-clients" id="exercise-identifying-vulnerable-clients"></a>

Complete the exercises in this lab to reinforce the material covered in the _Attacking WPA2-Enterprise Networks_ module. To complete these exercises, you will need the Kali Linux VM supplied on the SEC617 USB drive.

**Purpose:** This lab provides hands-on practice in evaluating a wireless packet capture to identify client devices probing for open networks that are vulnerable to network impersonation attacks.

**Description:** In this lab, you will target a customer network called corpmobile. The corpmobile network uses WPA2-Enterprise authentication, and so far, you haven’t had much luck in identifying vulnerabilities to exploit in the infrastructure.

Your goal in this assessment is to evaluate a supplied packet capture, called `client-probe-mapping.pcap`, in the Linux VM. You will identify client probe request activity in the packet capture, revealing the preferred network list (PNL) of client devices. From this PNL information, you will identify the client devices that have the corpmobile network in their PNLs.

After identifying the client devices configured to connect to the corpmobile network, you will identify the other SSIDs that the clients probe for, identifying a commonly open SSID that you could impersonate to lure the victim to your malicious network.

#### Convert the Pcap File to CSV <a href="#convert-the-pcap-file-to-csv" id="convert-the-pcap-file-to-csv"></a>

The Airgraph-ng utility cannot read from a libpcap packet capture file. Airgraph-ng expects the input data to be in a CSV file, the output of Airodump-ng.

Fortunately, Airodump-ng can read from a libpcap file, making it possible to capture the wireless traffic in Kismet, Wireshark, tcpdump, or any other libpcap-compatible tool and then produce the CSV file output needed for Airgraph-ng.

From the terminal, change to the `/root/sec617/airgraph-ng` directory. Process the contents of the `client-probe-mapping.pcap` packet capture file with Airodump-ng, as shown.

```
# cd /root/sec617/airgraph-ng
# airodump-ng -r client-probe-mapping.pcap -w CUST
```

After several seconds, Airodump-ng will complete processing the libpcap data, displaying the message `Finished reading input file . . .`, as shown below. Press **CTRL+C** to exit Airodump-ng.

#### Produce the Client Probe Graph <a href="#produce-the-client-probe-graph" id="produce-the-client-probe-graph"></a>

Airodump-ng will produce a CSV file called `CUST-01.csv` (matching the file name prefix specified with the `-w` argument, followed by a sequential number). This file is the input to the Airgraph-ng tool to produce the client probe graph.

From the terminal, run Airgraph-ng, using the `CUST-01.csv` file, producing an output PNG file, as shown.

```
# airgraph-ng -i CUST-01.csv -o cpg.png -g CPG

**** WARNING Images can be large, up to 12 Feet by 12 Feet****
Creating your Graph using, CUST-01.csv and writing to, cpg.png
Depending on your system this can take a bit. Please standby......
```

#### Examine the Client Probe Graph <a href="#examine-the-client-probe-graph" id="examine-the-client-probe-graph"></a>

Open the client probe graph image using the `display` utility, as shown below. Use the scroll window to move around the image. Identify the clients probing for the corpmobile SSID. Identify the commonly weak network SSID that one of these clients is also searching for.

```
# display cpg.png
```

#### (Optional) Evaluate the Local Client Preferred Networks <a href="#28optional-29-evaluate-the-local-client-preferred-networks" id="28optional-29-evaluate-the-local-client-preferred-networks"></a>

If you have time remaining in the exercise, start a live packet capture with Kismet or tcpdump for several minutes. Choose a Wi-Fi channel that has one or more active APs.

After capturing data for several minutes, stop the capture process, and evaluate the new capture file with Airodump-ng and Airgraph-ng. Evaluate the client probe graph data.

**Question:** What kind of associations can you identify among wireless clients?

**Question:** What commonly open SSIDs do you see disclosed by client devices?
