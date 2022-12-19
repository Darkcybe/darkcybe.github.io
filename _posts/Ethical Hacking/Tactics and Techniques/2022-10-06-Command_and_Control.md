---
title: Command and Control
categories: [Ethical Hacking, Tactics and Techniques]
tag: [command and control (TA0011)]
comments: true
---

## Overview

Command and control (C2 or C&C) refers to the communication and coordination between a cyber attacker and the infrastructure they use to launch an attack. This infrastructure can include compromised computers, servers, or other devices that are under the control of the attacker and used to execute their attack.

The C2 infrastructure is often used to send commands to the compromised devices and receive information back from them, such as the results of an attack or the data that has been exfiltrated. This infrastructure is a key part of many cyber attacks, as it allows the attacker to maintain control over the compromised devices and coordinate their actions.

C2 can take many forms, including using a specific website or server to communicate with the compromised devices, or using more covert methods such as embedding the C2 functionality within legitimate network traffic or using encrypted channels to communicate.

Defending against C2 is an important aspect of cybersecurity, as it can help to disrupt the attacker's ability to control their compromised devices and carry out their attack. This can be achieved through a variety of methods, such as blocking access to the C2 infrastructure, monitoring network traffic for signs of C2 activity, and using defensive technologies such as firewalls and intrusion detection systems to detect and prevent C2 communication.

MITRE describe Command and Control as; The adversary is trying to communicate with compromised systems to control them.

Command and Control consists of techniques that adversaries may use to communicate with systems under their control within a victim network. Adversaries commonly attempt to mimic normal, expected traffic to avoid detection. There are many ways an adversary can establish command and control with various levels of stealth depending on the victim’s network structure and defenses. [^1]

## Command and Control Mechanisms.

There are several types of command and control (C2) that can be used in cyber attacks:

1. **Centralized C2:** In this type of C2, the attacker maintains a central server or infrastructure that is used to send commands to and receive information from the compromised devices. This type of C2 is relatively easy to detect and disrupt, as all of the communication passes through a single point.
2. **Decentralized C2:** In decentralized C2, the attacker uses a distributed network of servers or infrastructure to communicate with the compromised devices. This makes it more difficult to disrupt the C2 infrastructure, as the attacker can continue to operate even if some of the servers or infrastructure are taken offline.
3. **Covert C2:** In covert C2, the attacker uses subtle or disguised methods to communicate with the compromised devices. This can include using encrypted channels, hiding the C2 communication within legitimate network traffic, or using steganography to embed the C2 communication within other types of data. Covert C2 is often more difficult to detect and disrupt.

### Centralized C2

Centralized command and control (C2) refers to a type of C2 in which the attacker maintains a central server or infrastructure that is used to send commands to and receive information from the compromised devices. Some examples of centralized C2 include:

1. **Websites:** The attacker can set up a specific website or server that the compromised devices communicate with in order to receive commands and send back information. This type of C2 is relatively easy to detect, as all of the communication is passing through a single point.
2. **IRC (Internet Relay Chat):** The attacker can use an IRC server to communicate with the compromised devices. IRC is a messaging protocol that allows for real-time communication over the internet.
3. **Email:** The attacker can use email to send commands to the compromised devices and receive information back. This can be done using a specific email account or by embedding the C2 communication within legitimate email traffic.
4. **VPN (Virtual Private Network):** The attacker can use a VPN to establish a secure, encrypted connection between the compromised devices and the C2 infrastructure. This can make it more difficult to detect and disrupt the C2 communication.

Defending against centralized C2 attacks involves monitoring network traffic for signs of C2 activity, using defensive technologies such as firewalls and intrusion detection systems, and implementing measures to disrupt the C2 infrastructure.

### Decentralized C2

Decentralized command and control (C2) refers to a type of C2 in which the attacker uses a distributed network of servers or infrastructure to communicate with the compromised devices. Some examples of decentralized C2 include:

1. **P2P (Peer-to-Peer) Networks:** The attacker can use a peer-to-peer network to communicate with the compromised devices. In this case, the compromised devices communicate directly with each other, rather than through a central server or infrastructure.
2. **Tor Network:** The attacker can use the Tor network to communicate with the compromised devices. The Tor network is a decentralized network of servers that allows for anonymous communication over the internet.
3. **Botnets:** The attacker can use a botnet to communicate with the compromised devices. A botnet is a network of compromised devices that can be used to launch coordinated attacks against a target.

Defending against decentralized C2 attacks involves monitoring network traffic for signs of C2 activity, using defensive technologies such as firewalls and intrusion detection systems, and implementing measures to disrupt the C2 infrastructure. It can be more difficult to disrupt decentralized C2, as the attacker can continue to operate even if some of the servers or infrastructure are taken offline.

### Covert C2

