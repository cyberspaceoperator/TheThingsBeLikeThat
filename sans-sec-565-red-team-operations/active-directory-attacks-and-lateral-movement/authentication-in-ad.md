# Authentication in AD

## Components

Winlogon.exe - Process responsible for passing credentials to the Local Security Authority (LSA)

The LSASS Local Authority Subsystem process is resposible for user authentication and authorization

LSA will make tyhe decision whether the authentication is local and should be handled by the Security Accounts Manager (SAM) or the Key Distribution Center (KDC)

<figure><img src="../../.gitbook/assets/image (123).png" alt=""><figcaption><p>authentication flow</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

### Kerberos

* Supports ~~<mark style="color:red;">des</mark>~~, rc4, aes128, aes256

<figure><img src="../../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>
