---
title: WxTcmd
categories: [DFIR Tools, Program Execution]
tags: [wxtcmd, activitiescache, execution (T0002)]
comments: true
---

# Overview
Windows 10 introduced a background feature that records recently used applications and files in a "timeline" accessible via the "WIN+TAB" key. The data is recorded in a SQLite database. Windows 11 removed the "WIN+TAB" functionality, however the ActivitiesCache.db still remains, providing a wealth of information in support of digital forensic investigations in which program execution is being traced.

Further research has identified that Windows Server 2022 also maintains an ActivitiesCache.db file.

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique |
| --------- | ------- | ------------------- | ---------------------- |
| [WxTCmd](https://ericzimmerman.github.io/#!index.md) | V0.6.0.0 | [Execution](https://attack.mitre.org/tactics/TA0002/) | 

# Instructions
## Extracting the ActivitiesCache.db file to a CSV

## Output
Interperating the output sections