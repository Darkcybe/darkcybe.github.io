---
title: DFIR Overview
categories: [DFIR]
tags: []
comments: true
---

## DFIR Overview

Digital Forensics and Incident Response (DFIR) describes the process of responding in a coordinated effort to a Cyber Security Incident. The main objective of DFIR is to return the organization or impacted assets to a Business as Usual (BaU) state whilst providing Root Cause Analysis on the incident in terms of executive and technical reporting. Typically, a DFIR process will incorporate several team members that form a Cyber Security Incident Response Team (CSIRT), activated upon the identification of a suspected breach and will respond to the incident collecting and recording evidence and carrying out mitigation actions in order to protect the organization.

DFIR consists of two main functions:

- **Digital Forensics:** Responsible for examining system data, user activity, and other evidence (digital or physical) to verbosely map the activities performed by and attacker and possible attribution of the actor.
- **Incident Response:** Covers personnel and the overarching process of the coordinated response to cyber security incidents.

There are several frameworks in existence that detail the response lifecycle when responding to cyber security incidents, such as the [Lockheed Martin Cyber Kill Chain](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html), [The Diamond Model](https://www.threatintel.academy/diamond/), [SANS PICERL](https://www.cynet.com/incident-response/incident-response-sans-the-6-steps-in-depth/) and [NIST SP 800-61r2](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf). There is also the [D3fend Framework](https://d3fend.mitre.org/) however this applies for to a SOC detection and mitigation stance rather than Incident Response. The most commonly used within the industry, within my experience anyway, are the PICERL and NIST frameworks which separate incident response into the following phases:

- **PICERL**
  1. Preparation
  2. Identification
  3. Containment
  4. Eradication
  5. Recovery
  6. Lessons Learned
- **NIST**
  1. Preparation
  2. Detection and Analysis
  3. Containment, Eradication, and Recovery
  4. Post-Incident Activity

Following an established response process or framework is crucial to the success in service restoration following an incident. Typically a process will contain several phases that the DFIR team must achieve in order to complete the response, NIST provide such a framework within publication [SP800-61: Computer Security Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final). A short summary of the phases can be found below, with more specific recommendations and guides found in the phase links above.

## Event Vs. Incident

Understanding fundamental terminology and how to correctly categorize events, incidents, risk potential and efficacy is key to effective response. Although organizations should and will individually categorize and define their own specific methodologies, the baseline standards as described in the aforementioned NIST Guide as well as incorporating my own understanding and preferences.

### Event

An event can span a wide variety of actions and behaviors and should be used to categorize system or network activities in which may be indicative of adverse actions. Alerts from products deployed in an environment should be considered initially as events before verification is carried out to determine whether is should be reclassified as an incident or not. Not every event will be an adverse incident, however ever incident will contain one or more events. The below quote comes directly from the NIST Computer Security Incident Handling Guide to define the term.

> An event is any observable occurrence in a system or network. Events include a user connecting to a file share, a server receiving a request
> for a web page, a user sending email, and a firewall blocking a connection attempt. Adverse events are events with a negative consequence, such
> as system crashes, packet floods, unauthorized use of system privileges, unauthorized access to sensitive data, and execution of malware that
> destroys data.

### Incident

Incidents encompass all events that negatively impact the Confidentiality, Integrity and/or Availability (CIA) of the Business in general. Again he below quote from the NIST Computer Security Incident Handling Guide defines the term and provides several examples of what may constitute an event to be escalated to Incident status.

> A computer security incident is a violation or imminent threat of violation of computer security policies, acceptable use policies, or standard
> security practices. Examples of incidents are:
>
> - An attacker commands a botnet to send high volumes of connection requests to a web server, causing it to crash.
> - Users are tricked into opening a “quarterly report” sent via email that is actually malware; running the tool has infected their computers
> and established connections with an external host.
> - An attacker obtains sensitive data and threatens that the details will be released publicly if the organization does not pay a designated sum
>  of money.
> - A user provides or exposes sensitive information to others through peer-to-peer file sharing services.

### Event and Incident Categorization

When data is ingested into a SIEM platform or ticketing system, it is the job of a security analyst to perform analysis of the events, initially determining the categorization of the event. This is perhaps the most challenging component of security monitoring, the determination of whether an incident has occurred. Categorization encompasses multiple considerations and objectives, such as:

- **Event Verification:** Initial review of event type and high level verification of data source/alert. Has a server shutdown due to legitimate Administrative tasks?
- **Scope:** Correlation and aggregation of events from multiple data sources. How wide spread is the event?
- **Lifecycle Identification:** Is the event a potential precursor to further nefarious activity or is the event a clear indication of malicious activity?
- **Efficacy:** Based upon the potential impact to business operations and using knowledge of system processes, networking, threat intelligence and honed skills in analysis a determination of alert efficacy will be made leading to a result of:
  - **Incident:** A valid threat was detected.
  - **False Positive:** – An event in which triggered an alert was incorrectly identified or posed no extant risk.
  - **Benign:** A valid threat was detected, however there is no apparent risk due to the condition being explained or expected.
  - **Normal:** Events that have been previously triaged and determined to be normal and expected within the environment.
  - **Indeterminable/Noteworthy:** Not enough evidence or context to confidently decide, events that appear suspicious however non-impactful to operations but do require further monitoring. Often events under this category require further investigation.

### Incident Prioritization

Once an event has been determined to be an incident that negatively impacts the CIA, prioritization is the next logical step and perhaps one of the most critical in organizing responsive actions. Determining incident priority should be based on relevant factors, such as the following:

- **Impact:** How the incident impacts existing functionality of the affected systems. Not only the current functional impact of the incident should be taken into consideration, but also the likely future functional impact of the incident if it is not immediately contained.

| Category |                                                   Definition                                                  |
|:--------:| ------------------------------------------------------------------------------------------------------------- |
|   None   | No effect to the organization’s ability to provide all services to all users                                  |
|    Low   | Minimal effect; the organization can still provide all critical services to all users but has lost efficiency |
|  Medium  | Organization has lost the ability to provide a critical service to a subset of system users                   |
|   High   | Organization is no longer able to provide some critical services to any users                                 |

[Impact Categories](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

- **Recoverability:** Possible responses that the team may take when handling the incident. An incident with a high impact and low effort to recover from is an ideal candidate for immediate action from the team. The team should prioritize the response to each incident based on its estimate of the business impact caused by the incident and the estimated efforts required to recover from the incident.

|     Category    |                                                        Definition                                                       |
| --------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Regular         | Time to recovery is predictable with existing resources                                                                 |
| Supplemented    | Time to recovery is predictable with additional resources                                                               |
| Extended        | Time to recovery is unpredictable; additional resources and outside help are needed                                     |
| Not Recoverable | Recovery from the incident is not possible (e.g., sensitive data exfiltrated and posted publicly); launch investigation |

[Recoverability Categories](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

## Response Lifecycle

The way in which a team or single incident responder approaches an incident will differ based on the attack stage and type of attack launched against the organization. The aforementioned NIST framework provides a great high level understanding of how each phase can be categorized. To expand on each phase, other frameworks exist such as the [RE&CT framework](https://atc-project.github.io/atc-react/), which is designed to build on top of IR phases and the MITRE ATT&CK framework to provide an actionable template for Incident Response techniques.

### Preparation

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

- **Containment**
  - Short-term methods
  - Data collection and retention
  - Artifact Interruption (Kill processes, Firewall Rule Modification, Account Blocking and Password Resets)
- **Eradication**
  - Removal of Artifacts (Files, Registry Entries, Backup Restorals)
  - System and Application Patching
- **Recovery**
  - Return to BaU
  - Monitoring and Environmental Baselines

### Post-Incident Activity

- Lessons Learned
- Preparation Review (People, Policy and Procedure)
- Funding Increase

## Goals of DFIR

Primarily IR is established to answer key facts for an organizations management team, who did what to which systems and how to we recover as quickly as possible. Digital Forensics on the other hand specialize in providing verbose evidence that can be used by an organization for any law or insurance proceedings arising from the incident. The work of both function is commonly joined during incidents which allows the simplification of achievable goals for the incident response team to follow into two main categories.

### Investigation

- Determine Pivot Points and Time frame
- Initial Entry Vector Identification
- Tools and Malware Used
- Incident Scope (Identification of Impacted Systems and Accounts)
- Damage Assessment

### Remediation

- Reverse Changes Made
- Protect Future Breaches from Occurring

## DFIR Analysis Techniques

There a number of specialist skills that a DFIR analyst or engineer may learn or individually specialize in, some of the more common are:

- Malware Analysis
- Cyber Threat Intelligence
- Threat Hunting
- System Forensics
  
### Malware Analysis

Malware can be categorized into several core types based upon their intended functions. Historically, early malware would be designed to perform a single or few tasks as developed by the attacker. Recently however, there has been an increasing amount of malware application designed in a modular fashion that enable an attacker to choose which functions the deployed malware can perform. The below image identifies some of the more common types of malware.

[![Types of Malware](/assets/img/posts/DFIR/Overview_Types_of_Malware.png "Types of Malware")](https://www.crowdstrike.com/cybersecurity-101/malware/types-of-malware/)

There are several challenges when analyzing a piece of malware. Each type of malware will have unique analysis phases and required environmental configurations in order to effectively understand it’s intended and possible functionality. Not only does each type of malware require a unique analysis technique, there are other factors that will be encountered such as encryption, programming languages, anti-debugging techniques, and many more.

#### Goal of Malware Analysis

During incident response, in-depth malware analysis or reverse engineering will not always be required. Known hacker tool kits are often detected via signature or behavioral characteristics that are used by security vendors to identify the malware family, type, and capabilities. In these circumstance, malware analysis is not always an efficient mechanism in assisting an organization with responding to and recovering from an attack.

The overarching goals of malware analysis are focused on observing malware behavior’s, deconstructing the file to gain import artifacts that can assist in understanding the capabilities and providing Indicators of Compromise (IOCs) containment and for future detection. Analyzing and de-constructing malware samples is a time consuming process, hence the opening paragraph stating that it is a techniques not always required during Incident Response engagements. Malware analysis and reverse engineering techniques really shine in circumstances where analysts are dealing with unknown or undetected malware strains, investigating suspicious behavior’s that do not trigger alerts, and for proving verbose documentation such as forensic reports.

#### Types of Malware Analysis

There are two common types of malware analysis that all malware professionals should be familiar with; Static and Dynamic Analysis.

- **Static Analysis:** Static analysis is the initial phase of malware analysis in which a malicious file is examined without running the file itself. Determinations of potential classification and artefact of interest are common outcomes of static analysis. However, there are complex scenarios in which static analysis can be hindered such as code packing and encryption which require an advanced level of understanding areas of disassembly, coding, and operating system architectures.
- **Dynamic Analysis:** Dynamic analysis is typically a secondary phase after static analysis has been performed. Dynamic analysis involves observing the malware's behaviors during and after execution. Highly detailed signatures can be discovered during dynamic analysis such as registry modifications, dropped files and many more. Debugging may also be covered within dynamic analysis which is the examination of a running processes internal constructs.

### Cyber Threat Intelligence (CTI)

Cyber Threat Intelligence is both a proactive mindset that can bolster Incident Response as well as a formalized output that can detail attack patterns that SOC teams can ingest to better and faster detect threats. Through the ongoing analysis of datasets containing capabilities, motivations, and goals of adversaries, threat reports can be created which enrich static data observed by many security or native tooling such as IP addresses, Domains, event log records, etc.

### Threat Hunting

Threat Hunting refers to the process of hypothesis generation for potential attacks and searching for indications that an environment has remnants of abuse. Threat Hunters generally leverage available CTI and expand on possible tactic and technique extensions that may occur via a particular threat actor. An example of this would be a threat hunter ingesting a report containing indictors of a particular PowerShell command to gather system information and hypothesizing other ways in which the same information can be returned, such as running the command via WMI. In Incident Response, threat hunters can be an initiating member responsible for the identification of a confirmed incident as well as providing tracing analysis through the incident engagement.

## System Forensics

- Memory Forensics
- Host Forensics (All OS's)
- Mobile Forensics
