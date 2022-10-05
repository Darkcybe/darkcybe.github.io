---
title: Registry Explorer
categories: [DFIR Tools, Toolkits]
tags: [registry, amcache, bam, dam, lastvisitedmru, ntuser.dat, execution (TA0002)]
comments: true
---

## Overview

Registry Explorer allows Windows registry hives to be interrogated and parsed for a wide variety of forensic artifacts. The tool comes in two versions, a GUI and a commandline interface. Eric Zimmerman has created several [plugins](https://github.com/EricZimmerman/RegistryPlugins) that allow automated parsing for certain forensic objects.

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique |
| --------- | ------- | ------------------- | ---------------------- |
| [Registry Explorer (RECmd)](https://github.com/EricZimmerman/RECmd) <br> [Registry Explorer (GUI)](https://www.sans.org/tools/registry-explorer/) | V1.6.0.0 | [Execution](https://attack.mitre.org/tactics/TA0002/) <br> [Persistence](https://attack.mitre.org/tactics/TA0003/) <br> [Defense Evasion](https://attack.mitre.org/tactics/TA0005/) <br> [Credential Access](https://attack.mitre.org/tactics/TA0006/) <br> | | 

## Instructions

### Loading a Hosts Registry via Registry Explorer
1. Run RegistryExplorer.exe as an Administrator if interrogating a live host
2. Hive Selection
- **Live Host:** Navigate to menu option File and Live System and then select the desired registry hive
- **Registry Dump:** Navigate to menu option Load Hive and navigate to the desired registry hive via Explorer.

> Hive details can be exported to several formats via the menu option File and Export
{: .prompt-info }

### Parsing the AmCache.hve for Evidence of Execution
Interesting Keys
- `Root\`
	- **InventoryDeviceContainer:** OS devices such as bluetooth, printers, etc. Has links to DevicePnps
	- **InventoryDevicePnP:** Plug and Play (PnP) devices such as bluetooth, USB, etc. More verbose details than those contained in DeviceContainers
	- **InventoryDriverBinary:** System Drivers
	- **InventoryDriverPackage:** Package information that links to both DeviceContainers and DevicePnPs
	- **InventoryApplicationShortcut:** .LNK files

![Registry Explorer - AmCache.hve](/assets/img/posts/DFIR/DFIR_Tools_Toolkits_RegistryExplorer_AmCache.png "Registry Explorer - AmCache.hve")

### Parsing the BAM/DAM for Evidence of Execution
- `Execution Time` is a reference to the last execution time.

![Registry Explorer - BAM/DAM](/assets/img/posts/DFIR/DFIR_Tools_Toolkits_RegistryExplorer_BAM_DAM.png "Registry Explorer - BAM/DAM")

### Parsing the LastVisitedMRU for Evidence of Execution
Interesting Fields
- **Executable:** Records the parent application
- **Absolute Path:** Records the file opened
- **Opened On:** Date-time-group of last access time

![Registry Explorer - LastVisitedMRU](/assets/img/posts/DFIR/DFIR_Tools_Toolkits_RegistryExplorer_LastVisitedMRU.png "Registry Explorer - LastVisitedMRU")

### Parsing the ShimCache (AppCompatCache) for Evidence of Execution
Interesting Fields
- **Program Name:** Records the full executable filepath
- **Modified Time:** Date-time-group of last access time

![Registry Explorer - ShimCache](/assets/img/posts/DFIR/DFIR_Tools_Toolkits_RegistryExplorer_ShimCache.png "Registry Explorer - ShimCache")