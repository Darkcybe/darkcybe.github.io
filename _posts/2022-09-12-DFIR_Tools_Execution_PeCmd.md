---
title: PeCmd
categories: [DFIR Tools,Program Execution]
tags: [pecmd,prefetch,execution (T0002),evidence of execution]
comments: true
---

PECmd is a command line tool developed by Eric Zimmerman, to process Prefetch files (.pf) on Windows operating systems, identifying items such as:

- Volume information
- Files and Directories referenced
- Executions time (up to last 8 for Win8 and Win10)
- Total execution count

PECmd can output the parsed prefect files into .csv, json and HTML formats for further analysis. It should be noted that Windows Servers do not have prefetch enabled by default.

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique |
| --------- | ------- | ------------------- | ---------------------- |
| [PeCmd](https://ericzimmerman.github.io/#!index.md) | V1.4 | [Execution](https://attack.mitre.org/tactics/TA0002/) | 

# Instructions
## Parsing a Single Prefetch File
Parses the prefetch file for bad.exe and writes the output to a .csv file for further analysis
```powershell
PECmd.exe -f bad.exe-2222BD1A.pf –csv G:\Cases\001\Suspect_Machine_1\bad_Prefetch.csv”
```
{: .nolineno }

## Parsing all Prefetch Files within a Directory
Parses all prefetch files within a supplied directory. The example depicts parsing all .pf files within the default Windows prefetch directory and writes the output to a .csv file for further analysis.
```powershell
PECmd.exe -d “E:\Windows\Prefetch” –csv “G:\Cases\001\Suspect_Machine_1\Prefetch_all.csv -q
```
{: .nolineno }

>**Date/Time of Execution**
> Prefetch files are created roughly ~10 seconds after an executable is executed, therefore the modification (last 
> execution) and creation (first execution) DTG’s may be 10 seconds after displayed times on the prefetch listings.
{: .prompt-info }

# Sources
- [Eric Zimmerman](https://ericzimmerman.github.io/#!documentation.md)