# Commonly Used // Useful Commands // Things to not forget

### Usefully Random

```
///If SMB_Login shows login works, but you can't login, \
check to see that the password doesn't need to be changed.



```

### Finding Passwords / Creds

{% hint style="info" %}
DO NOT RELY ON COMMON WORDLISTS, CREATE YOUR OWN IF POSSIBLE

**Always look for hidden files AND alternate data streams**
{% endhint %}

```
cewl <url> --with-numbers
```

### Disabling Firewall / Enabling RDP

```
# Disable Windows Firewall

NetSh Advfirewall set allprofiles state off

# Enable RDP through registry

reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-TCP" /v UserAuthentication /t REG_DWORD /d "0" /f
```

Starting / Stopping / Querying Windows services

```bash
# SC is Newer than Net, so if one doensn't work, 
# try the other one
sc start <service name>
sc stop <service name>
net start <service name>
net stop <service name>

sc query <service name>
sc query # queries all services (you might not have permission to do this)

sc sdshow <service name> 
#THIS SHOWS PERMISSIONS YOU HAVE TO A SERVICE

```

### Importing a Shell

```
python -c 'import pty; pty.spawn("/bin/bash")'
```

### SMB

```
# connect to an unauthenticated SMB Share
smbclient \\\\<ip>\\<sharename>
```

### Finding Files (Linux)

```bash
LINUX:

World writable directories:

find / \( -wholename '/home/homedir*' -prune \) -o \( -type d -perm -0002 \) -exec ls -ld '{}' ';' 2>/dev/null | grep -v root

World writable directories for root:

find / \( -wholename '/home/homedir*' -prune \) -o \( -type d -perm -0002 \) -exec ls -ld '{}' ';' 2>/dev/null | grep root

World writable files:

find / \( -wholename '/home/homedir/*' -prune -o -wholename '/proc/*' -prune \) -o \( -type f -perm -0002 \) -exec ls -l '{}' ';' 2>/dev/null

These next commands print the search results to the terminal without any additional information.

Find world writable files in /etc:

find /etc -perm -2 -type f 2>/dev/null

And the following command searches for world writable directories:

find / -writable -type d 2>/dev/null

WINDOWS:

Find ALL Hidden files (Powershell)
PS C:\&gt; Get-ChildItem C:\myfolder\ -Recurse -Force | Where { ($_.Attributes.ToString() -Split ", ") -Contains "Hidden" } | Select FullName


```

### Finding Files (Windows)

```
# Find ALL Hidden files (Powershell)
PS C:\&gt; Get-ChildItem C:\myfolder\ -Recurse -Force | Where { ($_.Attributes.ToString() -Split ", ") -Contains "Hidden" } | Select FullName
```

