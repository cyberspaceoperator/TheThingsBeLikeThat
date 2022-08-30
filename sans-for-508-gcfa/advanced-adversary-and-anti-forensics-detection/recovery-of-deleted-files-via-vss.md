# Recovery of Deleted Files via VSS

## Volume Shadow Copies

* Can provide backups of nearly the entire volume to earlier points in time
  * Triggered by events in Windows XP
  * DLLs, .exe's, drivers, and registry files
* Similar to virtual machine snapshots
* Recover key files (event logs, registry, malware, even wiped files)
* Introductions of "ScopeSnapshots" in windows 8/10 limits effectiveness&#x20;

### Volume Shadow Examination

* **Triage Analysis**
  * KAPE
  * Velociraptor
* **Full-Volume Analysis**
  * Arsensal Image Mounter
  * F-Response
  * vshadowmount ([https://github.com/libyal/libvshadow](https://github.com/libyal/libvshadow))

{% embed url="https://github.com/libyal/libvshadow" %}
llibvshadow
{% endembed %}

### vshadow

#### vshadowinfo

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>vshadowinfo</p></figcaption></figure>

#### vshadowmount

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption><p>vshadowmount</p></figcaption></figure>

