# Lateral Movement Adversary Tactics

## <mark style="color:blue;">Remote Desktop Services</mark>

### Source System Artifacts

### Destination System Artifacts

* AmCache.hve
  * rdpclip.exe
  * tstheme.exe
* ShimCache - SYSTEM
  * rdpclip.exe
  * tstheme.exe

## <mark style="color:blue;">Windows Admin Shares</mark>

### Source System Artifacts

{% hint style="info" %}
C$ , ADMIN$, IPC$ Important shares
{% endhint %}

* 4648 Event possibly
* ShimCache - SYSTEM
  * net.exe
  * net1.exe

### Destination System Artifacts

* 4624 Logon type 3
* 5140 - Share access

## <mark style="color:blue;">PsExec</mark>

### Source System Artifacts

{% hint style="danger" %}
Source is psexec.exe, destination is psexesvc.exe
{% endhint %}

* Shimcache / Amcache
  * PsExec.exe

### Destination System Artifacts

* Authenticates to destination system
* Named pipes are used to communicate between source and target
* Mounts hidden ADMIN$ share
* Copies PsExeSvc.exe and any other binaries to Windows folder
* Executes code via a service (PSEXESVC)\

* 4624 Logon type 3
* 5140 - Share access
* 7045 - Service install\

* Shimcache / amcache&#x20;
  * psexesvc.exe\

* Registry (new service creation)
  * SYSTEM\currentcontrolset\services\psexesvc\


## <mark style="color:blue;">Windows Remote Management Tools</mark>

### Remote Services(not inclusive list)

* sc
* at
* schtasks
* reg add
* winrs

### Scheduled Tasks

## <mark style="color:blue;">WMI</mark>

### Source System Artifacts

wmic.exe

### Destination System Artifacts

wmiprvse.exe

* Shimcache - SYSTEM
  * scrcons.exe
  * mofcomp.exe
  * wmiprvse.exe
  * evil.exe
* AmCache.hve - First time executed
  * scrcons.exe
  * mofcomp.exe
  * wmiprvse.exe
  * evil.exe

## <mark style="color:blue;">Powershell Remoting</mark>

### Source System Artifacts

### Destination System Artifacts

