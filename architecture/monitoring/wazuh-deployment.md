# Wazuh SIEM Deployment and Security Monitoring Infrastructure

> **Evidence scope:** Implementation and validation results below are historical observations reported by the lab author. Supporting sanitized outputs are not included in this repository; this documentation review did not rerun the commands or verify live system state.

## Project Overview

Deployed a self-hosted Wazuh security monitoring environment within the PORTERLAB Enterprise Cybersecurity Homelab to develop practical experience with SIEM architecture, endpoint monitoring, centralized security telemetry, and security infrastructure administration.

The deployment provides a foundation for security operations center (SOC) workflows, endpoint visibility, security event investigation, and future detection engineering exercises.

The environment was deployed using Ubuntu Linux on a Proxmox VE hypervisor, with a Windows 11 domain-joined endpoint integrated through the Wazuh agent.

> **Security Notice:** Actual IP addresses, management endpoints, authentication details, certificates, tokens, and sensitive infrastructure identifiers are intentionally excluded from this documentation.

## 1. Deployment Objectives

The deployment was designed to:

1. Establish a centralized security monitoring platform.
2. Gain hands-on experience deploying and administering SIEM components.
3. Integrate a Windows endpoint into centralized security monitoring.
4. Understand security telemetry collection, processing, indexing, and visualization.
5. Support future threat detection, incident investigation, and detection engineering exercises.
6. Develop practical Linux and security infrastructure troubleshooting skills.

## 2. Environment Architecture

| Component | Configuration |
|---|---|
| Hypervisor | Proxmox VE 8.4 |
| SIEM Virtual Machine | WAZUH01 |
| Operating System | Ubuntu Linux |
| SIEM Platform | Wazuh 4.14.8 |
| Allocated Memory | 8 GiB |
| Virtual CPUs | 4 |
| Virtual Disk | 50 GiB |
| Virtual Storage | LVM-thin |
| Monitored Endpoint | Domain-joined Windows 11 workstation |
| Endpoint Integration | Wazuh Agent |

### Architecture

```text
                 PORTERLAB NETWORK
                        |
            +-----------+-----------+
            |                       |
     Windows Server 2022       Windows 11
       Active Directory         Workstation
       DNS / Group Policy            |
                                     |
                                Wazuh Agent
                                     |
                                     v
                            WAZUH01 - Ubuntu
                       +-----------------------+
                       |                       |
                       |    Wazuh Manager      |
                       |          |            |
                       |    Event Processing   |
                       |          |            |
                       |    Wazuh Indexer      |
                       |          |            |
                       |    Wazuh Dashboard    |
                       |                       |
                       +-----------------------+
                                     |
                            Security Monitoring
                              and Visualization
```

*Conceptual diagram showing the Windows endpoint's integration with the Wazuh monitoring stack. Direct Windows Server log ingestion has not yet been independently documented.*

## 3. Wazuh Component Deployment

### Wazuh Manager

Installed and configured Wazuh Manager as the central security monitoring component.

The manager provides functionality for:

- Agent communication and management
- Security event processing
- Rule-based event analysis
- Endpoint security monitoring
- Security alert generation

The deployed manager package was confirmed as Wazuh 4.14.8 during installation and troubleshooting.

### Wazuh Indexer

Deployed the Wazuh Indexer to support storage, indexing, and retrieval of security monitoring data.

The indexer provides the data storage layer used by the Wazuh dashboard for security event exploration and visualization.

### Wazuh Dashboard

Installed and configured the Wazuh Dashboard to provide a web-based interface for:

- Security monitoring
- Agent visibility
- Alert and event summaries
- Security analytics and visualization
- Administrative monitoring

During deployment, the dashboard initially encountered service availability and configuration issues.

These issues were investigated and corrected, restoring access to the monitoring interface.

## 4. Windows Endpoint Integration

Installed and enrolled a Wazuh agent on the PORTERLAB Windows 11 workstation.

The workstation is joined to the lab's Active Directory domain and configured with Windows endpoint security controls.

### Completed Integration

- Installed the Windows Wazuh agent.
- Connected the endpoint to the Wazuh management environment.
- Confirmed the endpoint appeared in the Wazuh dashboard.
- Observed dashboard event counts and monitoring charts.

### Monitoring Scope

The workstation has also been configured with:

- Windows Firewall security controls
- PowerShell Script Block Logging
- PowerShell Module Logging
- Windows process creation auditing

These controls provide potential telemetry sources for future security investigations.

Although endpoint registration and dashboard event visualization have been completed, ingestion and correlation of specific Windows Event IDs into Wazuh remain to be documented.

## 5. Security Monitoring Architecture

The deployment establishes a centralized security monitoring architecture.

### Endpoint Collection

The Wazuh agent provides communication between the monitored Windows endpoint and the manager.

### Event Processing

