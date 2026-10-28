# Windows Group Policy Security Baseline

## Project Overview

Implemented Windows endpoint security hardening through Active Directory Group Policy in the PORTERLAB Enterprise Cybersecurity Homelab.

The project demonstrates centralized security policy administration, endpoint protection, authentication security, and enhanced Windows event logging in a domain-managed environment.

The security baseline was implemented using Windows Server 2022 and applied to a domain-joined Windows 11 workstation.

> **Security Notice:** This document excludes actual IP addresses, user identifiers, sensitive infrastructure information, and security credentials. Domain and computer identifiers are generalized where appropriate.

## Infrastructure

| Component | Technology |
|---|---|
| Domain Controller | Windows Server 2022 |
| Client Endpoint | Windows 11 |
| Identity Management | Active Directory Domain Services |
| Policy Management | Group Policy Management Console (GPMC) |
| Endpoint Security | Windows Defender, Windows Firewall |
| Logging | Windows Event Logs, PowerShell |
| Centralized Monitoring | Wazuh SIEM |

## Security Objectives

The baseline was designed to:

1. Reduce unauthorized access through account lockout controls.
2. Enforce host-based firewall protections.
3. Increase visibility into PowerShell activity.
4. Enable Windows process creation auditing.
5. Apply workstation security settings through centralized Group Policy administration.
6. Establish a foundation for security monitoring and detection engineering.

## 1. Account Lockout Policy

Configured an authentication lockout threshold to reduce exposure to repeated password-guessing attempts.

**Configured setting:**

| Policy | Value |
|---|---|
| Account lockout threshold | 10 failed authentication attempts |

Account lockout policies can help reduce the effectiveness of repeated password guessing against domain accounts.

**Security considerations:**

Account lockout controls should be balanced against denial-of-service risks, as excessive failed authentication attempts can also be used to intentionally lock out legitimate users.

**Relevant MITRE ATT&CK technique:**

- T1110 — Brute Force

## 2. Windows Firewall Hardening

Configured Windows Defender Firewall profiles to establish host-based network security protections.

### Domain Network Profile

| Setting | Configuration |
|---|---|
| Firewall enabled | Yes |
| Default inbound traffic | Block |
| Default outbound traffic | Allow |

The domain firewall configuration follows a default-deny approach for unsolicited inbound connections while allowing outbound network communication.

Private and Public firewall profiles were also configured during the endpoint hardening process.

Specific profile settings should be independently reviewed before treating them as a fully validated baseline.

### Security Benefits

- Restricts unsolicited inbound connections.
- Reduces unnecessary network exposure.
- Supports endpoint-level network traffic controls.
- Provides a foundation for implementing more restrictive firewall policies.

## 3. PowerShell Script Block Logging

Enabled PowerShell Script Block Logging to improve visibility into scripts and commands executed on the Windows endpoint.

**Configuration:**

| Setting | Value |
|---|---|
| Script Block Logging | Enabled |
| Windows Event ID | 4104 |
| Event Channel | Microsoft-Windows-PowerShell/Operational |

### Purpose

Script Block Logging records PowerShell script content that can be useful when investigating suspicious activity.

This supports analysis of:

- Suspicious PowerShell execution
- Encoded or obfuscated commands
- Unauthorized script execution
- Potential malicious administrative activity

### Validation

During the original endpoint configuration, PowerShell Script Block Logging was enabled and Event ID 4104 was observed in Windows Event Viewer.

**Relevant MITRE ATT&CK technique:**

- T1059.001 — Command and Scripting Interpreter: PowerShell

## 4. PowerShell Module Logging

Configured PowerShell Module Logging to improve visibility into module-based command execution.

**Configuration:**

| Setting | Value |
|---|---|
| Module Logging | Enabled |
| Windows Event ID | 4103 |
| Event Channel | Microsoft-Windows-PowerShell/Operational |

### Purpose

Module Logging provides additional information about PowerShell command activity, including execution details associated with configured modules.

