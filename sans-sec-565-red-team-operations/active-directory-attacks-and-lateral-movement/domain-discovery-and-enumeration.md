# Domain Discovery and Enumeration

## Enumeration Mental Model

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

*   <mark style="color:green;">**Host recon**</mark>

    * What defenses are in place?
    * What users are logged in?
    * What programs are running?
    * What privileges do I have?
    * Can I access shared?
    * Am I a local admin?
    * Can I modify properties of any objects in AD?



### IOCs

* Understand what things you are leaving behind

### Enumerate User Accounts

* Beware of honeypots
  * last logon time weird, maybe never logged on at all

### Enumerating Service accounts

* If password last set is years ago, this is common
* if service logon count is 0, that's weird

## Tool Overview

### Powerview / Sharpview

### Bloodhound

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>cheat sheet boundhound</p></figcaption></figure>

### ADExplorer



