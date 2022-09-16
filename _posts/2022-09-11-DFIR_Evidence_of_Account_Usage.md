---
title: Evidence of Account Usage
categories: [DFIR]
tags: [rdp, port:3389, evtx:security, evtx:system]
comments: true
---
Techniques that can be used to discover evidence in support of at attackes usage of a specific account.

# Windows

## Last Login and Last Password Change
Lists the local accounts of the system and their security identifiers (SIDS).

**WIN:** XP+ <br>
**SRV:** NULL

### Location
```plaintext
# WINDOWS XP+
C:\windows\System32\config\SAM

SAM\Domains\Account\Users
```

### Interpretation and Investigative Notes
- Only the last login time and last password change will be stored in the registry key
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)
- [Reg Ripper](https://github.com/keydet89/RegRipper3.0)

### Sources
- [Boncaldo Forensics - 4N6 Quick 01 Windows Users List Login Count](https://boncaldoforensics.wordpress.com/2018/08/01/4n6-quick-01-windows-users-list-login-count/)

## Remote Desktop Protocol (RDP) Usage
Track RDP logins between remote machines

**WIN:** 7+ <br>
**SRV:** 2003+

### Location
```plaintext
%SYSTEM ROOT%\System32\winevt\logs\Security.evtx
```

### Interpretation and Investigative Notes
- Event IDs
  - **4778**: Session Connected/Reconnected
  - **4779**: Session Disconnected
- Event log provides the hostname and IP address of the machine making the connection
- Typically you will see 4779 - Current console disconnected, followed by 4778 - Session Connected
  
### Tools
- [Event Log Explorer](https://www.eventlogxp.com/)
- [Event Log Parser (EvtxECmd)](https://github.com/EricZimmerman/evtx)
- Native Event Viewer

### Sources
- [Woshub - RDP Connection Logs Forensics Windows](http://woshub.com/rdp-connection-logs-forensics-windows/)

## Windows Service Events
Analyze logs for suspicious services starting at boot and review services that have started or stopped or anything to indicate potential compromise, the user in which was responsible for the action will be logged.

**WIN:** 7+ <br>
**SRV:** 2003+

### Location
```plaintext
%SYSTEM ROOT%\System32\winevt\logs\System.evtx
%SYSTEM ROOT%\System32\winevt\logs\Security.evtx
```

### Interpretation and Investigative Notes
- Event IDs
  - **7034:** Service crashed unexpectedly
  - **7035:** Service sent a Start/Stop control
  - **7036:** Service started or stopped
  - **7040:** Start type changed
    - Boot
    - On Request
    - Disabled
  - **7045:** A service was installed on the system (WINDOWS 2008R2 >)
  - **4697:** A service was installed on the system (Security.evtx)
- All event IDs, except 4697 reference the System event log.
- It is common for malware to utilize services, an example being Cobalt Strike.
- Services that are scheduled to start on boot may be indicative of persistence.
- Sudden crashing of service can indicate process injection failures.
  
### Tools
- [Event Log Explorer](https://www.eventlogxp.com/)
- [Event Log Parser (EvtxECmd)](https://github.com/EricZimmerman/evtx)
- Native Event Viewer

### Sources
- [Mosse Institute - Digital Forensics - Windows Event Logs](https://blog.mosse-institute.com/digital-forensics/2022/05/11/windows-event-logs.html)

## Sucessful and Failed Logons
Determine which accounts have been used for attempted logons. Track account usage for known compromised accounts.

**WIN:** XP+ <br>
**SRV:** 2003+

### Location
```plaintext
%SYSTEM ROOT%\System32\winevt\logs\Security.evtx
```

### Interpretation and Investigative Notes
- Event IDs
  - **4624:** Successful Logon
  - **4625:** Failed Logon
  - **4634 & 4647:** Sucessful Logoff
  - **4648:** Logon using explicit credentials (Runas)
  - **4672:** Account logon with supervisor rights (Administrator)
  - **4720:** An account was created
- Logon Events can give us very specific information regarding the nature of account authorizations on a system if we know where to look and how to decipher the data that we find. In addition to telling us the date, time, username, hostname, and success/failure status of a logon. Logon Events also enables us to determine by exactly what means a logon was attempted.
- Event ID 4624 Logon Types
  - **2:** Logon via console
  - **3:** Network Logon
  - **4:** Batch logon
  - **5:** Windows Service Logon
  - **7:** Credentials used to unlock screen
  - **8:** Network logon sending credentials (cleartext)
  - **9:** Different credentials used than logged on user
  - **10:** Remote interactive logon (RDP)
  - **11:** Cached credentials used to logon
  - **12:** Cached remote interactive (similar to type 10)
  - **13:** Cached unlock (similar to type 7)
  
### Tools
- [Event Log Explorer](https://www.eventlogxp.com/)
- [Event Log Parser (EvtxECmd)](https://github.com/EricZimmerman/evtx)
- Native Event Viewer

### Sources
- [Infosec Institute - Relevance of Windows Event IDs in Investigation](https://resources.infosecinstitute.com/topic/relevance-of-windows-eventids-in-investigation/)

## Account Authenication Events
Authentication mechanism artifacts, logging details regarding Local Account and Domain authentication.

**WIN:** XP+ <br>
**SRV:** 2003+

### Location
```plaintext
%SYSTEM ROOT%\System32\winevt\logs\Security.evtx
```

### Interpretation and Investigative Notes
- Event IDs
  - **4776:** Successful/Failed account authentication (NTLM)
  - **4768:** Ticket Granting Ticket (TGT) was granted (successful logon)
  - **4769:** Service ticket requested (access to server resource)
  - **4771:** Pre-authentication failed (failed logon)
- Recorded on system that authenticated the credentials (Domain Controller)
  - Local Account = Host Machine
  - Domain Account = Active Directory
  
### Tools
- [Event Log Explorer](https://www.eventlogxp.com/)
- [Event Log Parser (EvtxECmd)](https://github.com/EricZimmerman/evtx)
- Native Event Viewer

### Sources
- [Infosec Institute - Relevance of Windows Event IDs in Investigation](https://resources.infosecinstitute.com/topic/relevance-of-windows-eventids-in-investigation/)