This telemetry can support investigations involving administrative command execution and potentially suspicious PowerShell activity.

### Validation

Module Logging was configured during endpoint hardening, and Event ID 4103 was subsequently confirmed in Windows Event Viewer.

## 5. Windows Process Creation Auditing

Configured Windows process creation auditing to improve visibility into executable activity on the domain-joined workstation.

**Relevant Windows Security Event:**

- Event ID 4688 — A new process has been created

### Security Purpose

Process creation auditing can provide information about:

- Process execution
- Parent-child process relationships
- Executable names and paths
- User security context
- Command-line activity when the corresponding command-line logging policy is enabled

The original endpoint baseline included process creation auditing configuration and validation.

Command-line inclusion and specific auditing subcategory settings will be documented separately if additional evidence is collected.

**Relevant MITRE ATT&CK technique:**

- T1059 — Command and Scripting Interpreter

## 6. Windows Session Lock Policy

Configured a workstation inactivity-locking policy with a target timeout of 15 minutes.

This control was intended to reduce the risk of unauthorized access to unattended workstations.

### Security Purpose

- Reduce exposure from unattended sessions.
- Require reauthentication after inactivity.
- Support workstation access-control requirements.

During initial testing, the workstation was observed locking earlier than expected. The exact effective inactivity timeout was not conclusively established.

## 7. Centralized Group Policy Administration

The security baseline was implemented within an Active Directory-managed environment.

The domain structure includes separate Organizational Units for users, workstations, servers, service accounts, and groups.

This organizational design provides a foundation for applying different security policies according to system role.

### Policy Validation Methods

During implementation, Group Policy processing and effective policy application were examined using:

```powershell
gpupdate /force
```

```powershell
gpresult /r
```

Windows Event Viewer was also used to validate security logging behavior.

These checks supported troubleshooting and confirmation of applied workstation security controls.

## 8. Security Monitoring Integration

The hardened Windows 11 workstation was enrolled as an endpoint agent in the Wazuh security monitoring environment.

The Wazuh deployment established centralized endpoint visibility and a foundation for analyzing Windows security telemetry.

Specific detection rules, alert mappings, and ingestion of individual PowerShell or process creation events into Wazuh have not yet been fully documented.

Future work will validate those telemetry pipelines and develop detection logic.

## 9. MITRE ATT&CK Alignment

The implemented security controls support prevention, investigation, or visibility related to several MITRE ATT&CK techniques.

| ATT&CK Technique | Relevance |
|---|---|
| T1110 — Brute Force | Account lockout policy |
| T1059.001 — PowerShell | PowerShell script and module logging |
| T1059 — Command and Scripting Interpreter | Process execution visibility |

These mappings describe control relevance. They do not represent completed adversary-emulation tests or proven detection coverage.

## 10. Skills Demonstrated

- Active Directory Group Policy administration
- Windows endpoint security hardening
- Authentication security controls
- Windows Firewall configuration
- PowerShell security logging
- Windows event auditing
- Security policy application and troubleshooting
- Endpoint security monitoring foundations
- MITRE ATT&CK control mapping
- Technical security documentation

## 11. Future Improvements

- Export sanitized Group Policy reports.
- Validate effective firewall settings across all profiles.
- Verify process creation command-line auditing.
- Test account lockout behavior using dedicated lab accounts.
- Confirm Windows event ingestion into Wazuh.
- Develop Sigma and Wazuh detections.
- Simulate controlled adversary techniques using Atomic Red Team.
- Document detection results and incident investigations.

## Project Outcome

Implemented a domain-managed Windows security baseline incorporating authentication protections, Windows Firewall controls, PowerShell logging, process creation auditing, and workstation inactivity-locking policies.

The project demonstrates practical experience with enterprise endpoint security configuration, Active Directory administration, Windows telemetry, and foundational security engineering.

The environment provides a platform for future detection engineering, incident response, and security monitoring exercises.
