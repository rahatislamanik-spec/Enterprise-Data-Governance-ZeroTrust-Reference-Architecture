# Data Classification and Governance Taxonomy

## Document Purpose

This document defines a target-state data classification and handling model for a fictional mid-sized financial institution using Microsoft 365 and Microsoft Purview.

The taxonomy establishes the business language that later architecture artifacts will use to connect data risk with access controls, sensitivity labels, Data Loss Prevention (DLP), retention, monitoring, investigation, and incident response.

> **Architecture status:** Target-state reference design. This document does not represent a completed production deployment, regulatory certification, or legal determination.

## Design Objectives

The taxonomy is designed to:

- Give employees four understandable classification choices.
- Apply stronger controls as business impact and data sensitivity increase.
- Separate business classification from technology labels and retention rules.
- Support Zero Trust decisions using identity, device, location, application, data, and risk signals.
- Establish ownership, approval, exception, and review responsibilities.
- Create traceability identifiers for the future risk matrix, reference architecture, and investigation scenario.

## Governing Principles

| Principle | Application to data governance |
|---|---|
| Verify explicitly | Access decisions use current identity, device, location, application, data classification, and risk signals. |
| Use least privilege | Users, administrators, applications, and workloads receive only the access required for an approved business purpose. |
| Assume breach | Sensitive data is labeled, encrypted, monitored, and protected against inappropriate movement even after access is granted. |
| Defense in depth | Classification works with identity controls, device compliance, encryption, DLP, audit, retention, and response processes. |
| Governance first | Business owners define sensitivity and permitted use before technical teams configure controls. |
| Risk-based controls | Control strength increases according to impact, exposure, legal obligations, and business context. |
| Lifecycle management | Data is governed from creation and use through retention, legal hold, archival, and defensible disposal. |

## Scope

The taxonomy applies conceptually to information created, received, processed, or stored in:

- Exchange Online
- SharePoint Online
- OneDrive for Business
- Microsoft Teams
- Microsoft 365 desktop and web applications
- Managed endpoints
- Approved line-of-business applications
- Approved third-party services and external collaboration channels

The taxonomy covers structured and unstructured information. Exact technical coverage depends on licensing, workload support, device onboarding, and implementation decisions.

## Classification Decision Factors

Data owners classify information using the highest applicable impact factor.

| Factor | Key question |
|---|---|
| Confidentiality | What harm could result from unauthorized disclosure? |
| Integrity | What harm could result from unauthorized modification or inaccurate data? |
| Availability | What business impact would result if the information became unavailable? |
| Privacy | Does the information identify, describe, or materially affect an individual? |
| Financial sensitivity | Could the information expose accounts, transactions, payment data, credit decisions, or market-sensitive activity? |
| Legal or contractual duty | Is access, preservation, disclosure, or disposal governed by an approved obligation? |
| Business criticality | Does the information support a critical service, decision, or operational process? |
| Aggregation risk | Would a large collection create greater risk than an individual record? |

If classification remains uncertain, the information is assigned the more restrictive level until the data owner or designated governance authority decides otherwise.

## Classification Model

| ID | Level | Definition | Financial-institution examples | Expected impact if disclosed or altered |
|---|---|---|---|---|
| `DATA-01` | Public | Approved for unrestricted external distribution. | Published annual reports, approved marketing material, public product information, published job postings | Minimal, provided integrity and publication approval are maintained |
| `DATA-02` | Internal | Intended for authorized workforce use and not approved for public distribution. | Internal procedures, general meeting notes, internal announcements, non-sensitive training material | Limited operational, reputational, or productivity impact |
| `DATA-03` | Confidential | Sensitive business or personal information requiring need-to-know access and controlled sharing. | Customer correspondence, employee records, internal financial analysis, contracts, non-public operational reports | Material privacy, financial, contractual, or reputational impact |
| `DATA-04` | Highly Confidential / Restricted | Highest-impact information requiring explicit authorization, strong protection, and enhanced monitoring. | Authentication secrets, privileged-access material, payment card data, government identifiers, investigation evidence, merger information, bulk customer financial records | Severe financial, legal, privacy, security, operational, or systemic impact |

## Classification and Handling Matrix

