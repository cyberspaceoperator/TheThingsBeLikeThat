# Command Line, PowerShell, and WMI Analysis

## Evidence of Malware Execution

System event log

* Review Critical, Warning, and Error events for system and process **crashes**
* Application Event Log
  * Event ID 1000-1002 - Windows Error Reporting (WER), Application crashes and hangs

### Windows Error Reporting (report.wer) \[C:\Program Data\Microsoft\Windows\WER

### Process Tracking and Capturing Command Lines

* 4688 (Security log): New process created (includes executable path)
* 4689 (Security log): Process exit

![](<../../.gitbook/assets/image (24) (2).png>)

## WMI Attacks

### Reconnaissance

* Example wmic recon (also run by admins, filter the noise):
  * wmic process get CSName, Description, ExecutablePath, ProcessId
  * wmic useraccount list full
  * wmic group list full
  * wmic netuse list full
  * wmic qfe get Caption,Description,HotFixID,InstalledOn
  * wmic startup get Caption,Command,Location,User

### Privilege Escalation

*   **PowerUp.ps1** (leverages WMI) from the PowerShellEmpire suite of tools

    * Find unquoted services set to auto-start
    * Find highly-privleged processes that can be attacked
    * Find all paths to service .exe's that have a space in the path and aren't quotes


* [https://github.com/PowerShellEmpire/PowerTools/blob/master/PowerUp/PowerUp.ps1](https://github.com/PowerShellEmpire/PowerTools/blob/master/PowerUp/PowerUp.ps1)

![](<../../.gitbook/assets/image (54).png>)

### Lateral Movement

* "WMIC process call create" - designed to allow local and remote code execution. You can think of it as a built-in version of PsExec with a lot more stealth.&#x20;

### Capturing WMI Command Lines

* Look for 4688 EID
* Use SysInternals "Sysmon"&#x20;
* Commercial EDR tools

### Auditing WMI Persistence

* EIDs
  * 5858 (**WMI-Activity/Operational log**) records query errors, including host and username
  * 5857-5861 (**WMI-Activity/Operational log**) record filter/consumer activity
  * 5861 (**WMI-Activity/Operational log**): most useful: new permanent event consumer creation (uses **WMIPrvSE.exe** to execute)

![](<../../.gitbook/assets/image (42).png>)

## PowerShell (best logging is on PSv5)

### PowerShell-Specific Logging (<mark style="color:blue;">Microsoft-Windows-Powershell/Operational log</mark>)

* 4103: Module logging and pipeline output
* 4104: Script block logging
* 4105/4106: Script start/stop (not recommended)

### Powershell stealth (powershell one-liners)

&#x20;

![](<../../.gitbook/assets/image (58) (1).png>)

### Powershell Cheatsheet

![](<../../.gitbook/assets/image (40) (2).png>)

### Powershell Obfuscation

![](<../../.gitbook/assets/image (44).png>)

### Powershell Transcript Logs

* Turn on Powershell transcription (can be forwarded to offsite/remote server to protect/duplicate)
  * Enabled by group policy
    * Computer Configuration/Administrative/Templates/Windows Components/Windows Powershell/Turn on Powershell Transcription
* Logs all commands types and the output of those commands
  * Written to \Users\\\<account>\Documents

### PSReadline \[ConsoleHost\_history.txt] (on by default)

* Records last 4096 commands types in PS console (not ISE)
* Attackers can disable (or remove PSReadLine module)
  * Set-PSReadLineOption - HistorySaveStyle SaveNothing
  * Remove-Module -Name PsReadLine

![event log cheatsheet / summary](<../../.gitbook/assets/image (26) (2).png>)

![Get-WinEvent and Powershell](<../../.gitbook/assets/image (85).png>)

### Sysmon logging

* Microsoft-Windows-Sysmon/Operational log

{% hint style="info" %}
Try to utilize this for the final challenge
{% endhint %}

![](<../../.gitbook/assets/image (53) (1).png>)
