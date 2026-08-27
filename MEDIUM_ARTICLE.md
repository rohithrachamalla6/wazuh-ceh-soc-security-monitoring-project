# Building a Mini SOC with Wazuh: A CEH Cybersecurity Project

## Introduction

As part of my **Certified Ethical Hacker (CEH)** training at
**Skillogic**, I completed a practical SOC project using **Wazuh** to
monitor a Windows endpoint and investigate security activity.

The objective was not simply to generate an alert, but to follow a
complete SOC workflow:

**Data Collection → Monitoring → Detection → Triage → Investigation →
Response → Reporting**

This project provided hands-on experience with endpoint monitoring,
vulnerability visibility, File Integrity Monitoring, Security
Configuration Assessment, and authentication-event investigation.

## Lab Environment

The practical environment included:

-   Wazuh
-   Wazuh Agent
-   Windows 11
-   Windows Security Event Logs
-   Vulnerability Detection
-   File Integrity Monitoring (FIM)
-   Security Configuration Assessment (SCA)
-   MITRE ATT&CK

The monitored Windows endpoint was represented in Wazuh as **Agent 004
-- Rachamalla**.

## Vulnerability Investigation

One part of the project involved reviewing vulnerabilities reported by
Wazuh.

Several findings were remediated by updating or removing software:

-   ASP.NET Core was updated to 8.0.29.
-   WinRAR was updated from 6.21 to 7.23.
-   Oracle VirtualBox was updated from 7.0.8 to 7.2.16.
-   Advanced IP Scanner 2.5.4594.1 was removed.

I also investigated a WhatsApp version-identification issue. Windows
AppX metadata reported version `2.2632.100.0`, while the application
reported `2.3000.1046156219.263200`. Microsoft Store reported no
available upgrade, so the application was retained and the discrepancy
was documented rather than forcing an unnecessary removal.

## Detecting Repeated Failed Logons

The most important SOC detection exercise involved Windows
authentication failures.

Windows Security Event ID **4625** represents a failed logon. Wazuh
initially identified the individual failures with Rule **60122**.

When the failures were repeated, Wazuh escalated the activity to:

**Rule 60204 --- Multiple Windows Logon Failures**

with:

**Level 10**

The alert was mapped to:

**MITRE ATT&CK T1110 --- Brute Force**

## Controlled Test

The test generated sequential failed network logons for:

`WazuhTestUser1` through `WazuhTestUser8`

The Windows event extraction showed the attempts occurring within a
short period.

The latest test characteristics included:

-   Logon Type: **3 --- Network**
-   Source: **127.0.0.1**
-   Authentication: **NTLM**

The local source address was important because it showed that the
controlled authentication test originated from the monitored Windows
machine rather than an external host.

## Investigation

Detecting an alert is only one part of a SOC investigation.

I then checked for a successful authentication using Event ID **4624**
and searched Wazuh for:

``` text
data.win.system.eventID:4624 AND data.win.eventdata.targetUserName:WazuhTestUser
```

The search returned:

**No results match your search criteria**

This meant that the investigation found no matching successful
authentication for the test account in the checked period.

## Response Decision

Because the activity was intentionally generated as part of an
authorized CEH security test, the alert was classified as:

**True Positive Detection --- Benign/Authorized Test**

No account disabling, IP blocking, endpoint isolation, firewall
modification, or malware remediation was required.

Post-test SMB verification also showed no active SMB session or open SMB
files.

## File Integrity Monitoring and SCA

The project also included Wazuh File Integrity Monitoring and Security
Configuration Assessment.

FIM evidence demonstrated monitoring of integrity-related activity,
including registry-related events.

SCA evidence demonstrated configuration assessment and CIS-related
failed checks.

These capabilities show why a SOC platform benefits from combining
multiple security telemetry sources rather than looking only at
authentication events.

## What I Learned

This project helped me understand that SOC work is not just about
finding alerts.

A practical investigation involves:

1.  Collecting reliable telemetry.
2.  Monitoring the environment.
3.  Identifying suspicious activity.
4.  Prioritizing alerts.
5.  Investigating the underlying events.
6.  Deciding whether response is necessary.
7.  Documenting the outcome.

The distinction between **detecting an attack pattern** and **confirming
compromise** was particularly important in this exercise.

## Final Result

The Wazuh deployment successfully detected the controlled repeated
authentication failures and escalated them to a Level 10 alert.

The subsequent investigation did not identify a successful login for the
test account.

Therefore, the incident was closed as:

**CLOSED --- Benign/Authorized Security Test**

## Conclusion

This CEH practical project provided hands-on experience with a realistic
SOC workflow using Wazuh.

It demonstrated how an open-source security platform can provide:

-   Endpoint monitoring
-   Centralized security-event visibility
-   Brute-force detection
-   Vulnerability monitoring
-   File Integrity Monitoring
-   Security Configuration Assessment
-   Investigation support
-   Incident-response decision making
-   Security reporting

The project also reinforced an important SOC principle: **an alert is
the beginning of an investigation, not the end.**

## Portfolio Notes

This project is being published for educational and portfolio purposes.

Before publishing screenshots publicly, private IP addresses, internal
hostnames, personal usernames, credentials, tokens, certificates, and
other sensitive information should be masked or removed.

All authentication testing described here was performed in a controlled
training environment as part of the CEH course.

## Project Repository

GitHub repository: *Add your GitHub URL here after creating the
repository.*
