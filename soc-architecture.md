# Security Operations Center (SOC) Notes

# Introduction to Security Operations Center (SOC)

A **Security Operations Center (SOC)** is a centralized function within an organization that continuously monitors, detects, analyzes, investigates, and responds to cybersecurity incidents. The SOC serves as the frontline defense against cyber threats by combining people, processes, and technology to protect an organization's digital assets, networks, applications, and data.

Modern SOCs operate 24/7 and are designed to identify threats before they cause significant damage. They collect security telemetry from various sources, correlate events, prioritize alerts, investigate suspicious activities, and coordinate incident response efforts.

---

# Core Objectives of a SOC

The primary objectives of a SOC include:

1. Continuous security monitoring.
2. Threat detection and analysis.
3. Incident investigation and response.
4. Threat intelligence integration.
5. Vulnerability management.
6. Compliance monitoring.
7. Security reporting and metrics.
8. Security tool management.
9. Digital forensics and evidence collection.
10. Security awareness and improvement.

---

# SOC Concentration Areas

SOC operations generally focus on several key concentration areas:

## 1. Security Monitoring

Monitoring infrastructure, applications, networks, cloud environments, and endpoints for suspicious activities.

Activities include:

* Log collection
* Event monitoring
* Alert generation
* Performance monitoring
* User activity tracking

## 2. Threat Detection

Identifying malicious activities using:

* Signature-based detection
* Behavioral analytics
* Machine learning models
* Threat intelligence feeds
* Correlation rules

## 3. Incident Response

Managing security incidents from identification through remediation.

Key phases:

* Detection
* Triage
* Investigation
* Containment
* Eradication
* Recovery
* Lessons learned

## 4. Threat Hunting

Proactively searching for threats that bypass automated detection systems.

Focus areas:

* Advanced Persistent Threats (APT)
* Insider threats
* Malware persistence
* Lateral movement
* Credential abuse

## 5. Digital Forensics

Investigating compromised systems and collecting evidence.

Includes:

* Memory analysis
* Disk analysis
* Network forensics
* Malware analysis
* Timeline creation

## 6. Vulnerability Management

Identifying and remediating security weaknesses.

Tasks include:

* Vulnerability scanning
* Risk assessment
* Patch management
* Verification testing

---

# Different Teams Involved in SOC

A mature SOC consists of multiple specialized teams.

## 1. SOC Tier 1 Analysts (L1)

### Responsibilities

* Monitor alerts.
* Initial triage.
* Alert validation.
* Ticket creation.
* Escalation to higher tiers.

### Daily Activities

* Review SIEM alerts.
* Classify incidents.
* Document findings.
* Follow runbooks.

### Required Skills

* Networking basics
* Log analysis
* Security fundamentals
* Incident categorization

---

## 2. SOC Tier 2 Analysts (L2)

### Responsibilities

* Deep investigation.
* Incident validation.
* Root cause analysis.
* Advanced threat analysis.

### Daily Activities

* Analyze attack patterns.
* Investigate malware.
* Examine endpoint activity.
* Review threat intelligence.

### Required Skills

* Threat hunting
* Malware analysis
* Incident response
* Endpoint investigation

---

## 3. SOC Tier 3 Analysts (L3)

### Responsibilities

* Advanced threat analysis.
* Complex investigations.
* Detection engineering.
* Security automation.

### Daily Activities

* Develop detection rules.
* Conduct threat hunts.
* Analyze sophisticated attacks.
* Improve SOC processes.

### Required Skills

* Scripting
* Reverse engineering
* Security architecture
* Threat intelligence

---

## 4. Incident Response Team

### Responsibilities

* Respond to active incidents.
* Coordinate containment efforts.
* Perform eradication.
* Restore affected services.

### Tasks

* Host isolation.
* Credential resets.
* Malware removal.
* Communication with stakeholders.

---

## 5. Threat Intelligence Team

### Responsibilities

* Gather intelligence.
* Analyze threat actors.
* Monitor emerging threats.

### Sources

* Open Source Intelligence (OSINT)
* Commercial feeds
* Government advisories
* Dark web monitoring

---

## 6. Threat Hunting Team

### Responsibilities

* Proactively search for threats.
* Identify hidden attackers.
* Develop hypotheses.

### Techniques

* Behavioral analysis
* IOC hunting
* TTP mapping
* Anomaly detection

---

## 7. Detection Engineering Team

### Responsibilities

* Build detection logic.
* Improve alert quality.
* Reduce false positives.

### Deliverables

* SIEM use cases
* Correlation rules
* Detection content
* Security analytics

---

## 8. Security Engineering Team

### Responsibilities

