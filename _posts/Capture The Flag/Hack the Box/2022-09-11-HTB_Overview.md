---
title: Hack the Box Overview
categories: [Capture The Flag, Hack the Box]
tags: [ctf , htb]
comments: true
---

## Hack The Box (HTB) Labs

Hack The Box is a cloud based Capture The Flag (CTF) platform that offers a variety of practical cybersecurity challenges, covering categories such as penetration testing, cryptography, and digital forensics to name a few. The platform itself is based on a gamified scoring system, where challengers are rewarded with points based upon their ‘pwnage’ of the individual challenges which are marked by providing a reward key in the form of a hidden flag on the target system. As of writing, there are 294 machines of varying difficulty, operating systems, and attack paths for challengers to hone their skills on.

Hack The Box provides several pricing options, from a free basic account with access to general level challenges, VIP, and VIP+ tiers offer additional challenges and features requiring a paid subscription. The free tier subscription does require a VPN connection to the vulnerable servers in order to begin any of the challenges, this post aims to show how to initially establish the VPN connection and start a lab.

### Required Software and Platforms

| Title | Description |
| ----- | ----------- |
| [ParrotOS](https://parrotlinux.org/) or [Kali Linux](https://www.kali.org/) | ParrotOS and Kali Linux are the two major pen testing distributions that you will run into. Both have OpenVPN pre-installed making connection to the HTB servers that little bit easier. |
| [OpenVPN](https://openvpn.net/) | If you would prefer to use a customized environment for connecting to the HTB servers, OpenVPN will need to be installed. |

### Connecting to HTB Labs via OpenVPN

ParrotOS is my preference when performing HTB challenges, so we will start here with how to connect to the vulnerable challenge subnets. The process is basically replicated for what ever environment you are connecting from.

1. Login to the HTB platform using your account credentials and select the ‘Connect to HTB’ option at the top right of the screen to open the connection settings.
   - Three options will be displayed; Machines, Starting Point, and Release Area. Select Machines, and then OpenVPN.
   - A connection window will appear prompting for the best server connection based on geographic area. Select the appropriate VPN Access and VPN Server selections and chose between UDP or TCP for network traffic (may be required if your attacking host is behind a firewall or proxy), press ‘Download VPN’ to download the VPN ticket.
2. Once the VPN ticket has downloaded `lab_<username>.ovpn`, move the file to your attacker machine if it was not downloaded onto it and run the following command to initiate the connection.

   ```bash
   sudo openvpn /path/to/lab_<username>.ovpn
   ```
   {: .nolineno }

3. Once the host establishes a connection to the HTB servers, the platform VPN configuration should change to a green status, providing a connected IP address within the HTB servers subnet.

### Starting and Connecting to a Lab

Once the above VPN connection is established, the next phase is to chose and connect to a challenge.

1. On the left hand platform menu, chose ‘Labs’ to open a drop down selection.
   - There are multiple options here; Starting Point, Tracks, Machines, Challenges, Fortresses, Endgames, and Pro Labs. For this example, we will select Machines which are a variety of various hosts.
   - Upon selecting the challenge, select the machine (in this example ‘Support’) which will present another option window, select ‘Spawn Machine’ to initiate the instance.
   - You will now be connected to the instance with connection details for the challenge and can start attacking the host on the displayed IP address.
2. Once the flag has been found, select the menu option ‘Submit Flag’ to input the hash value and pass the challenge.

### Handy Notes

- When searching for challenge flags, the command `find / -name flag.txt >2/dev/null` can be run. The last portion of the command filters out errors from returning to the terminal.

## Sources

- [Hack The Box](https://www.hackthebox.com/)
- [Hack The Box - Introduction to Lab Access](https://help.hackthebox.com/en/articles/5185687-introduction-to-lab-access)
