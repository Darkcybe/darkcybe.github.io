---
title: Lateral Movement
categories: [Ethical Hacking, Tactics and Techniques]
tag: [lateral movement (TA0008)]
comments: true
---

## Overview

The adversary is trying to move through your environment.

Lateral Movement consists of techniques that adversaries use to enter and control remote systems on a network. Following through on their primary objective often requires exploring the network to find their target and subsequently gaining access to it. Reaching their objective often involves pivoting through multiple systems and accounts to gain. Adversaries might install their own remote access tools to accomplish Lateral Movement or use legitimate credentials with native network and operating system tools, which may be stealthier. [^1]

### Use Alternate Authentication Material

Adversaries may use alternate authentication material, such as password hashes, Kerberos tickets, and application access tokens, in order to move laterally within an environment and bypass normal system access controls. [^2]

#### Pass The Hash

Adversaries may "pass the hash" using stolen password hashes to move laterally within an environment, bypassing normal system access controls. Pass the hash (PtH) is a method of authenticating as a user without having access to the user's cleartext password. This method bypasses standard authentication steps that require a cleartext password, moving directly into the portion of the authentication that uses the password hash.

There are multiple tools available to conduct Pass The Hash attacks, such as:

- CrackMapExec
- PowerShell-Empire
- Mimikatz
- Cobalt Strike

Specific techniques to conduct PtH attacks can be found on the [CAPEC 644 - Use of Captured Hashes (Pass The Hash)](https://darkcybe.github.io/posts/644-Pth/) post.

## Sources

[^1]: [MITRE ATT&CK - Lateral Movement](https://attack.mitre.org/tactics/TA0008/)
[^2]: [MITRE ATT&CK - Use Alternate Authentication Material](https://attack.mitre.org/techniques/T1550/)
