# Credential Theft

**Credential Theft Mitigations**

* [https://www.microsoftpressstore.com/articles/article.aspx?p=2228450\&seqNum=9](https://www.microsoftpressstore.com/articles/article.aspx?p=2228450\&seqNum=9)
* [https://docs.microsoft.com/en-us/previous-versions/technet-magazine/dd822916(v=msdn.10)?redirectedfrom=MSDN](https://docs.microsoft.com/en-us/previous-versions/technet-magazine/dd822916\(v=msdn.10\)?redirectedfrom=MSDN)
* [https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560633(v=ws.10)?redirectedfrom=MSDN](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560633\(v=ws.10\)?redirectedfrom=MSDN)
* [https://docs.microsoft.com/en-us/security-updates/SecurityAdvisories/2016/2871997?redirectedfrom=MSDN](https://docs.microsoft.com/en-us/security-updates/SecurityAdvisories/2016/2871997?redirectedfrom=MSDN)
* [https://docs.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/configuring-additional-lsa-protection](https://docs.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/configuring-additional-lsa-protection)
* [https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn466518(v=ws.11)?redirectedfrom=MSDN](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn466518\(v=ws.11\)?redirectedfrom=MSDN)
* [https://docs.microsoft.com/en-us/windows/security/identity-protection/credential-guard/credential-guard](https://docs.microsoft.com/en-us/windows/security/identity-protection/credential-guard/credential-guard)

![](<../../.gitbook/assets/image (92).png>)

## Compromising Credentials

![](<../../.gitbook/assets/image (73) (1).png>)

![Logon types](<../../.gitbook/assets/image (28).png>)

### Hashes

* The password for each user account in Windows is stored in multiple formats, LM and NT hashes are most well-known. TsPkg, WDigest and LiveSSP can be decrypted to provide plaintext passwords (Prior to Windows 8.1)
* Dump LSASS with admin privs, then crack or Pass-The-Hash (preferred)

Tools: Mimikatz, fgdump, gsecdump, Metasploit, AceHash, PWDumpX, creddump, WCE

{% embed url="https://www.sans.org/blog/protecting-privileged-domain-accounts-safeguarding-password-hashes/" %}

### Tokens

* Delegate tokens are powerful auth resources used for SSO. They allow attackers to impersonate a users security context including over the network
*   How are they used/acquired?

    * The **SeImpersonate** privilege lets tokens be copies from processes, the new token can then be used to authenticate as the new user. A target user or service must be logged on or have running processes



Tools: Incognito, Metasploit, Powershell, mimikatz&#x20;

### Cached Credentials

* Stored domain credentials to allow logons when domain controller access is unavailable. Most systems cache the last 10 logon hashes by default
* Cached creds must be cracked (they're store in a registry hive \[**SECURITY\Cache key**]). Hashes are salted and case-sensitive, making decryption very slow. These hashes cannot be used for PtH attacks

Tools: cachedump, Metasploit, PWDumpX, creddump, AceHash

### LSA Secrets

* Credentials stored in the registry \[**SECURITY/Policy/Secrets**] to allow services or tasks to be run with user privs without user intervention. In addition to **service accounts,** may also hold application passwords like VPN or auto-logon credentials.
* How are they used / acquired
  * Admin privs allow access to encrypted registry data and the keys necessary to decrypt. Passwords are **plaintext**
* **Decrypting LSA Secrets (Nishang) \[ requires an administrator 32-bit Powershell console)**
  * **Get-LsaSecret.ps1** used from Nishang Powershell pentest framework used to dump LSA Secrets&#x20;

![](<../../.gitbook/assets/image (81).png>)

Tools: Cain, metasploit, mimikatz, gsecdump, acehash, creddump, powershell

### Tickets

* Kerberos issues tickets to authenticated users that can be reused without additional authentication. Tickets are cached in memory and are valid for 10 hours
* How are they acquired / used?
  * Tickets can be stolen from memory and used for authentication somewhere else (**Pass the ticket**)
  * Access to DC allows tickets to be created for any user with no expiration (**Golden Ticket**)
  * Service account tickets can be requested and forges, including offline cracking of service account hashes (**Kerberoasting**)

Tools: mimikatz, WCE, kerberoast

### NTDS.DIT

{% embed url="https://github.com/SecureAuthCorp/impacket/blob/master/examples/secretsdump.py" %}

## Defending Credentials

### Hashes

* Prevent admin account compromise
* Stop remote interactive sessions with highly priv accounts
* Proper termination of RDP sessions
  * Win 8.1 force the use of Restricted admin
  * Win 10 -> deploy Remote Credential Guard
* Upgrade to Windows 10
  * Credential Guard
  * Device Guard
  * Domain Protected User Accounts

### Tokens

* Stop remote interactive sessions with highly priv accounts
* Proper termination of RDP sessions
  * Win 8.1 -> force the use of Restricted admin
  * Win 10 -> deploy Remote Credential Guard
* Account designation of "Account is Sensitive and Cannot be Delegated" in Active Directory (this will block the token from being used over the network)
* Domain Protected User Account/Group

### Cached Credentials

### LSA Secrets

* Prevent admin account compromise (least priv)
* Do not employ services or schedule tasks requiring priv accounts on low-trust systems
* Reduce number of services that require domain accounts to execute
  * Heavily audit any accounts that must be used
* (Group) Managed Service Accounts (Changes password every 30 days by default + long complex passwords required)

### Tickets

{% embed url="https://dfirblog.wordpress.com/2015/12/13/protecting-windows-networks-kerberos-attacks/" %}

{% embed url="https://cert.europa.eu/static/WhitePapers/CERT-EU-SWP_14_07_PassTheGolden_Ticket_v1_1.pdf" %}

![](<../../.gitbook/assets/image (39) (1).png>)

* Credential Guard (Win 10+)
  * Domain protected users group (Win 8+): Some attacks
* Remote Credential Guard (Win 10+)
  * Restricted Admin (Win8+)
* Long and complex passwords on service accounts
  * Change service account passwords regularly
  * Group Managed Service Accounts are a great mitigation
* Audit service accounts for unusual activity
* Limit and protect Domain Admin
  * Change KRBTGT password regularly (yearly)

### NTDS.DIT
