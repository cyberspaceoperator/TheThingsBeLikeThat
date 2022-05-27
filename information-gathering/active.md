# Active

## DNS

Summary of tools

* dig
* host&#x20;
* dnsenum // swiss-army knife of dns
* dnsrecon
* fierce **** // **bruteforce** subdomains
* Sublist3r

Learn more about / practice these commands @ [https://digi.ninja/projects/zonetransferme.php](https://digi.ninja/projects/zonetransferme.php)

### Types of DNS Records

* **A** (address) records containing the IP address of the domain.
* **MX** records, which stands for Mail Exchange, contain the mail exchange servers.
* **CNAME** records used for aliasing domains. CNAME stands for Canonical Name and links any sub-domains with existing domain DNS records.
* **NS** records, which stands for Name Server, indicates the authoritative (or main) name server for the domain.
* **SOA** records, which stands for State of Authority, contain important information about the domain such as the primary name server, a timestamp showing when the domain was last updated and the party responsible for the domain.
* **PTR** or Pointer Records map an IPv4 address to the CNAME on the host. This record is also called a ‘reverse record’ because it connects a record with an IP address to a hostname instead of the other way around.
* **TXT** records contain text inserted by the administrator (such as notes about the way the network has been configured).

### **Fierce**&#x20;

A reconnaissance tool written in Perl to locate non-contiguous IP space and hostnames using DNS. This tool helps to locate likely targets both inside and outside corporate networks.

```
// Performing a dns bruteforce, 
// Fierce uses a wordlist, or you can use your own
$ fierce -dns google.com
$ fierce -dns google.com --wordlist
$ fierce -h
```

![Output of fierce](<../.gitbook/assets/image (15).png>)

### Dig

```
// Find MX records

$ dig -t mx google.com

// Find any records

$ dig -t any google.com

// Perform (or attempt to) a domain zone transfer
// You MUST know the IP/Host AND Domain name server IP/Name

$ dig axfr <host> <domain>
```

### Host

```
$ host -t axfr -l google.com ns1.google.com
//-l lists all hosts in a domain, using AXFR
$ host -l google.com
```

### DNSENUM

DNSenum is a Perl script that can be used to enumerate the DNS information of a domain and to discover non-contiguous IP blocks. This tool will also attempt zone transfers on all the related domain name servers.

![](<../.gitbook/assets/image (16).png>)

### DNSRECON

DNSrecon is another automated tool that can be used to query DNS records, check for zone transfers and other DNS related information. This tool shows more or less the same output as we’ve already seen in the other (automated) DNS reconnaissance tools.

```
dnsrecon -d google.com
```

### Sublist3r

Sublist3r utilises popular search engines such as Google, Bing, Yahoo and Baidu to discover subdomains for a selected domain name.

![](<../.gitbook/assets/image (17).png>)

