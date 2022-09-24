---
title: Evidence of Download
categories: [DFIR, Evidence Artifacts]
tags: [opensavemru, outlook, skype, browser, zone.identifier, alternate data stream]
comments: true
---
Techniques that can be used to discover evidence in support of program or file download by an attacker post-breach or during an attack.

# Windows

## OpenSaveMRU
Tracks files that have been opened or saved within a Windows shell dialog box. This happens to be a big data set, not only including web browsers like Internet Explorer and Firefox, but also a majority of commonly used applications.

**WIN:** XP+ <br>
**SRV:** NULL

### Location
```plaintext
# WINDOWS XP
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSaveMRU

# WINDOWS 7+
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDIMRU
```

### Interpretation and Investigative Notes
- The `*` key
  - Tracks the most recent files of any extension input in an OpenSave dialog
- The `.%%%` key (Three Letter Extension)
  - Stores the file info from the OpenSave dialog by specific extension
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)

### Sources
- [SANS Blog - OpenSaveMRU and LastVisitedMRU](https://www.sans.org/blog/opensavemru-and-lastvisitedmru/)
- [Sploited - SANS Forensic Artifact 1 OpenSaveMRU](https://sploited.blogspot.com/2012/10/sans-forensic-artifact-1-opensave-mru.html)

## Outlook Email Attachments
Around 80% of email is stored via attachments and are encoded with MIME/Base64 standard.

**WIN:** XP+ <br>
**SRV:** NULL

### Location
```plaintext
# WINDOWS XP
%USERPROFILE%\Local\Settings\ApplicationData\Microsoft\Outlook

# WINDOWS 7+
%USERPROFILE%\AppData\Local\Microsoft\Outlook
```

### Interpretation and Investigative Notes
- MS Outlook data file found in these locations include OST and PST files. One should also check the OLK and Content.Outlook folder, which might roam depending on the specific version of Outlook used.
  
### Tools
- [OST Viewer](https://www.acquireforensics.com/blog/outlook-ost-file-analysis.html)

### Sources
- [Microsoft - Introduction to Outlook Data Files (.pst and .ost)](https://support.microsoft.com/en-us/office/introduction-to-outlook-data-files-pst-and-ost-222eaf92-a995-45d9-bde2-f331f60e2790)

## Skype History
Skype history keeps a log of chat sessions and files transferred from one machine to another and is enabled by default.

**WIN:** XP+ <br>
**SRV:** NULL

### Location
```plaintext
# WINDOWS XP
C:\Documents and Settings<USERNAME>\Application\Skype<SKYPE-NAME>

# WINDOWS 7+
%USERPROFILE%\AppData\Roaming\Skype<SKYPE-NAME>
```

### Interpretation and Investigative Notes
- Each entry will have a date/time value and a Skype username associated with the action
- The main database file is in SQLite3 format so can be parsed relatively easily.
  
### Tools
- [SQLECmd](https://github.com/EricZimmerman/SQLECmd)
- [Log2TimeLine - SQLite/Skype Parser](https://github.com/log2timeline)

### Sources
- [Acquire Forensics - Skype](https://www.acquireforensics.com/services/tech/skype.html)

## Browser Artifacts
Not directly related to 'file download', however can give insight into pages visited which may link to other forensic artifacts such as prefetch etc. More verbose download details can be found int he Browser Download Manager artifact

**WIN:** XP+ <br>
**SRV:** NULL

### Location
```plaintext
# INTERNET EXPLORER
# Version 8/9
%USERPROFILE%\AppData\Roaming\Microsoft\Windows\IEDownloadHistory\Index.dat

# Version 10/11
%USERPROFILE%\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat

#MOZILLA FIREFOX
# Versions 3-25
%USERPROFILE%\AppData\Roaming\Mozilla\Firefox\Profiles<RANDOM-TEXT>.default\downloads.sqlite

# Versions 26+
%USERPROFILE%\AppData\Roaming\Mozilla\Firefox\Profiles<RANDOM-TEXT>.default\places.sqlite
  -- Table:moz_annos

#GOOGLE CHROME
%USERPROFILE%\AppData\Local\Google\Chrome\User Data\Default\History
```

### Interpretation and Investigative Notes
Many sites in history will list the files that were opened from remote sites and downloaded to the local system. History will record the access to the file on the website that was access via a link.
  
### Tools
- [SQLECmd](https://github.com/EricZimmerman/SQLECmd)
- [Chromium Parser](https://tzworks.com/prototype_page.php?proto_id=45)
- [Mozilla Cache Parser](https://tzworks.com/prototype_page.php?proto_id=50)

### Sources
- [Nasbench - Web Browser Forensics](https://nasbench.medium.com/web-browsers-forensics-7e99940c579a)

## Browser Download Manager
Firefox and Internet Explorer have built-in download manager applications which keeps a history of every file downloaded by the user. This browser artifact can provide excellent information about what sites a user has been visiting and what finds of files they have been downloading.

**WIN:** XP+ <br>
**SRV:** NULL

### Location
```plaintext
# INTERNET EXPLORER
# Versions 8-9
%USERPROFILE%\AppData\Roaming\Microsoft\Windows\IEDownloadHistory\

# Versions 10-11
%USERPROFILE%\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat

# MOZILLA FIREFOX
# WINDOWS XP
%USERPROFILE%\Application Data\Mozilla\Firefox\Profiles<RANDOM-TEXT>.default\downloads.sqlite

# WINDOWS 7+
%USERPROFILE%\AppData\Roaming\Mozilla\Firefox\Profiles<RANDOM-TEXT>.default\downloads.sqlite
```

### Interpretation and Investigative Notes
Downloads will Include the following data types:
- Filename, Size, and Type
- Download from and Referring page
- File Save Location
- Application Used to Open File
- Download Start and End Timers
  
### Tools
- [SQLECmd](https://github.com/EricZimmerman/SQLECmd)
- [Mozilla Cache Parser](https://tzworks.com/prototype_page.php?proto_id=50)

### Sources
- [Nasbench - Web Browser Forensics](https://nasbench.medium.com/web-browsers-forensics-7e99940c579a)

## Alternate Data Stream (ADS) Zone.Identifier
Starting with XP SP2 when files are downloaded from the "Internet Zone" via a browser to an NTFS volume, an alternate data stream is added to the file which is named the Zone Identifier.

**WIN:** XP SP2+ <br>
**SRV:** 2003+

### Location
```plaintext
C:\Users<USERNAME>\Downloads\

C:\%Path_to_File%
```

### Interpretation and Investigative Notes
Files with an ADS Zone.Identifier containing ZoneID=3 were downloaded from the Internet
- URLZONE_TRUSTED = ZONEID = 2
- URLZONE_INTERNET = ZONEID = 3
- URLZONE_UNTRUSTED = ZONEID = 4
  
### Tools
- [Alternate Stream View](https://www.nirsoft.net/utils/alternate_data_streams.html)
- PowerShell `Get-Content .\download.exe -Stream Zone.Identifier`

### Sources
- [Microsoft - Known Alternate Stream Names](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-fscc/6e3f7352-d11c-4d76-8c39-2516a9df36e8)
- [BleepingComputer - Windows Alternate Data Streams](https://www.bleepingcomputer.com/tutorials/windows-alternate-data-streams/)
- [Malware Bytes - Introduction to Alternate Data Streams](https://blog.malwarebytes.com/101/2015/07/introduction-to-alternate-data-streams/)