## Windows-Based SOC Monitoring Notes

# Introduction

In a Windows-focused Security Operations Center (SOC), analysts spend a significant amount of time monitoring operating system activities, processes, user actions, authentication events, PowerShell execution, privilege escalation, persistence mechanisms, and attack techniques used by adversaries.

Understanding Windows internals is essential because most enterprise environments run on Windows servers, Active Directory, and Windows endpoints. Attackers commonly abuse legitimate Windows tools such as PowerShell, WMI, CMD, LSASS, and scheduled tasks to evade detection.

These notes cover the major categories of monitoring required in a Windows SOC environment, including process monitoring, Event IDs, parent-child process relationships, authentication monitoring, PowerShell analysis, Sysinternals tools, and attack detection techniques.

---

# Windows Monitoring Categories in SOC

A Windows SOC generally focuses on:

1. Authentication Monitoring
2. Process Monitoring
3. Parent-Child Process Analysis
4. PowerShell Monitoring
5. Command Line Monitoring
6. Service Monitoring
7. Registry Monitoring
8. File Monitoring
9. Network Monitoring
10. Scheduled Task Monitoring
11. WMI Monitoring
12. Active Directory Monitoring
13. LSASS Monitoring
14. Persistence Monitoring
15. Privilege Escalation Monitoring
16. Credential Theft Detection
17. Endpoint Detection Monitoring

---

# Windows Logging Sources

SOC analysts collect logs from multiple sources.

```text
Windows Endpoint
      ↓

Security Logs
System Logs
Application Logs
PowerShell Logs
Sysmon Logs
Defender Logs
DNS Logs

      ↓

SIEM
```

---

# Important Windows Event Logs

Windows Event Viewer contains multiple log channels.

## Security Logs

Location:

```text
Event Viewer
   ↓
Windows Logs
   ↓
Security
```

Contains:

* Logon events
* Logoff events
* Account changes
* Privilege assignments
* Process creation
* Kerberos activity

---

## System Logs

Contains:

* Service starts
* Driver loading
* System crashes
* Hardware events

---

## Application Logs

Contains:

* Application errors
* Software crashes
* Program activity

---

## PowerShell Logs

Contains:

* PowerShell execution
* Script activity
* Module loading

---

## Sysmon Logs

Provides advanced endpoint visibility.

Includes:

* Process creation
* Network connections
* Registry changes
* File creation
* DNS requests

---

# Process Monitoring

One of the most important SOC activities.

Every executable running on Windows creates a process.

Examples:

```text
chrome.exe
powershell.exe
cmd.exe
explorer.exe
svchost.exe
notepad.exe
```

Analysts monitor:

* New processes
* Suspicious processes
* Hidden processes
* Malicious process chains

---

# Process ID (PID)

Every process receives a unique Process ID.

Example:

```text
Process Name : powershell.exe

PID : 4872
```

SOC analysts use PID values to:

* Track execution
* Correlate events
* Investigate attacks

---

# Parent Process and Child Process

Every process originates from another process.

Example:

```text
explorer.exe
     ↓
cmd.exe
     ↓
powershell.exe
```

### Parent Process

The process that launches another process.

### Child Process

The process created by the parent.

---

# Why Parent-Child Analysis Matters

Attackers often abuse trusted applications.

Example:

```text
winword.exe
     ↓
powershell.exe
```

Normal:

```text
explorer.exe
    ↓
notepad.exe
```

Suspicious:

```text
winword.exe
    ↓
powershell.exe
```

This may indicate:

* Malicious macro
* Malware infection
* Initial access attack

---

# Common Malicious Parent-Child Relationships

## Word Launching PowerShell

```text
winword.exe
      ↓
powershell.exe
```

Possible:

* Macro attack
* Malware execution

---

## Excel Launching CMD

```text
excel.exe
     ↓
cmd.exe
```

Possible:

* VBA malware
* Script execution

---

## Outlook Launching PowerShell

```text
outlook.exe
      ↓
powershell.exe
```

Possible:

* Phishing payload

---

## Browser Launching PowerShell

```text
chrome.exe
      ↓
powershell.exe
```

Possible:

* Drive-by compromise

---

## WMI Launching CMD

```text
wmiprvse.exe
      ↓
cmd.exe
```

Possible:

* Remote execution

---

# Event ID Monitoring

Windows Event IDs are critical for SOC investigations.

---

# Authentication Event IDs

## Event ID 4624

Successful Logon

