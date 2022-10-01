---
title: Active Directory
categories: [Ethical Hacking, Windows]
tag: [active directory]
comments: true
---
# Overview

Active Directory enables system administrators to build and manage domains, users, and objects on a network. Active Directory offers a means to categorize Users into logical groups and subgroups in objects named Organizational Units (OUs) while supplying access control at each level. Active Directory servers are often promoted to Domain Controllers (DCs) as they are a central point of management for network authentication, authorization and core domain functionality.

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

# Techniques

## Reconnaissance

## 

# Sources
- [Microsoft - Active Directory Domain Services Overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)