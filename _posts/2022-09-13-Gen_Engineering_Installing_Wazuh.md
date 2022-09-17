---
title: Installing and Configuring Wazuh EDR
categories: [General IT and Security,Engineering]
tags: [wazuh,]
comments: true
---

# Overview

Wazuh is a security platform that provides unified XDR and SIEM protection for endpoints and cloud workloads. The solution is composed of a single universal agent and three central components: the Wazuh server, the Wazuh indexer, and the Wazuh dashboard.

This post will run through the steps involved in the installation and configuration of the server-side components on a single server instance in order to begin ingesting events from agents. The following stages and requirements imply that all of these components are installed on the single-node server and will vary if configuring Wazuh is a distributed deployment.

Wazuh is free and open source. Its components abide by the GNU General Public License, version 2, and the Apache License, Version 2.0 (ALv2).

# Hardware and OS Dependencies

| Agents |                    CPU                   | RAM | Storage (90 Days) |
|:------:|:----------------------------------------:|:---:|:-----------------:|
|  1-25  |                  4 vCPU                  | 8GB |        50GB       |
|  25-50 |                  8 vCPU                  | 8GB |       100GB       |
| 50-100 |                  8 vCPU                  | 8GB |       200GB       |

> For monitoring over 100 agents, a distributed deployment is recommended. 

|    OS   |        Version        |
|:-------:|:---------------------:|
|   AWS   |     Amazon Linux 2    |
| Red Hat | Enterprise Linux 7, 8, 9 |
|  CentOS |          7,8          |
|  Ubuntu |  16.04, 18.04, 20.04, 22.04  |

## Exposed Ports

| Port      | Use                                            |
|-----------|------------------------------------------------|
| 1514-1515 | Wazuh TCP (Agent enrollment and communication) |
| 514       | Wazuh UDP (SYSLOG)                             |
| 55000     | Wazuh API                                      |
| 9200      | Wazuh indexer HTTPS (Elasticsearch)            |
| 443       | Wazuh dashboard HTTPS                          |

# Installing Wazuh 4.3.6 on Ubuntu 20.04

1. Download and execute the pre-built installation wizard provided by Wazuh. This will automatically run through the installation of pre-requisuite packages, Elasticsearch (indexer, Kibana, filebeat) and Wazuh server setup and configuration
   - Once the installation is complete, initial admin credentials will be displayed to the terminal. Make sure they are noted down as they are required to access the web interface in the following step.
  
  ```bash
  curl -sO https://packages.wazuh.com/4.3/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
  ```

2. Access the Wazuh web interface at `https://<wazuh-dashboard-ip/` and your credentials from Step 1.
   - Username: `admin`
   - Password: `%%%%%%%%%`
3. Communication between the web interface and backend is via HTTPS by default. However, the API accounts created during the installation utilize default credentials and should be changed.
   - There are two main accounts used which required modification via the Wazuh API console:
     - wazuh
     - wazuh-wui
   - Changing the wazuh-wui user password will impede the Wazuh UI. The Wazuh configuration file accordingly with the new credentials. 
     - `/usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml`
  
  ```plaintext
  GET /security/users
  PUT /security/users/#
    {
        "password": "@BC1"
    }
  ```
# Configurations

## Adding Agents to Wazuh

The Wazuh agent is deployed to hosts in order to monitor various artifacts and report the telemetry back to the Wazuh server in near real time. Key features of the agent include:

- Log Collection
- System Inventory
- Active Response
- Command Execution
- Malware Detection
- File Integrity Monitoring (FIM)
- Cloud Security
- Security Configuration Assessment (SCA)
- Container Security

> Ports 1514-1515 TCP must be open on the firewall to allow Wazuh agent enrollment and connection to the Wazuh Manager

### Windows Agents

1. Choose Windows as the Operating System
2. Enter the Wazuh-Manager IP address or FQDN
3. Assign the agent to a group (can be assigned to default if no groups exist)
4. Execute the PowerShell script supplied on the target host (required administrative privileges)
5. Once installed, start the agent via PowerShell. After which, the agent should be displayed via the Agents menu on the Wazuh dashboard.
   
   ```powershell
   NET START WazuhSvc
   ```

### Ubuntu Agents

1. Choose Ubuntu as the Operating System
2. Choose the correct architecture, in this example 'x86_64' to indicate Ubuntu is running on a AMD 64 architecture.
3. Enter the Wazuh-Manager IP address or FQDN
4. Assign the agent to a group (can be assigned to default if no groups exist)
5. Execute the bash command supplied on the target host
6. Once installed, start the agent using the below bash command. After which, the agent should be displayed via the Agents menu on the Wazuh dashboard.
   
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable wazuh-agent
   sudo systemctl start wazuh-agent
   ```

## Adding Integrations to Wazuh

Wazuh integrations are configured on the Wazuh server `ossec.conf` file, located at `/var/ossec/etc/` on the manager

### Slack

1. Add the integration configuration code to the `ossec.conf` file under the `<ossec_config>` block, also consider altering the `<level>` field to filter which events to trigger slack notifications for, see the block quote for further information regarding event level thresholds.
   
   ```conf
   <integration> 
     <name>slack</name>
     <hook_url>https://hooks.slack.com/services/...</hook_url> <!-- Replace with your Slack hook URL -->
     <alert_format>json</alert_format>
     <level>%</level>
   </integration>
   ```

2. After saving the configuration, restart the Wazuh-Manager service 
   
   ```bash
   sudo systemctl restart wazuh-manager
   ```

> The default alert threshold is set to 3 - Successful/Authorized events. This will cause a lot of unnecessary alerts to be generated. Changing the threshold to 6 - Low relevance attack can assist in limiting noise. The [Rules Classification](https://documentation.wazuh.com/current/user-manual/ruleset/rules-classification.html) table describes each alert level from 0 - 16.

# Sources
- [Wazuh](http://wazuh.com/)
- [Wazuh - Defining an Alert Level Threshold](https://documentation.wazuh.com/current/user-manual/manager/alert-threshold.html)