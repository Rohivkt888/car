---
title: "CAR-2013-10-001: User Login Activity Monitoring"
layout: analytic
submission_date: 2013/10/03
information_domain: Host, Network
subtypes: Login, Netflow
analytic_type: Situational Awareness
contributors: MITRE
---

Monitoring logon and logoff events for hosts on the network is very important for situational awareness. This information can be used as an indicator of unusual activity as well as to corroborate activity seen elsewhere.

Could be applied to a number of different types of monitoring depending on what information is desired. Some use cases include monitoring for all remote connections and building login timelines for users.
Logon events are Windows Event Code 4624 for Windows Vista and above, 518 for pre-Vista. Logoff events are 4634 for Windows Vista and above, 538 for pre-Vista.

### Output Description

The time of login events for distinct users on individual systems


### ATT&CK Detection

|Technique|Tactic|Level of Coverage|
|---|---|---|
|[Remote Desktop Protocol](https://attack.mitre.org/techniques/T1076/)|[Lateral Movement](https://attack.mitre.org/tactics/TA0008/)|Moderate|
|[Valid Accounts](https://attack.mitre.org/techniques/T1078/)|[Defense Evasion](https://attack.mitre.org/tactics/TA0005/)|Moderate|


### Implementations

#### Account Logon with Filtering (Pseudocode)


This base pseudocode looks for user logon events and filters out the top 30 account names to reduce the occurrence of noisy service accounts and the like. It is meant as a starting point for situational awareness around such events.


```
logon_events = search User_Session:Login
filtered_logons = filter logon_events where (
  user NOT IN TOP30(user))
output filtered_logons
```


#### Account Logon with Filtering (Splunk)


Splunk version of the above pseudocode. NOTE - this is liable to be quite noisy and will need tweaking, especially in terms of the number of top users filtered out.


```
index=__your_win_event_log_index__ EventCode=4624|search NOT [search index=__your_win_event_log_index__ EventCode=4624|top 30 Account_Name|table Account_Name]
```


