# Live Azure Honeypot & SOC: Cyber Attacks in Real Time
 ![SIEM Topology](https://imgur.com/Ue5OzUS.png)<br>

## Introduction

 In this project, I am creating a small-scale honeynet on Microsoft Azure to attract real-world attackers from around the globe. The purpose is to demonstrate best security practices, incident response strategies, and the outcomes of strengthening the environment. The approach involves deliberately deploying vulnerable virtual machines without internet safeguards, inviting attackers into the environment. After collecting relevant logs in the Log Analytics Workspace, Microsoft Sentinel will be utilized to create attack maps, alerts, and incidents. The objective is to showcase the metrics before and after implementing security measures, based on incidents observed during a 24-hour capture period.

## Microsoft Components Utilized

- Azure Virtual Network (VNet)
- Azure Network Security Group (NSG)
- Virtual Machines (2x Windows, 1x Linux)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault 
- Azure Storage Account 
- Microsoft Sentinel 
- Microsoft Defender for Cloud
- Powershell


## Architecture Before Hardening

 - In the initial 'BEFORE' stage of the project, resources were deployed with the intention of attracting attention from the public internet. The Virtual Machines had their Network Security Groups (NSGs) and built-in firewalls configured with wide-open settings, permitting unrestricted access from any source. Furthermore, other resources like storage accounts and databases were deployed with public endpoints accessible to the internet, without implementing Private Endpoints for enhanced security. These configurations were maintained for 24 hours, allowing the public to access the machines and generate the attack maps mentioned earlier.
 <br />
 
 <b>This attack map showcases the number of incidents generated by leaving the Network Security Group (NSG) open. </b>
 
   ![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/bNOtiKt.png)<br>

 <br />
 <br />
 
 <b>This attack map highlights the incidents for syslog authentication failures experienced by the Linux server. </b>
 
![Linux Syslog Auth Failures](https://i.imgur.com/nFR0Ehf.png)<br>

 <br />
 <br />
 
 <b>This attack map shows RDP and SMB failures against the Window machine.</b>
 
![Windows RDP/SMB Auth Failures](https://i.imgur.com/4bylhdW.png)<br>

 <br />
 <br />
 
 <b>This attack map shows failures against the MSSQL server.</b>
 
![MSSQL](https://i.imgur.com/TYIlNVI.png)<br>

 <br />
 <br />
 
 ## After Hardening Measures and Security Controls

In the "AFTER" stage, based off the incidents created from the "Before" 24 hour capture, I implemented hardening measures and security controls to improve the environment's security from attackers.<br /> 
These improvements included:

- <b>Network Security Groups (NSGs)</b>: I hardened the NSGs by only allowing my own public IP address to come thrugh otherwise all other traffic would be blocked by the new parameteres created.

- <b>Built-in Firewalls</b>: In my virtual machines I configured the built-in firewalls so that it would deny access from unauthorized users. 

- <b>Private Endpoints</b>: For other Azure resources, I replaced the public endpoints with Private Endpoints. This ensured that access to sensitive resources, such as storage accounts and databases, was limited to only the virtual network.

## Attack Maps After Hardening / Security Controls

<br />

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

 <br />
 
## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:<br/>
Start Time 2023-10-30 08:30 AM<br/>
Stop Time 2023-31-31 08:30 AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent (Windows VM)            | 4358
| Syslog (Linux VM)                   | 2345
| SecurityAlert (Microsoft Defender for Cloud            | 6
| SecurityIncident (Sentinel Incidents)        | 73
| NSG Inbound Malicious Flows Allowed | 103



## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our secure environment for 24 hours:<br/>
Start Time 2023-10-31 5:30 PM<br/>
Stop Time	2023-11-01 5:30 PM


| Metric                   | Count
| ------------------------ | -----
| SecurityEvent (Windows VM)            | 2364
| Syslog (Linux VM)                   | 24
| SecurityAlert (Microsoft Defender for Cloud            | 0
| SecurityIncident (Sentinel Incidents)        | 0
| NSG Inbound Malicious Flows Allowed | 0

<br/>


## Utilizing NIST 800.61r2 Computer Incident Handling Guide

For each simulated attack I practiced incident responses following NIST SP 800-61 R2.

![NIST 800.61](https://i.imgur.com/6PTG7c0l.png)

Each organization will have policies related to an incident response that should be followed. This event is just a walkthrough for possible actions to take in the detection of malware on a workstation.  

#### Preparation

- The Azure lab was prepared to ingest all logs into the Log Analytics Workspace. Sentinel and Defender were configured, and alert rules were established.

#### Detection & Analysis

- Malware was identified on a workstation, posing a threat to system and data confidentiality, integrity, or availability.
- An alert was assigned to an owner, categorized as "High" severity, and marked as "Active."
- The primary user account and all affected systems were identified.
- A comprehensive system scan, utilizing updated antivirus software, was performed to detect the malware.
- Verified the authenticity of the alert as a "True Positive".
- Sent notifications to appropriate personnel as required by the organization's communication policies.

#### Containment, Eradication & Recovery

- The infected system and any other affected systems were isolated in quarantine.
- If malware removal failed or the system sustained damage, the system would have been shut down and disconnected from the network.
- Based on organizational policies, affected systems might be restored to a known clean state, using methods like system images or clean OS installations. Alternatively, up-to-date antivirus solutions could be employed for cleaning.

#### Post-Incident Activity

- In this simulated scenario, an employee had downloaded a game containing malware.
- All information was gathered and analyzed to identify the root cause, assess damage extent, and evaluate response effectiveness.
- A report detailing the incident was shared with all stakeholders.
- Corrective actions were implemented to address the root cause.
- A review of the incident was conducted to extract valuable lessons learned.


<br />

## Conclusion

In this project, a small-scale honeynet was set up on Microsoft Azure, and log sources were ingested into a Log Analytics workspace. Microsoft Sentinel was utilized to generate alerts and incidents based on the collected logs. Metrics were initially measured in the insecure environment, and then again after implementing security measures. The significant decrease in security events and incidents after the security controls were applied highlights their effectiveness in securing the environment.

It's important to note that if the network resources were extensively used by regular users, it's likely that more security events and alerts could have been generated within the 24-hour period after implementing the security controls.
