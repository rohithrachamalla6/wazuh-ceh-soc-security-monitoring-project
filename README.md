# Wazuh SOC Incident Detection & Vulnerability Investigation

A practical Security Operations Center (SOC) project completed as part
of the **Certified Ethical Hacker (CEH)** course at **Skillogic**.

## Project Overview

This project demonstrates how Wazuh can be used to monitor a Windows
endpoint, collect security events, identify repeated authentication
failures, investigate alerts, review vulnerabilities, perform File
Integrity Monitoring (FIM) and Security Configuration Assessment (SCA),
and document an incident through the SOC lifecycle.

**Course:** Certified Ethical Hacker (CEH)\
**Training Institute:** Skillogic\
**Platform:** Wazuh\
**Endpoint:** Windows 11\
**Wazuh Agent:** 004 -- Rachamalla

## SOC Workflow

``` text
Data Collection
      ↓
Monitoring
      ↓
Detection
      ↓
Triage
      ↓
Investigation
      ↓
Response
      ↓
Reporting
```

## Key Project Areas

### 1. Endpoint Monitoring

Wazuh Agent 004 was used to monitor the Windows endpoint and collect
relevant security telemetry.

### 2. Vulnerability Detection & Remediation

The project investigated software vulnerability findings and documented
remediation, including:

-   ASP.NET Core --- updated to 8.0.29
-   WinRAR --- updated from 6.21 to 7.23
-   Oracle VirtualBox --- updated from 7.0.8 to 7.2.16
-   Advanced IP Scanner 2.5.4594.1 --- removed
-   WhatsApp --- investigated as a version-identification/mapping
    finding; the application reported a newer version than the AppX
    metadata, and Microsoft Store reported no available upgrade.

### 3. File Integrity Monitoring

Wazuh FIM dashboards and events were reviewed, including
registry-related integrity evidence.

### 4. Security Configuration Assessment

Wazuh SCA/CIS assessment dashboards and failed-check evidence were
reviewed to identify configuration weaknesses.

### 5. Brute-Force Detection

A controlled authentication-failure test generated Windows Security
Event ID **4625**.

Wazuh identified:

-   Rule **60122** --- Logon Failure - Unknown user or bad password
-   Rule **60204** --- Multiple Windows Logon Failures
-   Severity --- **Level 10**
-   MITRE ATT&CK --- **T1110: Brute Force**

The controlled test produced sequential failed network logons for
`WazuhTestUser1` through `WazuhTestUser8`.

The latest event extraction showed:

-   Logon Type: **3 (Network)**
-   Source: **127.0.0.1**
-   Authentication: **NTLM**

### 6. Investigation

The alert was investigated using Windows Security events and Wazuh
Threat Hunting.

The following search was performed:

``` text
data.win.system.eventID:4624 AND data.win.eventdata.targetUserName:WazuhTestUser
```

No matching successful authentication was found in the checked period.

### 7. Response

The activity was part of an authorized security test. No account
disabling, IP blocking, endpoint isolation, firewall modification, or
malware remediation was required.

Post-test SMB verification showed no active SMB session or open SMB
files.

## Result

The project demonstrated that Wazuh can:

-   Centralize endpoint security monitoring
-   Collect Windows security events
-   Detect repeated failed authentication
-   Escalate repeated failures to a higher-severity alert
-   Support investigation of authentication activity
-   Identify vulnerabilities and configuration issues
-   Provide File Integrity Monitoring
-   Support Security Configuration Assessment
-   Document response and incident closure

## Incident Classification

**Status:** CLOSED -- Benign/Authorized Security Test

The brute-force detection was successfully validated, with no evidence
of successful authentication or confirmed compromise.

## Evidence

The `screenshots/` directory should contain sanitized screenshots
demonstrating:

1.  Wazuh dashboard
2.  Active Windows agent
3.  Threat Hunting
4.  Windows Event 4625
5.  Rule 60122
6.  Rule 60204 Level 10
7.  Detailed authentication event
8.  Event ID 4624 investigation
9.  FIM
10. SCA/CIS
11. Vulnerability Detection
12. Vulnerability Inventory

## Security & Privacy Notice

This repository is intended for educational and portfolio purposes.

Before publishing screenshots or logs publicly:

-   Remove or mask private IP addresses.
-   Remove personal usernames and machine names where appropriate.
-   Never publish passwords, API keys, tokens, certificates, private
    URLs, or other secrets.
-   Do not publish internal network information that should remain
    private.

## Disclaimer

All authentication testing described in this project was performed in a
controlled lab/training environment as part of the CEH practical work.

## Learning Outcomes

This project provided practical exposure to:

-   SOC monitoring
-   Security event analysis
-   Authentication-failure investigation
-   Brute-force detection
-   Vulnerability management
-   FIM
-   Security configuration assessment
-   Incident triage
-   Incident response decisions
-   Security reporting
