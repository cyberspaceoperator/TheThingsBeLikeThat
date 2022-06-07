# Exercise 2.3 - Tracking Lateral Movement

### Tracking mounted shared

* Identify 5140 events
  * Look for accounts
  * Look for if people are mounting remotely
  * Look at what people are mounting in general
* Identify people adding shared (5142)
* Identify all SMB Share events (5140-5145)
  * Did anybody add a share?
* Look for 4697 event (new service installations)
  * Look for 7045 events which only occur the first time a service is installed
* Need to map a SID in the System log to a username in the security log? Say no more
  * All you need to do is filter for  New Logon\Security ID \<input SID here>&#x20;
*

### Finding suspicious Scheduled tasks

* Filter for event ID 106 in the Microsoft-Windows-TaskScheduler%4Operational.evtx log
* Filter for suspicious tasks
  * Scheduled tasks create XML files in \Windows\System32\Tasks folder

### Audit event log clearing

Event ID 1102
