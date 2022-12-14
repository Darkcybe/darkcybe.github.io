---
title: Nmap
categories: [Ethical Hacking Tools,Reconnaissance]
tags: [nmap,reconnaissance (TA0043),active scanning (T1595), nse]
comments: true
---

“Nmap ("Network Mapper") is a free and open source utility for network discovery and security auditing. Many systems and network administrators also find it useful for tasks such as network inventory, managing service upgrade schedules, and monitoring host or service uptime. Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics. It was designed to rapidly scan large networks, but works fine against single hosts. Nmap runs on all major computer operating systems, and official binary packages are available for Linux, Windows, and Mac OS X.” - [nmap.org](https://nmap.org/)

It is perhaps the most utilized tool when initiating a pentest or challenge for a CTF event and is also used for system administration purposes to map network devices. Nmap can be found on popular pentesting environments natively such as Kali Linux and ParrotOS.

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique |
| --------- | ------- | ------------------- | ---------------------- |
| [Nmap](https://nmap.org/download) | V7.92 | [Reconnaissance](https://attack.mitre.org/tactics/TA0043/) | [Active Scanning](https://attack.mitre.org/techniques/T1595/)

# Nmap Help Page
```
root@kali:~# nmap -h
Nmap 7.92 ( https://nmap.org )
Usage: nmap [Scan Type(s)] [Options] {target specification}
TARGET SPECIFICATION:
  Can pass hostnames, IP addresses, networks, etc.
  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
  -iL <inputfilename>: Input from list of hosts/networks
  -iR <num hosts>: Choose random targets
  --exclude <host1[,host2][,host3],...>: Exclude hosts/networks
  --excludefile <exclude_file>: Exclude list from file
HOST DISCOVERY:
  -sL: List Scan - simply list targets to scan
  -sn: Ping Scan - disable port scan
  -Pn: Treat all hosts as online -- skip host discovery
  -PS/PA/PU/PY[portlist]: TCP SYN/ACK, UDP or SCTP discovery to given ports
  -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
  -PO[protocol list]: IP Protocol Ping
  -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
  --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
  --system-dns: Use OS's DNS resolver
  --traceroute: Trace hop path to each host
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>: Only scan specified ports
    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
  --exclude-ports <port ranges>: Exclude the specified ports from scanning
  -F: Fast mode - Scan fewer ports than the default scan
  -r: Scan ports consecutively - don't randomize
  --top-ports <number>: Scan <number> most common ports
  --port-ratio <ratio>: Scan ports more common than <ratio>
SERVICE/VERSION DETECTION:
  -sV: Probe open ports to determine service/version info
  --version-intensity <level>: Set from 0 (light) to 9 (try all probes)
  --version-light: Limit to most likely probes (intensity 2)
  --version-all: Try every single probe (intensity 9)
  --version-trace: Show detailed version scan activity (for debugging)
SCRIPT SCAN:
  -sC: equivalent to --script=default
  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
           directories, script-files or script-categories
  --script-args=<n1=v1,[n2=v2,...]>: provide arguments to scripts
  --script-args-file=filename: provide NSE script args in a file
  --script-trace: Show all data sent and received
  --script-updatedb: Update the script database.
  --script-help=<Lua scripts>: Show help about scripts.
           <Lua scripts> is a comma-separated list of script-files or
           script-categories.
OS DETECTION:
  -O: Enable OS detection
  --osscan-limit: Limit OS detection to promising targets
  --osscan-guess: Guess OS more aggressively
TIMING AND PERFORMANCE:
  Options which take <time> are in seconds, or append 'ms' (milliseconds),
  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
  -T<0-5>: Set timing template (higher is faster)
  --min-hostgroup/max-hostgroup <size>: Parallel host scan group sizes
  --min-parallelism/max-parallelism <numprobes>: Probe parallelization
  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time>: Specifies
      probe round trip time.
  --max-retries <tries>: Caps number of port scan probe retransmissions.
  --host-timeout <time>: Give up on target after this long
  --scan-delay/--max-scan-delay <time>: Adjust delay between probes
  --min-rate <number>: Send packets no slower than <number> per second
  --max-rate <number>: Send packets no faster than <number> per second
FIREWALL/IDS EVASION AND SPOOFING:
  -f; --mtu <val>: fragment packets (optionally w/given MTU)
  -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
  -S <IP_Address>: Spoof source address
  -e <iface>: Use specified interface
  -g/--source-port <portnum>: Use given port number
  --proxies <url1,[url2],...>: Relay connections through HTTP/SOCKS4 proxies
  --data <hex string>: Append a custom payload to sent packets
  --data-string <string>: Append a custom ASCII string to sent packets
  --data-length <num>: Append random data to sent packets
  --ip-options <options>: Send packets with specified ip options
  --ttl <val>: Set IP time-to-live field
  --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address
  --badsum: Send packets with a bogus TCP/UDP/SCTP checksum
OUTPUT:
  -oN/-oX/-oS/-oG <file>: Output scan in normal, XML, s|<rIpt kIddi3,
     and Grepable format, respectively, to the given filename.
  -oA <basename>: Output in the three major formats at once
  -v: Increase verbosity level (use -vv or more for greater effect)
  -d: Increase debugging level (use -dd or more for greater effect)
  --reason: Display the reason a port is in a particular state
  --open: Only show open (or possibly open) ports
  --packet-trace: Show all packets sent and received
  --iflist: Print host interfaces and routes (for debugging)
  --append-output: Append to rather than clobber specified output files
  --resume <filename>: Resume an aborted scan
  --noninteractive: Disable runtime interactions via keyboard
  --stylesheet <path/URL>: XSL stylesheet to transform XML output to HTML
  --webxml: Reference stylesheet from Nmap.Org for more portable XML
  --no-stylesheet: Prevent associating of XSL stylesheet w/XML output
MISC:
  -6: Enable IPv6 scanning
  -A: Enable OS detection, version detection, script scanning, and traceroute
  --datadir <dirname>: Specify custom Nmap data file location
  --send-eth/--send-ip: Send using raw ethernet frames or IP packets
  --privileged: Assume that the user is fully privileged
  --unprivileged: Assume the user lacks raw socket privileges
  -V: Print version number
  -h: Print this help summary page.
EXAMPLES:
  nmap -v -A scanme.nmap.org
  nmap -v -sn 192.168.0.0/16 10.0.0.0/8
  nmap -v -iR 10000 -Pn -p 80
SEE THE MAN PAGE (https://nmap.org/book/man.html) FOR MORE OPTIONS AND EXAMPLES
```
# Instructions
## Performing Scans
Nmap scans networked devices based on 9 scan phases, that include; Target Enumeration, Host Discovery, Reverse-DNS Resolution, OS Detection, Version Detection, Port Scanning, Traceroute, Script Scanning (using the Nmap Scripting Engine (NSE)) and finally, output. Command switches are used to modify those phases for more simplified and passive, or complex and aggressive scanning. Some basic scans that are useful for system discovery are as below:
```bash
# Scan a host for a single port
nmap -p22 <IP ADDRESS>

# Scan a subnet for active hosts via ping scan and output to a file
nmap -sn -oA <output Name/Directory> <IP ADDRESS>/CIDR

# Scan a host, enabling OS detection, version detection, script scanning, and traceroute with verbose output
nmap -v -A <IP ADDRESS> or <DOMAIN>
```
{: .nolineno }

## Performing Complex Scans
As mentioned above, Nmap has the ability to conduct complex and aggressive scanning which can be very useful for internal security teams and penetration testers alike. Some handy scans are as below:
```bash
# Scan a host across ports 1-1023 and ports that are mapped to services, enabling OS detection, version detection, script scanning (default scripts), and traceroute with formatted output
nmap -sC -sV -p1-1023,[1024-] <IP ADDRESS> or <DOMAIN>

# Scan a host with aggressive detection, in fast mode, and across all ports
nmap -A -T4 -p- <IP ADDRESS>

# Scan a host and return vulnerability listings using NSE
nmap -sC --script=vulners <IP ADDRESS>

# Scan a web server and retrieve banner information via NSE
nmap -sC --script=http-enum <IP ADDRESS>
```
{: .nolineno }

> There are over 600 pre-packaged scripts and many more available through third-party repositories. By Default on Linux installs, scripts are kept in the directory `/usr/share/nmap/scripts`. 
{: .prompt-info }

## Searching the NSE Database

There are numerous pre-packaged scripts that ship default with Nmap, with additional extensions or specialized scripts able to be created or downloaded via repositories such as GitHub. The following are a few more precise commands that can be used to interact with the script database

```bash
# Searching the NSE Database
nmap --script-help "smb-*" | grep "^smb-"

# Displaying Script Help
nmap --script-help "smb-os-discovery"
```
{: .nolineno }

# Additional Info
## ZenMap

Nmap provides a GUI frontend for the application called ZenMap. Commands are still entered as previous examples display, however the GUI interface can display network topology information in a graphical view.

## Analyzing Output

```bash
Nmap scan report for 10.20.30.40
Host is up (0.0081s latency).
Not shown: 989 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2022-08-14 04:20:69Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: dark.cybe, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: dark.cybe, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2022-08-14T05:30:32
|_  start_date: N/A
```
{: .nolineno }

1. From the example above are several open ports, with Nmap attributing Service and Version details based on header details returned by the target host. This information can provide valuable insight into the hosts role, potential vulnerabilities, targets for further enumeration consequently and attack entry points.
2. Based on the above port details, we can ascertain the the server 10.20.30.40 is a Domain Controller, with ports relating to Kerberos and LDAP being observed. The trailing section details Nmap script execution details that identify that SMB version 3.1.1 is in use on the host.

# Sources
- [Nmap.org](https://nmap.org/)
- [Kali Linux Tools](https://www.kali.org/tools/nmap/#nmap)
- [NSE Doc Reference Portal](https://nmap.org/nsedoc/)