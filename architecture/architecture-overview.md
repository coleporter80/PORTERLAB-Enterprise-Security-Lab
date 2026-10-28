# PORTERLAB — Infrastructure Architecture

> **Evidence scope:** Implementation and validation results below are historical observations reported by the lab author. Supporting sanitized outputs are not included in this repository; this documentation review did not rerun the commands or verify live system state.

## Objective

Design and operate a virtualized enterprise-style environment for practicing Windows systems administration, identity management, endpoint hardening, centralized security monitoring, and detection engineering.

## Physical Infrastructure

| Component | Configuration |
|---|---|
| Host | Dell G15 5511 |
| Hypervisor | Proxmox VE 8.4 |
| Physical Memory | Approximately 24 GB |
| Physical Storage | 256 GB-class NVMe SSD |
| Virtualization | KVM/QEMU |
| Storage Backend | LVM-thin |

## Virtual Machine Inventory

| VMID | VM label / documented name | Role | RAM | Virtual Disk |
|---|---|---|---|---|
| 100 | ubuntu-docker1 | Container experimentation | 2 GB | 32 GB |
| 101 | WinServ22 | Windows Server 2022 / AD DS / DNS | 6 GB | 64 GB |
| 102 | WIN11-01 | Domain-joined Windows 11 endpoint | 4 GB | 64 GB |
| 103 | WAZUH01 | Ubuntu / Wazuh SIEM | 8 GB | 50 GB |

These figures represent configured virtual resources. Guest RAM allocation totals 20 GiB, with additional memory needed for Proxmox and virtualization overhead. Thin-provisioned virtual disk capacity differs from actual physical usage.

The inventory records `WinServ22` for VM 101, while the diagram and DNS guide use `DC01`. These labels have not been verified as the guest hostname.

[View the conceptual architecture diagram](../PORTERLAB%20Homelab%20Architecture%20Overview.png)

## Identity Infrastructure

The environment uses the `porterlab.local` Active Directory domain.

The Windows Server 2022 domain controller provides:

- Active Directory Domain Services
- Domain authentication
- Organizational Unit management
- DNS services
- Centralized Group Policy administration

The Windows 11 workstation is joined to the domain and used to validate domain security configurations.

## Endpoint Security

The domain-joined Windows workstation serves as a test endpoint for enterprise security policies.

Implemented controls include:

- Windows Firewall configuration
- Account lockout policy
- PowerShell Script Block Logging
- PowerShell Module Logging
- Group Policy application and verification

## Security Monitoring

Wazuh is deployed on a dedicated Ubuntu virtual machine.

The environment includes the Wazuh management and indexing components, dashboard, and supporting services.

The Windows 11 workstation has a Wazuh agent enrolled, providing the foundation for centralized endpoint telemetry and future detection engineering.

## Infrastructure Constraints

The environment runs on repurposed consumer hardware with limited physical memory and storage capacity.

To avoid unnecessary resource consumption, the lab uses selective VM operation and prioritizes documenting deployed configurations and security testing rather than expanding infrastructure.

The next development stage focuses on detection logic, incident investigation, automation, and cloud-based security exercises.

## Future Improvements

- MITRE ATT&CK-aligned detection engineering
- Sigma and custom Wazuh rules
- Adversary-emulation exercises
- Incident investigation documentation
- AWS cloud security infrastructure
- Infrastructure-as-Code and repeatable deployments

## Project Scope

This is an educational enterprise-style environment, not a production enterprise network. Configuration details and findings are documented to demonstrate practical technical knowledge and security engineering methodology.
