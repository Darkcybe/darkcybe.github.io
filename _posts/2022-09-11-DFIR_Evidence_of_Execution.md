---
title: Evidence of Execution
categories: [DFIR]
tags: []
comments: true
---

# Windows
## UserAssist
GUI-based programs launched from the desktop are tracked in the launcher on a windows system
### Location
```plaintext
# Windows XP/7/8/10
HKEY_USERS{SID}\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist{GUID}\Count

HKEY_CURRENT_USER\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist{GUID}\Count
```

### Interpretation and Investigative Notes
All values are ROT-13 Encoded
- GUID for XP
  - 75048700 - Active Desktop
- GUID for Win7/8/10
  - CEBFF5CD - Executable File Execution
  - F4E57C4B - Shortcut File Execution
  
### Tools
- [UserAssist View](http://www.nirsoft.net/utils/userassist_view.html)
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)
- [RECmd - Registry Plugins](https://github.com/EricZimmerman/RegistryPlugins)

### Sources
- [Aldeid - Windows UserAssist Keys](https://www.aldeid.com/wiki/Windows-userassist-keys)
- [Didier Stevens - UserAssist](https://blog.didierstevens.com/programs/userassist/)

## Shimcache or AppCompatCache
Windows Application Compatibility Database is used by Windows to identify possible application compatibility challenges with executables. Tracks the executables filename, file size, last modified time, and in Windows XP the last update time.
### Location
```plaintext
# WINDOWS XP
SYSTEM\CurrentControlSet\Control\SessionManager\AppCompatability

# WINDOWS 7/8/10
SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache
```

### Interpretation and Investigative Notes
Any executable run on the Windows system can be found in this key. You can use this key to identify systems specific malware was executed on. In addition, based on the interpretation of the time-based data you might be able to determine the last time of execution or activty on the system.
- Windows XP - Contains 96 entries at most
  - LastUpdateTime is updated when the files are executed
- Windows 7/8/10 - Contains 1024 entries at most
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
ProgramDataUpdater (a task associated with the Application Experience Service) uses the registry file Amcache.hve to store data during process creation.
### Location
```plaintext
# Windows 7/8/10
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

## Last-Visited MRU
Tracks the Specific executable used by an application to open files documented in the OpenSaveMRU key. In addition, each value also tracks the directory location for the last file that was accessed by that application.

**Example:** Notepad.exe was last run using the C:\%USERPROFILE%\Desktop folder

### Location