* Manage SOC infrastructure.
* Tool integration.
* Platform maintenance.

### Activities

* SIEM deployment
* Log onboarding
* Automation implementation
* Tool upgrades

---

## 9. Forensics Team

### Responsibilities

* Evidence collection.
* Malware analysis.
* Legal support.

### Investigations

* Data breaches
* Insider threats
* Malware infections
* Advanced attacks

---

## 10. SOC Manager

### Responsibilities

* Team leadership.
* KPI monitoring.
* Resource planning.
* Stakeholder communication.

### Key Metrics

* Mean Time to Detect (MTTD)
* Mean Time to Respond (MTTR)
* False Positive Rate
* Incident Volume

---

# SOC Operational Workflow

A typical SOC workflow follows:

```text
Log Collection
       ↓
Event Normalization
       ↓
Correlation
       ↓
Alert Generation
       ↓
Triage
       ↓
Investigation
       ↓
Response
       ↓
Recovery
       ↓
Reporting
```

---

# Commonly Used SOC Tools

## SIEM (Security Information and Event Management)

Central platform for log management and threat detection.

Examples:

* Splunk Enterprise Security
* IBM QRadar
* ArcSight
* Microsoft Sentinel
* LogRhythm
* Elastic SIEM

### Functions

* Log aggregation
* Correlation
* Alerting
* Dashboarding
* Reporting

---

## EDR (Endpoint Detection and Response)

Monitors endpoint activities.

Examples:

* CrowdStrike Falcon
* Microsoft Defender for Endpoint
* SentinelOne
* Carbon Black
* Cortex XDR

### Functions

* Process monitoring
* Threat detection
* Endpoint isolation
* Forensics

---

## SOAR (Security Orchestration Automation and Response)

Automates repetitive SOC tasks.

Examples:

* Cortex XSOAR
* Splunk SOAR
* Tines
* Swimlane

### Functions

* Playbooks
* Automated response
* Case management
* Ticketing integration

---

## IDS/IPS

Intrusion Detection and Prevention Systems.

Examples:

* Snort
* Suricata
* Zeek

### Functions

* Traffic inspection
* Threat detection
* Attack prevention

---

## Vulnerability Management Tools

Examples:

* Nessus
* Qualys
* Rapid7 InsightVM
* OpenVAS

### Functions

* Vulnerability scanning
* Risk scoring
* Patch tracking

---

## Threat Intelligence Platforms

Examples:

* MISP
* Recorded Future
* ThreatConnect
* Anomali

### Functions

* IOC management
* Threat enrichment
* Intelligence sharing

---

## Ticketing Systems

Examples:

* ServiceNow
* Jira
* Remedy

### Functions

* Incident tracking
* Workflow management
* Documentation

---

## Network Monitoring Tools

Examples:

* Wireshark
* SolarWinds
* PRTG
* Nagios

### Functions

* Packet analysis
* Performance monitoring
* Troubleshooting

---

# SOC Infrastructure Overview

A SOC infrastructure is designed to collect, process, store, analyze, and respond to security events from across an organization.

The infrastructure consists of multiple layers.

---

# SOC Infrastructure Components

## 1. Data Sources Layer

This is where security logs originate.

### Sources

#### Network Devices

* Routers
* Switches
* Firewalls
* VPN gateways

#### Security Devices

* IDS
* IPS
* WAF
* NAC

#### Endpoints

* Workstations
* Servers
* Laptops
* Mobile devices

#### Applications

* Web applications
* ERP systems
* CRM systems

#### Cloud Platforms

* AWS
* Azure
* GCP

#### Identity Systems

* Active Directory
* LDAP
* IAM platforms

---

## 2. Log Collection Layer

Logs are collected using:

* Syslog
* Agents
* APIs
* Event Forwarders

Examples:

```text
Windows Logs → Agent
Linux Logs → Syslog
Firewall Logs → Syslog
Cloud Logs → API
```

---

## 3. Data Ingestion Layer

Processes incoming data.

Functions:

* Parsing
* Filtering
* Deduplication
* Normalization

Example:

```text
Raw Logs
    ↓
Parser
    ↓
Normalized Events
```

---

## 4. Data Storage Layer

Stores collected logs.

Types:

### Hot Storage

Recent logs.

Examples:

* Elasticsearch
* Splunk Indexers

### Warm Storage

Medium-term logs.

### Cold Storage

Long-term archival.

Examples:

* AWS S3
* Azure Blob Storage

---

## 5. Analytics Layer

Performs detection and analysis.

Capabilities:

* Correlation
* Machine learning
* Behavioral analytics
* Risk scoring

Example Rule:

