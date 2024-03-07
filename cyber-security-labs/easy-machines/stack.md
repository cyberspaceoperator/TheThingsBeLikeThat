---
description: Stack employs GitStack 2.3.10 for User access
---

# Stack

Initial scan indicates that port 80 (among other ports) are open

![](<../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png>)

In the next services (like 445) I also ran smbmap -H \<ip> but it did not return anything. It's important to make sure your smb.conf file has the correct configuration to allow for SMBv2, otherwise it will not be allowed if the host you're trying to connect to uses a newer version.

```
# Nmap 7.80 scan initiated Wed Jul 15 17:33:10 2020 as: nmap -sC -sV -oA nmapInitial 172.31.1.12
Nmap scan report for 172.31.1.12
Host is up (0.048s latency).
Not shown: 989 closed ports
PORT      STATE SERVICE            VERSION
80/tcp    open  http               Apache httpd 2.2.22 ((Win32) mod_ssl/2.2.22 OpenSSL/0.9.8u mod_wsgi/3.3 Python/2.7.2 PHP/5.4.3)
|_http-server-header: Apache/2.2.22 (Win32) mod_ssl/2.2.22 OpenSSL/0.9.8u mod_wsgi/3.3 Python/2.7.2 PHP/5.4.3
|_http-title: Page not found at /
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
| rdp-ntlm-info: 
|   Target_Name: STACK
|   NetBIOS_Domain_Name: STACK
|   NetBIOS_Computer_Name: STACK
|   DNS_Domain_Name: Stack
|   DNS_Computer_Name: Stack
|   Product_Version: 6.3.9600
|_  System_Time: 2020-07-15T22:34:23+00:00
| ssl-cert: Subject: commonName=Stack
| Not valid before: 2020-04-14T12:36:44
|_Not valid after:  2020-10-14T12:36:44
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49156/tcp open  msrpc              Microsoft Windows RPC
49163/tcp open  msrpc              Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: STACK, NetBIOS user: <unknown>, NetBIOS MAC: 02:0b:e9:5a:4e:52 (unknown)
|_smb-os-discovery: ERROR: Script execution failed (use -d to debug)
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2020-07-15T22:34:23
|_  start_date: 2020-07-15T22:30:31

```

From here I see that the website is using GitStack, there is only really one good exploit on ExploitDB for it that matches the version. It can be used in Metasploit but I think I also found a python script that could be used.

{% embed url="https://www.exploit-db.com/exploits/43777" %}

After using that exploit with no special parameters, I did normal enumeration of services, users, permissions, etc. Although I did find that I was a service account and had SEImpersonatePrivilege and I could have probably just have done it with JuicyPotato, I didn't. I tried to find it.

What I did find was a KeePass database file

![KeePass database file](<../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1).png>)

I was able to use keepass2john.py&#x20;

{% embed url="https://gist.github.com/HarmJ0y/116fa1b559372804877e604d7d367bbc" %}

From there I threw that file into hashcat with rockyou.txt and the password was something in that file.

Here I actually got stuck because I wasn't really writing things down and didn't ask myself "What do I have"\
I kept trying to use the command line version of keepass, then i tried to RDP in, I tried lots of dumb stuff. \
Here what I had:

* the .kdbx file
* my own windows machine
* the master passwd from the cracked kdbx file

So I just downloaded KeePass on my Windows Environment and then opened the database file that way. From there I used psexec.py

![](<../../.gitbook/assets/image (4) (1) (1) (1) (1).png>)

