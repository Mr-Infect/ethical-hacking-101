
Modern Security Operations Centers (SOCs) rely on multiple layers of visibility to detect cyber threats. No single tool can provide complete visibility across endpoints, networks, applications, users, and cloud infrastructure. Therefore, SOCs deploy a combination of endpoint monitoring, network monitoring, intrusion detection, packet analysis, threat intelligence, and SIEM technologies.

Among the most powerful open-source and enterprise SOC technologies are:

Wazuh
Sysmon
Zeek
Suricata
Wireshark

When integrated correctly, these tools provide deep visibility across the entire attack lifecycle and significantly improve detection capabilities aligned with the MITRE ATT&CK Framework.

Why Multiple Security Tools Are Required

Consider a phishing attack:

User Opens Email
       ↓
Word Document Opens
       ↓
PowerShell Executes
       ↓
Payload Downloaded
       ↓
C2 Communication
       ↓
Credential Theft
       ↓
Lateral Movement

A single tool may only see one stage.

Tool	Visibility
Sysmon	Endpoint Activity
Wazuh	Log Correlation
Suricata	Network Attacks
Zeek	Network Metadata
Wireshark	Raw Packets

Together they provide complete visibility.

SOC Architecture with Wazuh, Sysmon, Zeek and Suricata
Endpoints
Servers
Domain Controllers
Applications
      ↓

Sysmon Logs
Windows Events
Linux Logs
      ↓

Wazuh Agents
      ↓

Wazuh Manager
      ↓

SIEM Dashboard

──────────────────────────

Network Traffic
      ↓

SPAN Port / TAP
      ↓

Suricata
Zeek
      ↓

Security Events
      ↓

Wazuh / SIEM

──────────────────────────

Packet Capture
      ↓

Wireshark
      ↓

Threat Investigation
Wazuh
What is Wazuh?

Wazuh is an open-source SIEM and XDR platform that provides:

Log management
Endpoint security
Threat detection
File integrity monitoring
Vulnerability assessment
Security compliance monitoring

It acts as the central brain of many SOC environments.

Wazuh Components
Wazuh Agent

Installed on:

Windows
Linux
macOS

Collects:

Logs
Events
Sysmon data
Security alerts
Wazuh Manager

Responsible for:

Event analysis
Correlation
Rule matching
Alert generation
Wazuh Dashboard

Provides:

Alert visualization
Threat monitoring
Investigation tools
Reporting
Why SOC Analysts Love Wazuh

Provides:

Centralized Visibility

Instead of checking multiple systems:

Server A
Server B
Server C
Windows PC
Linux Server

Everything appears in one dashboard.

Correlation

Example:

4625 Failed Login
+
PowerShell Execution
+
Network Connection

=
High Severity Alert
Threat Hunting

Analysts can search:

powershell.exe

mimikatz

rundll32

certutil

across thousands of systems.

Wazuh Use Cases
Brute Force Detection

Monitors:

4625
4625
4625
4625

Alert generated.

Malware Detection

Detects:

Encoded PowerShell
Suspicious DLLs
Known Hashes
File Integrity Monitoring

Monitors:

System32
Registry
Configuration Files
Sysmon
What is Sysmon?

Sysmon (System Monitor) is part of Microsoft Sysinternals.

It extends Windows logging dramatically.

Default Windows logs are limited.

Sysmon provides:

Process Creation
Network Connections
Registry Changes
DNS Queries
File Activity
Process Injection Detection
Why Sysmon is Important

Without Sysmon:

User Logged In
Process Started

Limited information.

With Sysmon:

Process Name
Parent Process
PID
Command Line
Hash
User
Network Activity

Full visibility.

Key Sysmon Events
Event ID 1

Process Creation

Contains:

Parent Process
Child Process
Command Line
Hash

Example:

winword.exe
      ↓
powershell.exe

Potential malicious macro.

Event ID 3

Network Connection

Example:

