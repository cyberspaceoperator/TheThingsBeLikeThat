---
description: >-
  Here I document what I've found works best for me, you might find that
  something works better for you. There is a TON of great documentation online
  and reference points.
---

# Methodology

It is most important to stick to what YOU find works best, not necessarily what is written in this document.\
This is simply an area of living documentation of the steps I normally take in an engagement.

{% hint style="danger" %}
ALWAYS KEEP ENUMERATING, DO NOT ASSUME YOU ARE DONE. AT EACH AND EVERY LEVEL  OF COMPROMISE YOU SHOULD BE ENUMERATING

IF YOU ARE STUCK FOR MORE THAN 1 HOUR... BREAK, MOVE ON, AND COME BACK LATER

YOU SHOULD ALSO ALWAYS **RETURN TO PREVIOUS SECTIONS** AS PREVIOUSLY AREAS YOU DIDN'T HAVE ACCESS TO YOU MAY NOW HAVE ACCESS TO **(IE: WITH NEW CREDENTIALS FOR INSTANCE)**
{% endhint %}

{% hint style="info" %}
Keep in mind that my typical engagement starts at enumeration, not necessarily information gathering like open source intelligence, though that may be involved as the situation dictates
{% endhint %}

## Step 1: Active Information Gathering

* Open Ports / Services
  * nmap -sC -sV \<ip> -oA nmapInitial
  * nmap -sC -sV -p- \<ip> -oA nmapFull
  * nmap -sU -p- \<ip> -oA UDPFull
  * **sudo** autorecon \[options] \<ip> // Autorecon is great, just make sure you have all dependencies installed otherwise it might not catch everything (or be able to).

{% hint style="info" %}
Always **MANUALLY enumerate** with nmap and/or by opening up wireshark

Come across ports you didn't know? (like **9419,9420,9421,9425**?**)**

[**https://www.speedguide.net/port.php?port=9419**](https://www.speedguide.net/port.php?port=9419)
{% endhint %}

## Step 2: Gaining access / Enumeration

* **Don't rely on common wordlists! Create your own with cewl!**
* **Found creds somewhere? Try them on previously inaccessible services**

## Logic Tree (living)

1. **For each port found open**
   1. Enumerate port manually using this guide or any guide (like HackTrickz.xyz)
      1. Do not spend more than 10-20 minutes enumerating each port, unless you are making progress
   2. Search for public-available exploits on exploit-db and github, those will be the main two sources
      1. **If an exploit is found, ask:**
         1. Does the exploit match the version, build, OS, release, etc
            1. If yes:
               1. Do not use it just yet, list it as possible, but do not go throwing exploits everywhere
      2. **If an exploit is not found:**
         1. **Ask yourself:** Do I even need an exploit in the first place? **(most compromises are a result of incorrectly configuring, neglecting, or not understanding the program/service)**
   3. Copy / Paste / Write / Document all of your findings in a note-taking software **that is EASY to read.** I've found that using things like cherry tree only work if I use a nice font, make it larger, color code certain things, etc. Otherwise if I'm just copying/pasting from console, the text is small, thin, hard to see and I go cross-eyed.









