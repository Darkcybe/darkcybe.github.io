---
title: Credential Access
categories: [Ethical Hacking, Tactics and Techniques]
tags: [credential access (TA0006), AiTM (T1557)]
comments: true
---

# Overview

The adversary is trying to steal account names and passwords.

Credential Access consists of techniques for stealing credentials like account names and passwords. Techniques used to get credentials include keylogging or credential dumping. Using legitimate credentials can give adversaries access to systems, make them harder to detect, and provide the opportunity to create more accounts to help achieve their goals. [^1]

# Techniques

## Adversary-in-the-Middle (AiTM) [^2]

Adversaries may attempt to position themselves between two or more networked devices using an adversary-in-the-middle (AiTM) technique to support follow-on behaviors such as Network Sniffing or Transmitted Data Manipulation. By abusing features of common networking protocols that can determine the flow of network traffic (e.g. ARP, DNS, LLMNR, etc.), adversaries may force a device to communicate through an adversary controlled system so they can collect information or perform additional actions.

Specific procedures to conduct AiTM attack can be seen the [Darkcybe - CAPEC 94](/CAPEC/2022-10-05-94-AiTM.md) post.

### Tools
- Inveigh
- Responder

# Sources
- [^1]: [MITRE ATT&CK - Credential Access TA0006](https://attack.mitre.orc/tactics/TA0006/)
- [^2]: [MITRE ATT&CK - Adversary-in-the-Middle T1557](https://attack.mitre.org/techniques/T1557/)