powershell.exe
       ↓
185.x.x.x

Possible C2 communication.

Event ID 7

DLL Load Monitoring

Useful for malware detection.

Event ID 8

Create Remote Thread

Often indicates:

Process injection
Malware
Event ID 10

Process Access

Critical for:

LSASS Access
Credential Dumping
Event ID 22

DNS Query

Example:

powershell.exe
      ↓
malicious-domain.com
Sysmon and MITRE ATT&CK

Sysmon maps to many ATT&CK techniques.

Sysmon Event	ATT&CK Technique
Event 1	Execution
Event 3	Command & Control
Event 8	Process Injection
Event 10	Credential Access
Event 13	Persistence
Event 22	DNS Tunneling
Zeek
What is Zeek?

Zeek is a Network Security Monitoring (NSM) platform.

Formerly called:

Bro IDS

Unlike Suricata, Zeek focuses on:

Network Metadata

rather than signatures.

What Zeek Collects

Zeek generates logs for:

HTTP
DNS
SSL/TLS
FTP
SSH
SMTP
SMB
DHCP
Example Zeek DNS Log
Host A
   ↓
evil-domain.com

Recorded in:

dns.log
Example Zeek HTTP Log
GET /payload.exe

Logged in:

http.log
Why SOC Analysts Use Zeek

Zeek answers questions like:

Who talked to whom?
10.1.1.5
   ↓
185.12.4.8
What was downloaded?
payload.exe
malware.zip
Which domains were visited?
evil.com
bad-domain.net
Zeek Logs

Important files:

conn.log
dns.log
http.log
ssl.log
files.log
smtp.log
Zeek for Threat Hunting

Analysts investigate:

Rare Domains
Long Connections
Beaconing
Unusual TLS Traffic
MITRE ATT&CK and Zeek

Useful for detecting:

ATT&CK Tactic	Example
Discovery	SMB Scanning
Command & Control	DNS Beaconing
Exfiltration	Large Transfers
Lateral Movement	SMB Connections
Suricata
What is Suricata?

Suricata is a Network Intrusion Detection and Prevention System.

Functions:

IDS
IPS
Network Monitoring
Threat Detection
How Suricata Works

Network packets enter:

Traffic
    ↓
Suricata Engine
    ↓
Rule Matching
    ↓
Alert
Signature-Based Detection

Suricata uses signatures.

Example:

ET TROJAN
ET MALWARE
ET EXPLOIT
Example Detection
Known Malware Traffic

Generates:

High Severity Alert
Important Suricata Logs
eve.json

Most important.

Contains:

Alerts
DNS
HTTP
TLS
Flow records
Suricata Detection Examples
Exploit Detection

Detects:

Log4j
SQL Injection
Remote Exploits
Malware Detection

Detects:

Emotet
Cobalt Strike
TrickBot
Command and Control Detection

Detects:

Known C2 Domains
Known C2 IPs
SOC Analyst Perspective

Suricata provides:

Immediate Alerts

Example:

ET MALWARE
Known Malware Traffic

SOC analyst immediately investigates.

Threat Prioritization

Alerts already contain:

Severity
Category
Signature
Source IP
Destination IP
MITRE ATT&CK Mapping

Suricata commonly detects:

ATT&CK Phase	Example
Initial Access	Exploits
Execution	Payload Download
Command and Control	Beaconing
Exfiltration	Data Transfer
Wireshark
What is Wireshark?

Wireshark is a packet analysis tool.

Unlike Suricata and Zeek:

Wireshark captures raw packets.
Why SOC Analysts Use Wireshark

When an alert occurs:

Suricata Alert
        ↓
Zeek Metadata
        ↓
Wireshark Validation
Example Investigation

Suricata Alert:

Possible Cobalt Strike

Analyst opens PCAP.

Wireshark shows:

