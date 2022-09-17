---
title: DensityScout
categories: [DFIR Tools, Malware and File Analysis]
tags: [densityscout, defence evasion (TA0005), obfuscated files or information (T1027)]
comments: true
---

# Overview

This tool calculates density (entropy) for files of any file-system-path to finally output an accordingly descending ordered list. This makes it possible to quickly find (even unknown) malware on a potentially infected Microsoft Windows driven machine.

Entropy is used to represent a measurement of code density. Results with higher entropy indicate that a code is randomized, and no meaningful patterns can be identified. Low entropy results are likely indicative of normal or unpacked files. Usually, Microsoft Windows executables are not packed or encrypted therefore any abnormalities detected by running the DensityScout should be further investigated.

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique | MITRE ATT&CK Sub-Technique |
| --------- | ------- | ------------------- | ---------------------- | --------------------------
| [DensityScount](https://www.cert.at/en/downloads/software/software-densityscout) | Build 45 | [Defence Evasion](https://attack.mitre.org/tactics/TA0005/) | [Obfuscated Files or Information](https://attack.mitre.org/techniques/T1027/) [Software Packing](https://attack.mitre.org/techniques/T1036/002/) |

# Instructions

## Search the Windows System32 Directory

```powershell
densityscout -pe -p 0.1 -o results.txt c:\Windows\System32
```
{: .nolineno }

- `-pe` Searches for files with magic number “MZ” representing a PE file.
- `-p 0.1` Instructs DensityScout to highlight files identified with a density below 0.1 on the command line screen. This is a quick reference to display data prior to the tools function completing.
- `-r` Omitted from the above example, however when included searches the directory specified recursively. Listing the directory without -r only searches that directory without checking subdirectories.

# Sources
- [YouTube - Aether Lab - Quick Search for Malware with DensityScout](https://www.youtube.com/watch?v=vMu912jV6uc)
- [SANS Blog - Finding Unknown Malware with DensityScout](https://www.sans.org/blog/finding-unknown-malware-with-densityscout/)