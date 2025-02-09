---
title: "CAR-2013-07-005: Command Line Usage of Archiving Software"
layout: analytic
submission_date: 2013/07/31
information_domain: Host
subtypes: Process
analytic_type: TTP
contributors: MITRE
---

Before [exfiltrating data](https://attack.mitre.org/tactics/TA0010) that an adversary has [collected](https://attack.mitre.org/tactics/TA0009), it is very likely that a [compressed archive](https://attack.mitre.org/techniques/T1002) will be created, so that transfer times are minimized and fewer files are transmitted. There is variety between the tools used to compress data, but the command line usage and context of archiving tools, such as ZIP, RAR, and 7ZIP, should be monitored.

In addition to looking for RAR or 7z program names, command line usage of 7Zip or RAR can be detected with the flag usage of "`\* a \*`". This is helpful, as adversaries may change program names.


### ATT&CK Detection

|Technique|Tactic|Level of Coverage|
|---|---|---|
|[Data Compressed](https://attack.mitre.org/techniques/T1002/)|[Exfiltration](https://attack.mitre.org/tactics/TA0010/)|Moderate|

### Data Model References

|Object|Action|Field|
|---|---|---|
|[process](/data_model/process) | [create](/data_model/process#create) | [command_line](/data_model/process#command_line) |


### Implementations

#### Pseudocode

This analytic looks for the command line argument `a`, which is used by RAR. However, there may be other programs that have this as a legitimate argument and may need to be filtered out.


```
processes = search Process:Create
rar_argument = filter processes where (command_line == "* a *")
output rar_argument
```



### Unit Tests

#### Test Case 1

**Configurations:** Windows 7

Download 7zip or other archiving software you plan to monitor. Create an innocuous text file for testing, or substitute an existing file.

```
7z.exe a test.zip test.txt
```
