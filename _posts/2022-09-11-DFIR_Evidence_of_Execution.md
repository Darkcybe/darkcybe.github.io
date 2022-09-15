---
title: Evidence of Execution
categories: [DFIR]
tags: [userassist, shimcache, appcompatcache, amcache, lastvisitedmru, activitiescache, recentapps, jumplists, srum, prefetch, bam, dam]
comments: true
---
Techniques that can be used to discover evidence in support of program execution post-breach or during an attack.

# Windows

## UserAssist
GUI-based programs launched from the desktop are tracked in the launcher on a windows system

**WIN:** XP, 7, 8, 10, 11<br>
**SRV:** NULL

### Location
```plaintext
HKEY_USERS{SID}\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist{GUID}\Count

HKEY_CURRENT_USER\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist{GUID}\Count
```

### Interpretation and Investigative Notes
All values are ROT-13 Encoded
- GUID for XP
  - `75048700` - Active Desktop
- GUID for Win7/8/10/11
  - `CEBFF5CD` - Executable File Execution
  - `F4E57C4B` - Shortcut File Execution
  
### Tools
- [UserAssist View](http://www.nirsoft.net/utils/userassist_view.html)
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)
- [RECmd - Registry Plugins](https://github.com/EricZimmerman/RegistryPlugins)

### Sources
- [Aldeid - Windows UserAssist Keys](https://www.aldeid.com/wiki/Windows-userassist-keys)
- [Didier Stevens - UserAssist](https://blog.didierstevens.com/programs/userassist/)

## Shimcache (AppCompatCache)
Windows Application Compatibility Database is used by Windows to identify possible application compatibility challenges with executables. Tracks the executables filename, file size, last modified time, and in Windows XP the last update time.

WIN: XP, 7, 8, 10, 11
SRV: NULL

### Location
```plaintext
# WINDOWS: XP
SYSTEM\CurrentControlSet\Control\SessionManager\AppCompatability

# WINDOWS: 7+
SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache
```

### Interpretation and Investigative Notes
Any executable run on the Windows system can be found in this key. You can use this key to identify systems specific malware was executed on. In addition, based on the interpretation of the time-based data you might be able to determine the last time of execution or activty on the system.
- Windows XP - Contains 96 entries at most
  - LastUpdateTime is updated when the files are executed
- Windows 7+ - Contains 1024 entries at most
  - LastUpdateTime does not exist on Windows 7 systems
  
### Tools
- [ShimCacheParser](https://github.com/mandiant/ShimCacheParser)
- [AppCompatCache Parser)](https://github.com/EricZimmerman/AppCompatCacheParser)
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)
- [RECmd - Registry Plugins](https://github.com/EricZimmerman/RegistryPlugins)
- [AppCompatibilityCache Utility](https://tzworks.com/prototype_page.php?proto_id=29)

### Sources
- [Chris Menne - Windows Shimcache Analysis](https://chrismenne.com/windows-shimcache-analysis/)

## AmCache.hve
ProgramDataUpdater (a task associated with the Application Experience Service) uses the registry file Amcache.hve to store data during process creation. Details of program installation and execution are stored.

WIN: 7, 8, 10, 11
SRV: 2008 R2+

### Location
```plaintext
C:\Windows\AppCompat\Programs\Amcache.hve
```

### Interpretation and Investigative Notes
- Amcache.hve - Keys = `Amcache.hve\Root\File\{Volume GUID}\#######`
- Entry for every executable run, full path information, files `$StandardInfo` Last Modification Time, and Disk Volume the executable was run from.
- Fire Run Time = Last Modification Time of key
- SHA1 hash of executable also contained in the key.
  
### Tools
- [AmCache Parser](https://github.com/EricZimmerman/AmcacheParser)
- [Get-ForensicAmcache)](https://powerforensics.readthedocs.io/en/latest/modulehelp/Get-ForensicAmcache/)
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)
- [RECmd - Registry Plugins](https://github.com/EricZimmerman/RegistryPlugins)

### Sources
- [Andrea Fortuna - AmCache and Shimcache in Forensic Analysis](https://www.andreafortuna.org/2017/10/16/amcache-and-shimcache-in-forensic-analysis/)

## LastVisited MRU
Tracks the Specific executable used by an application to open files documented in the OpenSaveMRU key. In addition, each value also tracks the directory location for the last file that was accessed by that application.

**Example:** Notepad.exe was last run using the C:\%USERPROFILE%\Desktop folder

WIN: XP, 7, 8, 10, 11
SRV: NULL

### Location
```plaintext
# WINDOWS: XP
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedMRU
# WINDOWS: 7+
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU
```

### Interpretation and Investigative Notes
Tracks the application executables used to open files in OpenSaveMRU and the last file path used.
  
### Tools
- [Forensafe](https://www.forensafe.com/free.html)
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)
- [RECmd - Registry Plugins](https://github.com/EricZimmerman/RegistryPlugins)

### Sources
- [SANS - OpenSaveMRU and LastVisitedMRU](https://www.sans.org/blog/opensavemru-and-lastvisitedmru/)
- [Forensafe - LastVisitedMRU](https://www.forensafe.com/blogs/lastvisitedmru.html)

## Windows Timeline (Activities Cache.db)
Windows 10 records recently used applications and files in a "timeline" accessible via the "WIN+TAB" key. The data is recorded in a SQLite database. Windows 11 removed the "WIN+TAB" functionality, however the ActivitiesCache.db still remains.

WIN: 10+
SRV: NULL

### Location
```plaintext
C:\Users\%PROFILE%\AppData\Local\ConnectedDevicesPlatform\L.%PROFILE%\ActivitiesCache.db
```

### Interpretation and Investigative Notes
Tracks application execution and provides a focus count per application.
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)
- [RECmd - Registry Plugins](https://github.com/EricZimmerman/RegistryPlugins)
- [WxTCmd](https://ericzimmerman.github.io/#!index.md)
- [ActivitiesCache Parser](https://tzworks.com/prototype_page.php?proto_id=41)

### Sources
- [Kacos2000 - Windows Timeline](https://kacos2000.github.io/WindowsTimeline/)
- [Microsoft - Windows Activity History and Your Privacy](https://support.microsoft.com/en-us/windows/-windows-activity-history-and-your-privacy-2b279964-44ec-8c2f-e0c2-6779b07d2cbd)

## RecentApps
GUI Program execution launched on the Windows 10 System is tracked in the RecentApps key.

WIN: 10 
SRV: NULL

### Location
```plaintext
NTUSER.DAT\Software\Microsoft\Windows\Current Version\Search\RecentApps
```

### Interpretation and Investigative Notes
- Each GUID key points to a recent application.
  - `AppID` = Name of Application
  - `LastAccessTime` = Last execution time in UTC
  - `LaunchCount` = Number of times executed
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)

### Sources
- [Df-Stream - Recent Apps](https://df-stream.com/2017/10/recentapps/)
- [Think DFIR - When Did RecentApps Go?](https://thinkdfir.com/2020/10/23/when-did-recentapps-go/)

## Jump Lists
The Windows task bar (Jump List) is engineered to allow users to "jump" or access items they have frequently or recently used quickly and easily. This funcationality cannot only include recent media files; it must also include recent tasks. 

The data stored in the AutomaticDestinations folder will each have a unique file prepended with the AppID of the associated application on Windows 7 through 10 machines. Windows 11 contains a shortcut (.LNK) files that direct to the application, file, or directory.

WIN: 7+
SRV: NULL

### Location
```plaintext
# WINDOWS: 7, 8, 9, 10
C:%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations
# WINDOWS: 11+
C:%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Recent\
```

### Interpretation and Investigative Notes
- First time of execution of application
  - Creation Time = First time item added to the AppID file.
- Last time of execution with file open.
  - Modification Time = Last time item added to the AppID file.
- [List of Jump List IDs](https://forensicswiki.xyz/wiki/index.php?title=List_of_Jump_List_IDs)
  
### Tools
- [Jump List Explorer (JLE)](https://www.sans.org/tools/jumplist-explorer/)
- [Windows Jump List Parser](https://tzworks.com/prototype_page.php?proto_id=20)

### Sources
- [Superuser - What is a Jump List in Windows?](https://superuser.com/questions/496059/what-is-a-jump-list-in-windows)

## System Resource Usage Monitor (SRUM)
Records 30 to 60 days of historical system performance. Applications run, user account responsible for each, and application and bytes sent/received per application per hour.

WIN: 8+
SRV: NULL

### Location
```plaintext
SOFTWARE\Microsoft\WindowsNT\CurrentVersion\SRUM\Extensions{d10ca2fe-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

C:\Windows\System32\SRU\
```

### Interpretation and Investigative Notes
- SRUM Extension GUIDs
  - `973F5D5C-1D90-4944-BE8E-24B94231A174` - Network Data Usage Monitor
  - `D10CA2FE-6FCF-4F6D-848E-B2E99266FA86` - Push Notifications (WPN) Provider
  - `D10CA2FE-6FCF-4F6D-848E-B2E99266FA89` - Application Resource Usage Provider
  - `DD6636C4-8929-4683-974E-22C046A43763` - Network Connectivity Usage Monitor
  - `FEE4E14F-02A9-4550-B5CE-5FA2DA202E37` - Energy Usage Provider
  
### Tools
- [Forensafe](https://www.forensafe.com/free.html)
- [SRUM Parser (SrumECmd)](https://github.com/EricZimmerman/Srum)
- [SRUM Dump](https://github.com/MarkBaggett/srum-dump)

### Sources
- [Velociraptor - Digging Into the System Resource Usage Monitor (SRUM)](https://velociraptor.velocidex.com/digging-into-the-system-resource-usage-monitor-srum-afbadb1a375)

## Prefetch
Increases performance of a system by pre-loading code pages of commonly used applications. Cache Manager monitors all files and directories referenced for each application or process and maps them into a .pf file. Utilized to know an application was executed on a system.
- Limited to 128 files on XP and Windows 7
- Limited to 1024 files on Windows 8
- `<EXE_NAME>-<HASH>.pf`

WIN: XP+
SRV: NULL

### Location
```plaintext
C:\Windows\Prefetch
```

### Interpretation and Investigative Notes
- Each .pf file will include last time of execution, number of times run, and device and file handles used by the program.
- Date/Time file by that name and path was first executed
  - Creation Date of .pf file (-10 seconds)
- Date/Time file by that name and path was last executed
  - Windows 8+ will contain last 8 times of execution.
  
### Tools
- [Darkcybe - PeCmd](https://darkcybe.github.io/posts/DFIR_Tools_Execution_PeCmd/)
- [Windows Prefetch Parser](https://tzworks.com/prototype_page.php?proto_id=1)

### Sources
- [Geeks for Geeks - Prefetch Files in Windows](https://www.geeksforgeeks.org/prefetch-files-in-windows/)

## Windows Backgroud Activity (BAM) and Desktop Activity Moderator (DAM)
Windows BAM is updated when Windows boots and controls the activity of background applications and is found on all Windows devices. DAM on the otherhand is only found on Windows Tablets and Mobile devices.

WIN: 10+
SRV: NULL

### Location
```plaintext
SYSTEM\CurrentControlSet\Services\bam\UserSettings{SID}

SYSTEM\CurrentControlSet\Services\dam\UserSettings{SID}
```

### Interpretation and Investigative Notes
Provides full path of the executable file that was run on the system and last time of execution date/time.
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)

### Sources
- [Costas K - An Alternative to Prefetch -> BAM](https://www.linkedin.com/pulse/alternative-prefetch-bam-costas-katsavounidis/)