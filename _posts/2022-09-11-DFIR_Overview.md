---
title: DFIR Overview
categories: [DFIR]
tags: []
comments: true
---
# DFIR Overview
Digital Forensics and Incident Response (DFIR) describes the process of responding in a coordinated effort to a Cyber Security Incident. The main objective of DFIR is to return the organization or impacted assets to a Business as Usual (BaU) state whilst providing Root Cause Analysis on the incident in terms of executive and technical reporting. Typically, a DFIR process will incorporate several team members that form a Cyber Security Incident Response Team (CSIRT), activated upon the identification of a suspected breach and will respond to the incident collecting and recording evidence and carrying out mitigation actions in order to protect the organisation.

DFIR consists of two main functions:
- **Digital Forensics:** Responsible for examining system data, user activity, and other evidence (digtial or physical) to verbosely map the activities performed by and attacker and possible attribution of the actor.
- **Incident Response:** Covers personnel and the overarching process of the coordinated response to cyber security incidents. 

There are several frameworks in existence that detail response lifecycles when responding to cyber security incidents, such as the [Lockheed Martin Cyber Kill Chain](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html), [The Diamond Model](https://www.threatintel.academy/diamond/), [SANS PICERL](https://www.cynet.com/incident-response/incident-response-sans-the-6-steps-in-depth/) and [NIST SP 800-61r2](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf). The most commonly used within the industry, within my experience anyway, are the PICERL and NIST frameworks which separate incident response into the following phases:

**PICERL**
1. Preperation
2. Identifiication
3. Containment
4. Eradication
5. Recovery
6. Lessons Learned

**NIST**
1. Preperation
2. Detection and Analysis
3. Containment, Eradication, and Recovery
4. Post-Incident Activity

Following an established reponse process or framework is crucial to the success in service restoration following an incident. Typically a process will contain several phases that the DFIR team must achieve in order to complete the response, NIST provide such a framework within publication [SP800-61: Computer Security Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final). A short summary of the phases can be found below, with more specific recommendations and guides found in the phase links above.

## NIST Response Lifecycle
### Preperation
The goal of the preparation phase in perspective of IR is to identify the specific requirements of the organization to effectively respond to a multitude of different incident scenarios. Core components off include:
- People (Roles, Training, Vendor Relationships)
- Process (Policies, Communications Plans)
- Procedures (Tools, Preventative Methods, Reactive Methods)

### Detection and Analysis
Due to the multitude of avenues in which an incident can occur, several layers of detection methods should be employed to ensure robust detection and monitoring capabilities. This phases is the starting point of an incident, the detection of suspicious activity having occurred and the characterization of that activity as either expected or unexpected warranting further investigation and engagement of a wider response. Common detection methods include:
- Detection Alerts (AV, IPS, HIDS, SIEM)
- Unusual Activity (Processes, Network Behavior, Artifacts, Monitoring Anomalies, External Notification)
- Event and Incident Declaration (Efficacy and Categorization, Initial Analysis)

### Containment, Eradication, and Recovery
**Containment**
- Short-term methods
- Data collection and retention
- Artifact Interruption (Kill processes, Firewall Rule Modification, Account Blocking and Password Resets)

**Eradication**
- Removal of Artifacts (Files, Registry Entries, Backup Restoral)
- System and Application Patching

**Recovery**
- Return to BaU
- Monitoring and Environmental Baselining

**Post-Incident Activity**
- Lessons Learned
- Preparation Review (People, Policy and Procedure)
- Funding Increase

# Goals of DFIR
Primarily IR is established to answer key facts for an organizations management team, who did what to which systems and how to we recover as quickly as possible. Digital Forensics on the other hand specialize in providing verbose evidence that can be used by an organization for any law or insurance proceedings arising from the incident. The work of both function is commonly joined during incidents which allows the simplification of achievable goals for the incident response team to follow into two main categories. 

**Investigation**
- Determine Pivot Points and Timeframe
- Initial Entry Vector Identification
- Tools and Malware Used
- Incident Scope (Identification of Impacted Systems and Accounts)
- Damage Assessment

**Remediation**
- Reverse Changes Made
- Protect Future Breaches from Occurring

# DFIR Analysis Techniques
There a number of specalist skills that a DFIR analyst or engineer may learn or individually speacalise in, some of the more common are:
- Malware Analysis
- Memory Forensics
- Host Forensics (All OS's)
- Mobile Forensics
- Log Analysis
- Threat Hunting
- Cyber Threat Intelligence

## Malware Analysis
Malware can be categorized into several core types based upon their intended functions. Historically, early malware would be designed to perform a single or few tasks as developed by the attacker. Recently however, there has been an increasing amount of malware application designed in a modular fashion that enable an attacker to choose which functions the deployed malware can perform. The below image identifies some of the more common types of malware.

[![Types of Malware](/assets/img/posts/DFIR/Overview_Types_of_Malware.png "Types of Malware")](https://www.crowdstrike.com/cybersecurity-101/malware/types-of-malware/)

There are several challenges when analyzing a piece of malware. Each type of malware will have unique analysis phases and required environmental configurations in order to effectively understand it’s intended and possible functionality. Not only does each type of malware require a unique analysis technique, there are other factors that will be encountered such as encryption, programming languages, anti-debugging techniques, and many more.

### Goal of Malware Analysis
During incident response, in-depth malware analysis or reverse engineering will not always be required. Known hacker toolkits are often detected via signature or behavioral characteristics that are used by security vendors to identify the malware family, type, and capabilities. In these circumstance, malware analysis is not always an efficient mechanism in assisting an organization with responding to and recovering from an attack.

The overarching goals of malware analysis are focused on observing malware behavior’s, deconstructing the file to gain import artifacts that can assist in understanding the capabilities and providing Indicators of Compromise (IOCs) containment and for future detection. Analyzing and de-constructing malware samples is a time consuming process, hence the opening paragraph stating that it is a techniques not always required during Incident Response engagements. Malware analysis and reverse engineering techniques really shine in circumstances where analysts are dealing with unknown or undetected malware strains, investigating suspicious behavior’s that do not trigger alerts, and for proving verbose documentation such as forensic reports.

### Types of Malware Analysis
There are two common types of malware analysis that all malware professionals should be familiar with; Static and Dynamic Analysis.
**Static Analysis**
Static analysis is the initial phase of malware analysis in which a malicious file is examined without running the file itself. Determinations of potential classification and artefacts of interest are common outcomes of static analysis. However, there are complex scenarios in which static analysis can be hindered such as code packing and encryption which require an advanced level of understanding areas of disassembly, coding, and operating system architectures.
**Dynamic Analysis**
Dynamic analysis is typically a secondary phase after static analysis has been performed. Dynamic analysis involves observing the malwares behaviors during and after execution. Highly detailed signatures can be discovered during dynamic analysis such as registry modifications, dropped files and many more. Debugging may also be covered within dynamic analysis which is the examination of a running processes internal constructs.

# Sources
- 