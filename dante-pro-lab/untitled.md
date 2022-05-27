# Information Gathering

## Nmap

```bash
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-10 00:15 EDT
Nmap scan report for 10.10.110.100
Host is up (0.079s latency).
Not shown: 997 filtered ports
PORT      STATE SERVICE VERSION


21/tcp    open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV IP 172.16.1.100 is not the same as 10.10.110.100
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.14.2
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status


22/tcp    open  ssh     OpenSSH 8.2p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)


65000/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-robots.txt: 2 disallowed entries 
|_/wordpress DANTE{Y0u_Cant_G3t_@_M3_Br0!}
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.02 seconds

```

**Port 21 is always fun when it has anonymous access, let's try it out.**

![finding todo.txt](<../.gitbook/assets/image (19).png>)

**The contents of todo.txt are revealing ( I added some notes underneath the original notes.**

> ```bash
> - Finalize Wordpress permission changes - PENDING
>    → maybe default creds?
>    → james:Toyota
>       ⇒ Creds dont work for ftp or ssh
>       ⇒ Plugin upload does not work
>       ⇒ Template editor does not work
>       
> - Update links to to utilize DNS Name prior to changing to port 80 - PENDING
>    → Maybe not port 80 yet? Higher port....ahhh 65000
>    
> - Remove LFI vuln from the other site - PENDING
>    → what other site?
>    
> - Reset James' password to something more secure - PENDING
>    → Okay, cool story bro.
>    → james:Toyota
>    
> - Harden the system prior to the Junior Pen Tester assessment - IN PROGRESS
>    → Worst....admin....ever.
> ```

**Port 65000 is pretty weird, let's check that out.**\
**Just an apache default page, which is suspect. could be a new server / misconfig.**

![](<../.gitbook/assets/image (18).png>)

**Lets dig deeper to find anything in here**

```bash
# gobuster dir -u http://10.10.110.100 -w /usr/share/wordlists/dirbuster/directory-2.3-medium.txt

/wordpress
```

```bash
wpscan identifies users admin and james
wpscan also finds james password "Toyota"
```

