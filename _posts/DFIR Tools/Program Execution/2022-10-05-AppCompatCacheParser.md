---
title: AppCompatCacheParser
categories: [DFIR Tools, Program Execution]
tags: [shimcache, appcompatcache, appcompatcacheparser, ez tools, execution (T0002)]
comments: true
---

# Overview
AppCompatCacheParser is a command line tool developed by Eric Zimmerman, to process the ShimCache (AppCompatCache) on Windows operating systems, identifying items such as:

- Executable filepaths
- Timestamp of last execution

Results can output the hive entries files into .csv for further analysis. Further Information the ShimCache can be found on [Darkcybe - Evidence of Execution](https://darkcybe.github.io/posts/DFIR_Evidence_of_Execution/#shimcache-appcompatcache)

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique |
| --------- | ------- | ------------------- | ---------------------- |
| [AppCompatCacheParser](https://ericzimmerman.github.io/#!index.md) | V1.5 | [Execution](https://attack.mitre.org/tactics/TA0002/) | 

# Instructions

**Interesting Fields**
  - **Path:** Full filepath of executable
  - **LastModifiedTimeUTC:** Timestamp in UTC of last modification
  - **Executed:** Execution flag (applications can be shimmed without being executed)

## Parsing the ShimCache (AppCompatCache) on a Live System

```powershell
appcompatcacheparser.exe --csvf %OUTPUT_FILENAME%.csv --csv %OUTPUT_DIRECTORY%
```
{: .nolineno }

## Parsing the ShimCache (AppCompatCache) from a Forensic Copy

```powershell
appcompatcacheparser.exe -f /PATH/TO/SYSTEM hive --csvf %OUTPUT_FILENAME%.csv --csv %OUTPUT_DIRECTORY%
```
{: .nolineno }

### Output

![AppCompatCacheParser - Live CSV](/assets/img/posts/DFIR/DFIR_TOOLS/AppCompatCacheParser.png "AppCompatCacheParser - Live CSV")

# Sources
- [Eric Zimmerman](https://ericzimmerman.github.io/#!documentation.md)