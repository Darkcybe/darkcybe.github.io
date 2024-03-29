---
title: Evidence of Execution
categories: [DFIR, Evidence Artifacts]
tags: [userassist, shimcache, appcompatcache, amcache, lastvisitedmru, ntuser.dat, activitiescache, recentapps, jump list, automaticdestinations, srum, prefetch, bam, dam, execution (TA0002), registry]
comments: true
---
Techniques that can be used to discover evidence in support of program execution post-breach or during an attack.

## Windows

### ActivitiesCache.db (Windows Timeline)

Windows 10 introduced a background feature that records recently used applications and accessed files over a 30 day duration in a "timeline" accessible via the "WIN+TAB" key. The data is recorded in a SQLite database. Windows 11 removed the "WIN+TAB" functionality, however the ActivitiesCache.db still remains.

Research identified that Windows Server 2016 also maintains an ActivitiesCache.db file, however `ActivityOperation`, `Activity_PackageId`, and `Activity` entries were not recorded.

| Windows | XP     | 7      | 8      | 10   | 11   |
|:-------:|:------:|:------:|:------:|:----:|:----:|
|         |        |        |        | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

| Server | 2003R2 | 2008R2 | 2012R2 | 2016 | 2019 | 2022 |
|:------:|:------:|:------:|:------:|:----:|:----:|:----:|
|        |        |        |        |      | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

#### Location

```plaintext
# Local Account
C:\Users\%PROFILE%\AppData\Local\ConnectedDevicesPlatform\L.%PROFILE%\ActivitiesCache.db

# Online Account
C:\Users\%PROFILE%\AppData\Local\ConnectedDevicesPlatform\%CID%\ActivitiesCache.db
```

#### Interpretation and Investigative Notes

- Files within the `L.%USERPROFILE%` directory.
  - **ActivitiesCache.db:** SQLite Database
  - **ActivitiesCache.db-wal:** 'Write ahead' Log (Holds activity events prior to pushing them to the database - Reboot system to write to database)
  - **ActivitiesCache.db-shm:** 'Shared memory' file
- The database contains evidence such as;
  - `ActivityType`: Activity performed (ExecuteOpen, InFocus, CopyPaste, etc)
  - `Executable`: Parent application used to open or execute file
  - `DisplayText`: Title of file or application
  - `ContentInfo`: Possible content information, not always present
  - Timestamps: `StartTime`, `EndTime`, and `Duration` are some of the most interesting recorded events.
  
#### Tools

