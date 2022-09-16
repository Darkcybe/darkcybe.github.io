---
title: SigCheck
categories: [DFIR Tools, Malware and File Analysis]
tags: [sigcheck, sysinternals, defencse evasion (TA0005), masquearading (T1036), subvert trust controls (T1553)]
comments: true
---
# Overview

SigCheck is a command line tool from the SysInternals Suite developed to scan PE files and verify if they’re signed. A majority of malware identified in the wild is not signed, however it should be kept in mind that advanced malware have leveraged stolen certificates. SigCheck also contains an option to check files hashes against [VirusTotal](https://www.virustotal.com/gui/home/upload).

Unsigned files within legitimate paths such as `\System32` should be investigated.

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique | MITRE ATT&CK Sub-Technique |
| --------- | ------- | ------------------- | ---------------------- | --------------------------
| [SigCheck](https://docs.microsoft.com/en-us/sysinternals/downloads/sigcheck) | V2.90 | [Defence Evasion](https://attack.mitre.org/tactics/TA0005/) | [Masquearading](https://attack.mitre.org/techniques/T1036/)<br> [Subvert Trust Controls](https://attack.mitre.org/techniques/T1553/)| [Invalid Code Signature](https://attack.mitre.org/techniques/T1036/001/)<br> [Code Signing](https://attack.mitre.org/techniques/T1553/002/)|

# Instructions
## Scanning a Single File
General output of information to the command line, including:
Signature Verification, Publisher Information, Entropy, Hashes, VirusTotal Detections, etc.
```powershell
sigcheck64.exe -a -vt -h ruby.exe
```

### Output
```plaintext
Sigcheck v2.90 - File version and signature viewer
Copyright (C) 2004-2022 Mark Russinovich
Sysinternals - www.sysinternals.com

C:\Ruby31-x64\bin\ruby.exe:
        Verified:       Unsigned
        Link date:      10:00 01-Jan-70
        Publisher:      n/a
        Company:        http://www.ruby-lang.org/
        Description:    Ruby interpreter (CUI) 3.1.2p20 [x64-mingw-ucrt]
        Product:        Ruby interpreter 3.1.2p20 [x64-mingw-ucrt]
        Prod version:   3.1.2p20
        File version:   3.1.2p20
        MachineType:    64-bit
        Binary Version: 3.1.2.20
        Original Name:  ruby.exe
        Internal Name:  ruby.exe
        Copyright:      Copyright (C) 1993-2022 Yukihiro Matsumoto
        Comments:       2022-04-12
        Entropy:        5.294
        MD5:    A0E29BD17600C72A9472187FAB9E8CEE
        SHA1:   FC4B38E05DF834A3ADFCF310EBCF2561D1D66952
        PESHA1: 20675D526BCAD96E5CEF3C54253AEFDDA290D53E
        PE256:  72D1B920F65B035B0D61123CD499CAB7AFE61F9F3081131143228555881600DB
        SHA256: A42943060B508B9C559BB77A8B059E4F3FFACB955DC8AB532D61314945EFC8A9
        IMP:    63736A2F715AAEA097231DF6FA236320
        VT detection:   0/74
        VT link:        https://www.virustotal.com/gui/file/a42943060b508b9c559bb77a8b059e4f3ffacb955dc8ab532d61314945efc8a9/detection
```

## Scanning Files Within a Directory
Scans identified directory for executable files. Results are then written to a csv file.

**Additional Parameters:**
- `-tv` and `-tuv`: Lists all trusted root certificates that weren’t explicitly trusted by Microsoft. Good way to identify a cloned and trusted MS root cert.
```powershell
sigcheck64.exe -a -s -e -v -vt -h -c -w C:\Users\User\Desktop\Cases\sigcheck_Case1.csv C:\Program Files\Slack\
```
### Output
![SigCheck .csv Output](/assets/img/posts/DFIR/DFIR_Tools_Execution_SigCheck.png "SigCheck .csv Output")

# Additional Detail
## Importing of Digital Signature Catalog for Offline Analysis
Before running SigCheck against an acquired suspect victim machine via forensic imaging, the machines Digital Signature Catalog must be uploaded to the analysis machine. Without doing this, SigCheck will be unable to verify most of the signed file from the image.

1. Copy the signature folders from the victim machine image to your analysis machine:
   - `\C\Windows\System32\CatRoot\{127D0A1D-4EF2-11D1-8608-00C04FC295EE}`
2. Change the name of the GUID so that they will not conflict with the analysis machines digital signature catalog store. Move the two renamed folder to the same location as below on the analysis machine.
   - `\C\Windows\System32\CatRoot\{127D0A1D-4EF2-11D1-8608-00C04FC295E9}`
3. Open `Services.msc`, find and restart the ‘`Cryptographic Services`’ service.
4. SigCheck may now be run against the collected victim image.

# Sources
- [Channel 9 - License to Kill: Malware Hunting with the SysInternals Tools](https://channel9.msdn.com/events/teched/northamerica/2013/atc-b308#fbid=mb6_bvqq9jj)
- [The Windows Club - Check for Dangerous or Unsigned Certificates](https://www.thewindowsclub.com/sigcheck-unsigned-certificates-windows)
- [Microsoft - SysInternals/SigCheck](https://docs.microsoft.com/en-us/sysinternals/downloads/sigcheck)