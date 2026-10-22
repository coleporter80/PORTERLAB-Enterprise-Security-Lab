# PORTERLAB-Enterprise-Security-Lab
Enterprise cybersecurity homelab featuring Proxmox, Active Directory, Windows endpoint hardening, Wazuh SIEM, security monitoring, and infrastructure troubleshooting.
# PORTERLAB — Enterprise Cybersecurity Homelab

## Project Overview

PORTERLAB is a self-directed enterprise cybersecurity homelab designed to simulate a small corporate IT environment and provide hands-on experience in systems administration, infrastructure security, identity management, security monitoring, and incident response.

The environment is hosted on a dedicated Dell G15 5511 running Proxmox VE 8.4 and includes Windows Server 2022, a domain-joined Windows 11 workstation, and an Ubuntu-based Wazuh SIEM deployment.

The project focuses on implementing enterprise security controls, validating their operation, integrating endpoint telemetry into centralized monitoring, and troubleshooting real infrastructure failures.

## Technologies Used

| Category | Technologies |
|---|---|
| Virtualization | Proxmox VE 8.4 |
| Server Administration | Windows Server 2022, Ubuntu Linux |
| Identity & Access Management | Active Directory Domain Services, Group Policy |
| Networking | DNS, TCP/IP, Tailscale |
| Endpoint Security | Windows Defender, Windows Firewall |
| SIEM / Monitoring | Wazuh, Filebeat, Wazuh Indexer |
| Scripting / Logging | PowerShell, Windows Event Logs |
| Infrastructure Operations | Virtual machine management, snapshots, storage diagnostics |

## Environment Architecture

**Hypervisor:** Dell G15 5511 running Proxmox VE 8.4

**Domain:** `porterlab.local`

**Deployed systems:**

- **DC01:** Windows Server 2022 domain controller providing Active Directory Domain Services and DNS.
- **Windows 11 Workstation:** Domain-joined endpoint used to validate Group Policy, security configurations, and centralized monitoring.
- **wazuh01:** Ubuntu Linux server hosting Wazuh security monitoring components.
- **Ubuntu Docker VM:** Additional Linux environment provisioned for container experimentation.

### Architecture Diagram

*Network architecture diagram to be added.*

## Completed Implementations

### 1. Virtualization Infrastructure

- Installed and configured Proxmox VE on dedicated hardware.
- Provisioned Windows Server, Windows 11, and Ubuntu virtual machines.
- Configured virtual machine installation media and VirtIO drivers.
- Performed virtual machine recovery and infrastructure troubleshooting.
- Used snapshots to preserve selected environment states.

### 2. Active Directory and DNS

- Deployed Windows Server 2022 as the `DC01` domain controller.
- Configured the `porterlab.local` Active Directory domain.
- Established Active Directory organizational units.
- Joined a Windows 11 workstation to the domain.
- Validated domain authentication and Group Policy application.
- Troubleshot DNS resolution and domain connectivity.

### 3. Windows Endpoint Security Hardening

- Configured an account lockout threshold of 10 failed sign-in attempts.
- Implemented Windows Firewall configurations for Domain, Private, and Public network profiles.
- Configured inbound traffic restrictions and outbound traffic policies.
- Enabled PowerShell Script Block Logging (Event ID 4104).
- Enabled PowerShell Module Logging (Event ID 4103).
- Used Group Policy updates and reporting tools to validate security policy application.

### 4. Wazuh SIEM Deployment

- Installed and configured Wazuh security monitoring components on Ubuntu Linux.
- Deployed and verified the Wazuh indexer, dashboard, and supporting services.
- Restored functionality following installation and service configuration issues.
- Enrolled a Windows endpoint agent in Wazuh.
- Verified endpoint visibility in the Wazuh dashboard.
- Established a foundation for endpoint event analysis and security detection engineering.

### 5. Infrastructure Troubleshooting

Investigated and resolved several infrastructure issues, including:

- Windows installation and VirtIO driver problems.
- Domain DNS resolution errors.
- Wazuh installation failures and service startup issues.
- Proxmox storage capacity exhaustion.
- Virtual machines entering an `io-error` state.
- Recovery and verification of affected virtual machines.

These experiences reinforced the importance of capacity planning, service dependencies, log analysis, and methodical infrastructure troubleshooting.

## Skills Demonstrated

- Enterprise Windows and Linux administration
- Active Directory and identity management
- Domain-based endpoint administration
- Group Policy implementation and verification
- Windows endpoint security hardening
- PowerShell security telemetry configuration
- SIEM deployment and agent management
- Networking and DNS troubleshooting
- Virtualization and storage management
- Technical troubleshooting and documentation

## Current Project Status

The core infrastructure is deployed, including Active Directory, a domain-joined Windows 11 endpoint, and a functioning Wazuh monitoring environment.

Due to physical hardware storage and memory constraints, further infrastructure expansion has been paused. The project is transitioning toward evidence-driven security testing, detection engineering, log analysis, and incident-response simulations using the existing infrastructure and cloud/browser-hosted resources.

## Planned Enhancements

- Validate PowerShell security event ingestion and alerting in Wazuh.
- Implement custom Wazuh detection rules.
- Create Sigma detection rules mapped to MITRE ATT&CK.
- Run controlled adversary-emulation tests.
- Document simulated security incidents and investigation workflows.
- Expand into AWS identity, logging, detection, and cloud security engineering.

## Documentation

Detailed configuration guides, technical evidence, screenshots, troubleshooting reports, and detection engineering exercises will be added as the project is documented.

## Disclaimer

This environment is a personal cybersecurity research and education project. All security testing is conducted within authorized lab systems. The project does not represent a production enterprise deployment.

Sensitive information, credentials, tokens, and infrastructure identifiers are excluded or sanitized before publication.