Example:

```text
User successfully logged in
```

Monitor:

* Logon type
* Source IP
* Username

---

## Event ID 4625

Failed Logon

Possible indicators:

* Password spraying
* Brute force attacks

Monitor:

* Multiple failures
* Source host

---

## Event ID 4634

Logoff Event

Indicates user logout.

---

## Event ID 4648

Explicit Credential Usage

Occurs when alternate credentials are used.

Example:

```text
Run As
Remote Login
```

Potential lateral movement.

---

## Event ID 4672

Special Privileges Assigned

Indicates admin login.

Monitor carefully.

---

# Account Management Event IDs

## Event ID 4720

User Account Created

---

## Event ID 4722

Account Enabled

---

## Event ID 4725

Account Disabled

---

## Event ID 4726

Account Deleted

---

## Event ID 4732

User Added to Group

Monitor:

```text
Domain Admins
Administrators
Enterprise Admins
```

---

# Process Creation Event

## Event ID 4688

Most Important Event

Records process creation.

Example:

```text
Process Name
Parent Process
Command Line
PID
User
```

SOC analysts use 4688 extensively.

Example:

```text
powershell.exe
```

Command line:

```text
powershell.exe -enc XXXXXXXXX
```

Strong indicator of malicious activity.

---

# Sysmon Event IDs

Sysmon provides much richer visibility.

---

## Sysmon Event ID 1

Process Creation

Contains:

* Parent process
* Child process
* Hashes
* Command line

---

## Sysmon Event ID 3

Network Connection

Example:

```text
powershell.exe
      ↓
185.23.x.x
```

Useful for C2 detection.

---

## Sysmon Event ID 7

Image Loaded

Detects DLL loading.

---

## Sysmon Event ID 8

Create Remote Thread

Common in malware injection.

---

## Sysmon Event ID 10

Process Access

Very important for LSASS attacks.

---

## Sysmon Event ID 11

File Creation

Useful for ransomware monitoring.

---

## Sysmon Event ID 13

Registry Modification

Persistence detection.

---

## Sysmon Event ID 22

DNS Query

Useful for:

* Malware domains
* Beaconing activity

---

# LSASS Monitoring

## What is LSASS?

LSASS stands for:

```text
Local Security Authority Subsystem Service
```

Process:

```text
lsass.exe
```

Role:

* Authentication
* Password handling
* Kerberos tickets
* NTLM credentials

---

# Why Attackers Target LSASS

Credentials are stored in memory.

Tools:

* Mimikatz
* Procdump
* Custom malware

Goal:

```text
Credential Dumping
```

---

# Suspicious LSASS Activity

Normal:

```text
lsass.exe
```

Suspicious:

```text
procdump.exe
     ↓
lsass.exe
```

or

```text
rundll32.exe
     ↓
lsass.exe
```

or

```text
powershell.exe
     ↓
lsass.exe
```

---

# Sysmon Detection for LSASS

Monitor:

```text
Event ID 10
```

Process Access Event

Indicators:

* Granted Access
* Memory Read
* Handle Request

---

# PowerShell Monitoring

PowerShell is one of the most abused tools.

Process:

```text
powershell.exe
```

Modern:

```text
pwsh.exe
```

---

# Why Attackers Love PowerShell

Features:

* Native Windows tool
* Powerful scripting
* Memory execution
* Network communication

---

# Suspicious PowerShell Indicators

## Encoded Commands

```powershell
powershell.exe -enc
```

---

## Hidden Execution

```powershell
powershell.exe -windowstyle hidden
```

---

## Download Cradles

```powershell
Invoke-WebRequest
```

or

```powershell
IEX(New-Object Net.WebClient)
```

---

## Base64 Encoded Commands

Common malware technique.

---

# PowerShell Logging

Enable:

## Module Logging

Tracks cmdlets.

---

## Script Block Logging

Captures executed scripts.

Event ID:

```text
4104
```

Very important.

---

## PowerShell Engine Logs

Event IDs:

```text
400
403
600
```

---

# Command Line Monitoring

Command-line arguments often reveal malicious intent.

Example:

```text
cmd.exe /c whoami
```

---

Suspicious:

```text
cmd.exe /c net user
```

---

Suspicious:

```text
cmd.exe /c net localgroup administrators
```

---

# WMI Monitoring

WMI:

```text
Windows Management Instrumentation
```

Process:

```text
wmiprvse.exe
```

Used for:

* Administration
* Remote execution

