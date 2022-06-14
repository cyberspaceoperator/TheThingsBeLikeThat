# Attacking WPA2-Enterprise Networks

### Exploiting client PNLs

![](<../../../.gitbook/assets/image (50).png>)

{% hint style="info" %}
airgraph-ng is kinda old
{% endhint %}

```
//convert pcap to csv
airodump-ng -r <filname.pcap> -w <outputfile>

//create graph
airgraph-ng -i <previously output file above> -g CPG -o <outputfile>
```

#### Beacongraph (better than airgraph, uses Neo4J, like Bloodhound)

* [https://github.com/daddycocoaman/BeaconGraph](https://github.com/daddycocoaman/BeaconGraph)

![](<../../../.gitbook/assets/image (45) (1) (1).png>)

### PEAP

{% embed url="https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-xp/bb457039(v=technet.10)?redirectedfrom=MSDN" %}

{% embed url="https://en.wikipedia.org/wiki/Extensible_Authentication_Protocol" %}

* To use TLS as a component of PEAP, the server needs to be configured with a digital certificate

![](<../../../.gitbook/assets/image (61).png>)

![](<../../../.gitbook/assets/image (58) (1) (1).png>)

![](<../../../.gitbook/assets/image (34) (1) (1).png>)

#### Sniffair [https://github.com/Tylous/SniffAir](https://github.com/Tylous/SniffAir)

* Password spraying against PEAP
