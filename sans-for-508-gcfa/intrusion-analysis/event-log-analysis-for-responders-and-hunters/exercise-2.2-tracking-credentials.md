# Exercise 2.2 - Tracking Credentials

## Objectives

* Learn to filter with Event Log Explorer
* Identify suspicious local account usage
* Use logon types to identify interesting logon activity
* Profile remote desktop (RDP) usage
* Audit accounts with administrator privileges

## Option 1: Tracking Credential Use with Event Log Explorer

1. Identify date range in security event log
   1. sort by date column to find high and low range
2. Filter log to audit successful console logins (4624 Type 2 events)
   1. Found any unsuccessful? Investigate and filter for the account that failed login
3. Filter for 4776 Account logon authentication events (local accounts over the network as opposed to domain accounts)
4. Audit RDP activity: 4778
5. Audit Admin account activity: 4672 / Text in description \<possiblle compromised account>
6. Filter for account abuse: 4672 / Text in description: "\<example SID from network that isn't from a built-in account>"
7. <img src="../../../.gitbook/assets/image (66).png" alt="" data-size="original">
8.

## Option 2: Tracking Credential use with EvtxECmd
