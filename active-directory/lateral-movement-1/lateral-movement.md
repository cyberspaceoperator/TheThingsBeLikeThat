---
description: Move laterally w/ Bloodhound
---

# SharpHound / BloodHound



{% hint style="info" %}
Always up to date Bloodhound / Sharphound files
{% endhint %}

{% embed url="https://github.com/BloodHoundAD/BloodHound/tree/master/Ingestors" %}

## Bloodhound / SharpHound

If you find yourself in a situation where you have compromised an Active Directory host as a domain user and can upload files to that computer some way, consider trying to run SharpHound.exe on the computer.\
Run Sharphound.exe first, which will then be imported into BloodHound on your Kali Host.

![Running Sharphound.exe](<../../.gitbook/assets/image (5) (1).png>)

From there we start neo4j database ($ neo4j start) and then run bloodhound

{% hint style="warning" %}
**`To get the database populated, you must drag and drop the .zip file into Bloodhound, I found it will not accept separated .json files. It must be the entire zip.`**
{% endhint %}

![](<../../.gitbook/assets/image (6).png>)

You can see that that in this example Administrator is a member of the Domain Admins group, not really a surprise. So given that we have access to a normal user, we would need to query the user.

After querying th user we see no path to the Domain Admins group, so either we are missing something or (more likely) we need to get access to another user.

![](<../../.gitbook/assets/image (7).png>)
