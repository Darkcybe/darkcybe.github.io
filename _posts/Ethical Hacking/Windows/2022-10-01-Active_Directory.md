---
title: Active Directory
categories: [Ethical Hacking, Windows]
tag: [active directory]
comments: true
---

## Overview

Active Directory (AD) enables system administrators to build and manage domains, users, and objects on a network. Active Directory offers a means to categorize Users into logical groups and subgroups in objects named Organizational Units (OUs) while supplying access control at each level. Active Directory servers are often promoted to Domain Controllers (DCs) as they are a central point of management for network authentication, authorization and core domain functionality.

The Active Directory is divided into three main tiers: domains, trees, and forests. A domain is a collection of objects (users or devices). Multiple domains can be grouped together under a single domain, sub-domains are known as trees. A forest is a collection of trees that can be grouped together. Each of these levels can be given different access and communication privileges.

Active Directory offers a variety of services that are grouped together as "Active Directory Domain Services," or AD DS. Among these services are:

Domain Services
: Stores centralized data and manages user-domain communication; includes login authentication and search functionality.

Certificate Services
: Responsible for the creation, distribution, and management of secure certificates.

Lightweight Directory Services (LDAP)
: Provides support for directory-enabled applications.

Directory Federation Services
: Enables a user to authenticate in multiple web applications in a single session using single-sign-on (SSO).

Rights Management
: Safeguards intellectual property by preventing unauthorized use and distribution of digital content.

DNS/NTP/DHCP Services
: This service is used to resolve domain names, act as a centralized time source, and can effectively manage a networks IP ranges and leases via DHCP.

Without needing much mention, Active Directory/Domain Controllers are an extremely high valued target.

## Techniques

### Reconnaissance

Identifying an Active Directory server can be achieved via the exposed ports and often the nomenclature of the server naming. Often Active Directory or Domain Controller servers will have the terms AD or DC within the hostname to identify it locally. Examples could appear like `abcAD01` or `abcDC02`. These servers are not typically publicly facing, unless misconfigured.

Port scanning can reveal specific common ports that are exposed on Active Directory servers, such as the table below. Using Nmap to gain further information on specific ports can be achieved via NSE scripts. Reconnaissance activities assume that tasks are unauthenticated.

| Port                                | Description                                                                    |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| 389                                 | Lightweight Directory Access Protocol (LDAP)                                   |
| 445                                 | Server Message Block (SMB)                                                     |
| 464                                 | Kerberos Password Change                                                       |
| 500                                 | IPsec ISAKMP                                                                   |
| 53                                  | Domain Name Services (DNS)                                                     |
| 636                                 | Lightweight Directory Access Protocol over SSL Server (LDAPS)                  |
| 88                                  | Kerberos Authentication                                                        |
| 4500                                | NAT-T                                                                          |
| 9389                                | Active Directory Web Services <br> Active Directory Management Gateway Service |
| 138 <br> 139                        | NetBIOS                                                                        |
| 3268 <br> 3269                      | Global Catalog                                                                 |
| 135 <br> 1024-5000 <br> 49152-65535 | Remote Procedure Call (RPC) and Windows Management Instrumentation (WMI)       |

### Credential Access

If access has been gained to the same network as the AD server, there are various attacks that can be used to gain credentials or relay credentials gathered. Some of these tools and techniques that aid in credential access against AD controlled networks are:

1. [CAPEC 94 - Adversary-in-the-Middle (AiTM)](https://darkcybe.github.io/posts/94-AiTM/)

### Discovery

Post exploitation discovery can yield a lot of valuable information regarding an AD controller environment. Some tools and techniques available to perform discovery tasks to extract these objects of interest are:

1. [BloodHound](HOLDER)
2. [PowerView](HOLDER)

## Sources

- [Microsoft - Active Directory Domain Services Overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
