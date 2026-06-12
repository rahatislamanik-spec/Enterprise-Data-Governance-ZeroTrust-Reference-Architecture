# Enterprise Data Governance and Zero Trust Reference Architecture

Target-state Microsoft 365 data governance and security architecture for a fictional mid-sized financial institution.

This repository demonstrates how business ownership, data classification, risk decisions, identity and device trust, Microsoft Purview controls, monitoring, investigation, and response can operate as one traceable governance system.

> **Portfolio status:** Phase 1 architecture design. This is not a production deployment, regulatory certification, legal opinion, or claim of implemented control effectiveness.

## Executive Summary

Financial institutions must protect customer, employee, financial, operational, and investigation data without preventing legitimate business activity. That requires more than enabling individual Microsoft 365 features. Governance decisions must be translated into layered controls, accountable operating procedures, evidence, exceptions, and residual-risk decisions.

This reference architecture begins with a four-level data classification taxonomy and traces material risks into preventive, detective, corrective, recovery, and governance controls. It connects Microsoft Entra ID, Conditional Access, MFA, Privileged Identity Management, Microsoft Intune, Microsoft Purview, Microsoft Defender, eDiscovery, and operational response processes under Zero Trust and defense-in-depth principles.

## Business Problem

Common data-governance failures include:

- Sensitive information is not consistently classified or owned.
- Access decisions do not account for identity, device, application, location, and risk context.
- DLP policies are deployed without simulation, business-process testing, or exception planning.
- Sensitivity and retention are treated as the same governance decision.
- Privileged administrators have excessive standing access to policies or evidence.
- Investigations lack clear authority, preservation, privacy safeguards, and role separation.
- Exceptions remain open without expiration, monitoring, or accountable risk acceptance.
- Product configuration is mistaken for compliance or operating effectiveness.

## Architecture Objectives

1. Establish a business-led classification and ownership model.
2. Translate data risks into traceable controls and procedures.
3. Verify access using identity, device, application, location, data, and risk signals.
4. Apply least privilege to users, workloads, administrators, and investigators.
5. Protect data at rest, in use, and in motion through layered controls.
6. Govern retention, preservation, and disposition independently from sensitivity.
7. Detect and investigate risky activity using authorized, auditable workflows.
8. Make exceptions time-bound, monitored, and explicitly accepted.
9. Define target measures without fabricating implementation results.

## Design Principles

| Principle | Architectural application |
|---|---|
| Zero Trust | No access request or data movement is trusted solely because of network location or prior authentication. |
| Verify explicitly | Decisions use current identity, device, application, location, data classification, and risk context. |
| Least privilege | Access and administrative authority are scoped, time-bound, approved, and reviewed. |
| Assume breach | Monitoring, evidence preservation, containment, and recovery are built into the design. |
| Defense in depth | Governance, identity, device, data, detection, investigation, and response controls reinforce one another. |
| Governance first | Business, legal, privacy, records, and risk decisions precede technical implementation. |
| Risk-based controls | Control strength and review frequency increase with impact, exposure, and uncertainty. |
| Data lifecycle management | Information is governed from creation through use, preservation, retention, and disposal. |

## Phase 1 Artifacts

| Artifact | Purpose |
|---|---|
| [Data Classification and Governance Taxonomy](governance/data-classification-governance-taxonomy.md) | Defines `DATA-01` through `DATA-04`, handling requirements, ownership, exceptions, and lifecycle governance. |
| [Risk Control Matrix](governance/risk-control-matrix.md) | Maps financial-sector risks to 15 controls, 9 procedures, monitoring sources, evidence, owners, review frequency, and residual risk. |
| [Reference Architecture](architecture/reference-architecture.md) | Presents the layered governance, identity, device, data, monitoring, investigation, and response architecture in Mermaid. |
| [Data Exfiltration Investigation Scenario](operations/data-exfiltration-investigation-scenario.md) | Applies the design to a tabletop scenario involving a departing financial advisor and customer records. |

## Reference Architecture

The [Mermaid reference architecture](architecture/reference-architecture.md) is organized into seven layers:

1. Business Governance
2. Identity and Access
3. Device Trust
4. Data Governance
5. Monitoring and Detection
6. Investigation
7. Response

The design treats Microsoft products as control-enabling platforms inside a broader operating model. Business owners determine purpose and acceptable risk; legal, privacy, and records stakeholders authorize preservation and lifecycle decisions; technical teams implement approved controls; and security operations investigate and respond using auditable procedures.

## Traceability Model

Every remaining artifact inherits from the approved taxonomy:

```text
DATA-XX -> RISK-XX -> CTRL-XX -> PROC-XX -> Evidence -> Residual Risk
```

Example:

```text
DATA-04 Highly Confidential customer records
  -> RISK-02 departing advisor attempts export
  -> CTRL-06 DLP + CTRL-07 Endpoint DLP + CTRL-10 Insider Risk
  -> PROC-04 investigation + PROC-08 containment and recovery
  -> alert, activity timeline, preserved evidence, decision record
  -> residual risk accepted or remediated by an authorized owner
```

