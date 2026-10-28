# DNS Server Configuration and Active Directory Integration

## Project Overview

Configured and validated Windows Server DNS within the PORTERLAB Enterprise Cybersecurity Homelab to support Active Directory Domain Services (AD DS), internal hostname resolution, and domain controller service discovery.

The implementation uses Windows Server 2022 as the domain controller and DNS server for the isolated lab domain `porterlab.local`.

> **Security Notice:** All IP addresses in this document are illustrative and use reserved documentation ranges. Actual infrastructure addresses, management endpoints, credentials, and sensitive network identifiers have been excluded.

## Infrastructure

| Component | Configuration |
|---|---|
| DNS Server | DC01 — Windows Server 2022 |
| Active Directory Domain | `porterlab.local` |
| DNS Server IPv4 Address | `192.0.2.10` (illustrative) |
| Forward Lookup Zone | `porterlab.local` |
| AD Service Discovery Zone | `_msdcs.porterlab.local` |
| Upstream DNS Forwarder | `192.0.2.1` (illustrative) |
| DNS Integration | Active Directory-integrated primary zones |
| Client System | Domain-joined Windows 11 workstation |

## Deployment Objectives

1. Establish DNS services to support Active Directory authentication.
2. Implement internal name resolution for domain-joined systems.
3. Configure Active Directory-integrated DNS zones.
4. Configure DNS forwarding for external name resolution.
5. Validate domain controller discovery using DNS SRV records.
6. Troubleshoot and verify DNS connectivity between domain resources.

## Implementation

### 1. Active Directory-Integrated DNS

Deployed the Windows Server DNS role as part of the Active Directory infrastructure.

Validated the following Active Directory-integrated primary DNS zones:

- `porterlab.local`
- `_msdcs.porterlab.local`

These zones support name resolution and Active Directory domain controller discovery.

Active Directory-integrated DNS stores zone information within Active Directory, allowing DNS data to be managed through the domain infrastructure.

### 2. DNS Forwarding Configuration

Configured upstream DNS forwarding to allow the domain controller to resolve queries outside its authoritative DNS zones.

The configuration includes IPv4 and IPv6 forwarders.

**Illustrative network configuration:**

| Component | Example IP |
|---|---|
| Domain Controller / DNS Server | `192.0.2.10` |
| Upstream DNS Forwarder | `192.0.2.1` |
| IPv6 Address Example | `2001:db8::10` |

The actual addresses are intentionally omitted from public documentation.

### 3. Domain Controller Service Discovery

Verified Active Directory LDAP service discovery through DNS SRV records.

**Validation command:**

```powershell
Resolve-DnsName -Type SRV _ldap._tcp.dc._msdcs.porterlab.local
```

**Verified result:**

| Property | Value |
|---|---|
| Record Type | SRV |
| Service | LDAP |
| Target | `dc01.porterlab.local` |
| Port | 389 |
| Priority | 0 |
| Weight | 100 |

The successful response confirms that the DNS infrastructure publishes the domain controller's LDAP service record.

### 4. Forward DNS Resolution

Validated internal domain name resolution using:

```powershell
Resolve-DnsName porterlab.local
```

**Sanitized example output:**

```text
Name             Type    TTL    Section    IPAddress
----             ----    ---    -------    ---------
porterlab.local  A       600    Answer     192.0.2.10
porterlab.local  AAAA    600    Answer     2001:db8::10
```

The actual test returned valid IPv4 and IPv6 address records for the lab domain.

### 5. DNS Zone Verification

Inspected configured zones using:

```powershell
Get-DnsServerZone |
    Select-Object ZoneName,ZoneType,IsDsIntegrated
```

**Verified configuration:**

| DNS Zone | Zone Type | AD Integrated |
|---|---|---|
| `porterlab.local` | Primary | Yes |
| `_msdcs.porterlab.local` | Primary | Yes |
| `0.in-addr.arpa` | Primary | No |
| `127.in-addr.arpa` | Primary | No |
| `255.in-addr.arpa` | Primary | No |
| `TrustAnchors` | Primary | Yes |

The domain forward lookup zones are integrated with Active Directory.

Reverse DNS for the lab's actual subnet has not yet been independently verified.

## Troubleshooting and Validation

### Initial Challenge

During initial deployment, the Windows 11 workstation experienced DNS resolution problems that interfered with domain connectivity and service discovery.

Initial troubleshooting revealed inconsistent DNS resolution results and required additional review of the domain's DNS configuration.

### Troubleshooting Approach

The investigation included:

- Reviewing the Windows Server DNS configuration.
- Examining DNS name resolution results.
- Verifying domain controller connectivity.
- Checking Active Directory domain service discovery.
- Validating DNS functionality after configuration corrections.

### Final Verification

Performed the following tests:

**Domain name resolution:**

```powershell
Resolve-DnsName porterlab.local
```

**LDAP service discovery:**

```powershell
Resolve-DnsName -Type SRV _ldap._tcp.dc._msdcs.porterlab.local
```

**Domain controller health:**

```powershell
dcdiag /test:DNS /test:Advertising /test:Services /q
```

**Results:**

- Forward DNS resolution: Successful
- Domain controller LDAP SRV discovery: Successful
- Active Directory-integrated DNS zones: Confirmed
- DNS service: Running
- Domain controller diagnostic tests: No errors reported during the latest validation
- Domain-joined Windows 11 workstation: Connectivity and secure-channel validation successful

## Security Considerations

DNS is a critical dependency for Active Directory authentication, domain service discovery, and enterprise network communications.

Security and operational considerations include:

- Maintaining accurate internal DNS records.
- Restricting unauthorized modifications to DNS configuration.
- Using centralized DNS administration through Active Directory.
- Monitoring DNS functionality and service availability.
- Protecting domain controller and management interfaces.
- Avoiding unnecessary disclosure of internal network addressing and infrastructure details.

The public project documentation uses reserved example addresses rather than real network identifiers.

## Skills Demonstrated

- Windows Server DNS administration
- Active Directory-integrated DNS configuration
- DNS forwarding
- DNS A, AAAA, and SRV record validation
- LDAP service discovery
- Domain connectivity troubleshooting
- PowerShell-based infrastructure verification
- Windows Server service health diagnostics
- Secure infrastructure documentation

## Future Improvements

- Verify reverse DNS resolution for the lab network.
- Capture and sanitize DNS configuration evidence.
- Validate DNS client configuration across domain-joined systems.
- Implement additional DNS security monitoring.
- Integrate DNS-related telemetry with centralized security monitoring.

## Project Outcome

Successfully configured and validated the Windows Server DNS infrastructure supporting the PORTERLAB Active Directory domain.

Verified forward DNS resolution, Active Directory LDAP service discovery, and domain controller health, establishing a functioning DNS foundation for centralized identity management and enterprise security testing.
