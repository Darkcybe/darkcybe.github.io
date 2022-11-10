---
title: Volatility
categories: [DFIR Tools, Memory Forensics]
tags: [volatility, memory]
comments: true
---

## Overview

Volatility is the world's most widely used framework for extracting digital artifacts from volatile memory (RAM) samples. The extraction techniques are performed completely independent of the system being investigated but offer visibility into the runtime state of the system. The framework is intended to introduce people to the techniques and complexities associated with extracting digital artifacts from volatile memory samples and provide a platform for further work into this exciting area of research. [^1]

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique |
| --------- | ------- | ------------------- | ---------------------- |
| [Volatility3](https://github.com/volatilityfoundation/volatility3) | V2.0.1 | |

## Instructions

### Memory Image Identification (imageinfo)

Provides profile information from the memory image, including OS type, KDBG, suggested profiles, DTB, and DTG of the image acquisition.
The DTB and KDBG output locations can be used with other vol.py plugins to speed-up processing times by identify the locations bypassing a pre-scan. (--dtb=0x1aa002L)(-g 0xf8012536c910L)

```bash
Vol.py -f <memory-image-name.img> imageinfo
```

### Memory Image KDGB Identification (kdbgscan)

Provides profile information from the memory image, including OS type, KDBG, DTB, Build Number and DTG of image acquisition.

```bash
Vol.py -f <memory-image-name.img> kdbgscan
```

### Hibernation and Crash Dump Image Conversion (imagecopy)

```bash
Vol.py -f <hiberfil.sys> imagecopy -O hiberfil.raw –profile=WinXPSP2x86
```

### Process Identification

#### Running Processes (pslist)

Profile must be specified. Returns a list of all running processes for further analysis. The `-p` switch can be used to identify a specific PID

```bash
Vol.py -f <memory-image-name.img> pslist –profile=<profile>
```

#### Running Processes (pstree)

Profile must be specified. Returns a list of all running processes in a parent (PPID) and Process (PID) tree view. The `-v` switch can be used to include verbose logging including command line parameters.

```bash
Vol.py -f <memory-image-name.img> pstree –profile <profile>
```

#### Running and Terminated Processes (psscan)

Profile must be specified. Returns a list of all processes, running and terminated for further analysis.

```bash
Vol.py -f <memory-image-name.img> psscan –profile <profile>
```

## Sources

[1]: [Volatility Foundation](https://www.volatilityfoundation.org/)