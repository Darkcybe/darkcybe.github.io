---
title: AmcacheParser
categories: [DFIR Tools, Program Execution]
tags: [amcache]
comments: true
---

# Overview
AmcacheParser different from other Amcache parsers in that it does not dump everything available. Rather, it looks at both File entries and Program entries.

Program entries are found under `Root\Programs` and File entries are found under `Root\File`.

AmcacheParser gathers information about all the Program entries, then looks at all the File entries. In each file entry is a pointer to a Program ID (value 100). If this Program ID exists in Program entries, the File entry is associated with the Program entry.

At the end of this process you are left with things that didn't come from some kind of installed application.

Using the minimum options, AmcacheParser will only export Unassociated file entries.

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique |
| --------- | ------- | ------------------- | ---------------------- |
| [AmCacheParser](https://github.com/EricZimmerman/AmcacheParser) | V1.5.1.0 | [Execution](https://attack.mitre.org/tactics/TA0002/) | | 

# Instructions
## Extracting Application Data from a Live Host
The AmCache Parser can be deployed onto a host system to extract hive details. If a forensic image or copy of the amcache.hve file has been collected, the tool csn also parse these in place of live extraction. 

'''powershell
amcacheparser.exe -f "C:\Path\To\amcache.hve" --csv "C:\Path\To\Output"
'''
{: .nolineno }

> must be run as Administrator in order to interrogate the live hive.
{: .prompt-info }

## Extracting Appliction Data with Exclusion List Post-processing
AmCache Parser allows for exclusion lists to be configured during processing of the hive data. The format should be in the form of a new line separate .txt document containing single SHA1 hash entries for all applications wishing to be excluded from the results. This is a great option to minimize results for common or expected applications.

```powershell
amcacheparser.exe -f "C:\Path\To\amcache.hve" -w "C:\Path\To\Exclusions.txt" --csv "C:\Path\To\Output"
```
{: .nolineno }

> must be run as Administrator in order to interrogate the live hive.
{: .prompt-info }

## Output
### DeviceContainers
Contains a list of OS devices such as bluetooth, printers, etc. Has links to DevicePnps

![AmCacheParser DeviceContainers (Filtered)](/assets/img/posts/DFIR/DFIR_Tools_Execution_AmCacheParser_DeviceContainers.png "AmCacheParser DeviceContainers (Filtered)")

### DevicePnPs
Contains a list of Plug and Play devices such as bluetooth, USB, etc. More verbose details than those contained in DeviceContainers

![AmCacheParser DevicePnPs (Filtered)](/assets/img/posts/DFIR/DFIR_Tools_Execution_AmCacheParser_DevicePnPs.png "AmCacheParser DevicePnPs (Filtered)")

### DriveBinaries
Contains a list of drivers

![AmCacheParser DriveBinaries (Filtered)](/assets/img/posts/DFIR/DFIR_Tools_Execution_AmCacheParser_DriveBinaries.png "AmCacheParser DriveBinaries (Filtered)")

### DriverPackages
Contains of list of package information that links to both DeviceContainers and DevicePnPs

![AmCacheParser DriverPackages (Filtered)](/assets/img/posts/DFIR/DFIR_Tools_Execution_AmCacheParser_DriverPackages.png "AmCacheParser DriverPackages (Filtered)")

### ShortCuts
Contains a listing of identified .LNK files

![AmCacheParser ShortCuts (Filtered)](/assets/img/posts/DFIR/DFIR_Tools_Execution_AmCacheParser_ShortCuts.png "AmCacheParser ShortCuts (Filtered)")

### UnassociatedFileEntries
Contains a list of file entries that are unassociated with an installer. The command switch `-i` can be added to the execution command to include all file entries.

![AmCacheParser UnassociatedFileEntries (Filtered)](/assets/img/posts/DFIR/DFIR_Tools_Execution_AmCacheParser_UnassociatedFileEntries.png "AmCacheParser UnassociatedFileEntries (Filtered)")