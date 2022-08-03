# Event Log Analysis for Responders and Hunters

## <mark style="color:orange;">Event Log Summary</mark>

![](<../../../.gitbook/assets/image (26) (2).png>)

### How to collect logs

![](<../../../.gitbook/assets/image (45).png>)

![](<../../../.gitbook/assets/image (85).png>)

### Scaling event log analysis

* Step 1: Collect event logs
  * SIEM, Splunk, **GrayLog (popular),** etc
  * Event forwarding
* Step 2: Build triggers (examples)
  * Powershell Using "IEX"
  * 4624 Type 10 to workstations
  * Service acct interactive logons

## Finding Event Logs

NT / Win2000 / XP / Server 2003

* .evt file type
* %systemroot%\System32\config
* Filenames: SecEvent.evt, AppEvent.evt, SysEvent.evt

Vista / Win 7 / Win 8 / 2008 / 2012 / Win10 / 2016

* .evtx file type
* %systemroot%\system32\winevt\logs
* Remote log server
* Filenames: Security.evtx, Application.evtx, System.evtx, etc

Can change log locations from registry key

### Security log

{% embed url="https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-vista/cc766468(v=ws.10)?redirectedfrom=MSDN" %}

What is recorded?

* Account logon
* Account mgmt
* Directory service
* Logon Events
* Object Access
* Policy Change
* Privilege Use
* Process Tracking
* System Events

![](<../../../.gitbook/assets/image (84).png>)

### Logon Types

![](<../../../.gitbook/assets/image (85) (1).png>)

Event IDs 528 and 540 signify a successful logon, event ID 538 a logoff and all the other events in this category identify different reasons for a logon failure. However, just knowing about a successful or failed logon attempt doesn’t fill in the whole picture. Because of all the services Windows offers, there are many different ways you can logon to a computer such as interactively at the computer’s local keyboard and screen, over the network through a drive mapping or through terminal services (aka remote desktop) or through IIS. Thankfully, logon/logoff events specify the Logon Type code which reveals the type of logon that prompted the event.

