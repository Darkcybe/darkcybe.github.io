---
title: Defense Evasion
categories: [Ethical Hacking, Tactics and Techniques]
tag: [defense evasion (TA0005)]
comments: true
---

## Overview

The adversary is trying to avoid being detected.

Defense Evasion consists of techniques that adversaries use to avoid detection throughout their compromise. Techniques used for defense evasion include uninstalling/disabling security software or obfuscating/encrypting data and scripts. Adversaries also leverage and abuse trusted processes to hide and masquerade their malware. Other tactics’ techniques are cross-listed here when those techniques include the added benefit of subverting defenses. [^1]

## Defense Evasion Techniques

### Use Alternate Authentication Material

Adversaries may use alternate authentication material, such as password hashes, Kerberos tickets, and application access tokens, in order to move laterally within an environment and bypass normal system access controls. [^2]

| Sub Technique ID | Title                        |
| ---------------- | ---------------------------- |
| T1550.001        | Application Access Token     |
| T1550.002        | Pass The Hash                |
| T1550.003        | Pass The Ticket              |
| T1550.004        | Web Session Cookie           |

### Access Token Manipulation

Adversaries may modify access tokens to operate under a different user or system security context to perform actions and bypass access controls. Windows uses access tokens to determine the ownership of a running process. A user can manipulate access tokens to make a running process appear as though it is the child of a different process or belongs to someone other than the user that started the process. When this occurs, the process also takes on the security context associated with the new token. [^3]

| Sub Technique ID | Title                        |
| ---------------- | ---------------------------- |
| T1134.001        | Token Impersonation/Theft    |
| T1134.002        | Create Process with Token    |
| T1134.003        | Make and Impersonate Token   |
| T1134.004        | Parent PID Spoofing          |
| T1134.005        | SID-History Injection        |

## Sources

[^1]: [MITRE ATT&CK - Defense Evasion](https://attack.mitre.org/tactics/TA0005/)
[^2]: [MITRE ATT&CK - Use Alternate Authentication Material](https://attack.mitre.org/techniques/T1550/)
[^3]: [MITRE ATT&CK - Access Token Manipulation](https://attack.mitre.org/techniques/T1134/)
