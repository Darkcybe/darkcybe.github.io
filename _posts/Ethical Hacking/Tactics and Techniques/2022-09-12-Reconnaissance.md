---
title: Reconnaissance
categories: [Ethical Hacking, Tactics and Techniques]
tags: [reconnaissance (TA0043), active scanning (T1595)]
comments: true
---
# Overview

Gathering information on target infrastructure, operations, and personnel.

"Reconnaissance consists of techniques that involve adversaries actively or passively gathering information that can be used to support targeting. Such information may include details of the victim organization, infrastructure, or staff/personnel. This information can be leveraged by the adversary to aid in other phases of the adversary lifecycle, such as using gathered information to plan and execute Initial Access, to scope and prioritize post-compromise objectives, or to drive and lead further Reconnaissance efforts." [MITRE ATT&CK](https://attack.mitre.org/tactics/TA0043/)

# Techniques

Different techniques can be used when conducting reconnaissance against a target or targets to identify hosts, networks, and users. Mapping a targets infrastructure and and environment conditions allows an attacker to uncover weaknesses that can provide a high chance of being exploitable. Generally, reconnaissance can be classified into the below categories:

1. Infrastructure
   1. Gaining intelligence surrounding the physical and digital infrastructure of the target.
      - IP Addresses, physical security controls, domains, technologies, etc
2. Environmental
   1. Gaining intelligence on the targets organization operational details.
      - Email and User nomenclature, website scraping, document parsing, etc 
3. Users
   1. Gaining intelligence on specific user weaknesses
      - Breached credential searches, social media searches, etc
4. Vulnerabilities
   1. Scanning infrastructure to identify potentially exploitable conditions
   2. Investigation on avenues to exploit identified conditions

## Passive Scanning and Open Source Intelligence (OSINT)
Adversaries may execute passive reconnaissance via services or gather information from open source repositories that can be used for target profiling. Typically passive reconnaissance will be near impossible to attribute back to an actor.

| Detection      | Mitigation                                                                     |
| -------------- | ------------------------------------------------------------------------------ |
| Threat Hunting | Passive scanning is outside of internal defensive scopes, however reviews of externally <br> passive scanning and OSINT sources including employee password breaches can help |

### Identifying Target Infrastructure
Passive reconnaissance can be performed against a target or in an attempt to identify a potential target by searching for information such as IP address spaces, domains, subdomains, services, etc.

#### Tools
- [Shodan](https://www.shodan.io/)
- [Censys](https://censys.io/)
- [Crt - Certificate Search](https://crt.sh)
- [Netcraft - Webserver Infrastructure](https://www.netcraft.com/)
- [Built With - Webserver Infrastructure](https://builtwith.com/)
- [WappAlyzer - Webserver Infrastructure](https://www.wappalyzer.com/)
- [WhatWeb - Webserver Infrastructure](https://www.kali.org/tools/whatweb/)
- [Sublist3r - Subdomain Enumeration](https://www.kali.org/tools/sublist3r/)

### Discovering Email Addresses
Identifying the email addresses of key contacts within an organization can be a valuable method to scope potential targets. There are a variety of different methods to scope email accounts and personnel attached to a target, even identifying things such as management structures, work hours, phone numbers, and much more from a single name or email address.

#### Tools
- [Hunter](http://hunter.io/)

### Discovering Breached Credentials
Historic and new collation of breached credentials contain millions of username, email, and password combinations resulting from user database breaches and hacks. Collating information contain across multiple breaches can provide password patterns for users with multiple breaches making password guessing easier. Some dumps main contain clear-text credentials and others may contain hashed credentials that will require further processing to use or [decrypt](https://hashes.com/en/decrypt/hash).

#### Tools
- [Breach Parse](https://github.com/hmaverickadams/breach-parse)
- [DeHashed](https://dehashed.com/)
- [Have I Been Pwned?](https://haveibeenpwned.com/)

## Active Scanning [T1595](https://attack.mitre.org/techniques/T1595/)
Adversaries may execute active reconnaissance scans to gather information that can be used during targeting. Active scans are those where the adversary probes victim infrastructure via network traffic, as opposed to other forms of reconnaissance that do not involve direct interaction. Scanning can be achieved via several categories such as port scanning, vulnerability scanning, and wordlist scanning to name a few of the most common.

| Detection       | Mitigation                                                                     |
| ---------       | ------------------------------------------------------------------------------ |
| Network Traffic | Scanning is outside of internal defensive scopes, however reviews of externally facing <br> ports and services should regularly be monitored |

### Tools
- Scanning IP Blocks
  - [Darkcybe - Nmap Guide](https://darkcybe.github.io/posts/ETH_Tools_Nmap/)
- Vulnerablity Scanning
  - [Darkcybe - Nmap Guide](https://darkcybe.github.io/posts/ETH_Tools_Nmap/)
  - [Darkcybe - Nessus Guide] Comming Soon
  - [Darkcybe - Nikto Guide] Comming Soon
- Wordlist Scanning
  - [Darkcybe - Nmap Guide](https://darkcybe.github.io/posts/ETH_Tools_Nmap/)
  - [Darkcybe - Dirbuster Guide] Comming Soon

## Gather Victim Host Information [T1592](https://attack.mitre.org/techniques/T1592/)
Adversaries may gather information about the victim's hosts that can be used during targeting. Information about hosts may include a variety of details, including administrative data (ex: name, assigned IP, functionality, etc.) as well as specifics regarding its configuration (ex: operating system, language, etc.).