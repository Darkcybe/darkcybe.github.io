---
title: Evidence of Download
categories: [DFIR]
tags: [evidence, opensavemru]
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

# WINDOWS +
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