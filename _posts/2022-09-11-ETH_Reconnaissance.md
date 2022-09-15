---
title: Reconnaissance
categories: [Ethical Hacking]
tags: [reconnaissance (TA0043), active scanning (T1595)]
comments: true
---
Gathering information on target infrastructure, operations, and personnel.

"Reconnaissance consists of techniques that involve adversaries actively or passively gathering information that can be used to support targeting. Such information may include details of the victim organization, infrastructure, or staff/personnel. This information can be leveraged by the adversary to aid in other phases of the adversary lifecycle, such as using gathered information to plan and execute Initial Access, to scope and prioritize post-compromise objectives, or to drive and lead further Reconnaissance efforts." [MITRE ATT&CK](https://attack.mitre.org/tactics/TA0043/)

# Techniques

## Passive Scanning and Open Source Intelligence (OSINT)
Adversaries may execute passive reconnaissance via services or gather information from open source repositories that can be used for target profiling. Typically passive reconnaissance will be near impossible to attribute back to an actor.

| Detection      | Mitigation                                                                     |
| -------------- | ------------------------------------------------------------------------------ |
| Threat Hunting | Passive scanning is outside of internal defensive scopes, however reviews of <br> externally passive scanning and OSINT sources including employee password breaches can help |

### Identifying Target Infrastructure
Passive reconnaissance can be performed against a target or in an attempt to identify a potetial target by searching for information such as IP address spaces, domains, subdomains, services, etc.

#### Tools
- [Shodan](https://www.shodan.io/)
- [Censys](https://censys.io/)
- [Crt - Certificate Search](https://crt.sh)
- [Netcraft - Webserver Infrastructure](https://www.netcraft.com/)

## Active Scanning [T1595](https://attack.mitre.org/techniques/T1595/)
Adversaries may execute active reconnaissance scans to gather information that can be used during targeting. Active scans are those where the adversary probes victim infrastructure via network traffic, as opposed to other forms of reconnaissance that do not involve direct interaction.

| Detection       | Mitigation                                                                     |
| ---------       | ------------------------------------------------------------------------------ |
| Network Traffic | Scanning is outside of internal defensive scopes, however reviews of <br> externally facing ports and services should regularly be monitored |

### Scanning IP Blocks
Target IP address can be identified 

#### Tools
- [Darkcybe - Nmap Guide](https://darkcybe.github.io/posts/ETH_Tools_Nmap/)

### Vulnerablity Scanning
Scaning target hosts for exploitable vulnerabilities

#### Tools
- [Darkcybe - Nmap Guide](https://darkcybe.github.io/posts/ETH_Tools_Nmap/)
- [Darkcybe - Nessus Guide] Comming Soon
- [Darkcybe - Nikto Guide] Comming Soon

### Wordlist Scanning
Brute-force or directory crawling technique that allows for content identification

#### Tools
- [Darkcybe - Nmap Guide](https://darkcybe.github.io/posts/ETH_Tools_Nmap/)
- [Darkcybe - Dirbuster Guide] Comming Soon

## Gather Victim Host Information [T1592](https://attack.mitre.org/techniques/T1592/)
Adversaries may gather information about the victim's hosts that can be used during targeting. Information about hosts may include a variety of details, including administrative data (ex: name, assigned IP, functionality, etc.) as well as specifics regarding its configuration (ex: operating system, language, etc.).