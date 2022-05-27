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

![](<../../.gitbook/assets/image (73).png>)

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
* Cached creds must be cracked (they're store in a registry hive \[SECURITY\Cache key]). Hashes are salted and case-sensitive, making decryption very slow. These hashes cannot be used for PtH attacks

Tools: cachedump, Metasploit, PWDumpX, creddump, AceHash

### LSA Secrets

### Tickets

### NTDS.DIT

## Defending Credentials

### Hashes

* Prevent admin account compromise
* Stop remote interactive sessions with highly priv accounts
* Proper termination of RDP sessions
  * Win 8.1 force the use of Restricted admin
  * Win 10 -> deploy Remote Credential Guard
* Upgrade to Windows 10
  * Credential guard
  * Domain Protected User Accounts

### Tokens

* Stop remote interactive sessions with highly priv accounts
* Proper termination of RDP sessions
  * Win 8.1 force the use of Restricted admin
  * Win 10 -> deploy Remote Credential Guard
* Account designation of "Account is Sensitive and Cannot be Delegated" in Active Directory (this will block the token from being used over the network)
* Domain Protected User Account/Group

### Cached Credentials

### LSA Secrets

### Tickets

### NTDS.DIT
