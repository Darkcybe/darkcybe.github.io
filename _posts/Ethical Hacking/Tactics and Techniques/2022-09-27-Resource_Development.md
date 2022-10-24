---
title: Resource Development
categories: [Ethical Hacking, Tactics and Techniques]
tags: [resource development (TA0042), acquire infrastructure (T1583), compromise accounts (T1586), compromise infrastructure (T1586), develop capabilities (T1587), establish accounts (T1585), obtain capabilities (T1588), stage capabilities (T1608)]
comments: true
---
## Overview

The adversary is trying to establish resources they can use to support operations.

Resource Development consists of techniques that involve adversaries creating, purchasing, or compromising/stealing resources that can be used to support targeting. Such resources include infrastructure, accounts, or capabilities. These resources can be leveraged by the adversary to aid in other phases of the adversary lifecycle, such as using purchased domains to support Command and Control, email accounts for phishing as a part of Initial Access, or stealing code signing certificates to help with Defense Evasion. [^1]

## Techniques

Resources can be split into distinguishable categories;

1. **Infrastructure**
    Physical or Virtualized servers that are stood up to enable an attacker to perform activities. Infrastructure can include infrastructure on cloud service providers, previously compromised infrastructure leveraged for further attacks and specifically created or leased infrastructure.

2. **Web Services**
    Services that are offered via third parties. Often these services include cloud storage hosting, remote access subscriptions, and social media platforms.

3. **Capabilities**
    Resources that are used during an attack such as tools, malware, digital certificates, exploits, etc. These capabilities can be obtained from existing sources, or developed for more targeted approaches. Staging of resources is also an important consideration for the deployment of malware and other resources that are required to be transferred to targeted hosts.

| Detection      | Mitigation                                                                     |
| -------------- | ------------------------------------------------------------------------------ |
| Threat Hunting | Resource Development techniques are difficult to efficiently track and defend against. However <br> threat hunting tasks such as new [domain registration](https://attack.mitre.org/datasources/DS0038/#Domain%20Registration) and scans on threat related <br> infrastructure such as Cobalt Strike servers through tools like [Shodan](https://shodan.io) can provide early warning <br> detections for potential attack vectors. |

## Tools

There are various tools and services for the establishment of infrastructure. For more verbose procedures to carry out some of the techniques, see the [Resource Development - Capabilities]({% post_url 2022-09-27-Capabilities %}) post.

## Sources

[^1]:  [MITRE ATT&CK - Resource Development TA0042](https://attack.mitre.org/tactics/TA0042/)
