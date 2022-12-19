---
title: Evidence of Account Usage
categories: [DFIR, Evidence Artifacts]
tags: [rdp, port:3389, evtx:security, evtx:system, persistence (TA0003), privilege escalation (TA0004)]
comments: true
---

## Overview

Forensic evidence of account usage refers to the evidence that can be collected and analyzed to determine who used a particular account, when the account was accessed, and what actions were taken while the account was in use. This type of evidence can be useful in a variety of situations, including investigations into cyber crimes, unauthorized access to systems or data, and employee misconduct.

There are several types of forensic evidence that can be used to identify account usage:

1. **Log files:** Log files, such as system logs, network logs, and application logs, can contain information about who accessed a particular account and when. This information can be analyzed to determine the actions that were taken while the account was in use.
2. **System artifacts:** System artifacts, such as registry keys, system files, and application data, can contain information about the accounts that have been used on a particular system. This information can be analyzed to determine the actions that were taken while the account was in use.
3. **Network traffic:** Network traffic, such as packets and traffic logs, can contain information about the accounts that were used to access systems or data over the network. This information can be analyzed to determine the actions that were taken while the account was in use.
4. **User artifacts:** User artifacts, such as files and documents, can contain information about the accounts that were used to create or modify them. This information can be analyzed to determine the actions that were taken while the account was in use.

## Windows

### Last Login and Last Password Change

The "Last Login" and "Last Password Change" artifacts refer to the time stamps that are recorded when a user logs into an account or changes their password. These artifacts can be useful in forensic investigations to determine the usage of an account and to identify any suspicious activity.

The "Last Login" artifact is a time stamp that is recorded when a user logs into an account. This artifact can be useful in determining the last time that the account was accessed and can help to identify any suspicious activity.

The "Last Password Change" artifact is a time stamp that is recorded when a user changes their password. This artifact can be useful in determining the last time that the password for the account was changed and can help to identify any suspicious activity.

**WIN:** XP+ <br>
**SRV:** NULL

#### Location
```plaintext
# WINDOWS XP+
C:\windows\System32\config\SAM

SAM\Domains\Account\Users
```

#### Interpretation and Investigative Notes

The System32\config\SAM file is a system file that is used to store information about user accounts on a Windows system. This file can be a useful source of forensic evidence in investigations into cyber crimes, unauthorized access to systems or data, and employee misconduct.

The SAM file contains several types of forensic evidence that can be useful in investigations, including:

1. **User account information:** The SAM file contains information about the user accounts that have been created on the system, including the user names, account types, and security identifiers (SIDs). This information can be useful in determining who has accessed the system and what actions they have taken.
2. **Hashed passwords:** The SAM file contains hashed versions of the passwords for the user accounts.
3. **Last login and password change timestamps:** The SAM file contains time stamps for the last time that each user account was accessed and the last time that the password was changed. These timestamps can be useful in determining the usage of the accounts and identifying any suspicious activity.
  
#### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)
- [Reg Ripper](https://github.com/keydet89/RegRipper3.0)

#### Sources
- [Boncaldo Forensics - 4N6 Quick 01 Windows Users List Login Count](https://boncaldoforensics.wordpress.com/2018/08/01/4n6-quick-01-windows-users-list-login-count/)

### Remote Desktop Protocol (RDP) Usage - Security.evtx

The security.evtx event log is a system log that is used to record security-related events on a Windows system. This log can contain forensic evidence related to the usage of the Remote Desktop Protocol (RDP), which is a network protocol that allows users to remotely connect to and control another computer.

The security.evtx event log can contain several types of forensic evidence related to RDP usage, including:

1. **Remote login and logoff events:** These events are recorded when a user logs in or logs off of the system using RDP. The log entries may contain information about the user who logged in, the IP address of the remote system, and the date and time of the login or logoff.
2. **Remote connection and disconnection events:** These events are recorded when a user establishes or terminates an RDP connection to the system. The log entries may contain information about the user who connected, the IP address of the remote system, and the date and time of the connection or disconnection.
3. **Remote desktop session events:** These events are recorded when a user starts or ends a remote desktop session using RDP. The log entries may contain information about the user who started or ended the session, the IP address of the remote system, and the date and time of the session start or end.

**WIN:** 7+ <br>
**SRV:** 2003+

#### Location
```plaintext
%SYSTEM ROOT%\System32\winevt\logs\Security.evtx
```

#### Interpretation and Investigative Notes

Here is a table listing some event IDs that may be useful for tracking Remote Desktop Protocol (RDP) usage in the security.evtx log:

| Event ID | Description                  | Details Recorded                                                                          |
|:--------:|------------------------------|-------------------------------------------------------------------------------------------|
| 4624     | Remote login                 | User who logged in, IP address of remote system, date and time of login                   |
| 4634     | Remote logoff                | User who logged off, IP address of remote system, date and time of logoff                 |
| 4778     | Remote connection            | User who connected, IP address of remote system, date and time of connection              |
| 4779     | Remote disconnection         | User who disconnected, IP address of remote system, date and time of disconnection        |
| 4799     | Remote desktop session start | User who started the session, IP address of remote system, date and time of session start |
| 4800     | Remote desktop session end   | User who ended the session, IP address of remote system, date and time of session end     |
  
#### Tools
- [Event Log Explorer](https://www.eventlogxp.com/)
- [Event Log Parser (EvtxECmd)](https://github.com/EricZimmerman/evtx)
- Native Event Viewer

#### Sources
- [Woshub - RDP Connection Logs Forensics Windows](http://woshub.com/rdp-connection-logs-forensics-windows/)

## Windows Service/Process Events

The system.evtx and security.evtx logs are system logs that are used to record system related events. These logs can contain forensic evidence related to service execution including the user of whom interacted with a service, which can be useful in investigations into cyber crimes, unauthorized access to systems or data, and employee misconduct.

The system.evtx and security.evtx logs can contain several types of forensic evidence related to service execution and user names, including:

1. **Service start and stop events:** These events are recorded when a service is started or stopped on the system. The log entries may contain information about the service that was started or stopped, the user who initiated the action, and the date and time of the event.

**WIN:** 7+ <br>
**SRV:** 2003+

### Location
```plaintext
%SYSTEM ROOT%\System32\winevt\logs\System.evtx
%SYSTEM ROOT%\System32\winevt\logs\Security.evtx
```

### Interpretation and Investigative Notes

Here is a table listing some event IDs that may be useful for tracking service and process interaction in the security.evtx and system.evtx logs:

| Event Log     | Event ID | Description                              | Details Recorded                                                                                                                                 |
|---------------|----------|------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| security.evtx | 4688     | A new process has been created.          | The name and command line of the process, the security identifier (SID) of the user who created the process, and the logon ID.                   |
| security.evtx | 4697     | A service was installed in the system.   | The name of the service, the display name of the service, the security identifier (SID) of the user who installed the service, and the logon ID. |
| system.evtx   | 7034     | A service terminated unexpectedly.       | The name of the service, the exit code, and the time the service stopped. Sudden crashing of service can indicate process injection failures.                                                                       |
| system.evtx   | 7035     | A service was stopped.                   | The name of the service and the time the service stopped.                                                                                        |
| system.evtx   | 7036     | A service was started.                   | The name of the service and the time the service started.                                                                                        |
| system.evtx   | 7040     | The start type of a service was changed. | The name of the service, the old start type, and the new start type. Services that are scheduled to start on boot may be indicative of persistence.                                                                             |
| system.evtx   | 7045     | A service was installed in the system.   | The name and display name of the service, the name of the account under which the service is running, and the path to the service executable.    |

#### Tools
- [Event Log Explorer](https://www.eventlogxp.com/)
- [Event Log Parser (EvtxECmd)](https://github.com/EricZimmerman/evtx)
- Native Event Viewer

#### Sources
- [Mosse Institute - Digital Forensics - Windows Event Logs](https://blog.mosse-institute.com/digital-forensics/2022/05/11/windows-event-logs.html)

### Account Interaction

The security.evtx event log can be a useful source of forensic evidence related to user account interaction.

**WIN:** XP+ <br>
**SRV:** 2003+

#### Location
```plaintext
%SYSTEM ROOT%\System32\winevt\logs\Security.evtx
```

#### Interpretation and Investigative Notes

Here is a table of some useful event IDs for Account interaction in the security.evtx log:

| Event ID | Description                                       | Details Recorded                                                                                                                                               |
|:--------:|---------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 4624     | An account was successfully logged on.            | The security identifier (SID) of the user, the user's account name and domain, the logon ID, and the logon type (e.g. interactive logon, network logon, etc.). |
| 4625     | An account failed to log on.                      | The security identifier (SID) of the user, the user's account name and domain, the logon ID, and the logon type.                                               |
| 4634     | An account was logged off.                        | The security identifier (SID) of the user, the user's account name and domain, the logon ID, and the logon type.                                               |
| 4647     | User initiated logoff.                            | The security identifier (SID) of the user, the user's account name and domain, the logon ID, and the logon type.                                               |
| 4648     | A logon was attempted using explicit credentials. | The security identifier (SID) of the user, the user's account name and domain, the logon ID, and the logon type.                                               |
| 4672     | Special privileges assigned to new logon.         | The security identifier (SID) of the user, the user's account name and domain, and the logon ID.                                                               |
| 4720     | A user account was created.                       | The security identifier (SID) of the user, the user's account name and domain, and the logon ID.                                                               |  

The Event ID 4624 also contains logon type codes that are used to classify the type of logon that occurred, and can be helpful in understanding how a user logged on to the device. The details recorded for each logon type may vary, but generally include the security identifier (SID) of the user, the user's account name and domain, the logon ID, and the logon type. The logon type codes are in the table below:

| Logon Type Code | Logon Type              | Description                                                          |
|:---------------:|-------------------------|----------------------------------------------------------------------|
| 2               | Interactive             | Logon at the console of the local computer.                          |
| 3               | Network                 | Logon through a network. The account used is a normal account.       |
| 4               | Batch                   | Batch logon.                                                         |
| 5               | Service                 | Service started by the Service Control Manager.                      |
| 7               | Unlock                  | Unlocking the workstation.                                           |
| 8               | NetworkCleartext        | Network logon with credentials sent in the clear.                    |
| 9               | NewCredentials          | Network logon with credentials encrypted in the Kerberos ticket.     |
| 10              | RemoteInteractive       | Logon to a terminal server.                                          |
| 11              | CachedInteractive       | Cached domain credentials.                                           |
| 12              | CachedRemoteInteractive | Cached domain credentials for a terminal server. Similar to 10       |
| 13              | CachedUnlock            | Unlocking a workstation with cached domain credentials. Similar to 7 |
  
#### Tools
- [Event Log Explorer](https://www.eventlogxp.com/)
- [Event Log Parser (EvtxECmd)](https://github.com/EricZimmerman/evtx)
- Native Event Viewer

#### Sources
- [Infosec Institute - Relevance of Windows Event IDs in Investigation](https://resources.infosecinstitute.com/topic/relevance-of-windows-eventids-in-investigation/)

## Account Authentication Events

Authentication mechanism artifacts, logging details regarding Local Account and Domain authentication. Details are recorded on system that authenticated the credentials (Domain Controller).
  - Local Account = Host Machine
  - Domain Account = Active Directory

**WIN:** XP+ <br>
**SRV:** 2003+

### Location
```plaintext
%SYSTEM ROOT%\System32\winevt\logs\Security.evtx
```

### Interpretation and Investigative Notes

Here is a table of some useful event IDs for account authentication in the security.evtx log:

| Event ID | Description                                                                 | Details Recorded                                                                                                                                             |
|----------|-----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 4776     | The Domain Controller attempted to validate the credentials for an account. | The security identifier (SID) of the user, the user's account name and domain, the logon ID, and the logon type.                                             |
| 4768     | A Kerberos authentication ticket (TGT) was requested.                       | The security identifier (SID) of the user, the user's account name and domain, and the logon ID.                                                             |
| 4769     | A Kerberos service ticket was requested.                                    | The security identifier (SID) of the user, the user's account name and domain, the name of the service for which the ticket was requested, and the logon ID. |
| 4771     | Kerberos pre-authentication failed.                                         | The security identifier (SID) of the user, the user's account name and domain, the type of failure, and the logon ID.                                        |
  
### Tools
- [Event Log Explorer](https://www.eventlogxp.com/)
- [Event Log Parser (EvtxECmd)](https://github.com/EricZimmerman/evtx)
- Native Event Viewer

### Sources
- [Infosec Institute - Relevance of Windows Event IDs in Investigation](https://resources.infosecinstitute.com/topic/relevance-of-windows-eventids-in-investigation/)