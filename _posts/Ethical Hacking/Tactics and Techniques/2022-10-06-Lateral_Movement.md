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

### Access Token Manipulation

Adversaries may modify access tokens to operate under a different user or system security context to perform actions and bypass access controls. Windows uses access tokens to determine the ownership of a running process. A user can manipulate access tokens to make a running process appear as though it is the child of a different process or belongs to someone other than the user that started the process. When this occurs, the process also takes on the security context associated with the new token. [^3]

#### Token Impersonation/Theft

Adversaries may duplicate then impersonate another user's token to escalate privileges and bypass access controls. An adversary can create a new access token that duplicates an existing token using DuplicateToken(Ex). The token can then be used with ImpersonateLoggedOnUser to allow the calling thread to impersonate a logged on user's security context, or with SetThreadToken to assign the impersonated token to a thread.

An adversary may do this when they have a specific, existing process they want to assign the new token to. For example, this may be useful for when the target user has a non-network logon session on the system. [^4]

Specific techniques to conduct token impersonation/theft attacks can be found on the [CAPEC 633 - Token Impersonation](https://darkcybe.github.io/posts/633-Token_Impersonation/) post.

## Sources

[^1]: [MITRE ATT&CK - Lateral Movement](https://attack.mitre.org/tactics/TA0008/)
[^2]: [MITRE ATT&CK - Use Alternate Authentication Material](https://attack.mitre.org/techniques/T1550/)
[^3]: [MITRE ATT&CK - Access Token Manipulation](https://attack.mitre.org/techniques/T1134/)
[^4]: [MITRE ATT&CK - Access Token Manipulation: Token Impersonation/Theft](https://attack.mitre.org/techniques/T1134/001/)