| Control area | `DATA-01` Public | `DATA-02` Internal | `DATA-03` Confidential | `DATA-04` Highly Confidential / Restricted |
|---|---|---|---|---|
| Default access | Anyone after publication approval | Authenticated workforce and approved guests | Named groups with business need | Explicitly approved users and tightly scoped privileged roles |
| External sharing | Permitted after content-owner approval | Approved business channels only | Restricted to approved recipients, purpose, and expiration | Prohibited by default; exception requires documented senior approval |
| Authentication | Appropriate to publishing platform | MFA for organizational access | MFA plus risk-based Conditional Access | Phishing-resistant authentication where supported, compliant device, and enhanced session controls |
| Device posture | No special requirement for published copies | Managed or approved access channel preferred | Managed or compliant device required for full access | Managed, compliant, monitored device required unless formally excepted |
| Encryption | Platform-standard transport protection | Encryption in transit and at rest | Encryption in transit and at rest; rights protection when sharing risk warrants it | Rights-based encryption and restricted access by default where supported |
| Download and synchronization | Unrestricted after publication | Allowed to approved devices | Limited to managed devices and approved applications | Blocked or tightly controlled outside approved managed environments |
| Copy, print, and removable media | Allowed | Business use only | Monitored or restricted according to risk | Blocked by default; exception is time-bound and logged |
| Email and collaboration | Unrestricted after approval | Internal collaboration; external use requires business justification | DLP monitoring, policy tips, and controlled external sharing | Restrictive DLP action, enhanced alerting, and investigation workflow |
| Logging and monitoring | Publication and integrity records | Standard audit logging | DLP, sharing, label, and access activity monitored | Enhanced monitoring, alert triage, and investigation-ready evidence |
| Retention | Approved business or publishing schedule | Business schedule | Approved records schedule and legal requirements | Approved records schedule, legal hold, and defensible disposition controls |
| Disposal | Remove obsolete published copies as appropriate | Standard approved deletion | Authorized deletion after retention and hold checks | Controlled disposition with approval and evidence where required |

## Microsoft Purview Control Mapping

Classification is a business decision. A Microsoft Purview sensitivity label is one technical mechanism used to communicate and enforce that decision. A retention label answers a different question: how long an item must be retained and what happens at the end of that period.

| Classification ID | Conceptual sensitivity label | Typical protection posture | DLP posture | Retention relationship |
|---|---|---|---|---|
| `DATA-01` | Public | Visual marking optional; no access restriction after approval | Detect accidental inclusion of higher-classification content | Apply the approved record category when the content is a business record |
| `DATA-02` | Internal | Organization-wide access; external sharing warning or restriction | Monitor external sharing and unusual movement | Retention depends on record function, not the Internal label alone |
| `DATA-03` | Confidential | Controlled access, marking, and encryption when required | Detect sensitive types and labels; restrict risky sharing with justified exceptions | Apply the relevant records schedule independently or through approved automation |
| `DATA-04` | Highly Confidential / Restricted | Restricted groups, encryption, access expiry where appropriate, and strong device/session requirements | Block high-risk exfiltration by default and generate prioritized alerts | Preserve according to approved schedule and holds; use record controls where justified |

### Separation of Concerns

| Governance question | Primary mechanism |
|---|---|
| How sensitive is this information? | Data classification and sensitivity label |
| Who may access it and under what conditions? | Entra ID, Conditional Access, application permissions, and rights management |
| What risky movement must be monitored or prevented? | Microsoft Purview DLP and Endpoint DLP |
| How long must it be kept? | Retention policy or retention label based on an approved schedule |
| Must normal deletion be suspended? | eDiscovery hold or another authorized preservation mechanism |
| Who investigates suspicious activity? | Security operations, insider risk, privacy, legal, and business stakeholders according to the operating model |

## Roles and Decision Rights

| Role | Accountability |
|---|---|
| Executive risk sponsor | Approves risk appetite, material exceptions, and governance priorities. |
| Data governance council | Owns the taxonomy, resolves cross-functional disputes, and approves major changes. |
| Business data owner | Determines business sensitivity, authorized use, access need, and acceptable residual risk. |
| Records management | Defines approved retention and disposition requirements with legal input. |
| Privacy and legal | Interpret privacy, legal, contractual, investigation, and preservation obligations. |
| Information security | Defines security control requirements and monitors material data-risk events. |
| Microsoft 365 / Purview administration | Implements approved labels, policies, scopes, alerts, and reports without independently defining legal obligations. |
| Data steward | Supports classification quality, metadata, user guidance, and periodic review. |
| Security operations | Triages alerts, preserves evidence, coordinates containment, and escalates according to the incident process. |
| Information user | Applies classifications correctly, follows handling requirements, and reports suspected exposure. |