The Wazuh Manager processes incoming agent information and applies its analysis capabilities.

### Indexing and Storage

For the documented Wazuh 4.x architecture, Filebeat forwards generated alert data from the manager to the Wazuh Indexer. The indexer stores that data for retrieval and analysis; the dashboard queries it for visualization. This does not imply that every collected event is indexed.

Reference: [Wazuh server components](https://documentation.wazuh.com/current/getting-started/components/wazuh-server.html).

### Visualization

The Wazuh Dashboard provides an interface for reviewing the monitored environment and investigating events.

## 6. Deployment Troubleshooting

Several infrastructure and service configuration challenges were encountered during implementation.

### Dashboard Service Availability

**Issue:** The Wazuh dashboard initially failed to load, and a restart attempt reported that the dashboard service could not be found.

**Troubleshooting:** Investigated installed Wazuh components, service registration, and service status.

**Outcome:** Corrected the deployment issue and restored dashboard accessibility.

### Component Service Management

Reviewed and administered the Wazuh Manager, Indexer, and Dashboard services using Linux service management tools.

```bash
systemctl status wazuh-manager
systemctl status wazuh-indexer
systemctl status wazuh-dashboard
```

These commands were used to inspect service availability and operational status.

### Storage Exhaustion and Recovery

**Issue:** The Proxmox LVM-thin storage pool reached full utilization, resulting in virtual machine I/O errors and service availability problems.

**Investigation:**

- Examined Proxmox storage utilization.
- Inspected LVM logical volumes and thin-pool allocation.
- Identified significant storage allocation associated with the Wazuh virtual disk.
- Compared guest filesystem usage with hypervisor-reported disk allocation.
- Investigated guest TRIM and virtual disk Discard settings.
- Identified that a virtual disk Discard configuration change was pending rather than active.

**Remediation:**

- Extended the thin storage pool using available volume group capacity.
- Applied virtual disk Discard support through a controlled VM shutdown and startup.
- Executed filesystem TRIM to reclaim unused storage blocks.
- Confirmed a substantial reduction in allocated physical storage.

**Outcome:** Reduced overall Proxmox thin-pool utilization from 100% to approximately 83% immediately following recovery. Storage utilization subsequently increased during normal virtual machine activity.

The incident reinforced the importance of virtualization storage capacity planning, thin provisioning, guest filesystem behavior, and storage monitoring.

## 7. Security Engineering Skills Demonstrated

- Linux server administration
- SIEM component deployment
- Wazuh Manager administration
- Security event indexing infrastructure
- SIEM dashboard configuration
- Windows endpoint agent enrollment
- Centralized security monitoring architecture
- Linux service troubleshooting
- Hypervisor storage diagnostics
- Virtual disk Discard/TRIM configuration
- Infrastructure incident troubleshooting
- Technical documentation

## 8. Current Implementation Status

### Completed

- Wazuh Manager installation
- Wazuh Indexer installation
- Wazuh Dashboard installation
- Windows 11 endpoint agent enrollment
- Endpoint visibility within Wazuh
- Basic event-count and monitoring visualization
- Wazuh service recovery and management
- Virtual storage troubleshooting and recovery

### Not Yet Completed or Fully Documented

- Custom Wazuh detection rules
- Sigma rule development
- Complete Windows Event ID correlation
- Structured incident investigation scenarios
- MITRE ATT&CK detection coverage validation
- Automated incident response actions
- Long-term event retention optimization

## 9. Planned Detection Engineering

Future exercises will use the existing Wazuh deployment to investigate controlled security activity and develop detection logic.

Potential scenarios include:

| Scenario | Relevant Telemetry |
|---|---|
| Suspicious PowerShell execution | Event ID 4104 |
| PowerShell module activity | Event ID 4103 |
| Process execution monitoring | Event ID 4688 |
| Failed authentication attempts | Event ID 4625 |
| Successful authentication events | Event ID 4624 |
| Privileged security group changes | Event IDs 4728, 4732, 4756 |

These are planned detection scenarios, not claims of completed detection coverage.

Detection testing will be performed only against authorized lab systems.

## 10. Future Improvements

- Validate and document individual security events in Wazuh.
- Create custom Wazuh XML detection rules.
- Develop Sigma detection rules.
- Map completed detections to MITRE ATT&CK.
- Simulate controlled endpoint attack techniques.
- Implement appropriate index and event retention policies.
- Develop repeatable security investigation procedures.
- Create incident reports with supporting evidence.

## Project Outcome

Successfully deployed a functioning Wazuh security monitoring stack and integrated a domain-joined Windows 11 workstation through a Wazuh endpoint agent.

The environment establishes practical experience with SIEM infrastructure, centralized monitoring, Linux administration, endpoint integration, service troubleshooting, and virtualization storage recovery.

It provides a technical foundation for future detection engineering, adversary simulation, and incident response exercises.
