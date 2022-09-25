---
title: JumpListExplorer (JLE)
categories: [DFIR Tools, Program Execution]
tags: [jump list, automaticdestinations, execution (TA0002)]
comments: true
---

# Overview
The JumpListExplorer (JLE) is a tool that parsers Windows AutomaticDestinations files to provide information relating to application execution. Results are recorded per application Id (AppID) and show folders and applications spawned via a parent application. Eric Zimmerman maintains a repository of common AppID mappings on his [GitHub](https://github.com/EricZimmerman/JumpList/blob/master/JumpList/Resources/AppIDs.txt).

Parsing the Windows Jump List entries can aid forensic investigations by providing evidence of program execution and file and folder interaction activities. Further information about the Jump List can be found on the [Evidence of Execution](https://darkcybe.github.io/posts/DFIR_Evidence_of_Execution/#jump-list) page.

There are two versions of the JLE available, a GUI and a Command Line parser.

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique |
| --------- | ------- | ------------------- | ---------------------- |
| [Jump List Explorer (JLE GUI)](https://f001.backblazeb2.com/file/EricZimmermanTools/net6/JumpListExplorer.zip) <br> [Jump List Explorer Cmd (JLECmd)](https://f001.backblazeb2.com/file/EricZimmermanTools/net6/JLECmd.zip) | V1.4.1.0 | [Execution](https://attack.mitre.org/tactics/TA0002/) | | 

# Instructions

## Loading AutomaticDestinations Files via JumpListExplorer
1. Run JumpListExplorer, the application does not require Administrative permissions
2. Select menu option File and Load Jump Lists, navigate to the directory containing the automaticDestinations files you wish to parse. If running the tool on a live system, automaticDestinations files are stored in the following directory by default `C:%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations`
3. Select a single or multiple automaticDestinations files and select Open.

## Output
Interesting Fields
- TargetCreationDate
- TargetModificationDate
- TargetLastAccessedDate
- LocalPath
- Interaction count

![JumpListExplorer](/assets/img/posts/DFIR/DFIR_Tools_Execution_JumpListExplorer.png "JumpListExplorer")