- [Darkcybe - WxTCmd](https://darkcybe.github.io/posts/DFIR_Tools_Execution_WxTcmd/)
- [ActivitiesCache Parser -- Requires License](https://tzworks.com/prototype_page.php?proto_id=41)
- [Log2timeline (windows-timeline parser)](https://github.com/log2timeline/plaso)

#### Sources

- [Kacos2000 - Windows Timeline](https://kacos2000.github.io/WindowsTimeline/)
- [Microsoft - Windows Activity History and Your Privacy](https://support.microsoft.com/en-us/windows/-windows-activity-history-and-your-privacy-2b279964-44ec-8c2f-e0c2-6779b07d2cbd)
- [Salt 4N6 - Windows 10 Timeline Forensic Artifacts](https://salt4n6.com/2018/05/03/windows-10-timeline-forensic-artefacts/)

### AmCache.hve

ProgramDataUpdater (a task associated with the Application Experience Service) uses the registry file Amcache.hve to store data during process creation. Details of program installation and execution are stored.

| Windows | XP     | 7      | 8      | 10   | 11   |
|---------|:------:|:------:|:------:|:----:|:----:|
|         |        | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

| Server | 2003R2 | 2008R2 | 2012R2 | 2016 | 2019 | 2022 |
|--------|:------:|:------:|:------:|:----:|:----:|:----:|
|        |        |        |        |      | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

#### Location

```plaintext
C:\Windows\AppCompat\Programs\Amcache.hve
```

#### Interpretation and Investigative Notes

- Amcache.hve - Keys = `Amcache.hve\Root\File\{Volume GUID}\#######`
- Entry for every executable run, full path information, files `$StandardInfo` Last Modification Time, and Disk Volume the executable was run from.
- First Run Time = Last Modification Time of key
- SHA1 hash of executable also contained in the key.
  
#### Tools

- [Darkcybe - AmCache Parser](https://darkcybe.github.io/posts/DFIR_Tools_Execution_AmCacheParser/)
- [Get-ForensicAmcache)](https://powerforensics.readthedocs.io/en/latest/modulehelp/Get-ForensicAmcache/)
- [Log2timeline (amcache parser)](https://github.com/log2timeline/plaso)
- [Darkcybe - Registry Explorer](https://darkcybe.github.io/posts/DFIR_Tools_Toolkits_RegistryExplorer/#loading-a-hosts-registry-via-registry-explorer)

#### Sources

- [Andrea Fortuna - AmCache and Shimcache in Forensic Analysis](https://www.andreafortuna.org/2017/10/16/amcache-and-shimcache-in-forensic-analysis/)

### Background Activity (BAM) and Desktop Activity Moderator (DAM)

Windows BAM and DAM are updated when Windows boots and controls the activity of background applications and is found on all Windows devices and is managed by `C:\Windows\System32\drivers\bam.sys`. DAM is only populated with details of applications on Windows Tablets and Mobile devices although the empty registry key will be present on host devices.

BAM and DAM entries are only stored during a session, with events clearing upon reboot or when entries have been present in the key for over 7 days. Another item to consider is that executables hosted on removable media are not recorded in the BAM or DAM.

| Windows | XP     | 7      | 8      | 10   | 11   |
|---------|:------:|:------:|:------:|:----:|:----:|
|         |        |        |        | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

| Server | 2003R2 | 2008R2 | 2012R2 | 2016 | 2019 | 2022 |
|--------|:------:|:------:|:------:|:----:|:----:|:----:|
|        |        |        |        |      | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

#### Location

```plaintext
# BAM
HKLM\System\CurrentControlSet\Services\bam\state\UserSettings\{SID}

# DAM
HKLM\System\CurrentControlSet\Services\dam\state\UserSettings\{SID}
```

> `CurrentControlSet` may be substituted by `ControlSet001` or `ControlSet002`. The `ControlSet00x` are alternating backups of the `CurrentControlSet`.
{: .prompt-info }

#### Interpretation and Investigative Notes

Provides full path of the executable file that was run on the system and the last time date time group of execution.
  
#### Tools

- [Darkcybe - Registry Explorer](https://darkcybe.github.io/posts/DFIR_Tools_Toolkits_RegistryExplorer/#Parsing-the-BAM/DAM-for-Evidence-of-Execution)

#### Sources

- [Costas K - An Alternative to Prefetch -> BAM](https://www.linkedin.com/pulse/alternative-prefetch-bam-costas-katsavounidis/)
- [DFIR.ru - BAM Internals](https://dfir.ru/2020/04/08/bam-internals/)

### Jump List

The Windows task bar (Jump List) is engineered to allow users to “jump” or access items they have frequently or recently. This can include files, applications, and directories to name the major items of significance for forensic investigations.

The data stored in the AutomaticDestinations directory contains a unique file for each application prepended with a unique Application ID (AppID) correlated to the associated application, such as the following example which depicts the AppID of Windows Explorer 8.1: `f01b4d95cf55d32a.automaticDestinations-ms`.

| Windows | XP     | 7      | 8      | 10   | 11   |
|---------|:------:|:------:|:------:|:----:|:----:|
|         |        | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

| Server | 2003R2 | 2008R2 | 2012R2 | 2016 | 2019 | 2022 |
|--------|:------:|:------:|:------:|:----:|:----:|:----:|
|        |        |        | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

#### Location

```plaintext
C:%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations
```

#### Interpretation and Investigative Notes

- The Jump List files contain information relating to program execution times, execution count and local file paths of the application being investigated. 
- AppIDs are universally used across Windows and Server versions from 2007/2012 onwards. Correlations for common applications can be found [here](https://forensicswiki.xyz/wiki/index.php?title=List_of_Jump_List_IDs).
  
#### Tools

- [Darkcybe - JumpListExplorer (JLE)](https://darkcybe.github.io/posts/DFIR_Tools_Execution_JLE/)
- [Windows Jump List Parser -- Requires License](https://tzworks.com/prototype_page.php?proto_id=20)

#### Sources

- [Superuser - What is a Jump List in Windows?](https://superuser.com/questions/496059/what-is-a-jump-list-in-windows)
- [DFRWS - Windows 10 Jump Lists and Link File Artifacts - Saved, Copied and Moved](https://dfir.pubpub.org/pub/wfuxlu9v/release/1)

### LastVisitedMRU

The LastVisitedMRU is responsible for tracking specific executables used by an application to open files documented under the OpenSaveMRU registry key. In addition, each value tracks the directory location for the last file that was accessed by that application. The information can provide forensic insight into an applications execution and file and folder interaction.

| Windows | XP     | 7      | 8      | 10   | 11   |      |
|---------|--------|--------|--------|------|------|------|
|         | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

| Server | 2003R2 | 2008R2 | 2012R2 | 2016 | 2019 | 2022 |
|--------|--------|--------|--------|------|------|------|
|        | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

#### Location

```plaintext
# WINDOWS: XP
# SERVER: 2003
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDIg32\LastVisitedMRU

# WINDOWS: 7+
# SERVER: 2008+
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDIg32\LastVisitedPidlMRU
```

> The NTUSER.DAT file is unique for each user on the target host.
{: .prompt-info }

#### Interpretation and Investigative Notes

Tracks the application executables used to open files in OpenSaveMRU and the last file path used, for example: Notepad.exe was last run under the `C:\%USERPROFILE%\Desktop` directory.

#### Tools

- [Darkcybe - Registry Explorer](https://darkcybe.github.io/posts/DFIR_Tools_Toolkits_RegistryExplorer/#Parsing-the-LastVisitedMRU-for-Evidence-of-Execution)
- [ArtiFast Lite 4](https://www.forensafe.com/free.html)

#### Sources

- [SANS - OpenSaveMRU and LastVisitedMRU](https://www.sans.org/blog/opensavemru-and-lastvisitedmru/)
- [Forensafe - LastVisitedMRU](https://www.forensafe.com/blogs/lastvisitedmru.html)

### Prefetch

Increases performance of a system by pre-loading code pages of commonly used applications. Cache Manager monitors all files and directories referenced for each application or process and maps them into a .pf file. Utilized to know an application was executed on a system.

- Limited to 128 files on XP and Windows 7
- Limited to 1024 files on Windows 8
- `<EXE_NAME>-<HASH>.pf`

| Windows | XP     | 7      | 8      | 10   | 11   |
|---------|--------|--------|--------|------|------|
|         | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

| Server | 2003R2 | 2008R2 | 2012R2 | 2016 | 2019 | 2022 |
|--------|--------|--------|--------|------|------|------|
|        | <i class='fa fa-check-circle'></i> * | <i class='fa fa-check-circle'></i> * | <i class='fa fa-check-circle'></i> * | <i class='fa fa-check-circle'></i> * | <i class='fa fa-check-circle'></i> * | |

> Although prefetch is available on Windows Servers, it is disabled by default. To enable Prefetch on Windows Servers (I was unable to get this working on Windows Server 2022), the following steps can be taken. However, keep in mind that it will need to be enable prior to any nefarious activities occurring and will not provide retrospective artifacts. Prefetch can also be disabled by default when the system detected an SSD being used, enabling can be configured by the same.
> 1. Update or create the `EnablePrefetcher` registry key in `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PrefetchParameters`
> 2. Set the `EnablePrefetcher` key value: `0` = Disabled, `1` = Application launch prefetching enabled, `2` = Boot prefetching enabled, `2` = Application launch and boot prefetching enabled.

#### Location

```plaintext
C:\Windows\Prefetch
```

#### Interpretation and Investigative Notes

- Each .pf file will include last time of execution, number of times run, and device and file handles used by the program.
- Date/Time file by that name and path was first executed
  - Creation Date of .pf file (-10 seconds)
- Date/Time file by that name and path was last executed
  - Windows 8+ will contain last 8 times of execution.
  
#### Tools

- [Darkcybe - PeCmd](https://darkcybe.github.io/posts/DFIR_Tools_Execution_PeCmd/)
- [Windows Prefetch Parser - Requires License](https://tzworks.com/prototype_page.php?proto_id=1)

#### Sources

- [Geeks for Geeks - Prefetch Files in Windows](https://www.geeksforgeeks.org/prefetch-files-in-windows/)
- [Microsoft - Disabling/Enabling Prefetch](https://learn.microsoft.com/en-us/previous-versions/windows/embedded/ms940847(v=winembedded.5)?redirectedfrom=MSDN)

### RecentApps

Windows 10 tracked Program execution via the RecentApps registry key for particular versions.

| Windows | XP     | 7      | 8      | 10   | 11   |
|---------|--------|--------|--------|------|------|
|         |        |        |        | <i class='fa fa-check-circle'></i> | |

> Only Microsoft Windows 10 versions V1607-1709 appear to populate the RecentApps registry key.

| Server | 2003R2 | 2008R2 | 2012R2 | 2016 | 2019 | 2022 |
|--------|--------|--------|--------|------|------|------|
|        |        |        |        |      |      |      |

#### Location

```plaintext
NTUSER.DAT\Software\Microsoft\Windows\Current Version\Search\RecentApps
```

#### Interpretation and Investigative Notes

- Each GUID key points to a recent application.
  - `AppID` = Name of Application
  - `LastAccessTime` = Last execution time in UTC
  - `LaunchCount` = Number of times executed
  
#### Tools

- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)

#### Sources

- [Df-Stream - Recent Apps](https://df-stream.com/2017/10/recentapps/)
- [Think DFIR - When Did RecentApps Go?](https://thinkdfir.com/2020/10/23/when-did-recentapps-go/)

### Shimcache (AppCompatCache)

The Shimcache (also known as AppCompatCache) allows applications to call properties from earlier Windows versions, avoiding the need to rewrite an application when the host operating system is upgraded. When an application launches, it checks for compatibility and creates a Shimcache item that maintains the executable's last modification date, file path, and file size.

| Windows | XP     | 7      | 8      | 10   | 11   |
|---------|--------|--------|--------|------|------|
|         | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

| Server | 2003R2 | 2008R2 | 2012R2 | 2016 | 2019 | 2022 |
|--------|--------|--------|--------|------|------|------|
|        | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

#### Location

```plaintext
# WINDOWS: XP
SYSTEM\CurrentControlSet\Control\SessionManager\AppCompatability

# WINDOWS: 7+
SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache
```

#### Interpretation and Investigative Notes

This key contains any executable that runs on the Windows system. This key can be used to determine which computers malware or other applications of interest was executed on. Furthermore, depending on the interpretation of time-based data, you may be able to ascertain the last time the system was executed or that activity occurred.

- Windows XP has a maximum of 96 entries, with the `LastUpdateTime` field being changed when the executable is run.
- Windows 7 and later systems have a maximum of 1024 entries., However the `LastUpdateTime` field does not exist.

The registry key itself is not displayed in an easy to read format if utilizing the native `regedit.exe` tool, therefore parsing with a third party tool is a requirement to analyze the data.

> Entries do not always indicate execution as they may be shimmed prior to execution when first dropped on disk. Execution should be be proven via multiple methods where able.
{: .prompt-info }
  
#### Tools

- [Darkcybe - Registry Explorer](https://darkcybe.github.io/posts/DFIR_Tools_Toolkits_RegistryExplorer/#Parsing-the-ShimCache-for-Evidence-of-Execution)
- [Darkcybe - AppCompatCacheParser](https://darkcybe.github.io/posts/AppCompatCacheParser)
- [AppCompatibilityCache Utility - Requires License](https://tzworks.com/prototype_page.php?proto_id=29)

#### Sources

- [Chris Menne - Windows Shimcache Analysis](https://chrismenne.com/windows-shimcache-analysis/)

### System Resource Usage Monitor (SRUM)

The System Resource Usage Monitor (SRUM) records 30 to 60 days of historical system performance, including details in support of applications run, user account responsible for each, and application and bytes sent/received per application per hour. A lot of this information assists in populating the 'App History' tab on the Task Manager.

| Windows | XP     | 7      | 8      | 10   | 11   |
|---------|--------|--------|--------|------|------|
|         |        |        | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> | <i class='fa fa-check-circle'></i> |

| Server | 2003R2 | 2008R2 | 2012R2 | 2016 | 2019 | 2022 |
|--------|--------|--------|--------|------|------|------|
|        |        |        |        |      |      | <i class='fa fa-check-circle'></i> |

#### Location

```plaintext
# Registry - Artifacts cannot be parsed from the registry location
SOFTWARE\Microsoft\WindowsNT\CurrentVersion\SRUM\Extensions{d10ca2fe-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

# Filepath
C:\Windows\System32\SRU\
```

#### Interpretation and Investigative Notes

- SRUM Extension GUIDs
  - `973F5D5C-1D90-4944-BE8E-24B94231A174` - Windows Network Data Usage Monitor
  - `D10CA2FE-6FCF-4F6D-848E-B2E99266FA86` - Push Notifications (WPN) Provider
  - `D10CA2FE-6FCF-4F6D-848E-B2E99266FA89` - Application Resource Usage Provider
  - `DD6636C4-8929-4683-974E-22C046A43763` - Windows Network Connectivity Usage Monitor
  - `FEE4E14F-02A9-4550-B5CE-5FA2DA202E37` - Energy Usage Provider
  
#### Tools

- [Darkcybe - SrumECmd Guide](https://darkcybe.github.io/posts/SrumECmd)
- [SRUM Dump](https://github.com/MarkBaggett/srum-dump)
- [Forensafe](https://www.forensafe.com/free.html)

#### Sources

- [Velociraptor - Digging Into the System Resource Usage Monitor (SRUM)](https://velociraptor.velocidex.com/digging-into-the-system-resource-usage-monitor-srum-afbadb1a375)
- [Forensafe - SRUM](https://forensafe.com/blogs/srudb.html)

### UserAssist
GUI-based programs launched from the desktop are tracked in the launcher on a windows system

**WIN:** XP, 7, 8, 10, 11<br>
**SRV:** NULL

#### Location

```plaintext
HKEY_USERS{SID}\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist{GUID}\Count

HKEY_CURRENT_USER\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist{GUID}\Count
```

#### Interpretation and Investigative Notes

- All values are ROT-13 Encoded
  - `.RKR` = .exe
  - `.YAX` = .lnk
- GUID for XP
  - `75048700` - Active Desktop
- GUID for Win7/8/10/11
  - `CEBFF5CD` - Executable File Execution
  - `F4E57C4B` - Shortcut File Execution
  
#### Tools

- [UserAssist View](http://www.nirsoft.net/utils/userassist_view.html)
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)
- [RECmd - Registry Plugins](https://github.com/EricZimmerman/RegistryPlugins)

#### Sources

- [Aldeid - Windows UserAssist Keys](https://www.aldeid.com/wiki/Windows-userassist-keys)
- [Didier Stevens - UserAssist](https://blog.didierstevens.com/programs/userassist/)

### PowerShell

These events can be useful for tracking the use of PowerShell on a device, including the specific cmdlets, scripts, and providers that are being invoked. By analyzing these events, you can get a sense of which users are using PowerShell, and what actions they are performing.

**WIN:** XP, 7, 8, 10, 11<br>
**SRV:** NULL

#### Location

```plaintext

```

#### Interpretation and Investigative Notes

| Event Log       | Event ID | Description                 | Details Recorded                                                                                                                                          |
|-----------------|----------|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| PowerShell.evtx | 4103     | A cmdlet was invoked.       | The name of the cmdlet, the parameters passed to the cmdlet, and the security identifier (SID) of the user who invoked the cmdlet.                        |
| PowerShell.evtx | 4104     | A script was executed.      | The name and path of the script, the parameters passed to the script, and the security identifier (SID) of the user who executed the script.              |
| PowerShell.evtx | 4105     | A script block was invoked. | The script block that was invoked, the parameters passed to the script block, and the security identifier (SID) of the user who invoked the script block. |
| PowerShell.evtx | 4106     | A provider was loaded.      | The name of the provider that was loaded, the security identifier (SID) of the user who loaded the provider, and the logon ID.                            |
| PowerShell.evtx | 4107     | A provider was unloaded.    | The name of the provider that was unloaded, the security identifier (SID) of the user who unloaded the provider, and the logon ID.                        |
| PowerShell.evtx | 4108     | A provider was invoked.     | The name of the provider that was invoked, the security identifier (SID) of the user who invoked the provider, and the logon ID.                          |
| PowerShell.evtx | 4109     | A provider was refused.     | The name of the provider that was refused, the security identifier (SID) of the user who attempted to load the provider, and the logon ID.                |

  
#### Tools

-

#### Sources

- 