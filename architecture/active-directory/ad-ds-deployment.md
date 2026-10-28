# Active Directory Domain Services Deployment

## Overview

Deployed Windows Server 2022 as an Active Directory domain controller within the PORTERLAB enterprise cybersecurity homelab.

The implementation provides centralized identity management, authentication, DNS resolution, and Group Policy administration for a domain-joined Windows 11 workstation.

## Environment

| Component | Configuration |
|---|---|
| Hypervisor | Proxmox VE 8.4 |
| Domain Controller VM | WinServ22 (VM 101) |
| Operating System | Windows Server 2022 |
| Domain | `porterlab.local` |
| Active Directory | Active Directory Domain Services (AD DS) |
| DNS | Windows Server DNS |
| Client VM | WIN11-01 (VM 102) |
| Domain Controller Memory | 6 GiB |
| Domain Controller Virtual Disk | 64 GiB |

## Deployment Objectives

1. Establish a functioning Windows Active Directory domain.
2. Centralize authentication for domain-joined Windows systems.
3. Configure DNS to support domain services and name resolution.
4. Establish organizational structures for centralized administration.
5. Apply domain security controls using Group Policy.
6. Validate domain connectivity, authentication, and service health.

## Implementation

### 1. Windows Server Provisioning

Provisioned a Windows Server 2022 virtual machine using Proxmox VE.

Configured virtual CPU, memory, storage, and networking resources to support domain controller operations.

### 2. Active Directory Domain Services

Installed and configured the Active Directory Domain Services role.

Promoted the Windows Server instance to a domain controller for the lab domain:

`porterlab.local`

The domain controller provides centralized identity services and supports domain-based security administration.

### 3. DNS Configuration

Configured Windows Server DNS to support Active Directory.

DNS provides name resolution and service discovery required for domain authentication and communication between domain-joined systems.

DNS configuration and domain resolution problems were investigated and corrected during deployment.

### 4. Organizational Structure

Created Active Directory organizational units to support structured administration and Group Policy management.

The domain is designed to support centralized administration of users, computers, and security policies.

### 5. Windows 11 Domain Integration

Provisioned a Windows 11 virtual machine and successfully joined it to the `porterlab.local` domain.

Validated communication with the domain controller and application of domain-based security policies.

### 6. Group Policy Integration

Implemented domain-based security controls, including:

- Account lockout threshold of 10 failed authentication attempts
- Windows Firewall configuration
- PowerShell Script Block Logging (Event ID 4104)
- PowerShell Module Logging (Event ID 4103)

Additional security controls and policy validation methods are documented separately in the endpoint hardening section.

## Verification and Testing

The following checks were performed to verify Active Directory functionality.

### Domain Controller Service Health

```powershell
Get-Service NTDS,DNS,Netlogon |
    Select-Object Name,Status
```

**Result:** Required services reported running.

### Domain Controller Diagnostics

```powershell
dcdiag /test:DNS /test:Advertising /test:Services /q
```

**Result:** No diagnostic errors reported during the latest validation.

### Domain Trust Validation

Executed on the domain-joined Windows 11 workstation:

```powershell
Test-ComputerSecureChannel
```

**Result:** `True`

### Group Policy Verification

```powershell
gpresult /r
```

**Result:** Domain membership and expected Group Policy application were verified.

## Skills Demonstrated

- Windows Server provisioning and administration
- Active Directory Domain Services deployment
- Domain controller configuration
- Active Directory-integrated DNS administration
- Windows endpoint domain integration
- Identity and authentication troubleshooting
- Group Policy administration
- Infrastructure health validation

## Future Enhancements

- Document the complete organizational unit hierarchy.
- Expand identity-management scenarios.
- Implement additional account and access security controls.
- Conduct controlled Active Directory security monitoring exercises.
- Integrate domain authentication telemetry into centralized security monitoring.

## Summary

Successfully deployed and validated a functioning Active Directory domain using Windows Server 2022 and a domain-joined Windows 11 workstation.

The environment establishes the identity and authentication foundation for subsequent endpoint hardening, centralized logging, and security detection engineering exercises.
