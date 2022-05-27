# Attacking DECT Wireless Exercise

### SEC617 Lab 3.3 <a href="#sec617-lab-3.3" id="sec617-lab-3.3"></a>

### Exercise: Attacking DECT Wireless <a href="#exercise-attacking-dect-wireless" id="exercise-attacking-dect-wireless"></a>

Complete the exercises in this lab to reinforce the material covered in the _Attacking Digital Enhanced Cordless Telephony Deployments_ module. To complete these exercises, you will need the SEC617 course USB drive.

**Purpose:** Gain skill in evaluating the structure of DECT traffic and leveraging commonly available attack tools to extract audio conversations from unencrypted DECT cordless phones.

**Description:** In this lab, we’ll leverage supplied packet captures to examine DECT packet captures, extracting audio traffic for an unencrypted DECT handset into a WAV file.

Sadly, DECT cards that are supported by the deDECTed tools are short in supply. As such, we are not able to supply them as a component of the SWAT Kit. As a next-best option, we have supplied some static DECT packet captures to use in this lab exercise to provide hands-on experience with the post-processing tools for analysis and audio extraction.

#### Inspecting DECT Captures <a href="#inspecting-dect-captures" id="inspecting-dect-captures"></a>

The DECT protocol uses a structured header format similar to that of other MAC-layer protocols. Let’s examine some of these fields in a sample packet capture file.

Open a terminal window and change to the `/root/sec617/dect` directory. Use Wireshark to display the contents of the capture file, as shown here.

```
# cd /root/sec617/dect
# wireshark -r dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap 
```

Wireshark will display the contents of the packet capture file. The deDECTed tools that capture DECT traffic do not use a native DECT packet capture type; instead, they use a fake Ethernet header and embedded protocol to tell Wireshark to expect DECT traffic as the payload protocol. As a result, we can ignore all the Ethernet header content for DECT packet captures.

In the packet dissector view, expand the DECT Protocol tree, as shown here.

Many of the DECT fields are not particularly interesting to us as analysts; however, we can observe from this packet capture several important details:

* The DECT operating channel number (27).
* The DECT slot number (6).
* The RSSI measurement (the RSSI measurement from the first frame appears anomalous).

Scrolling down further into the packet capture, we can see the A-Field and B-Field data. The A-Field data represents management overhead for the DECT protocol, while the B-Field data represents packet payload content. Expand the A-Field tree, and then expand the A-Field header and FP-Tail trees.

The A-Field header frame type field (TA) indicates that this packet is an identity informational packet. As a result, the subsequent FP-Tail data discloses the Radio Fixed Part Identifier (RFPI) information for this base station as 02:04:e7:4e:60, specifically noted in packet #2.

When locating unauthorized wireless devices, we often rely on collecting signal strength information over time. Unfortunately, there are no commercially or freely available tools to graph the signal strength of a DECT target, but we can leverage Wireshark’s IO Graphs feature to visualize the signal strength distribution over time.

With the packet capture file open in Wireshark, select **Statistics | IO Graphs**. Wireshark will display the IO Graphs window, as shown in the example below.

Using the IO Graphs feature, we can graph the features of packet capture activity through a given display filter. Using different colors, we can apply multiple display filters to visualize different sets of data together.

To display the flow of RSSI traffic over time, click on the **All packets** line. Change the Y Axis unit from _Packets_ to **SUM(Y Field)**. This will allow us to enter an arbitrary numeric display field name to use for graphing on the Y Axis. In the All Packets row enter **dect.rssi** into the Y Field column, and then press **Enter**. Finally, click the checkbox on the left side of the expression to graph the data.

The graph will update to look like the following.

By graphing the RSSI in this fashion, you can observe recorded signal strength information in a visual manner. Using this technique with a live packet capture would allow you to graph signal strength in Wireshark as you move closer or farther away from a target device, allowing you to locate an unknown transmitter.

Next, apply the following changes.

* Change the Y Axis graph from _SUM(Y Field)_ to **AVG(Y Field)**, clicking the All Packets box to create the graph again. Is this view more informative than the SUM(Y Field) view?
* Change the Style for the Y Axis from _Line_ to **Impulse**, **Bar** or **Dot**. Do any of these styles look better?

When you are finished, close Wireshark.

For unencrypted DECT devices, we can extract audio conversations, saving a conversation to a WAV file. This data extraction can be manually performed using the `pcapstein` tool included with the deDECTed tools and the G72x decoder tool, but this becomes a cumbersome process without knowledge of the specific codec version used by the target DECT cordless phone. Instead, we’ll leverage the `dect-decoder.sh` script to automate this process.

