---
title: Windows Security and Relative Identifiers (SIDS and RIDS)
categories: [General IT and Security, Deep Dives]
tags: [sid, rid]
comments: true
---

# Overview

During analysis of suspected or known breaches, the use of these identifiers can assist in various ways. One of the most common is looking for processes, including legitimate ones, that originate from incorrect file paths or running under the wrong account. This does delve into knowing common processes and being able to identified expected behaviors from those that may be malicious.

Security and Relative Identifiers (SID/RID) are utilized by Active Directory to identify objects known as Security Principals which include Users, Computers, and Groups. A SID value is constructed in several components with the RID being one of the more important components.

# Breaking Down SIDS

![SID and RID Example](/assets/img/posts/GEN/DD/Gen_DD_SID_RID_Example.png "SID and RID Example")

The above image depicts a the `whoami /all` command being run via the command prompt on a Windows Server. The top section depicts the current users SID with the bottom half displaying group SIDs. Below we can see the user SID for the domain user 'malnet\administrator' with a breakdown of the core components.

Digging further into these components can help with quickly identifying potentially nefarious behaviors. The following provides more context relating to the various SID components:

![SID Breakdown](/assets/img/posts/GEN/DD/Gen_DD_SID_RID_Breakdown.png "SID Breakdown")

- **Literal Prefix:** 'S' indicates that the string that follows is a SID object, '1' is a revision marker, indicating that this SID is the first revision.
- **Identifier Authority:** The issuing authority identification marker (0 - Null, 1 - World, 2 - Local, 3 - Creator, 5 - Security)
- **Sub Authorities:** Unique domain or local computer identifier
- **Relative ID:** The unique RID attached to the object

# Well Known SIDS

|           Name           |   SID/RID Value   | Identifies                                                                                     |
|:------------------------:|:-----------------:|------------------------------------------------------------------------------------------------|
|         Null SID         |      S-1-0-0      | A group with no members                                                                        |
|         Everyone         |      S-1-1-0      | Generic group that automatically includes all users of the computer                            |
|           Local          |      S-1-2-0      | Users who log on physically to terminals                                                       |
|          Console         |      S-1-2-1      | Users who log on to the physical console                                                       |
|          Network         |      S-1-5-2      | A group that includes users logged on through a network connection.                            |
|        Interactive       |      S-1-5-4      | A group that includes all users that have logged on interactively.                             |
|          Service         |      S-1-5-6      | A group that includes all service logons                                                       |
|         Anonymous        |      S-1-5-7      | A group that includes all anonymous logon sessions                                             |
|    Authenticated Users   |      S-1-5-11     | A group that includes all authenticated user logons                                            |
| Remote Interactive Logon |      S-1-5-14     | A group that includes all users logged on through terminal services                            |
|       Local System       |      S-1-5-18     | A service account used by the system                                                           |
|       Administrator      | S-1-5-21-xxxx-500 | A user account for the System Administrator. This account is <br> given full control of the system. |
|           Guest          | S-1-5-21-xxxx-501 | A built-in guest account used for users without an account <br> (disabled by default)               |
|          Krbtgt          | S-1-5-21-xxxx-502 | A service account used by the Kerberos KDC                                                     |
|       Domain Admins      | S-1-5-21-xxxx-512 | A global group of whose members are authorized to administer <br> the domain.                       |
|       Domain Users       | S-1-5-21-xxxx-513 | A global group that includes all domain users by default                                       |
|       Domain Guests      | S-1-5-21-xxxx-514 | A global group consisting of the built-in Guest account                                        |
|     Domain Computers     | S-1-5-21-xxxx-515 | A global group that includes all clients and servers connected <br> to the domain                   |
|      Administrators      | S-1-5-32-xxxx-544 | A built-in group that is populated with the Administrator and <br> Domain Admin accounts            |
|           Users          | S-1-5-32-xxxx-545 | A built-in group that contains all authenticated users                                         |
|          Guests          | S-1-5-32-xxxx-546 | A built-in group with the default guest account held within                                    |
|        Power Users       | S-1-5-32-xxxx-547 | A built-in group that by default has no members unless added <br> through associated group          |

# Sources
- [Microsoft - Well Known SIDS](https://docs.microsoft.com/en-us/windows/win32/secauthz/well-known-sids)