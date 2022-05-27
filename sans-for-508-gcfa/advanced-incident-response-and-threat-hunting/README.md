# Advanced Incident Response and Threat Hunting

## Incident Response Process (6 Step)

* <mark style="color:blue;">**Preparation**</mark>
* <mark style="color:blue;">**Identification and Scoping**</mark>
  * What, where, when
* <mark style="color:blue;">**Containment / Intel Dev (Goal: Degrade the capabilities of the adversary)**</mark>
  * **Intel Development**
    * TTP observation
    * Understand adversary intent
    * Malware Gathering
    * IOC dev
    * Campaign Identification
  * **Containment / Active Defense**
    * Prevent or slow additional access during monitoring and collection phase
    * Full-scale host/network monitoring
    * Data decoy
    * Bit mangling
    * Traffic shaping
    * Adversary network segmentation / kill-switch
* <mark style="color:blue;">**Eradication / Remediation (Goal: Get the adversary OFF the network)**</mark>
  * <mark style="color:blue;">**3 Steps:**</mark>
    1. &#x20;<mark style="color:blue;">****</mark> <mark style="color:blue;"></mark><mark style="color:blue;">Posture for remediation</mark>
    2. <mark style="color:blue;">Execute remediation</mark>
    3. <mark style="color:blue;">Implement and apply additional security controls</mark>
  * <mark style="color:blue;">**Example Tasks**</mark>
    1. <mark style="color:blue;">Disconnect environment from internet</mark>
    2. <mark style="color:blue;">Implement strict network segmentation not allowing subnets to communicate with each other</mark>
    3. <mark style="color:blue;">Block IP addresses and domain names for known C2 channels</mark>
    4. <mark style="color:blue;">Remove infected systems that maintained active or previous active malware on the host</mark>
    5. <mark style="color:blue;">Remove all systems identified as compromised but do not show signed of infection via malware</mark>
    6. <mark style="color:blue;">Restrict access to known compromised accounts</mark>
    7. <mark style="color:blue;">Restrict access to domain admin accounts</mark>
    8. <mark style="color:red;">Validate that everything is done properly</mark>
  * Block malicious IPs
  * Blackhole malicious domain names
  * Rebuild systems
  * Deny access to environment
  * Eliminate ability for adversary to react to the remediation
  * Remove presence of adversary from the environment
  * Degrade the ability of the adversary to return
* <mark style="color:blue;">**Recovery**</mark>
  * Network redesign
  * Enhanced Password Portal
  * Centralized logging
  * Enhanced Network Vis
  * Improve enterprise authentication model
  * etc
* <mark style="color:blue;">**Lessons Learned**</mark>

{% hint style="warning" %}
Don't just start pulling shit off the network as a knee-jerk reaction, it WILL tip off the adversary
{% endhint %}

## &#x20;Real-Time Remediation

![Traditional Incident Response Model](<../../.gitbook/assets/image (23).png>)

Once an organization works their way all the way up the pyramid, they should have the capability to remediate, respond, etc in real-time. Visibility of the entire network is crucial and takes a lot of time. Most organizations aren't all the way up the top of the pyramid acting at the same pace as attackers.

![](<../../.gitbook/assets/image (65).png>)

## Reactive Response vs. Threat Hunting

### Reactive Organization

* Incident starts when notification comes in
  * Call from agency
  * Vendor/threat info
  * Security appliance alert
  * "Five-alarm fire" response

### Hunting Organization

* Actively looking for incidents
  * Known malware and varients
  * Patterns of activity
  * Threat intel
  * Security 'patrols'
* Reduce adversary dwell time (Goal of Hunt)

## Threat Hunting Process

![](<../../.gitbook/assets/image (54).png>)

![Page 57 Book 1](<../../.gitbook/assets/image (32).png>)

### Team Roles

* Team Lead
* 1-2 Endpoint / host / cloud analysts
* 1-2 Network analysts
* 1 Reverse engineering malware specialist
* 1 DevOps / tool development resource&#x20;

## Threat Hunting: "Compromise Type"

### Detection Types

1. Systems with active malware
2. Systems with Dormant Malware (Not active or cleaned)
3. Systems without Tools of Malware (Living off the land)

![The numbers are associated with the Compromise Type above](<../../.gitbook/assets/image (41).png>)