We have also compiled all logon type codes and their explanations into a one-page cheat sheet that you can use as a quick reference. [Download it from here.](https://techgenix.com/tgwordpress/wp-content/uploads/2017/02/logon\_types\_combined.pdf)

#### Logon Type 2 – Interactive

This is what occurs to you first when you think of logons, that is, a logon at the console of a computer. You’ll see type 2 logons when a user attempts to log on at the local keyboard and screen whether with a domain account or a local account from the computer’s local SAM. To tell the difference between an attempt to logon with a local or domain account look for the domain or computer name preceding the user name in the event’s description. Don’t forget that logon’s through an KVM over IP component or a server’s proprietary “lights-out” remote KVM feature are still interactive logons from the standpoint of Windows and will be logged as such.

#### Logon Type 3 – Network

Windows logs logon type 3 in most cases when you access a computer from elsewhere on the network. One of the most common sources of logon events with logon type 3 is connections to shared folders or printers. But other over-the-network logons are classed as logon type 3 as well such as most logons to IIS. (The exception is basic authentication which is explained in Logon Type 8 below.)

#### Logon Type 4 – Batch

When Windows executes a scheduled task, the Scheduled Task service first creates a new logon session for the task so that it can run under the authority of the user account specified when the task was created. When this logon attempt occurs, Windows logs it as logon type 4. Other job scheduling systems, depending on their design, may also generate logon events with logon type 4 when starting jobs. Logon type 4 events are usually just innocent scheduled tasks startups but a malicious user could try to subvert security by trying to guess the password of an account through scheduled tasks. Such attempts would generate a logon failure event where logon type is 4. But logon failures associated with scheduled tasks can also result from an administrator entering the wrong password for the account at the time of task creation or from the password of an account being changed without modifying the scheduled task to use the new password.

#### Logon Type 5 – Service

Similar to Scheduled Tasks, each service is configured to run as a specified user account. When a service starts, Windows first creates a logon session for the specified user account which results in a Logon/Logoff event with logon type 5. Failed logon events with logon type 5 usually indicate the password of an account has been changed without updating the service but there’s always the possibility of malicious users at work too. However this is less likely because creating a new service or editing an existing service by default requires membership in Administrators or Server Operators and such a user, if malicious, will likely already have enough authority to perpetrate his desired goal.

#### Logon Type 7 – Unlock

Hopefully the workstations on your network automatically start a password protected screen saver when a user leaves their computer so that unattended workstations are protected from malicious use. When a user returns to their workstation and unlocks the console, Windows treats this as a logon and logs the appropriate Logon/Logoff event but in this case the logon type will be 7 – identifying the event as a workstation unlock attempt. Failed logons with logon type 7 indicate either a user entering the wrong password or a malicious user trying to unlock the computer by guessing the password.

#### Logon Type 8 – NetworkCleartext

This logon type indicates a network logon like logon type 3 but where the password was sent over the network in the clear text. Windows server doesn’t allow connection to shared file or printers with clear text authentication. The only situation I’m aware of are logons from within an ASP script using the ADVAPI or when a user logs on to IIS using IIS’s basic authentication mode. In both cases the logon process in the event’s description will list advapi. Basic authentication is only dangerous if it isn’t wrapped inside an SSL session (i.e. https). As far as logons generated by an ASP, script remember that embedding passwords in source code is a bad practice for maintenance purposes as well as the risk that someone malicious will view the source code and thereby gain the password.

#### Logon Type 9 – NewCredentials

If you use the RunAs command to start a program under a different user account and specify the /netonly switch, Windows records a logon/logoff event with logon type 9. When you start a program with RunAs using /netonly, the program executes on your local computer as the user you are currently logged on as but for any connections to other computers on the network, Windows connects you to those computers using the account specified on the RunAs command. Without /netonly Windows runs the program on the local computer and on the network as the specified user and records the logon event with logon type 2.

#### Logon Type 10 – RemoteInteractive

When you access a computer through Terminal Services, Remote Desktop or Remote Assistance windows logs the logon attempt with logon type 10 which makes it easy to distinguish true console logons from a remote desktop session. Note however that prior to XP, Windows 2000 doesn’t use logon type 10 and terminal services logons are reported as logon type 2.

#### Logon Type 11 – CachedInteractive

#### Windows supports a feature called Cached Logons which facilitate mobile users. When you are not connected to the your organization’s network and attempt to logon to your laptop with a domain account there’s no domain controller available to the laptop with which to verify your identity. To solve this problem, Windows caches a hash of the credentials of the last 10 interactive domain logons. Later when no domain controller is availab

### Built-in accounts (instructor recommends just ignoring these)

* SYSTEM
* LOCAL SERVIUCE
* NETWORK SERVICE
* \<HOSTNAME>$
* DWM AND UMFD
* ANONYMOUS LOGON

![](<../../../.gitbook/assets/image (87).png>)

![](<../../../.gitbook/assets/image (71).png>)

## Tracking Account Usage

### Brute Force Password Attack

Event IDs:

### Administrator Account Activity

Event IDs:

### Account creation

Event IDs:

* 4722: Account enabled
* 4724: An attempt was made to reset an accounts password
* 4728: Member was added to security enabled global group
* 4732: Member was added to a security-enabled local group
* 4735: A security-enabled local group was changed
* 4738: A user account was changed
* 4756: A member was added to a security-enabled universal group

### RDP Usage

Event IDs:

* 4778: Session reconnected
* 4779: Session disconnected

Logs: Remote Desktop Services-RDPCoreTS and TerminalServices-RdpClient

![](<../../../.gitbook/assets/image (47).png>)

### Account Logon Events

Event IDs:&#x20;

* 4776: Successful/Failed account authentication (NTLM authentication) ([https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4776](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4776))
* 4768: Ticket granting ticket was granted
* 4769: Service ticket requested
* 4771: Pre-authentication failed ([https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4771](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4771))
  * Corresponding Windows XP events: 672,673,675,680

Error codes:

![logon error codes screenshot](<../../../.gitbook/assets/image (73).png>)

## Tracking Reconnaissance

Event IDs (Windows 10 / Server 2016 and up):

* 4798: A user's local group membership was enumerated
* 4799: A security-enabled local group membership was enumerated

## Tracking Lateral Movement

### Network shares

Event IDs

* 5140: Network share was accessed (doesn't report file reference, turn on Detailed file share auditing to generate a 5145)
* 5145: Shared Object accessed (Turn on Detailed File Share auditing to get this)
* 5142-5144: created/modified/deleted

### Cobalt strike mapping shares



### Explicit credentials / runas

4624: Successful Logon (Logon type 9)

4648: Logon using explicit credentials

### Scheduled Tasks

Event IDs

Can be scheduled locally or remotely, remote scheduled tasks also cause Logom,. n ID 4624 Type 3

* 106 | 4698: Created (Task scheduler | security log)
* 140 | 4702: Updated (Task scheduler | security log)
* 141 | 4699: Deleted  (Task scheduler | security log)
* 200 / 201:Executed / Completed (Task scheduler log)
* 4700 / 4701: enabled / disabled (Security log)

If logging isn't enabled for scheduled tasks, you can also look in %systemroot%\system32\tasks folder

### Suspicious Services (lots of noise but might yield some results)

* 7034: Service crashed unexpectedly&#x20;
* 7035: Service sent a Start/Stop control
* 7036: Service started or stopped
* 7040: Start type changed (Boot | On Request | Disabled)
* 7045: A new service was installed on the system (Win2008R2+)
* 4697: A new service was installed on the system (**Security** Log)

### Event log clearing

Admin rights needed

Clearing is all or nothing

1102: Audit log cleared (Security log)

104: Audit log cleared (System log)

517 : Windows XP equivalent ID

## Event Log Attacks

* Mimikatz event::drop
* DanderSpritz eventlogedit [https://blog.fox-it.com/2017/12/08/detection-and-recovery-of-nsas-covered-up-tracks/](https://blog.fox-it.com/2017/12/08/detection-and-recovery-of-nsas-covered-up-tracks/)
* Invoke\_Phant0m :: thread killing [https://github.com/hlldz/Phant0m](https://github.com/hlldz/Phant0m)
* Event log service suspension; memory-based attacks
* Arbitrary events can be removed, including EID 1102

Mitigation:

* Event log forwarding
* Loggin "heartbeat"
* Log gap analysis

## Tools

{% embed url="https://github.com/EricZimmerman/evtx/tree/master/evtx/Maps" %}

{% embed url="https://eventlogxp.com/" %}

{% embed url="https://github.com/hlldz/Phant0m" %}

{% embed url="https://blog.fox-it.com/2017/12/08/detection-and-recovery-of-nsas-covered-up-tracks/" %}