**Note:** If you want details on using the `pcapstein`, `decode-g72x`, and sox tools to extract and convert audio from a DECT capture, please see the `dect-decoder.sh` shell script.

Change to the `/root/sec617/dect` directory. When run with no arguments, the `dect-decoder.sh` script attempts to extract all conversations from the packet capture ending in `.pcap` in the current directory. Alternatively, you can specify the filename of one or more packet capture files as `dect-decoder.sh` arguments to target specific packet capture files. Extract the audio from the supplied packet captures as shown.

```
# ./dect-decoder.sh 
Processing dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap
libpcap version 1.8.1
pcap file version 2.4
pcap_loop() = 0
You might want to "rm *.ima" now.
-rw-r--r-- 1 root root 287978 Jun  8 05:30 dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap_fp.ima.g721.wav
-rw-r--r-- 1 root root 575884 Jun  8 05:30 dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap_fp.ima.g726.L.wav
-rw-r--r-- 1 root root 575884 Jun  8 05:30 dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap_fp.ima.g726.R.wav
-rw-r--r-- 1 root root     58 Jun  8 05:30 dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap_pp.ima.g721.wav
-rw-r--r-- 1 root root     44 Jun  8 05:30 dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap_pp.ima.g726.L.wav
-rw-r--r-- 1 root root     44 Jun  8 05:30 dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap_pp.ima.g726.R.wav
```

After `dect-decoder.sh` finishes extracting the audio from DECT captures, the intermediate `.ima` captures can be removed, as shown.

```
# rm *.ima
```

Now we are left with several smaller WAV files that do not contain audible content. We can delete them without manually specifying each filename by identifying the files less than 100 bytes in size with the `find` utility, passing the list to the `rm` utility with `xargs`, as shown.

```
# find -name '*.wav' -size -100c -print | xargs rm
# ls -l *.wav
-rw-r--r-- 1 root root 287978 Jun  8 05:30 dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap_fp.ima.g721.wav
-rw-r--r-- 1 root root 575884 Jun  8 05:30 dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap_fp.ima.g726.L.wav
-rw-r--r-- 1 root root 575884 Jun  8 05:30 dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap_fp.ima.g726.R.wav
```

We are left with three remaining WAV files. Two of the files have the same file size because they are treated as mono audio in a stereo codec. We can remove them, as shown.

```
# rm dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap_fp.ima.g726.L.wav
```

Next, we can play the two remaining WAV files with the `play` utility. We can play each file, as shown.

**Note:** If you are unable to hear audio during the playback, adjust the volume output in both the Kali virtual machine and your host operating system.

> Without amplification, the audio content is too quiet to hear (though there is some noise at the beginning that is loud; use caution with the volume of your system if you are using headphones).

```
# play dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap_fp.ima.g721.wav vol 5
...
# play dump_2017-02-01_17_22_17_RFPI_02_04_e7_4e_60.pcap_fp.ima.g726.R.wav vol 5
...
```

**Question:** At the end of the audio in the `dump_2017-02-01_17_22_01_RFPI_02_04_e7_4e_60.pcap_fp.ima.g726.L.wav` file, what one-word answer is the definition of indifference?

**Bonus question:** What was the phone number dialed in the `dump_2017-02-01_17_20_01_RFPI_02_04_e7_4e_60.pcap_fp.ima.g726.L.wav` file?

#### \[OPTIONAL] Pop Culture Wireless Phone Capture <a href="#5boptional-5d-pop-culture-wireless-phone-capture" id="5boptional-5d-pop-culture-wireless-phone-capture"></a>

Repeat the steps in this exercise, this time targeting the packet capture in the `/root/sec617/dect/popculture` directory.

**Question:** What color is the animal identified in the audio of the `dump_2017-02-01_17_20_01_RFPI_02_04_e7_4e_60.pcap_fp.ima.g726.L.wav` file?

### Attacking DECT Wireless - Answers <a href="#attacking-dect-wireless---answers" id="attacking-dect-wireless---answers"></a>

**Question:** At the end of the audio, in `dump_2017-02-01_17_22_01_RFPI_02_04_e7_4e_60.pcap_fp.ima.g726.L.wav`, what one-word answer is the definition of indifference?

**Answer:** Punishment

**Bonus Question:** What was the phone number dialed in `dump_2017-02-01_17_20_01_RFPI_02_04_e7_4e_60.pcap_fp.ima.g726.L.wav`?

**Bonus Answer:** 1-401-413-9239

**Question:** What color is the animal identified in the audio of the `dump_2017-02-01_17_20_01_RFPI_02_04_e7_4e_60.pcap_fp.ima.g726.L.wav` file in the popculture directory?

**Answer:** White

**STOP**

This completes the lab exercise. Congratulations.
