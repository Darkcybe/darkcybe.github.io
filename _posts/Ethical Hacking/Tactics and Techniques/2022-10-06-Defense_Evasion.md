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

### Use Alternate Authentication Material [^2]

Adversaries may use alternate authentication material, such as password hashes, Kerberos tickets, and application access tokens, in order to move laterally within an environment and bypass normal system access controls.

| Sub Technique ID | Title                        |
| ---------------- | ---------------------------- |
| T1550.001        | Application Access Token     |
| T1550.002        | Pass The Hash                |
| T1550.003        | Pass The Ticket              |
| T1550.004        | Web Session Cookie           |

## Sources

[^1]: [MITRE ATT&CK - Defense Evasion](https://attack.mitre.org/tactics/TA0005/)
[^2]: [MITRE ATT&CK - Use Alternate Authentication Material](https://attack.mitre.org/techniques/T1550/)