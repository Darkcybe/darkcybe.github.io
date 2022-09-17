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
| --------- | ------- | ------------------- | ---------------------- | -------------------------- |
| [DensityScout](https://www.cert.at/en/downloads/software/software-densityscout) | Build 45 | [Defence Evasion](https://attack.mitre.org/tactics/TA0005/) | [Obfuscated Files or Information](https://attack.mitre.org/techniques/T1027/) | [Software Packing](https://attack.mitre.org/techniques/T1036/002/) |

# DensityScout Help

```plaintext
DensityScout (Build 45)

Author: Christian Wojner, CERT.at

Syntax: densityscout [options] file_or_directory

options: -a .............. Show errors and empties, too
         -d .............. Just output data (Format: density|path)
         -l density ...... Just files with density lower than the given value
         -g density ...... Just files with density greater than the given value
         -n number ....... Maximum number of lines to print
         -m mode ......... Mode ABS (default) or CHI (for filesize > 100 Kb)
         -o file ......... File to write output to
         -p density ...... Immediately print if lower than the given density
         -P density ...... Immediately print if greater than the given density
         -r .............. Walk recursively
         -s suffix(es) ... Filetype(s) (i.e.: dll or dll,exe,...)
         -S suffix(es) ... Filetype(s) to ignore (i.e.: dll or dll,exe)
         -pe ............. Include all portable executables by magic number
         -PE ............. Ignore all portable executables by magic number

Note:    Packed and/or encrypted data usually has a much higher density than
         normal data (like text or executable binaries).

Modes:   ABS ... Computes the average distance from the ideal quantity for each
                 byte-state according to the overall byte-quantity of the
                 evaluated file.
                 Typical ABS-density for a packed file: < 0.1
                 Typical ABS-density for a normal file: > 0.9

         CHI ... Just the same as ABS but actually squaring each distance.
                 Typical CHI-density for a packed file: < 100.0
                 Typical CHI-density for a normal file: > 1000.0
```

# Instructions

## Search the Windows System32 Directory

```powershell
densityscout -pe -p 0.1 -o results.txt c:\Windows\System32
```
{: .nolineno }

- `-pe` Searches for files with magic number “MZ” representing a PE file.
- `-p 0.1` Instructs DensityScout to highlight files identified with a density below 0.1 on the command line screen. This is a quick reference to display data prior to the tools function completing.
- `-o results.txt` Writes the full results set to a .txt file, matches from the `-p` switch above will still be written to STDOUT.
- `-r` Omitted from the above example, however when included searches the directory specified recursively. Listing the directory without -r only searches that directory without checking subdirectories.

### Output

![DensityScount Output](/assets/img/posts/DFIR/DFIR_Tools_MA_DensityScout.png "DensityScount Output")

# Sources
- [YouTube - Aether Lab - Quick Search for Malware with DensityScout](https://www.youtube.com/watch?v=vMu912jV6uc)
- [SANS Blog - Finding Unknown Malware with DensityScout](https://www.sans.org/blog/finding-unknown-malware-with-densityscout/)