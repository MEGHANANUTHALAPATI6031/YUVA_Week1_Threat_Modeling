# YUVA Week 1 - Virtual Threat Modeling and Vulnerability Assessment

## Project Overview

This project was completed as part of the YUVA Week 1 technical task, **Virtual Threat Modeling and Vulnerability Assessment**.

The objective was to perform a non-destructive security assessment of a Windows local host, identify active network services, review firewall configurations, analyze potential security risks, and recommend appropriate security mitigations.

## Tools Used

* Nmap 7.991
* Windows Command Prompt
* Netstat
* Tasklist
* Windows Defender Firewall
* STRIDE Threat Modeling

## Target

* Target Host: `127.0.0.1`
* Operating System: Windows
* Assessment Type: Localhost security assessment

## Scan Results

The assessment identified the following active services:

| Port      | Service               | Version / Details            | Risk Review |
| --------- | --------------------- | ---------------------------- | ----------- |
| 135/TCP   | Microsoft Windows RPC | MSRPC                        | Medium      |
| 445/TCP   | Microsoft-DS / SMB    | Windows file-sharing service | High        |
| 3306/TCP  | MySQL                 | MySQL 8.0.46                 | High        |
| 33060/TCP | MySQL X Protocol      | MySQL X Protocol listener    | High        |

A follow-up scan of TCP port `6646` confirmed that the port was **closed**, so it was excluded from the final findings.

## Process Verification

Netstat and Tasklist were used to identify processes associated with listening services.

* TCP 135 → `svchost.exe`
* TCP 445 → System process
* TCP 3306 → `mysqld.exe`
* TCP 33060 → `mysqld.exe`

## Firewall Review

Windows Defender Firewall rules were reviewed for the identified services.

The inbound rules for ports `3306` and `33060` were found to allow TCP traffic from **Any** remote IP across Domain, Private, and Public profiles. This configuration should be reviewed and restricted to trusted systems or networks where appropriate.

SMB-related firewall rules and RPC-EPMAP rules were also reviewed as part of the assessment.

## STRIDE Threat Model

### Spoofing

Unauthorized users may attempt to authenticate to exposed services.

**Mitigation:** Use strong authentication and restrict network access.

### Tampering

Unauthorized access to services could potentially allow modification of data or configurations.

**Mitigation:** Apply least-privilege access controls and security updates.

### Repudiation

Insufficient logging can make security events difficult to investigate.

**Mitigation:** Enable appropriate security and service logging.

### Information Disclosure

Exposed network services may provide information about systems or services.

**Mitigation:** Restrict unnecessary network access and minimize exposed services.

### Denial of Service

Unnecessary exposed services can increase the attack surface.

**Mitigation:** Apply firewall restrictions, security updates, and monitoring.

### Elevation of Privilege

A compromised service could potentially become a starting point for further unauthorized access.

**Mitigation:** Maintain updated software and use least-privilege configurations.

## Recommended Mitigations

1. Restrict inbound MySQL port `3306` access to trusted systems or networks.
2. Disable MySQL X Protocol port `33060` if it is not required.
3. Restrict port `33060` if the service is required.
4. Review MySQL network/interface binding configuration.
5. Review SMB port `445` exposure and related firewall rules.
6. Review RPC and remote-management firewall rules.
7. Maintain current security updates for Windows and installed services.
8. Use strong authentication and least-privilege permissions.
9. Maintain appropriate security logs and monitoring.
10. Perform another security scan after remediation.

## Evidence Files

The `Scan_Results` folder contains the assessment evidence:

* `nmap_scan.txt`
* `port_6646_check.txt`
* `service_ports.txt`
* `mysql_scan.txt`

## Conclusion

The assessment successfully identified active network services on the local Windows host and reviewed their associated processes and firewall configurations. The main areas requiring security review were Windows RPC, SMB, MySQL, and MySQL X Protocol exposure.

The assessment demonstrates practical application of network reconnaissance, service identification, firewall review, STRIDE threat modeling, risk assessment, and security mitigation planning.
