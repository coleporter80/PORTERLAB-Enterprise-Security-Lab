# Docker and Portainer Infrastructure Deployment

## Project Overview

Deployed an Ubuntu Linux virtual machine within the PORTERLAB Enterprise Cybersecurity Homelab to provide a containerized application environment for infrastructure experimentation, service administration, and future cybersecurity tooling.

Docker was implemented as the container runtime, with Portainer Community Edition deployed to provide a web-based container management interface.

The environment supports practical experience in Linux administration, virtualization, container management, and infrastructure operations.

> **Security Notice:** Actual IP addresses, management endpoints, credentials, authentication tokens, and sensitive infrastructure identifiers are intentionally excluded from this documentation.

## 1. Deployment Objectives

The container environment was designed to:

1. Establish a dedicated Linux host for containerized services.
2. Gain practical experience with Docker and container administration.
3. Deploy Portainer for centralized container management.
4. Support future deployment of security tools without requiring a dedicated virtual machine for every application.
5. Provide a reusable foundation for infrastructure automation and experimentation.

## 2. Infrastructure

| Component | Configuration |
|---|---|
| Hypervisor | Proxmox VE 8.4 |
| Virtual Machine | ubuntu-docker1 |
| Operating System | Ubuntu Linux |
| Container Runtime | Docker |
| Management Platform | Portainer Community Edition |
| Allocated CPU | Not documented |
| Allocated RAM | 2 GiB |
| Virtual Disk | 32 GiB |
| Storage Backend | Proxmox LVM-thin |
| Management Interface | Portainer HTTPS interface |

## 3. Ubuntu Virtual Machine Deployment

Provisioned a dedicated Ubuntu Linux virtual machine within Proxmox.

The VM was configured with virtualized compute, storage, and network resources to support container-based workloads.

The dedicated Linux environment separates container experimentation from the Windows Active Directory infrastructure and the Wazuh security monitoring server.

This allows container services to be independently administered, maintained, and modified.

## 4. Docker Deployment

Installed Docker to provide a container runtime for deploying and managing applications.

### Purpose

Docker enables applications to run in isolated containers with their required dependencies.

This approach offers several advantages for a resource-constrained homelab:

- Reduced overhead compared with deploying a separate VM for each application.
- Repeatable application deployment.
- Isolation between application environments.
- Simplified management of supporting services.
- A foundation for learning container security and automation.

The Docker host was established as a reusable environment for future security tools and supporting applications.

## 5. Portainer Community Edition

Deployed Portainer Community Edition to provide a graphical interface for administering Docker workloads.

### Completed Configuration

- Deployed Portainer CE.
- Established an administrative account.
- Configured access to the Portainer management interface.
- Confirmed the management interface was accessible.
- Preserved a baseline VM state using a Proxmox snapshot.

### Management Interface

The deployment used Portainer's HTTPS management interface on TCP port 9443.

The actual management address is intentionally excluded from public documentation.

### Management Capabilities

Portainer provides functionality for:

- Viewing container status.
- Managing running and stopped containers.
- Inspecting container configuration.
- Reviewing container logs.
- Managing Docker images, volumes, and networks.
- Deploying additional container-based services.

These represent available platform capabilities, not a claim that every management function has been exercised.

## 6. Remote Infrastructure Access

PORTERLAB also incorporates Tailscale for remote connectivity to the lab environment.

Tailscale was configured as part of the broader homelab infrastructure to support remote administration without exposing management services directly through conventional router port forwarding.

Remote-access addresses, device identifiers, and authentication details are excluded from this repository.

## 7. Infrastructure Management

The environment was administered using Proxmox virtual machine management and Linux administration tools.

A baseline snapshot was created following the initial Ubuntu and Portainer configuration.

The deployment also provided experience with:

- Linux virtual machine provisioning.
- Container platform installation.
- Web-based infrastructure administration.
- Virtual machine resource allocation.
- Service accessibility troubleshooting.
- Snapshot-based recovery planning.

## 8. Resource Constraints

The container host operates within a limited physical hardware environment.

The VM was allocated 2 GiB of memory and a 32 GiB virtual disk to balance container experimentation with the requirements of other PORTERLAB systems.

Due to physical storage constraints, further container deployment has been paused.

The Docker environment remains part of the lab infrastructure, but future exercises will prioritize efficient resource utilization and selective service operation.

## 9. Security Considerations

The deployment provides opportunities to practice container security principles, including:

- Restricting management interface access.
- Protecting administrative credentials.
- Limiting unnecessary network exposure.
- Maintaining updated container images.
- Managing application permissions.
- Monitoring container logs.
- Applying least-privilege principles.

These are security considerations and future hardening objectives. Comprehensive container security validation has not yet been completed.

## 10. Skills Demonstrated

- Linux server provisioning
- Docker runtime deployment
- Container management platform configuration
- Portainer Community Edition administration
- Proxmox virtualization
- Remote infrastructure access
- Resource allocation and capacity planning
- Infrastructure troubleshooting
- Technical documentation

## 11. Future Improvements

- Restore administrative access to the Ubuntu Docker environment.
- Document Docker container and volume configurations.
- Implement Docker Compose for repeatable deployments.
- Deploy lightweight cybersecurity utilities.
- Configure container monitoring and logging.
- Implement container security hardening.
- Develop infrastructure automation scripts.
- Evaluate Docker image vulnerability scanning.

## Project Outcome

Successfully established an Ubuntu-based Docker environment and deployed Portainer Community Edition for container management.

The environment provides a reusable platform for container experimentation and infrastructure administration while demonstrating practical experience with Linux, virtualization, and container technology.

Future development will focus on container security, automation, and resource-efficient security tooling.
