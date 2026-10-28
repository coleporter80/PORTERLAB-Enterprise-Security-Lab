# Active Directory Organizational Unit Design

> **Evidence scope:** Implementation and validation results below are historical observations reported by the lab author. Supporting sanitized outputs are not included in this repository; this documentation review did not rerun the commands or verify live system state.

## Overview

Designed and implemented a hierarchical Organizational Unit (OU) structure within the PORTERLAB Active Directory environment to support centralized identity administration, computer organization, security policy management, and future role-based access control.

The environment uses Windows Server 2022 and Active Directory Domain Services (AD DS) to simulate identity management in a small enterprise network.

> **Security Notice:** This document intentionally omits actual computer identifiers, full distinguished names, account membership details, and sensitive infrastructure information. The hierarchy below represents the implemented organizational structure using sanitized identifiers.

## Design Objectives

The Active Directory organizational structure was designed to:

1. Separate users, workstations, servers, and service accounts.
2. Support targeted deployment of security Group Policy Objects (GPOs).
3. Establish an organizational foundation for role-based administration.
4. Improve identity lifecycle management and resource organization.
5. Provide a scalable structure for future security engineering exercises.

## Organizational Unit Hierarchy

The following structure represents the configured OU hierarchy:

```text
Active Directory Domain
│
├── Domain Controllers
│
└── LAB
    ├── Users
    ├── Servers
    ├── Service Accounts
    ├── Groups
    └── Workstations
```

The LAB label is a sanitized representation of the project's root organizational container.

### OU Responsibilities

| Organizational Unit | Intended Purpose |
|---|---|
| Domain Controllers | Organize domain controller computer accounts |
| LAB | Parent OU for lab resources |
| Users | Organize standard domain user accounts |
| Servers | Organize member server accounts |
| Service Accounts | Separate accounts intended for application or service authentication |
| Groups | Organize security groups used for access management |
| Workstations | Organize managed endpoint computer accounts |

These OUs create a foundation for separating administrative responsibilities and applying different security policies to different types of resources.

## Computer Account Organization

Computer account placement was validated through Active Directory PowerShell commands.

**Verified configurations:**

- Windows Server domain controller is located within the Domain Controllers OU.
- Domain-joined Windows 11 endpoint is located within the Workstations OU.

Separating domain controllers from ordinary workstations supports more targeted security administration and Group Policy management.

## Security Group Structure

Reviewed Active Directory security groups using PowerShell.

The environment contains standard Active Directory security groups and a custom IT administration security group.

Security group categories observed include:

- Domain administrative groups
- Standard domain user and computer groups
- Built-in administrative and operational groups
- Custom IT administration group

A custom global security group was identified, demonstrating the ability to organize role-specific identities within Active Directory.

**Scope of verification:** Group existence, category, and scope were inspected. Group membership, delegated permissions, and effective access assignments have not yet been independently documented.

## Group Policy and Security Administration

The OU structure supports the application of different security policies to domain resources.

Security controls already implemented elsewhere in PORTERLAB include:

- Account lockout threshold configuration
- Windows Firewall security policies
- PowerShell Script Block Logging
- PowerShell Module Logging

The OU hierarchy provides an organizational foundation for targeted policy application.

Specific GPO links, inheritance behavior, and security filtering will be documented separately after verification.

## Configuration Verification

### Enumerating Organizational Units

Used the Active Directory PowerShell module to inspect the OU hierarchy:

```powershell
Get-ADOrganizationalUnit -Filter * |
    Select-Object Name, DistinguishedName
```

**Result:** Verified the parent lab OU, its five child OUs, and the Domain Controllers OU.

### Reviewing Security Groups

```powershell
Get-ADGroup -Filter * |
    Select-Object Name, GroupScope, GroupCategory |
    Sort-Object Name
```

**Result:** Identified standard Active Directory groups and a custom global security group.

### Reviewing Computer Account Placement

```powershell
Get-ADComputer -Filter * |
    Select-Object Name, DistinguishedName
```

**Result:** Confirmed that the domain controller and domain-joined Windows workstation reside in their intended organizational containers.

## Security Design Considerations

### Separation of Resources

Organizing accounts by function supports differentiated security policies and reduces the administrative complexity associated with managing diverse systems in a single OU.

### Service Account Organization

A dedicated Service Accounts OU provides a logical foundation for managing identities used by applications and services.

Future work will validate account-specific permissions, credential management, and privilege restrictions.

### Least Privilege

Security groups provide a foundation for implementing role-based access control and delegated administration.

Further testing is required before claiming that granular delegated permissions or comprehensive least-privilege enforcement have been implemented throughout the lab.

### Group Policy Targeting

Separating workstation and server accounts allows security configurations to be applied based on system roles.

Policy inheritance, link placement, and effective policy application will be verified and documented in the Group Policy security baseline.

## Skills Demonstrated

- Active Directory organizational design
- Organizational Unit creation and management
- Windows Server identity administration
- Security group identification and organization
- Computer account placement
- PowerShell-based Active Directory enumeration
- Group Policy administration foundations
- Enterprise identity infrastructure documentation

## Future Improvements

- Implement and validate delegated OU administration.
- Audit privileged security group memberships.
- Create role-specific standard user and administrator accounts.
- Validate least-privilege access assignments.
- Document GPO links, inheritance, and security filtering.
- Implement controlled identity security testing and monitoring.
- Develop security detections for suspicious Active Directory changes.

## Outcome

Established and verified a structured Active Directory OU hierarchy containing separate organizational containers for users, servers, service accounts, security groups, and workstations.

Confirmed appropriate computer account placement and inspected domain security group organization.

This structure provides the identity-management foundation for future role-based access control, endpoint hardening, Group Policy administration, and Active Directory security monitoring exercises.