TLS Traffic
DNS Queries
HTTP Requests
Payload Delivery
Common Wireshark Filters
HTTP
http
DNS
dns
SMB
smb
RDP
tcp.port == 3389
PowerShell Download
http contains "powershell"
Wireshark in Incident Response

Used for:

Malware Analysis
Data Exfiltration Investigation
Command and Control Analysis
Network Forensics
Integration of All Tools
Full SOC Visibility
Endpoint Side

Sysmon provides:

powershell.exe started
Network Side

Zeek provides:

evil-domain.com queried
IDS Side

Suricata provides:

Known Malware Signature
SIEM Side

Wazuh correlates:

Sysmon Alert
+
Suricata Alert
+
Zeek DNS Query

=
Critical Incident
Example Attack Detection Workflow
Stage 1 – Phishing
User Opens Word File

Sysmon:

winword.exe
     ↓
powershell.exe
Stage 2 – Download

PowerShell downloads payload.

Zeek:

HTTP Request Logged
Stage 3 – Malware Traffic

Suricata:

Known Malware Signature
Stage 4 – Correlation

Wazuh:

PowerShell
+
Malware Traffic
+
DNS Query

=
Critical Alert
Detection Engineering Perspective

Detection Engineers create rules.

Sysmon Rules

Detect:

Office
     ↓
PowerShell

ATT&CK:

T1059
Command Execution
Zeek Rules

Detect:

DNS Beaconing

ATT&CK:

T1071
Application Layer Protocol
Suricata Rules

Detect:

Known Malware Traffic

ATT&CK:

T1105
Ingress Tool Transfer
Wazuh Correlation Rules

Detect:

4625
+
4688
+
Network Connection

Generate high-priority incident.

Mapping to MITRE ATT&CK Framework
Initial Access

Tools:

Suricata
Zeek

Detect:

Exploits
Phishing payloads
Execution

Tools:

Sysmon
Wazuh

Detect:

PowerShell
CMD
Script execution
Persistence

Tools:

Sysmon
Wazuh

Detect:

Registry changes
Scheduled tasks
Privilege Escalation

Tools:

Sysmon
Windows Events

Detect:

Admin privilege abuse
Credential Access

Tools:

Sysmon
Wazuh

Detect:

LSASS access
Mimikatz activity
Discovery

Tools:

Zeek
Sysmon

Detect:

Network scans
Enumeration
Lateral Movement

Tools:

Zeek
Suricata
Sysmon

Detect:

SMB
PsExec
WMI
Command and Control

Tools:

Zeek
Suricata
Wireshark

Detect:

DNS Beaconing
HTTPS C2
Malware Communications
Exfiltration

Tools:

Zeek
Wireshark
Suricata

Detect:

Large Transfers
Cloud Uploads
Suspicious Connections
SOC Analyst Investigation Flow
Alert Generated
      ↓
Wazuh Alert Review
      ↓
Check Sysmon Process Tree
      ↓
Check Parent Process
      ↓
Review Command Line
      ↓
Check Zeek DNS Logs
      ↓
Review Zeek HTTP Logs
      ↓
Check Suricata Alerts
      ↓
Analyze Packet Capture
      ↓
Map to MITRE ATT&CK
      ↓
Determine Severity
      ↓
Contain Threat
      ↓
Document Incident
Conclusion

A mature SOC relies on the combined strengths of Wazuh, Sysmon, Zeek, Suricata, and Wireshark to achieve full-spectrum visibility across endpoints and networks. Sysmon provides deep endpoint telemetry, Zeek offers rich network metadata, Suricata delivers signature-based intrusion detection, Wireshark enables packet-level forensic analysis, and Wazuh acts as the central SIEM and correlation engine. Together, these tools allow SOC analysts, threat hunters, incident responders, and detection engineers to identify, investigate, and respond to threats across every stage of the MITRE ATT&CK lifecycle. By correlating endpoint events, network behavior, signatures, and packet evidence, organizations can significantly improve detection coverage, reduce dwell time, and strengthen overall cybersecurity operations.
