# Table of contents

* [OSCP / Pentesting](README.md)

## Tools / Command Reference

* [Methodology](tools-command-reference/methodology.md)
* [Commonly Used // Useful Commands // Things to not forget](tools-command-reference/commonly-used/README.md)
  * [Windows Commands for Pentesting](tools-command-reference/commonly-used/windows-commands-for-pentesting.md)
* [Transferring Files](tools-command-reference/transferring-files.md)
* [Reverse Shells](tools-command-reference/reverse-shells/README.md)
  * [Generating Reverse Shells with Metasploit](tools-command-reference/reverse-shells/generating-reverse-shells-with-metasploit.md)
  * [Upgrading Shells](tools-command-reference/reverse-shells/upgrading-shells.md)

***

* [Untitled](untitled.md)

## Information Gathering

* [Passive Information Gathering](information-gathering/passive.md)
* [Active](information-gathering/active.md)

## Enumeration

* [Windows](enumeration/windows/README.md)
  * [Checklist](enumeration/windows/checklist.md)
* [Linux](enumeration/linux/README.md)
  * [Checklist](enumeration/linux/checklist.md)

## HTTP - Web <a href="#web" id="web"></a>

* [Assess](web/assess.md)
* [Attack](web/attack.md)
* [SQLi](web/sqli-sql.md)

## Privilege Escalation

* [Windows](privilege-escalation/windows/README.md)
  * [Checklist](privilege-escalation/windows/checklist.md)
  * [JuicyPotato](privilege-escalation/windows/untitled.md)
* [Linux](privilege-escalation/linux.md)

## Buffer Overflows (Super Basic)

* [Windows](buffer-overflows-super-basic/windows.md)
* [Linux](buffer-overflows-super-basic/linux.md)
* [References](buffer-overflows-super-basic/references.md)

## Client-Side Attacks

* [Office](client-side-attacks/office.md)
* [Web](client-side-attacks/web.md)

## Password Attacks

* [Network Service Attacks](password-attacks/network-service-attacks.md)
* [Brute-Forcing](password-attacks/brute-forcing.md)
* [Leveraging Password Hashes](password-attacks/leveraging-password-hashes.md)
* [SSH](password-attacks/ssh.md)
* [Pass the Hash](password-attacks/pass-the-hash.md)
* [Password Cracking](password-attacks/password-cracking.md)

## Tunneling and Port Redirection

* [SSH](tunneling-and-port-redirection/ssh.md)
* [PLINK](tunneling-and-port-redirection/plink.md)
* [Cheat Sheet](tunneling-and-port-redirection/cheat-sheet.md)

## Active Directory

* [Enumerate](active-directory/enumerate.md)
* [Lateral Movement](active-directory/lateral-movement-1/README.md)
  * [SharpHound / BloodHound](active-directory/lateral-movement-1/lateral-movement.md)
* [Authenticate](active-directory/authenticate.md)
* [Persistence](active-directory/persistence.md)

## Exploitation

* [Linux](exploitation/linux.md)
* [Windows](exploitation/windows/README.md)
  * [How to Manually Exploit EternalBlue on Windows Server Using MS17-010 Python Exploit](exploitation/windows/eternal-blue.md)
  * [MS08\_67](exploitation/windows/ms08-67.md)

## Hack The Box (Based on TJNull OSCP-Like Machines) <a href="#hack-the-box-writeups" id="hack-the-box-writeups"></a>

* [Intro](hack-the-box-writeups/intro.md)
* [Fuse](hack-the-box-writeups/fuse.md)
* [Writeups (Retired Only)](hack-the-box-writeups/writeups-retired-only/README.md)
  * [Lame](hack-the-box-writeups/writeups-retired-only/lame.md)
  * [Untitled](hack-the-box-writeups/writeups-retired-only/untitled-8.md)
  * [Untitled](hack-the-box-writeups/writeups-retired-only/untitled-7.md)
  * [Untitled](hack-the-box-writeups/writeups-retired-only/untitled-6.md)
  * [Untitled](hack-the-box-writeups/writeups-retired-only/untitled-5.md)
  * [Untitled](hack-the-box-writeups/writeups-retired-only/untitled-4.md)
  * [Untitled](hack-the-box-writeups/writeups-retired-only/untitled-3.md)
  * [Untitled](hack-the-box-writeups/writeups-retired-only/untitled-2.md)
  * [Untitled](hack-the-box-writeups/writeups-retired-only/untitled-1.md)
* [Privilege Escalations Summary](hack-the-box-writeups/privilege-escalations-summary.md)

## Cyber Security Labs

* [Challenge Labs](cyber-security-labs/untitled/README.md)
  * [Mount](cyber-security-labs/untitled/mount.md)
  * [Spray](cyber-security-labs/untitled/spray.md)
  * [Dictionary](cyber-security-labs/untitled/dictionary.md)
  * [Brute](cyber-security-labs/untitled/brute.md)
* [Easy Machines](cyber-security-labs/easy-machines/README.md)
  * [Untitled](cyber-security-labs/easy-machines/untitled-2.md)
  * [Potato](cyber-security-labs/easy-machines/potato.md)
  * [Stack](cyber-security-labs/easy-machines/stack.md)
  * [Secret](cyber-security-labs/easy-machines/untitled-1.md)
  * [Cold](cyber-security-labs/easy-machines/untitled.md)

## Enumerating specific ports

* [SMB (445 / 139 )](enumerating-specific-ports/untitled-1.md)
* [Untitled](enumerating-specific-ports/untitled.md)
* [443 - HTTPS](enumerating-specific-ports/443-https.md)

## Dante Pro Lab

* [Information Gathering](dante-pro-lab/untitled.md)
* [Initial Access](dante-pro-lab/initial-access.md)

## SANS 617 - Wireless

* [Wi-Fi Data Collection & Analysis](sans-617-wireless/wi-fi-data-collection-and-analysis/README.md)
  * [Setup / Sniffing Wi-Fi](sans-617-wireless/wi-fi-data-collection-and-analysis/setup-sniffing-wi-fi.md)
  * [Rogue AP Analysis](sans-617-wireless/wi-fi-data-collection-and-analysis/rogue-ap-analysis.md)
  * [Bridging the Airgap](sans-617-wireless/wi-fi-data-collection-and-analysis/bridging-the-airgap.md)
  * [Utilities / Resources](sans-617-wireless/wi-fi-data-collection-and-analysis/utilities-resources.md)
  * [Definitions](sans-617-wireless/wi-fi-data-collection-and-analysis/definitions.md)
* [Wi-Fi Attack and Exploitation Techniques](sans-617-wireless/wi-fi-attack-and-exploitation-techniques/README.md)
  * [Exploiting Wi-Fi Hotspots](sans-617-wireless/wi-fi-attack-and-exploitation-techniques/exploiting-wi-fi-hotspots.md)
  * [Wi-Fi Client Attacks](sans-617-wireless/wi-fi-attack-and-exploitation-techniques/wi-fi-client-attacks.md)
  * [Attacking WEP](sans-617-wireless/wi-fi-attack-and-exploitation-techniques/attacking-wep.md)
  * [Denial-of-Service Attacks](sans-617-wireless/wi-fi-attack-and-exploitation-techniques/denial-of-service-attacks.md)
  * [Wi-Fi Fuzzing for Bug Discovery](sans-617-wireless/wi-fi-attack-and-exploitation-techniques/wi-fi-fuzzing-for-bug-discovery.md)
  * [Utilities / Resources](sans-617-wireless/wi-fi-attack-and-exploitation-techniques/utilities-resources.md)
  * [Definitions](sans-617-wireless/wi-fi-attack-and-exploitation-techniques/definitions.md)
* [Enterprise Wi-Fi, DECT, and ZIGBEE Attacks](sans-617-wireless/enterprise-wi-fi-dect-and-zigbee-attacks/README.md)
  * [To DO](sans-617-wireless/enterprise-wi-fi-dect-and-zigbee-attacks/to-do.md)
  * [Attacking WPA2 Pre-Shared Key Networks](sans-617-wireless/enterprise-wi-fi-dect-and-zigbee-attacks/attacking-wpa2-pre-shared-key-networks/README.md)
    * [Attacking WPA2-PSK Exercise](sans-617-wireless/enterprise-wi-fi-dect-and-zigbee-attacks/attacking-wpa2-pre-shared-key-networks/attacking-wpa2-psk-exercise.md)
  * [Attacking WPA2-Enterprise Networks](sans-617-wireless/enterprise-wi-fi-dect-and-zigbee-attacks/attacking-wpa2-enterprise-networks/README.md)
    * [Identifying Vulnerable Clients](sans-617-wireless/enterprise-wi-fi-dect-and-zigbee-attacks/attacking-wpa2-enterprise-networks/identifying-vulnerable-clients.md)
    * [Impersonating PEAP networks](sans-617-wireless/enterprise-wi-fi-dect-and-zigbee-attacks/attacking-wpa2-enterprise-networks/impersonating-peap-networks.md)
  * [Attacking Digital Enhances Cordless Telephony Deployments (DECT)](sans-617-wireless/enterprise-wi-fi-dect-and-zigbee-attacks/attacking-digital-enhances-cordless-telephony-deployments-dect/README.md)
    * [Attacking DECT Wireless Exercise](sans-617-wireless/enterprise-wi-fi-dect-and-zigbee-attacks/attacking-digital-enhances-cordless-telephony-deployments-dect/attacking-dect-wireless-exercise.md)
  * [Attacking Zigbee Deployments](sans-617-wireless/enterprise-wi-fi-dect-and-zigbee-attacks/attacking-zigbee-deployments.md)
  * [Utilities / Resources](sans-617-wireless/enterprise-wi-fi-dect-and-zigbee-attacks/utilities-resources.md)
  * [Definitions](sans-617-wireless/enterprise-wi-fi-dect-and-zigbee-attacks/definitions.md)
* [Bluetooth and Software Defined Radio Attacks](sans-617-wireless/bluetooth-and-software-defined-radio-attacks/README.md)
  * [Bluetooth Intro and Attack Techniques](sans-617-wireless/bluetooth-and-software-defined-radio-attacks/bluetooth-intro-and-attack-techniques.md)
  * [Bluetooth Low Energy Intro and Attack Techniques](sans-617-wireless/bluetooth-and-software-defined-radio-attacks/bluetooth-low-energy-intro-and-attack-techniques.md)
  * [Practical Application of Software Defined Radio](sans-617-wireless/bluetooth-and-software-defined-radio-attacks/practical-application-of-software-defined-radio.md)
  * [Definitions](sans-617-wireless/bluetooth-and-software-defined-radio-attacks/definitions.md)
  * [Resources / Utilities](sans-617-wireless/bluetooth-and-software-defined-radio-attacks/resources-utilities.md)
* [RFID, Smart Cards, and NFC Hacking](sans-617-wireless/rfid-smart-cards-and-nfc-hacking/README.md)
  * [RFID Tracking and Privacy Attacks](sans-617-wireless/rfid-smart-cards-and-nfc-hacking/rfid-tracking-and-privacy-attacks.md)
  * [Low-Frequency RFID Attacks](sans-617-wireless/rfid-smart-cards-and-nfc-hacking/low-frequency-rfid-attacks.md)
  * [Exploiting RFID Contactless Smart Cards](sans-617-wireless/rfid-smart-cards-and-nfc-hacking/exploiting-rfid-contactless-smart-cards.md)
  * [Attacking NFC](sans-617-wireless/rfid-smart-cards-and-nfc-hacking/attacking-nfc.md)
  * [Resources / Utilities](sans-617-wireless/rfid-smart-cards-and-nfc-hacking/resources-utilities.md)
  * [Definitions](sans-617-wireless/rfid-smart-cards-and-nfc-hacking/definitions.md)

## SANS FOR-508 - GCFA

* [Advanced Incident Response and Threat Hunting](sans-for-508-gcfa/advanced-incident-response-and-threat-hunting/README.md)
  * [Malware Persistence](sans-for-508-gcfa/advanced-incident-response-and-threat-hunting/malware-persistence.md)
  * [Hunting Across the Enterprise](sans-for-508-gcfa/advanced-incident-response-and-threat-hunting/hunting-across-the-enterprise.md)
  * [Credential Theft](sans-for-508-gcfa/advanced-incident-response-and-threat-hunting/credential-theft.md)
* [Intrusion Analysis](sans-for-508-gcfa/intrusion-analysis/README.md)
  * [Advanced Evidence of Execution](sans-for-508-gcfa/intrusion-analysis/advanced-evidence-of-execution/README.md)
    * [Exercise 2.1](sans-for-508-gcfa/intrusion-analysis/advanced-evidence-of-execution/exercise-2.1.md)
  * [Event Log Analysis for Responders and Hunters](sans-for-508-gcfa/intrusion-analysis/event-log-analysis-for-responders-and-hunters/README.md)
    * [Exercise 2.2 - Tracking Credentials](sans-for-508-gcfa/intrusion-analysis/event-log-analysis-for-responders-and-hunters/exercise-2.2-tracking-credentials.md)
  * [Lateral Movement Adversary Tactics](sans-for-508-gcfa/intrusion-analysis/lateral-movement-adversary-tactics/README.md)
    * [Exercise 2.3 - Tracking Lateral Movement](sans-for-508-gcfa/intrusion-analysis/lateral-movement-adversary-tactics/exercise-2.3-tracking-lateral-movement.md)
  * [Command Line, PowerShell, and WMI Analysis](sans-for-508-gcfa/intrusion-analysis/command-line-powershell-and-wmi-analysis.md)
* [Memory Forensics in Incident Response and Threat Hunting](sans-for-508-gcfa/memory-forensics-in-incident-response-and-threat-hunting.md)
  * [Enterprise & Remote System Analysis](sans-for-508-gcfa/memory-forensics-in-incident-response-and-threat-hunting/enterprise-and-remote-system-analysis.md)
  * [EDR Overview](sans-for-508-gcfa/memory-forensics-in-incident-response-and-threat-hunting/edr-overview.md)
  * [Memory Forensics](sans-for-508-gcfa/memory-forensics-in-incident-response-and-threat-hunting/memory-forensics.md)
  * [Acquiring Memory](sans-for-508-gcfa/memory-forensics-in-incident-response-and-threat-hunting/acquiring-memory.md)
  * [Introduction to Memory Analysis](sans-for-508-gcfa/memory-forensics-in-incident-response-and-threat-hunting/introduction-to-memory-analysis.md)
  * [Code Injection, Rootkits, and Extraction](sans-for-508-gcfa/memory-forensics-in-incident-response-and-threat-hunting/code-injection-rootkits-and-extraction.md)
* [Timeline Analysis](sans-for-508-gcfa/timeline-analysis.md)
  * [Malware Discovery](sans-for-508-gcfa/timeline-analysis/malware-discovery.md)
  * [Timeline Analysis Overview](sans-for-508-gcfa/timeline-analysis/timeline-analysis-overview.md)
  * [Filesystem Timeline Creation and Analysis](sans-for-508-gcfa/timeline-analysis/filesystem-timeline-creation-and-analysis.md)
  * [Introducing the Super Timeline](sans-for-508-gcfa/timeline-analysis/introducing-the-super-timeline.md)
  * [Targeted Super Timeline Creation](sans-for-508-gcfa/timeline-analysis/targeted-super-timeline-creation.md)
  * [Filtering the Super Timeline](sans-for-508-gcfa/timeline-analysis/filtering-the-super-timeline.md)
  * [Super Timeline Analysis](sans-for-508-gcfa/timeline-analysis/super-timeline-analysis.md)
* [Advanced Adversary and Anti-Forensics Detection](sans-for-508-gcfa/advanced-adversary-and-anti-forensics-detection/README.md)
  * [Anti-Forensics Overview](sans-for-508-gcfa/advanced-adversary-and-anti-forensics-detection/anti-forensics-overview.md)
  * [Recovery of Deleted Files via VSS](sans-for-508-gcfa/advanced-adversary-and-anti-forensics-detection/recovery-of-deleted-files-via-vss.md)
  * [Advanced NTFS Filesystem Tactics](sans-for-508-gcfa/advanced-adversary-and-anti-forensics-detection/advanced-ntfs-filesystem-tactics.md)
  * [Advanced Evidence Recovery](sans-for-508-gcfa/advanced-adversary-and-anti-forensics-detection/advanced-evidence-recovery.md)
  * [Defensive Countermeasures](sans-for-508-gcfa/advanced-adversary-and-anti-forensics-detection/defensive-countermeasures.md)
* [Excercises](sans-for-508-gcfa/excercises.md)
* [Tools / Resources](sans-for-508-gcfa/tools-resources.md)
* [Hunting Notes](sans-for-508-gcfa/hunting-notes.md)
* [Appendix](sans-for-508-gcfa/appendix.md)
* [Glossary](sans-for-508-gcfa/glossary.md)
* [Homework](sans-for-508-gcfa/homework.md)

## SANS FOR-610 - GREM

* [Malware Analysis Fundamentals](sans-for-610-grem/malware-analysis-fundamentals/README.md)
  * [Introduction to Malware Analysis](sans-for-610-grem/malware-analysis-fundamentals/introduction-to-malware-analysis.md)
  * [Malware Analysis Lab](sans-for-610-grem/malware-analysis-fundamentals/malware-analysis-lab.md)
  * [Static Properties Analysis](sans-for-610-grem/malware-analysis-fundamentals/static-properties-analysis.md)
  * [Behavioral Analysis Essentials](sans-for-610-grem/malware-analysis-fundamentals/behavioral-analysis-essentials.md)
  * [Code Analysis Essentials](sans-for-610-grem/malware-analysis-fundamentals/code-analysis-essentials.md)
  * [Exploring Network Interactions](sans-for-610-grem/malware-analysis-fundamentals/exploring-network-interactions.md)
* [Reversing Malicious Code](sans-for-610-grem/reversing-malicious-code/README.md)
  * [Core Reversing Concepts](sans-for-610-grem/reversing-malicious-code/core-reversing-concepts.md)
  * [Reversing Functions](sans-for-610-grem/reversing-malicious-code/reversing-functions.md)
  * [Control Flow In-Depth](sans-for-610-grem/reversing-malicious-code/control-flow-in-depth.md)
  * [API Patterns in Malware](sans-for-610-grem/reversing-malicious-code/api-patterns-in-malware.md)
  * [64-Bit Code Analysis](sans-for-610-grem/reversing-malicious-code/64-bit-code-analysis.md)
* [Analyzing Malicious Documents](sans-for-610-grem/analyzing-malicious-documents/README.md)
  * [Malicious PDF File Analysis](sans-for-610-grem/analyzing-malicious-documents/malicious-pdf-file-analysis/README.md)
    * [Tools](sans-for-610-grem/analyzing-malicious-documents/malicious-pdf-file-analysis/tools.md)
  * [VBA Macros in Microsoft Office Documents](sans-for-610-grem/analyzing-malicious-documents/vba-macros-in-microsoft-office-documents.md)
  * [Examining Malicious RTF Files](sans-for-610-grem/analyzing-malicious-documents/examining-malicious-rtf-files.md)
  * [Appendix: Making Sense of XLM Macros](sans-for-610-grem/analyzing-malicious-documents/appendix-making-sense-of-xlm-macros.md)
* [In-Depth Malware Analysis](sans-for-610-grem/in-depth-malware-analysis/README.md)
  * [Deobfuscating Malicious Javascript](sans-for-610-grem/in-depth-malware-analysis/deobfuscating-malicious-javascript.md)
  * [Recognizing Packed Malware](sans-for-610-grem/in-depth-malware-analysis/recognizing-packed-malware.md)
  * [Getting Started with Unpacking](sans-for-610-grem/in-depth-malware-analysis/getting-started-with-unpacking.md)
  * [Using Debuggers for Dumping](sans-for-610-grem/in-depth-malware-analysis/using-debuggers-for-dumping.md)
  * [Debugging Packed Malware](sans-for-610-grem/in-depth-malware-analysis/debugging-packed-malware.md)
  * [Analyzing Multi-Technology Malware](sans-for-610-grem/in-depth-malware-analysis/analyzing-multi-technology-malware/README.md)
    * [Exercise 4.7](sans-for-610-grem/in-depth-malware-analysis/analyzing-multi-technology-malware/exercise-4.7.md)
    * [Exercise 4.8](sans-for-610-grem/in-depth-malware-analysis/analyzing-multi-technology-malware/exercise-4.8.md)
    * [Exercise 4.9](sans-for-610-grem/in-depth-malware-analysis/analyzing-multi-technology-malware/exercise-4.9.md)
  * [Code Injection and API Hooking](sans-for-610-grem/in-depth-malware-analysis/code-injection-and-api-hooking/README.md)
    * [Exercise 4.10](sans-for-610-grem/in-depth-malware-analysis/code-injection-and-api-hooking/exercise-4.10.md)
    * [Exercise 4.11](sans-for-610-grem/in-depth-malware-analysis/code-injection-and-api-hooking/exercise-4.11.md)
* [Examining Self-Defending Software](sans-for-610-grem/examining-self-defending-software/README.md)
  * [Debugger Detection and Data Protection](sans-for-610-grem/examining-self-defending-software/debugger-detection-and-data-protection.md)
  * [Unpacking Process Hollowing](sans-for-610-grem/examining-self-defending-software/unpacking-process-hollowing.md)
  * [Detecting the Analysis Toolkit](sans-for-610-grem/examining-self-defending-software/detecting-the-analysis-toolkit.md)
  * [Handling Misdirection Techniques](sans-for-610-grem/examining-self-defending-software/handling-misdirection-techniques.md)
  * [Unpacking by Anticipating Actions](sans-for-610-grem/examining-self-defending-software/unpacking-by-anticipating-actions.md)
* [Malware Analysis Tournament (NETWARS)](sans-for-610-grem/malware-analysis-tournament-netwars.md)

## SANS SEC 565 - Red Team Operations

* [Planning Adversary Emulation and Threat Intelligence](sans-sec-565-red-team-operations/planning-adversary-emulation-and-threat-intelligence/README.md)
  * [Common Language](sans-sec-565-red-team-operations/planning-adversary-emulation-and-threat-intelligence/common-language.md)
  * [Why Red Team](sans-sec-565-red-team-operations/planning-adversary-emulation-and-threat-intelligence/why-red-team.md)
  * [Frameworks and Methodologies](sans-sec-565-red-team-operations/planning-adversary-emulation-and-threat-intelligence/frameworks-and-methodologies.md)
  * [Threat Intel](sans-sec-565-red-team-operations/planning-adversary-emulation-and-threat-intelligence/threat-intel.md)
  * [Planning](sans-sec-565-red-team-operations/planning-adversary-emulation-and-threat-intelligence/planning.md)
  * [Red Team Execution](sans-sec-565-red-team-operations/planning-adversary-emulation-and-threat-intelligence/red-team-execution.md)
* [Attack Infrastructure and OPSEC](sans-sec-565-red-team-operations/attack-infrastructure-and-opsec/README.md)
  * [Intro to C2](sans-sec-565-red-team-operations/attack-infrastructure-and-opsec/intro-to-c2.md)
  * [Red Team Tools](sans-sec-565-red-team-operations/attack-infrastructure-and-opsec/red-team-tools.md)
  * [Cobalt Strike Framework](sans-sec-565-red-team-operations/attack-infrastructure-and-opsec/cobalt-strike-framework.md)
  * [Attack Infrastructure](sans-sec-565-red-team-operations/attack-infrastructure-and-opsec/attack-infrastructure.md)
  * [Pivoting and Redirection](sans-sec-565-red-team-operations/attack-infrastructure-and-opsec/pivoting-and-redirection.md)
  * [Redirectors](sans-sec-565-red-team-operations/attack-infrastructure-and-opsec/redirectors.md)
  * [Operational Security](sans-sec-565-red-team-operations/attack-infrastructure-and-opsec/operational-security.md)
* [Getting In and Staying In](sans-sec-565-red-team-operations/getting-in-and-staying-in/README.md)
  * [Weaponization](sans-sec-565-red-team-operations/getting-in-and-staying-in/weaponization.md)
  * [Delivery](sans-sec-565-red-team-operations/getting-in-and-staying-in/delivery.md)
  * [Initial Access](sans-sec-565-red-team-operations/getting-in-and-staying-in/initial-access.md)
  * [Discovery](sans-sec-565-red-team-operations/getting-in-and-staying-in/discovery.md)
  * [Privilege Escalation](sans-sec-565-red-team-operations/getting-in-and-staying-in/privilege-escalation.md)
  * [Credential Access](sans-sec-565-red-team-operations/getting-in-and-staying-in/credential-access.md)
  * [Persistence](sans-sec-565-red-team-operations/getting-in-and-staying-in/persistence.md)
  * [Defense Evasion](sans-sec-565-red-team-operations/getting-in-and-staying-in/defense-evasion.md)
* [Active Directory Attacks and Lateral Movement](sans-sec-565-red-team-operations/active-directory-attacks-and-lateral-movement/README.md)
  * [Intro to Active Directory](sans-sec-565-red-team-operations/active-directory-attacks-and-lateral-movement/intro-to-active-directory.md)
  * [Authentication in AD](sans-sec-565-red-team-operations/active-directory-attacks-and-lateral-movement/authentication-in-ad.md)
  * [Domain Discovery and Enumeration](sans-sec-565-red-team-operations/active-directory-attacks-and-lateral-movement/domain-discovery-and-enumeration.md)
  * [Privilege Hunting](sans-sec-565-red-team-operations/active-directory-attacks-and-lateral-movement/privilege-hunting.md)
  * [User Impersonation](sans-sec-565-red-team-operations/active-directory-attacks-and-lateral-movement/user-impersonation.md)
  * [Lateral Movement](sans-sec-565-red-team-operations/active-directory-attacks-and-lateral-movement/lateral-movement.md)
* [Action On Objectives & Reporting](sans-sec-565-red-team-operations/action-on-objectives-and-reporting/README.md)
  * [AD Privilege Escalation](sans-sec-565-red-team-operations/action-on-objectives-and-reporting/ad-privilege-escalation.md)
  * [Domain Persistence](sans-sec-565-red-team-operations/action-on-objectives-and-reporting/domain-persistence.md)
  * [Database Attacks](sans-sec-565-red-team-operations/action-on-objectives-and-reporting/database-attacks.md)
  * [Collection](sans-sec-565-red-team-operations/action-on-objectives-and-reporting/collection.md)
  * [Exfiltration](sans-sec-565-red-team-operations/action-on-objectives-and-reporting/exfiltration.md)
  * [Impact](sans-sec-565-red-team-operations/action-on-objectives-and-reporting/impact.md)
  * [Engagement Closure](sans-sec-565-red-team-operations/action-on-objectives-and-reporting/engagement-closure.md)
  * [Retesting](sans-sec-565-red-team-operations/action-on-objectives-and-reporting/retesting.md)

## CISM

* [Page 1](cism/page-1.md)
