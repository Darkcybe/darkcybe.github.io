---
title: PowerShell-Empire
categories: [Ethical Hacking Tools, Frameworks and Suites]
tags: [powershell-empire, starkiller, powerview, mimkatz, credential access (TA0006), privilege escalation (TA0004), discovery (TA0007), collection (TA0009)]
comments: true
---

## Overview

PowerShell-Empire is a post-exploitation framework that is built upon a large collection of PowerShell modules and scripts. It also contains various scripts written in C# and Python that can be used against a target OS. The PowerShell-Empire framework currently has hundreds of modules that can aid in almost all penetration testing tactics and techniques.

PowerShell-Empire have also developed a front-end GUI called Starkiller for the framework which makes configuration and activities that little bit easier to manage, especially when dealing with multiple targets. However, there is still the CLI available, with the framework running a server/client model.

Kali Linux comes with PowerShell-Empire and Starkiller pre-installed. [^1]

| Tool Name | Version | MITRE ATT&CK Tactic | MITRE ATT&CK Technique |
| --------- | ------- | ------------------- | ---------------------- |
| [PowerShell-Empire](https://github.com/BC-SECURITY/Empire) | V4.8.0 | [Credential Access](https://attack.mitre.org/tactics/TA0006/) <br> [Privilege Escalation](https://attack.mitre.org/tactics/TA0004/) <br> [Discovery](https://attack.mitre.org/tactics/TA0007/) <br> [Collection](https://attack.mitre.org/tactics/TA0009/) <br> [Command and Control](https://attack.mitre.org/tactics/TA0011/) <br> [Exfiltration](https://attack.mitre.org/tactics/TA0010/) <br> [Execution](https://attack.mitre.org/tactics/TA0002/) <br> [Persistence](https://attack.mitre.org/tactics/TA0003/) <br> [Defense Evasion](https://attack.mitre.org/tactics/TA0005/) <br> [Lateral Movement](https://attack.mitre.org/tactics/TA0008/)| |

## Starting PowerShell-Empire with Starkiller on Kali Linux

1. Start the PowerShell-Empire Server and Client in separate terminal windows. Ensure that the server is operational prior to starting the client to ensure that they can communicate.

    ```bash
    sudo powershell-empire server
    sudo powershell-empire client
    ```

2. Create a new user on the PowerShell-Empire Client. Once the new user has been created, the PowerShell-Empire Client terminal can be closed if using the Starkiller GUI, otherwise the client is the interface to be used for configuring and interacting with agents.

    ```bash
    (Empire) > admin
    (Empire) > create_user %USERNAME% %PASSWORD%
    ```

3. Open Starkiller and logon using the credentials set in the previous section. The URL by default is set to `https://localhost:1337`.

## Adding a Listener through Starkiller

1. Navigate to the Listener window
2. Press the `create` button on the top right to enter the New Listener prompt and select the type of listener using the drop-down menu.
3. For this example, the `HTTP Listener` is selected. Configure the Listener as below:

    ```plaintext
    Name: darkcybeHttp
    Host: 172.16.2.2
    Port: 1335
    ```

4. Press `submit` to initiate the HTTP Listener

> Additional configuration can be set, such as:
> - KillDate: An expiration date that sets agent autocleanup
> - DefaultProfile: Sets the URL GET request parameters to blend into normal traffic
> - UserAgent: Set the UA string, useful to masquerade traffic
> - ServerVersion: Set to a common Server Header to masquerade traffic

## Adding a Stager through Starkiller

1. Navigate to the Stager window
2. Press the `create` button on the top right to enter the New Stager prompt and select the type of stager using the drop-down menu.
3. For this example, the `windows/reverseshell` is selected. Configure the Stager as below:

    ```plaintext
    Listener: darkcybeHttp
    LocalHost: 172.16.2.2
    ```

4. Press `submit` to create the stager. The stager can then be downloaded and transferred to the target host.

> Payloads can be created for Windows, MacOs, and Linux.

## Interacting with Agents through Starkiller

1. When a target executes the stager, the agent will connect to the Listener and be displayed on the Agents window, as per the image below:

    ![PowerShell Empire - Agents](/assets/img/posts/ETH/ETH_TOOLS/FRAMEWORKS/PsEmp_Agents.png "PowerShell Empire - Agents")

2. Selecting the agent will open an agent specific window containing a number of different options:

   - **Interact:** Allows for shell commands and PowerShell-Empire Modules to be executed on the agent.
   - **File Browser:** Directory traversal including file upload and download functionality.
   - **Tasks:** A history of actions carried out on the host
   - **View:** Agent specific configuration

## PowerShell-Empire Modules

### Discovery Modules

When executing the Bloodhound modules, the Sharphound archive defaults to `C:\Users\%USERNAME%\` on the target agent. The download directory on the attacking Kali host is `/var/lib/powershell-empire/server/downloads/%AGENT%/`.

| Action                   | Module                                                                   | Configuration |
| ------------------------ | ------------------------------------------------------------------------ | ------------- |
| Find a Domain Controller | powershell/situational_awareness/network/powerview/get_domain_controller |               |
| Identify Antivirus       | powershell/situational_awareness/host/antivirusproduct                   |               |
| Bloodhound AD Scanning   | powershell/situational_awareness/network/bloodhound3                     |               |

### Credential Access Modules

Using the Mimkatz LogonPasswords modules automatically adds collected credentials to the credentials window within PowerShell-Empire.

| Action                   | Module                                                                   | Configuration |
| ------------------------ | ------------------------------------------------------------------------ | ------------- |
| Mimkatz Password Dump    | powershell/credentials/mimikatz/logonpasswords                           |               |
| Browser Credential Dump  | powershell/collection/FoxDump <br> powershell/collection/ChromeDump      |               |
| Responder/Inveigh LLMNR Abuse | powershell/collection/inveigh                                       |               |

### Privilege Escalation Modules

| Action                   | Module                                                                   | Configuration |
| ------------------------ | ------------------------------------------------------------------------ | ------------- |
| Get SYSTEM Privileges    | powershell/privesc/getsystem                                             |               |
| Search for Privesc Vulnerabilities | powershell/privesc/sherlock <br> powershell/privesc/powerup/allchecks |        |

## Sources

[^1]: [Kali Linux - PowerShell-Empire](https://www.kali.org/tools/powershell-empire/)