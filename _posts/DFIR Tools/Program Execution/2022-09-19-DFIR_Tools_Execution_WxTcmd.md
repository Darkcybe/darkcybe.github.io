---
title: WxTcmd
categories: [DFIR Tools, Program Execution]
tags: [wxtcmd, activitiescache, execution (T0002)]
comments: true
---

# Overview
WxTcmd is a tool used to parse the SQLite ActivitiesCache.db file to provide forensic evidence of execution and file interaction.

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique |
| --------- | ------- | ------------------- | ---------------------- |
| [WxTCmd](https://ericzimmerman.github.io/#!index.md) | V0.6.0.0 | [Execution](https://attack.mitre.org/tactics/TA0002/) | 

# Instructions
## Extracting the ActivitiesCache.db file to a CSV
The ActivitiesCache database is stored under the userprofile and can be copied from the directory `C:\Users\%USERPROFILE%\AppData\Local\ConnectedDevicesPlatform\L.%USERPROFILE%\ActivitiesCache.db`

```powershell
WxTcmd.exe -f 'C:\Path\To\ActivitiesCache.db' --csv 'C:\Path\To\Output'
```
{: .nolineno }

## Output
Two .csv files will be output to the location succeeding the `--csv` parameter;
- Activity.csv
	- Contains verbose details for accessed files and program execution such as executable name, filepath, Explorer search terms, and timestamps including a duration count. 
- Activity_PackageIDs.csv
	- Contains a smaller subset of data and can provide full filepath for recently executed applications.

![WxTCmd Output (Filtered)](/assets/img/posts/DFIR/DFIR_Tools_Execution_WxTCmd.png "WxTCmd Output (Filtered)")