Covert command and control (C2) refers to a type of C2 in which the attacker uses subtle or disguised methods to communicate with the compromised devices. Some examples of covert C2 include:

1. **Encrypted Channels:** The attacker can use encrypted channels, such as HTTPS or SSL, to communicate with the compromised devices. This can make it more difficult to detect the C2 communication, as the encrypted traffic looks like normal, legitimate traffic.
2. **Hiding C2 Communication within Legitimate Network Traffic:** The attacker can hide the C2 communication within legitimate network traffic, such as HTTP or DNS traffic. This can make it more difficult to detect the C2 communication, as it is not visible to network monitoring tools.
3. **Steganography:** The attacker can use steganography to embed the C2 communication within other types of data, such as images or audio files. This can make it even more difficult to detect the C2 communication, as it is not visible to network monitoring tools and requires specialized tools to detect.

Defending against covert C2 attacks can be more difficult, as the C2 communication is not easily detectable. It may require more advanced techniques, such as traffic analysis and behavioral analysis, to detect and disrupt the C2 infrastructure.

## Reverse Shells

A reverse shell is a type of malicious software that allows an attacker to remotely control a compromised system. It allows the attacker to execute commands on the target system, as if they were sitting at the keyboard, and to access sensitive information and resources.

Reverse shells are typically used in conjunction with other types of attacks, such as phishing or malware infections, to gain initial access to a target system. Once the attacker has established a reverse shell, they can use it to move laterally within the network, gather sensitive information, and execute additional attacks.

There are many different ways that an attacker can create a reverse shell. One common method is to use a program called "netcat", which is a network utility that can be used to create a connection between two systems over a network. An attacker can use netcat to send a command to the target system that opens a shell, allowing the attacker to execute commands on the system.

## Remote Access Trojans

A remote access trojan (RAT) is a type of malware that allows an attacker to remotely control a compromised system. RATs are typically used to gain unauthorized access to a target system and can be used to steal sensitive information, execute additional attacks, and gain a foothold in a network.

RATs are often delivered through phishing campaigns or other types of social engineering attacks. They can be disguised as legitimate software or documents, and once installed on a system, they establish a connection to a remote server, allowing the attacker to control the system remotely.

There are many different types of RATs, and they can vary in their capabilities and features. Some RATs are simple and are only capable of basic functions, such as providing remote access to the system or gathering information about the system. Others are more complex and can perform a wide range of tasks, including keylogging, taking screenshots, and executing additional malware.

## Legitimate Remote Desktop Tools

Remote desktop applications, such as Remote Desktop Protocol (RDP), are useful tools that allow users to remotely access and control another system over a network. While these applications are useful for legitimate purposes, they can also be abused by attackers to gain command and control of a compromised system.

One way that attackers can abuse remote desktop applications is by installing a legitimate application such as AnyDesk on a target system and using the application to establish a connection to the system. This allows the attacker to remotely control the system and execute commands as if they were sitting at the keyboard, whilst masquerading under the execution of a legitimate application often used by system administrators.

## Command and Control Frameworks

There are many different command and control (C2) frameworks that are used by attackers to remotely control compromised systems and execute commands. Some popular C2 frameworks include:

1. **Cobalt Strike:** Cobalt Strike is a popular C2 framework that is used to conduct penetration testing and red teaming exercises, as well as to execute attacks in the wild. It includes a range of features, including keylogging, screen capture, and the ability to execute arbitrary commands.
2. **Metasploit:** Metasploit is a widely-used C2 framework that is used by attackers to exploit vulnerabilities and gain access to systems. It includes a range of tools and features, including a framework for developing and executing exploits, a database of known vulnerabilities, and the ability to run custom scripts.
3. **Empire:** Empire is a C2 framework that is designed to be used by attackers to conduct post-exploitation activities, such as privilege escalation and lateral movement. It includes a range of features, including the ability to execute arbitrary commands, gather system information, and generate malicious payloads.
4. **Pupy:** Pupy is a C2 framework that is designed to be used by attackers to conduct post-exploitation activities, such as lateral movement and privilege escalation. It includes a range of features, including the ability to execute arbitrary commands, gather system information, and generate malicious payloads.
5. **Sliver:** Sliver is a C2 framework that is designed to be used by attackers to remotely control compromised systems and execute commands. It is a lightweight framework that is designed to be easily deployable and scalable, and it includes a range of features that are useful for
6. **Brute Ratel:** Brute Ratel is a penetration testing tool created after reverse engineering multiple highest quality Endpoint Detection and Response (EDR) and antivirus dynamic-link libraries (DLLs). It is a post-exploitation toolkit designed to avoid detection by EDR and antivirus capabilities.

# Sources
- [^1]: [MITRE ATT&CK - Command and Control TA0011](https://attack.mitre.org/tactics/TA0011/)