Attackers abuse WMI for:

* Lateral movement
* Persistence

---

# WMI Suspicious Activity

```text
wmiprvse.exe
      ↓
cmd.exe
```

or

```text
wmiprvse.exe
      ↓
powershell.exe
```

---

# Service Monitoring

Windows services can be abused.

Commands:

```cmd
sc create
```

```cmd
sc start
```

Monitor:

* New services
* Service modification

---

# Scheduled Task Monitoring

Persistence method.

Commands:

```cmd
schtasks.exe
```

Examples:

```cmd
schtasks /create
```

Monitor:

* New tasks
* Hidden tasks

---

# Registry Monitoring

Attackers often establish persistence via registry.

Common Locations:

```text
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

```text
HKLM\Software\Microsoft\Windows\CurrentVersion\Run
```

Monitor:

* New keys
* Modified keys

---

# Network Monitoring

Monitor processes making connections.

Example:

```text
powershell.exe
      ↓
External IP
```

Monitor:

* Rare destinations
* High-risk countries
* Known malicious IPs

---

# Sysinternals Tools for SOC

Microsoft Sysinternals Suite is essential.

---

## Process Explorer

Shows:

* Process tree
* Parent-child relationships
* DLLs
* Handles

Used during investigations.

---

## Process Monitor (ProcMon)

Captures:

* Registry activity
* File activity
* Process activity

Useful for malware analysis.

---

## Autoruns

Shows persistence locations.

Detects:

* Startup entries
* Services
* Scheduled tasks

---

## TCPView

Displays:

* Active connections
* Listening ports

---

## PsExec

Remote administration tool.

Attackers often abuse it.

Monitor:

```text
psexec.exe
```

---

## Handle

Displays open handles.

Useful in investigations.

---

# Common Windows Attack Techniques

## Credential Dumping

Tools:

```text
Mimikatz
Procdump
```

Targets:

```text
lsass.exe
```

---

## Pass-the-Hash

Uses NTLM hashes instead of passwords.

Indicators:

* Lateral movement
* Authentication anomalies

---

## Kerberoasting

Targets service accounts.

Monitor:

```text
4769
```

Kerberos Service Ticket Requests.

---

## Golden Ticket Attack

Forged Kerberos ticket.

Monitor:

* Unusual TGT usage
* Long ticket lifetimes

---

## Lateral Movement

Tools:

```text
PsExec
WMI
RDP
PowerShell Remoting
```

---

# Essential Event IDs Every SOC Analyst Should Know

| Event ID  | Description             |
| --------- | ----------------------- |
| 4624      | Successful Login        |
| 4625      | Failed Login            |
| 4634      | Logoff                  |
| 4648      | Explicit Credentials    |
| 4672      | Admin Privileges        |
| 4688      | Process Creation        |
| 4698      | Scheduled Task Created  |
| 4720      | User Created            |
| 4726      | User Deleted            |
| 4732      | Added to Group          |
| 4768      | Kerberos TGT            |
| 4769      | Kerberos Service Ticket |
| 4776      | NTLM Authentication     |
| 4104      | PowerShell Script Block |
| Sysmon 1  | Process Creation        |
| Sysmon 3  | Network Connection      |
| Sysmon 10 | Process Access          |
| Sysmon 11 | File Creation           |
| Sysmon 13 | Registry Modification   |
| Sysmon 22 | DNS Query               |

---

# Windows SOC Investigation Workflow

```text
Alert Generated
       ↓
Identify Event ID
       ↓
Check User
       ↓
Check Host
       ↓
Review Process Tree
       ↓
Review Parent Process
       ↓
Review Command Line
       ↓
Review Network Activity
       ↓
Check Persistence
       ↓
Determine Impact
       ↓
Contain Incident
       ↓
Document Findings
```

# Conclusion

Windows SOC monitoring revolves around understanding how Windows systems operate internally and identifying deviations from normal behavior. The most critical areas include process creation (4688), parent-child process relationships, PowerShell activity (4104), authentication events (4624/4625), Sysmon telemetry, LSASS access attempts, scheduled tasks, services, registry persistence, and network connections. Effective analysts combine Event IDs, process trees, command-line analysis, and Sysmon logs to detect attacks such as credential dumping, privilege escalation, persistence, phishing execution, ransomware, and lateral movement. Mastery of Windows internals, Sysinternals tools, Active Directory events, and attacker tradecraft is fundamental for success in a Windows-based SOC environment.