### Identifier Families

| Prefix | Meaning |
|---|---|
| `DATA-XX` | Data classification requirement |
| `RISK-XX` | Business or security risk scenario |
| `CTRL-XX` | Governance, preventive, detective, corrective, or recovery control |
| `PROC-XX` | Operating procedure that makes the control actionable and auditable |

## Control Domains

| Domain | Representative capabilities |
|---|---|
| Governance | Data owners, governance council, records management, legal/privacy review, exception approval |
| Identity | Microsoft Entra ID, Conditional Access, MFA, authentication strength, PIM, application access |
| Device | Microsoft Intune, device compliance, approved applications, Microsoft Defender for Endpoint |
| Data | Microsoft Purview sensitivity labels, encryption, DLP, Endpoint DLP, retention policies and labels |
| Monitoring | Microsoft Purview Audit, Activity Explorer, Insider Risk Management, Microsoft Defender |
| Investigation | Security triage, authorized case management, Microsoft Purview eDiscovery, evidence preservation |
| Response | Session revocation, access restriction, endpoint isolation, legal/privacy escalation, recovery and tuning |

## Repository Structure

```text
Enterprise-Data-Governance-ZeroTrust-Reference-Architecture/
|-- README.md
|-- architecture/
|   `-- reference-architecture.md
|-- governance/
|   |-- data-classification-governance-taxonomy.md
|   `-- risk-control-matrix.md
|-- operations/
|   `-- data-exfiltration-investigation-scenario.md
|-- evidence/                  # Planned for approved Phase 2 evidence
`-- automation/                # Planned for reviewed Phase 3 automation
```

The reserved Phase 2 and Phase 3 boundaries allow implementation evidence and automation to be added without mixing them with target-state design or redesigning the repository.

## Assumptions

- The institution and business scenarios are fictional.
- The organization uses Microsoft 365 as a primary collaboration platform.
- Microsoft Entra ID provides identity control and Microsoft Intune supports managed-device posture.
- Microsoft Purview and Microsoft Defender capabilities are selected according to approved requirements.
- Exact features depend on licensing, workload support, tenant configuration, device onboarding, and data location.
- Legal, privacy, records, and business stakeholders approve requirements within their authority.
- Restrictive controls move through design, simulation or pilot, tuning, approval, enforcement, monitoring, and review.

## Limitations

- No production deployment or control effectiveness is claimed.
- No compliance certification, regulatory conclusion, or legal interpretation is provided.
- Retention periods and regulatory mappings are illustrative until approved by authorized stakeholders.
- The Phase 1 architecture contains no screenshots, tenant configuration, generated reports, or execution evidence.
- The tabletop scenario contains no real incident, affected person, fabricated alert, invented outcome, or performance metric.
- Microsoft product coverage does not replace organizational policy, training, ownership, investigation authority, or risk acceptance.
- Residual-risk ratings require future validation using implementation evidence, tests, telemetry, exceptions, and incident trends.

## Target Architecture Measures

These are future reporting requirements, not achieved outcomes:

- Coverage of high-risk repositories by accountable data owner and classification
- Coverage of `DATA-03` and `DATA-04` locations by approved labels and DLP policies
- DLP false-positive and justified-override rates
- Privileged-role activation and review coverage
- Exception review before expiration
- High-severity alert time to triage
- Retention disposition checks against active holds
- Control deficiencies with accountable remediation owners and dates

## Future Roadmap

### Phase 2 - Microsoft 365 E5 Implementation Evidence

- Map only observed tenant configuration to the Phase 1 controls.
- Add sanitized screenshots and evidence descriptions.
- Distinguish configured, simulated, enforced, observed, and unverified states.
- Preserve policy mode, scope, prerequisites, limitations, and evidence dates.
- Avoid treating screenshots as proof of effectiveness or compliance.

### Phase 3 - PowerShell Governance Automation

- Build read-only configuration validation and reporting scripts.
- Export structured evidence for labels, DLP, retention, audit, roles, and exceptions where supported.
- Validate modules, permissions, cmdlet availability, and tenant-specific properties.
- Protect credentials, tokens, tenant identifiers, and sensitive report content.
- Treat unavailable telemetry as unverified rather than healthy.

## Skills Demonstrated

`Enterprise Architecture` | `Data Governance` | `Microsoft Purview` | `Zero Trust` | `Defense in Depth` | `Risk Management` | `Information Protection` | `Data Loss Prevention` | `Identity and Access Management` | `Data Lifecycle Management` | `Insider Risk` | `eDiscovery` | `Incident Response` | `Control Traceability` | `Technical Documentation`

## Author

**Md Rahat Islam Anik**

Microsoft 365, identity, infrastructure, security, and governance portfolio