## Classification Lifecycle

```text
Create or receive information
        |
Identify business purpose, owner, and record category
        |
Assess confidentiality, integrity, availability, privacy, and legal impact
        |
Assign the highest applicable classification
        |
Apply approved technical controls and retention treatment
        |
Monitor access, sharing, movement, and policy outcomes
        |
Review after material change or on the scheduled review date
        |
Reclassify, preserve, archive, or dispose through an approved process
```

Classification must be reconsidered when:

- Information is aggregated into a higher-risk collection.
- A business process, audience, or permitted use changes.
- A legal hold or investigation begins or ends.
- Information becomes publicly approved or loses sensitivity over time.
- A contract, policy, or approved retention requirement changes.
- A security incident reveals that existing controls are insufficient.

## Exception Governance

Classification exceptions do not remove accountability or monitoring.

```text
Exception request
  -> Business justification and requested duration
  -> Data owner review
  -> Security, privacy, legal, or records review as applicable
  -> Risk acceptance by the authorized decision-maker
  -> Time-bound technical exception
  -> Monitoring and evidence collection
  -> Expiration, renewal, or removal
```

Every approved exception must record:

- Requestor and accountable business owner
- Affected data classification and business process
- Control being bypassed or reduced
- Threat, impact, and residual risk
- Compensating controls
- Approval authority
- Start date, expiration date, and review frequency
- Evidence required before closure or renewal

Emergency exceptions must be reviewed retrospectively within the period defined by the governance council.

## Minimum Governance Metadata

The following metadata supports policy administration and auditability:

| Field | Purpose |
|---|---|
| Classification ID | Connects the item or repository to the approved taxonomy. |
| Data owner | Identifies the accountable business decision-maker. |
| Data steward | Identifies the operational governance contact. |
| Business purpose | Explains why the information is collected and used. |
| Record category | Connects content to an approved retention schedule. |
| Authorized audience | Defines permitted users, groups, or external parties. |
| Review date | Triggers reconsideration of classification and access. |
| Legal-hold status | Prevents disposal when preservation is required. |
| Exception reference | Links temporary deviations to approval evidence. |

## Target Measures

These are target governance measures, not achieved results:

- Percentage of high-risk repositories with an assigned data owner
- Percentage of `DATA-03` and `DATA-04` content locations covered by approved labels and DLP policies
- Percentage of classification exceptions reviewed before expiration
- Rate of user overrides and rejected business justifications
- DLP false-positive rate by policy and workload
- Median time from high-severity alert creation to triage
- Percentage of disposition actions completed with required approval evidence

## Assumptions and Limitations

- The organization and examples are fictional and intended for portfolio demonstration.
- This is a target-state architecture artifact, not evidence of production implementation.
- Classification does not by itself grant access, establish retention, or demonstrate compliance.
- Retention periods and legal-hold decisions require authorization from legal and records-management stakeholders.
- Regulatory references and control mappings must be validated for the organization's jurisdiction, contracts, products, and risk appetite.
- Microsoft Purview capabilities depend on licensing, workload support, configuration, device onboarding, and data location.
- Automated classification requires testing, simulation, tuning, and human oversight before restrictive enforcement.
- Policy presence does not prove effective operation; evidence must include telemetry, review, testing, and governance decisions.

## Traceability to Future Artifacts

| Future artifact | How this taxonomy will be used |
|---|---|
| Risk Control Matrix | Risks and controls will reference `DATA-01` through `DATA-04`. |
| Reference Architecture | Trust decisions and control planes will show how each classification receives layered protection. |
| Data Exfiltration Investigation Scenario | The scenario will use the classification to determine severity, evidence handling, containment, and escalation. |
| Phase 2 implementation evidence | Screenshots will be mapped only to controls actually configured and observed. |
| Phase 3 automation | Scripts and reports will validate configuration and telemetry without making unsupported compliance conclusions. |

## Approval and Review

| Field | Target-state value |
|---|---|
| Document owner | Data Governance Council |
| Technical custodian | Microsoft 365 / Purview Administration |
| Required approvers | Information Security, Privacy, Legal, Records Management, and relevant business data owners |
| Review frequency | At least annually and after material regulatory, business, platform, or threat changes |
| Current status | Draft reference architecture artifact |