```text
IF

5 Failed Logins
AND

1 Successful Login

Within 10 Minutes

THEN

Generate Alert
```

---

## 6. Threat Intelligence Layer

Adds context to events.

Functions:

* IOC matching
* Threat enrichment
* Reputation checking

Data Sources:

* IP reputation feeds
* Malware feeds
* OSINT feeds

---

## 7. Response Layer

Handles incident response.

Actions:

* Disable account
* Block IP
* Isolate endpoint
* Quarantine file

---

# How SOC Infrastructure Is Built

SOC infrastructure is typically implemented in phases.

## Phase 1: Asset Identification

Identify:

* Servers
* Endpoints
* Applications
* Network devices
* Cloud assets

---

## Phase 2: Log Onboarding

Connect log sources to SIEM.

Example:

```text
Firewall
Server
Endpoint
Cloud

     ↓

     SIEM
```

---

## Phase 3: Use Case Development

Create detection rules.

Examples:

* Brute force attacks
* Privilege escalation
* Data exfiltration
* Malware execution

---

## Phase 4: Dashboard Creation

Develop dashboards for:

* Security posture
* Threat trends
* Incident metrics

---

## Phase 5: Automation

Implement SOAR playbooks.

Example:

```text
Alert Generated
        ↓
SOAR Playbook
        ↓
Validate IOC
        ↓
Block IP
        ↓
Create Ticket
```

---

# SOC Architecture

A standard SOC architecture follows a layered design.

## High-Level Architecture

```text
+---------------------------------------------------+
|                    SOC Dashboard                  |
+---------------------------------------------------+

                       ↑

+---------------------------------------------------+
|                SIEM / Analytics                   |
+---------------------------------------------------+

                       ↑

+---------------------------------------------------+
|          Log Collection and Aggregation           |
+---------------------------------------------------+

                       ↑

+---------------------------------------------------+
|                Security Tools Layer               |
| EDR | IDS | IPS | WAF | DLP | IAM | Cloud Logs   |
+---------------------------------------------------+

                       ↑

+---------------------------------------------------+
|             Enterprise Infrastructure             |
| Servers | Endpoints | Network | Applications     |
+---------------------------------------------------+
```

---

# Modern SOC Architecture

Modern SOCs integrate cloud-native technologies.

```text
Users
   ↓

Applications
   ↓

Cloud Environment
   ↓

Security Controls
   ↓

Log Collectors
   ↓

Data Lake
   ↓

SIEM
   ↓

Threat Intelligence
   ↓

SOAR
   ↓

SOC Analysts
```

---

# Key Security Controls Integrated into SOC

## Firewall

Controls network traffic.

## Web Application Firewall (WAF)

Protects web applications.

## Data Loss Prevention (DLP)

Prevents sensitive data leakage.

## Identity and Access Management (IAM)

Manages user access.

## Multi-Factor Authentication (MFA)

Enhances authentication security.

## Network Access Control (NAC)

Controls device access.

## Cloud Security Tools

Monitor cloud environments.

---

# SOC Metrics and KPIs

Important SOC measurements include:

| KPI                | Description                   |
| ------------------ | ----------------------------- |
| MTTD               | Mean Time to Detect           |
| MTTR               | Mean Time to Respond          |
| Alert Volume       | Total alerts received         |
| False Positives    | Incorrect alerts              |
| Incident Count     | Security incidents handled    |
| Detection Coverage | Coverage of attack techniques |
| SLA Compliance     | Incident response performance |

---

# Challenges Faced by SOC Teams

1. Alert fatigue.
2. False positives.
3. Lack of visibility.
4. Skills shortage.
5. Complex environments.
6. Cloud security challenges.
7. Advanced persistent threats.
8. Insider threats.
9. Tool integration issues.
10. Large log volumes.

---

# Conclusion

A Security Operations Center (SOC) is the central cybersecurity defense function of an organization. It combines specialized teams, security technologies, operational processes, and well-designed infrastructure to monitor, detect, investigate, and respond to cyber threats. A mature SOC consists of multiple teams including L1, L2, L3 analysts, incident responders, threat hunters, detection engineers, forensics specialists, and security engineers. These teams rely on technologies such as SIEM, EDR, SOAR, IDS/IPS, vulnerability scanners, and threat intelligence platforms.

The SOC infrastructure is built using layered architecture that collects data from endpoints, servers, networks, applications, and cloud platforms, processes the data through log collection and analytics systems, enriches events with threat intelligence, and enables rapid response through automation and skilled analysts. As organizations continue to adopt cloud computing, zero-trust security, and automation, modern SOCs are evolving into highly integrated, intelligence-driven security ecosystems capable of defending against increasingly sophisticated cyber threats.
