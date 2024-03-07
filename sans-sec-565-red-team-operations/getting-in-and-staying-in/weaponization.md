# Weaponization

## Weaponization

* <mark style="color:blue;">**Coupling a remote access method into a deliverable payload**</mark>
  * The goal is to tailor attacks for the highest probability of success during the delivery phase
  * Avoid using unmodified tools and payloads
  * Know your tooling and associated footprint
  * Red team must create the payload, test it, and identify IoCs



## Executables

### Binaries

* Generic executables have been highly signatured
* Keep engagement objectives in mind! Using generic executables to exercise detection capabilities might be the goal

### Custom Executables

* Packing
  * A way to compress an executable at the cost of increasing the amount of entropy. If found, would look weird because high entropy is not normal in a lot of executables. Tools for measuring entropy are covered in GREM, remember? lol
* Encoding
*   Modifying Source

    * Remove extraneous functionality
    * Remove usage statements and signatured strings

    Recompilation

    * Would change hash to attempt to avoid detection

## Defense Evasion and Execution

* Red team shoul be used against mature organizations who have:
  * Anti-Exploit
  * Application Control
  * EDR
  * Hunt Teams
* Red team does not want to get caught--yet
* Defense evasion supports other tactics

## Blending In

* There's a fine line between legit behavior and suspicious behavior
* Three popular scripting technologies at our disposal
  * Visual Basic
  * JavaScript (the windows one, JScript, which is diff than regular JavaScript I guess)
  * PowerShell

## Windows Script Host

* WScript / CScript
* Supports JavaScript and VBA

{% hint style="info" %}
Tip: Use the /t \<number of seconds> to set a kill timer on the script
{% endhint %}

## HTML Application

* Executed by **mshta.exe**
* <mark style="color:blue;">Can execute JavaScript(JScript), VBA, and PowerShell</mark>
* Delivered as an attachement or hosted file

### Demiguise: HTA Encryption Tool

* [https://github.com/nccgroup/demiguise](https://github.com/nccgroup/demiguise)
* Released by NCC Group
* Generates an HTML file with encrypted HTA (using RC4)
* Targets visits page, fetches key, and HTA is decrypted
* File-type will show text/html instead of HTA

## Visual Basic for Applications (VBA)

* Typically found in Microsoft Office, created in 1991
* Often used in macros to script repetitive tasks
* More powerful than VBScript
* VBA Source is stored insice a module stream of an Office Document

VBA Stomping

VBA Purging

## MSBuild

* Based off an XML schema for a project file
* Casey Smith @subTee discovered you can proxy execution through the trusted binary
* Steve Borosh @424f424f later released NoMSBuild to execute this technique without MSBuild.exe

## Intro to Powershell

Execution policy - helps avoid mistakingly executing a script

* Restricted - Script cannot be loaded via powershell.exe because of this policy

{% hint style="warning" %}
powershell -ExecutionPolicy Bypass -File test.ps1  bypasses execution policy temporarily
{% endhint %}

## C\#

## GhostPack

C# implementations of many techniques

Some GhostPack tools:

* Seatbelt
* SharpUp
* SharpRoast
* SharpDump
* SafetyKatz
* SharpWMI

{% embed url="https://github.com/matterpreter/OffensiveCSharp" %}

{% embed url="https://github.com/GhostPack" %}

## LOLBAS

{% embed url="https://github.com/LOLBAS-Project/LOLBAS" %}

## Regsvr32

* Used bywindows to register DLLs
* Bypasses application control
  * Trusted by Microsoft
* Can pull files from network or internet

### Empire SCT

## Rundll32

* Loads and runs dynamic-link libraries (DLLs)

## Shortcuts

<figure><img src="../../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

## Payload Testing

Test your payloads.... duh

## Execution Guardrails

Checks criteria on target system before payload execution

<figure><img src="../../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

## Keyed Payloads

Encrypting payloads with specific values, only decrypts on the correct target

<figure><img src="../../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

## Document IOCs
