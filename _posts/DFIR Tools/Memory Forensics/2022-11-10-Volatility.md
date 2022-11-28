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
| [Volatility3](https://github.com/volatilityfoundation/volatility3) | V2.4.1 | |

## Volatility Plugins

Volatility consists of many different plugins to execute various methods to provide evidence for analysis, a list of all plugins can be found at the [Volatility3 Docs Page](https://volatility3.readthedocs.io/en/stable/volatility3.plugins.html).

## Instructions

The below examples were based off of a Windows 10 Memory Image available for download via the [NIST CFReDS Portal](https://cfreds.nist.gov/all/MagnetForensics/2020WindowsMemoryMagnetCTF) which was provided from Magnet Forensics.

### Memory Image Identification (windows.info)

Show OS & kernel details of the memory sample being analyzed.

```bash
Python3 Vol.py -f <memory-image-name.img> windows.info
```
{: .nolineno }

```bash
Variable            Value

Kernel Base         0xf80002a48000
DTB                 0x187000
Symbols             file:///etc/volatility3/volatility3/symbols/windows/ntkrnlmp.pdb/ECE191A20CFF4465AE46DF96C2263845-1.json.xz
Is64Bit             True
IsPAE               False
layer_name          0 WindowsIntel32e
memory_layer        1 FileLayer
KdDebuggerDataBlock 0xf80002c2a120
NTBuildLab          7601.24384.amd64fre.win7sp1_ldr_
CSDVersion          1
KdVersionBlock      0xf80002c2a0e8
Major/Minor         15.7601
MachineType         34404
KeNumberProcessors  2
SystemTime          2020-04-20 23:23:26
NtSystemRoot        C:\Windows
NtProductType       NtProductWinNt
NtMajorVersion      6
NtMinorVersion      1
PE MajorOperatingSystemVersion  6
PE MinorOperatingSystemVersion  1
PE Machine          34404
PE TimeDateStamp    Thu Feb 21 03:36:29 2019
```
{: .nolineno }

### Running Processes (pslist)

Lists the processes present in a particular windows memory image. Specific PIDs can be processed via the `--pid` switch. Additionally, each process can be dumped to disk via the `--dump` switch.

```bash
python3 vol.py -f memdump-001.mem windows.pslist
```
{: .nolineno }

```bash
PID     PPID    ImageFileName   Offset(V)       Threads Handles SessionId       Wow64   CreateTime      ExitTime        File output

4       0       System          0xfa8030e57b00  108     572     N/A     False   2020-04-20 22:44:37.000000      N/A     Disabled
280     4       smss.exe        0xfa8032005aa0  2       30      N/A     False   2020-04-20 22:44:37.000000      N/A     Disabled
364     352     csrss.exe       0xfa8032f05b00  9       532     0       False   2020-04-20 22:44:38.000000      N/A     Disabled
408     352     wininit.exe     0xfa803254d580  3       76      0       False   2020-04-20 22:44:38.000000      N/A     Disabled
440     416     csrss.exe       0xfa8032a29350  11      534     1       False   2020-04-20 22:44:38.000000      N/A     Disabled
472     408     services.exe    0xfa803317e8e0  7       241     0       False   2020-04-20 22:44:38.000000      N/A     Disabled
```
{: .nolineno }

### Running Processes (pstree)

Plugin for listing processes in a tree based on their parent process ID. Specific PIDs can be processed via the `--pid` switch. Additionally, each process can be dumped to disk via the `--dump` switch.

```bash
python3 vol.py -f memdump-001.mem windows.pstree
```
{: .nolineno }

```bash
PID     PPID    ImageFileName   Offset(V)       Threads Handles SessionId       Wow64   CreateTime      ExitTime

4       0       System  0xfa8030e57b00  108     572     N/A     False   2020-04-20 22:44:37.000000      N/A
* 280   4       smss.exe        0xfa8032005aa0  2       30      N/A     False   2020-04-20 22:44:37.000000      N/A
364     352     csrss.exe       0xfa8032f05b00  9       532     0       False   2020-04-20 22:44:38.000000      N/A
408     352     wininit.exe     0xfa803254d580  3       76      0       False   2020-04-20 22:44:38.000000      N/A
* 472   408     services.exe    0xfa803317e8e0  7       241     0       False   2020-04-20 22:44:38.000000      N/A
** 2944 472     svchost.exe     0xfa803365b060  9       136     0       False   2020-04-20 22:46:40.000000      N/A
** 772  472     svchost.exe     0xfa8033266060  10      336     0       False   2020-04-20 22:44:39.000000      N/A
** 1160 472     svchost.exe     0xfa8033424860  21      668     0       False   2020-04-20 22:44:39.000000      N/A
** 660  472     svchost.exe     0xfa8033227b00  11      378     0       False   2020-04-20 22:44:38.000000      N/A
*** 3440        660     WmiPrvSE.exe    0xfa80333d6060  13      332     0       False   2020-04-20 23:17:13.000000      N/A
*** 2108        660     WmiPrvSE.exe    0xfa803379eb00  12      221     0       False   2020-04-20 22:44:40.000000      N/A
** 2324 472     msdtc.exe       0xfa803386db00  12      148     0       False   2020-04-20 22:44:40.000000      N/A
```
{: .nolineno }

### Running and Terminated Processes (psscan)

Returns a list of all processes, running and terminated for further analysis.

```bash
python3 vol.py -f memdump-001.mem windows.psscan
```
{: .nolineno }

```bash
PID     PPID    ImageFileName   Offset(V)       Threads Handles SessionId       Wow64   CreateTime      ExitTime        File output

4196    3384    chrome.exe      0x13d406b00     8       106     1       False   2020-04-20 23:17:15.000000      N/A     Disabled
2216    472     dllhost.exe     0x13d4145f0     13      195     0       False   2020-04-20 22:44:40.000000      N/A     Disabled
2324    472     msdtc.exe       0x13d46db00     12      148     0       False   2020-04-20 22:44:40.000000      N/A     Disabled
2848    2208    slack.exe       0x13d4cdb00     14      276     1       False   2020-04-20 23:17:00.000000      N/A     Disabled
2032    472     svchost.exe     0x13d62c060     6       105     0       False   2020-04-20 22:44:40.000000      N/A     Disabled
```
{: .nolineno }

### Installed Services (svcscan)

Returns a list of all installed services from the host system.

```bash
python3 vol.py -f memdump-001.mem windows.svcscan
```
{: .nolineno }

```bash
Offset  Order   PID     Start   State   Type    Name    Display Binary

0xb81200        233     660     SERVICE_AUTO_START      SERVICE_RUNNING SERVICE_WIN32_SHARE_PROCESS     PlugPlay        Plug and Play   C:\Windows\system32\svchost.exe -k DcomLaunch
0xb81200        232     N/A     SERVICE_DEMAND_START    SERVICE_STOPPED SERVICE_WIN32_SHARE_PROCESS     pla     Performance Logs & Alerts       N/A
0xb82670        231     N/A     SERVICE_DEMAND_START    SERVICE_STOPPED SERVICE_WIN32_OWN_PROCESS       PerfHost        Performance Counter DLL Host    N/A
0xb81110        230     N/A     SERVICE_DEMAND_START    SERVICE_STOPPED SERVICE_WIN32_SHARE_PROCESS     PeerDistSvc     BranchCache     N/A
0xb81020        229     N/A     SERVICE_AUTO_START      SERVICE_RUNNING SERVICE_KERNEL_DRIVER   PEAUTH  PEAUTH  \Driver\PEAUTH
0xb80f30        228     N/A     SERVICE_BOOT_START      SERVICE_RUNNING SERVICE_KERNEL_DRIVER   pcw     Performance Counters for Windows Driver \Driver\pcw
0xb82590        227     N/A     SERVICE_DEMAND_START    SERVICE_STOPPED SERVICE_KERNEL_DRIVER   pcmcia  pcmcia  N/A
0xb80e40        226     N/A     SERVICE_DEMAND_START    SERVICE_STOPPED SERVICE_KERNEL_DRIVER   pciide  pciide  N/A
```
{: .nolineno }

### Command Line Arguments (cmdline)

Lists process command line arguments.

```bash
python3 vol.py -f memdump-001.mem windows.cmdline
```
{: .nolineno }

```bash
PID     Process Args

4       System  Required memory at 0x20 is not valid (process exited?)
280     smss.exe        \SystemRoot\System32\smss.exe
364     csrss.exe       %SystemRoot%\system32\csrss.exe ObjectDirectory=\Windows SharedSection=1024,20480,768 Windows=On SubSystemType=Windows ServerDll=basesrv,1 ServerDll=winsrv:UserServerDllInitialization,3 ServerDll=winsrv:ConServerDllInitialization,2 ServerDll=sxssrv,4 ProfileControl=Off MaxRequestThreads=16
408     wininit.exe     wininit.exe
440     csrss.exe       %SystemRoot%\system32\csrss.exe ObjectDirectory=\Windows SharedSection=1024,20480,768 Windows=On SubSystemType=Windows ServerDll=basesrv,1 ServerDll=winsrv:UserServerDllInitialization,3 ServerDll=winsrv:ConServerDllInitialization,2 ServerDll=sxssrv,4 ProfileControl=Off MaxRequestThreads=16
```
{: .nolineno }

### Loaded DLLs (dlllist)

Displays the DLLs loaded by each process and their respective filepaths.

```bash
python3 vol.py -f memdump-001.mem windows.dlllist
```
{: .nolineno }

```bash
PID     Process Base    Size    Name    Path    LoadTime        File output

280     smss.exe        0x48010000      0x20000 smss.exe        \SystemRoot\System32\smss.exe   N/A     Disabled
280     smss.exe        0x76f10000      0x19f000        ntdll.dll       C:\Windows\SYSTEM32\ntdll.dll   N/A     Disabled
364     csrss.exe       0x499b0000      0x6000  csrss.exe       C:\Windows\system32\csrss.exe   N/A     Disabled
364     csrss.exe       0x76f10000      0x19f000        ntdll.dll       C:\Windows\SYSTEM32\ntdll.dll   N/A     Disabled
364     csrss.exe       0x7fefcbc0000   0x13000 CSRSRV.dll      C:\Windows\system32\CSRSRV.dll  2020-04-20 22:44:38.000000      Disabled
364     csrss.exe       0x7fefcba0000   0x11000 basesrv.DLL     C:\Windows\system32\basesrv.DLL 2020-04-20 22:44:38.000000      Disabled
364     csrss.exe       0x7fefcb60000   0x39000 winsrv.DLL      C:\Windows\system32\winsrv.DLL  2020-04-20 22:44:38.000000      Disabled
364     csrss.exe       0x76cf0000      0xfa000 USER32.dll      C:\Windows\system32\USER32.dll  2020-04-20 22:44:38.000000      Disabled
```
{: .nolineno }

### Scan for Injected Code (malfind)

Lists process memory ranges that potentially contain injected code.

```bash
python3 vol.py -f memdump-001.mem windows.malfind
```
{: .nolineno }

```bash
PID     Process Start VPN       End VPN Tag     Protection      CommitCharge    PrivateMemory   File output     Hexdump Disasm

3180    WINWORD.EXE     0x7ff072f0000   0x7ff072f9fff   VadS    PAGE_EXECUTE_READWRITE  10      1       Disabled
00 00 00 00 00 00 00 00 ........
00 00 00 00 00 00 00 00 ........
00 00 00 00 00 00 00 00 ........
00 00 00 00 00 00 00 00 ........
00 00 00 00 00 00 00 00 ........
00 00 00 00 00 00 00 00 ........
00 00 00 00 00 00 00 00 ........
00 00 00 00 00 00 00 00 ........
0x7ff072f0000:  add     byte ptr [rax], al
0x7ff072f0002:  add     byte ptr [rax], al
0x7ff072f0004:  add     byte ptr [rax], al
```
{: .nolineno }

### Network Connections (netstat)

Traverses network tracking structures present in a particular windows memory image.

```bash
python3 vol.py -f memdump-001.mem windows.netstat
```
{: .nolineno }

```bash
Offset  Proto   LocalAddr       LocalPort       ForeignAddr     ForeignPort     State   PID     Owner   Created

0xfa803340b500  TCPv4   192.168.10.146  49174   172.253.122.188 5228    FIN_WAIT2       -       -       N/A
0xfa803388f540  TCPv4   192.168.10.146  54279   151.101.116.106 443     ESTABLISHED     -       -       N/A
```
{: .nolineno }

### Network Connections (netscan)

Scans for network objects present in a particular windows memory image.

```bash
python3 vol.py -f memdump-001.mem windows.netscan
```
{: .nolineno }

```bash
Offset  Proto   LocalAddr       LocalPort       ForeignAddr     ForeignPort     State   PID     Owner   Created

0x13d48f540     TCPv4   192.168.10.146  54279   151.101.116.106 443     ESTABLISHED     -       -       N/A
0x13d518710     UDPv4   0.0.0.0 5355    *       0               1160    svchost.exe     2020-04-20 23:23:00.000000 
0x13d518710     UDPv6   ::      5355    *       0               1160    svchost.exe     2020-04-20 23:23:00.000000 
0x13d636ee0     TCPv4   0.0.0.0 49156   0.0.0.0 0       LISTENING       2032    svchost.exe     -
0x13d63eac0     TCPv4   0.0.0.0 49156   0.0.0.0 0       LISTENING       2032    svchost.exe     -
0x13d63eac0     TCPv6   ::      49156   ::      0       LISTENING       2032    svchost.exe     -
```
{: .nolineno }

### Dump LM/NT Hashes (hashdump)

Dump the LM and NT Hashes for accounts from the memory image.

```bash
python3 vol.py -f memdump-001.mem windows.hashdump
```
{: .nolineno }

```bash
User    rid     lmhash  nthash

Administrator   500     aad3b435b51404eeaad3b435b51404ee        31d6cfe0d16ae931b73c59d7e0c089c0
Guest   501     aad3b435b51404eeaad3b435b51404ee        31d6cfe0d16ae931b73c59d7e0c089c0
Warren  1000    aad3b435b51404eeaad3b435b51404ee        2aa81fb8c8cdfd8f420f7f94615036b0
```
{: .nolineno }

## Sources

[^1]: [Volatility Foundation](https://www.volatilityfoundation.org/)
