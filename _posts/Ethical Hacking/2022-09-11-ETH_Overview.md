---
title: Ethical Hacking Overview
categories: [Ethical Hacking]
tags: []
comments: true
---

> Using any mentioned tools, tactics or techniques against infrastructure that you do not own or have the express permission to perform select tasks on is not recommended nor condoned by myself. Always check local and corporate policy and regulation prior to downloading and using any of the tools listed in this repository. As a simple rule of measure, if in doubt of the actions you may be taking in terms of their applicability or legality, its safest to seek advice. A quick google search or a listen to the [Darknet Diaries](https://darknetdiaries.com/) podcast should fill you with enough fear of the consequences of performing any unsanctioned activity.
{: .prompt-warning }

# Overview

Generally when a person hears the term hacker, they immediately link the person behind it to an individual in their parents basement wearing a faded black hoodie, fingers stained with Doritos flavouring and listening to techno music whilst hacking the mainframe of the NSA. This stereotype is not accurate however, we don't live in our parents' basements. I'm kidding, about the the basement, no that's also a joke.

Hacking can take many forms, from the general stereotype depicted above of the lone hacker causing mischief for fun and clout, to more practical applications such as the modern penetration tester or red team analyst. Regardless of the role, the fundamental essence of hacking remains the same, identifying weaknesses and attempting to exploit them.

The posts under this category "Ethical Hacking" will aim to provide knowledge and advice surrounding hacking techniques, tools, and methodologies that can be used for conducting penetration tests and for use in capture the flag competitions. The term Ethical Hacking is used to describe legal activities surrounding the identification and exploitation of vulnerabilities on target systems. With this in mind, it would be careless for me not to include the warning presented at the top of this overview post.

# Types of Hackers

There is an established method of categorizing hackers based on the old 'hat' scenario.

White Hat
: Ethical Hackers who legally carry out hacking activities, largely to conduct penetration tests and vulnerability assessments or to complete bug bounty activities.

Black Hat
: The villains, these hackers are the ones behind some of the large scale attacks in the news and typically exploit vulnerability of target systems to steal information or gain a revenue via extortion or commonly in modern times via the deployment of Ransomware.

Grey Hat
: These folks are a mix of good and evil, depending on how they wake up in the morning and wether they had a decent cup of coffee. Grey Hat Hackers are typically people such as security researchers who carry out activities on target systems without permission and then report their findings to the organization in order to remedy the issues prior to a Black Hat Hacker exploiting them.

# Ethical Hacking Process

There are multiple variations or terminologies used to describe how to carryout an Ethical Hacking activity, let it be for the conduct of a penetesting engagement, vulnerability assessment or capture the flag event. However, in general I prefer to represent the process in stages, as per the below, to follow in order to ensure completeness for the activities that are being carried out and to link to the [MITRE ATT&CK](https://attack.mitre.org/matrices/enterprise/) Enterprise Matrix to allow for detection and mitigation strategies to easily to transposed.

1. **Pre-Exploitation**
   - [Reconnaissance]({ % post_url 2022-09-12-Reconnaissance %})
   - [Resource Development]({% post_url 2022-09-27-Resource_Development %})
2. **Exploitation**
   - [Initial Access](https://attack.mitre.org/tactics/TA0001/)
3. **Post-Exploitation and Entrenchment**
   - [Execution](https://attack.mitre.org/tactics/TA0002/)
   - [Persistence](https://attack.mitre.org/tactics/TA0003)
   - [Privilege Escalation](https://attack.mitre.org/tactics/TA0004/)
   - [Defense Evasion](https://attack.mitre.org/tactics/TA0005/)
   - [Credential Access](https://attack.mitre.org/tactics/TA0006/)
   - [Discovery](https://attack.mitre.org/tactics/TA0007/)
   - [Lateral Movement](https://attack.mitre.org/tactics/TA0008/)
   - [Collection](https://attack.mitre.org/tactics/TA0009/)
   - [Command and Control (C2)](https://attack.mitre.org/tactics/TA0011/)
   - [Exfiltration](https://attack.mitre.org/tactics/TA0010/)
4. **Actions on Objectives**
   -  [Impact](https://attack.mitre.org/tactics/TA0040/)
5. **Reporting**
   - Identified Vulnerabilities
   - Tools and Techniques
   - Remediation Recommendations

# Sources
- [EC-Council - Ethical Hacking](https://www.eccouncil.org/ethical-hacking/)