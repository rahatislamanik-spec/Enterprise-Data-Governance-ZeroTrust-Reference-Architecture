# Risk Control Matrix

## Purpose

This matrix translates the approved [Data Classification and Governance Taxonomy](data-classification-governance-taxonomy.md) into a target-state control model for a fictional mid-sized financial institution.

The traceability model is:

```text
DATA classification -> RISK scenario -> CTRL safeguard -> PROC operating procedure
```

> **Architecture status:** Target-state reference design. Control mappings are illustrative and do not represent production deployment, regulatory certification, or validated licensing.

## Risk Evaluation Model

| Rating | Definition |
|---|---|
| Low | Limited impact and exposure after planned controls; accepted through routine ownership. |
| Medium | Material risk remains and requires monitoring, periodic review, and accountable acceptance. |
| High | Significant financial, privacy, legal, security, or operational exposure requiring prioritized treatment and senior oversight. |
| Critical | Potential severe or systemic impact requiring immediate executive attention and strong preventive and response controls. |

Residual-risk ratings assume the target controls operate as designed. They must be reassessed using implementation evidence, telemetry, testing, exceptions, and incident trends.

## Control Catalog

| Control ID | Control | Type | Target-state intent | Primary owner |
|---|---|---|---|---|
| `CTRL-01` | Data ownership and classification | Governance / Preventive | Assign accountable owners and apply `DATA-01` through `DATA-04` using approved decision criteria. | Data Governance Council |
| `CTRL-02` | Sensitivity labels and rights protection | Preventive | Communicate classification and apply marking, access restrictions, and encryption where justified. | Microsoft 365 / Purview Administration |
| `CTRL-03` | Identity assurance and Conditional Access | Preventive | Verify identity, authentication strength, device, location, application, and risk before granting access. | Identity and Access Management |
| `CTRL-04` | Privileged Identity Management and role separation | Preventive / Detective | Use eligible, time-bound, approved administration and separate policy, investigation, and legal responsibilities. | Identity and Access Management |
| `CTRL-05` | Managed-device and application controls | Preventive | Restrict sensitive-data access, synchronization, download, and local handling to approved devices and applications. | Endpoint Management |
| `CTRL-06` | Microsoft Purview DLP | Preventive / Detective | Monitor and restrict risky sharing or movement according to classification, content, destination, user, and workload. | Information Security |
| `CTRL-07` | Endpoint DLP and egress controls | Preventive / Detective | Monitor or restrict copying, printing, upload, removable media, clipboard, and other endpoint exfiltration channels. | Information Security / Endpoint Management |
| `CTRL-08` | Retention and disposition governance | Governance / Preventive | Apply approved retention schedules, defensible disposition, and records controls independently of sensitivity. | Records Management |
| `CTRL-09` | Audit and activity monitoring | Detective | Record and review label, access, sharing, policy match, administrator, and content activity. | Security Operations |
| `CTRL-10` | Insider Risk Management | Detective | Correlate approved risk indicators and user activity under privacy, legal, and role-separation safeguards. | Insider Risk Program Owner |
| `CTRL-11` | Microsoft Defender investigation integration | Detective / Corrective | Correlate identities, endpoints, cloud activity, and DLP alerts for security triage and containment. | Security Operations |
| `CTRL-12` | eDiscovery preservation and legal hold | Preventive / Recovery | Preserve relevant content and maintain authorized case, custodian, search, review, and export controls. | Legal / eDiscovery |
| `CTRL-13` | Exception and risk-acceptance governance | Governance / Corrective | Require documented justification, compensating controls, approval, expiration, monitoring, and renewal or removal. | Enterprise Risk / Data Owner |
| `CTRL-14` | Security awareness and policy tips | Preventive | Provide contextual guidance and require justified overrides to reduce accidental misuse. | Security Awareness / Data Governance |
| `CTRL-15` | Incident containment and recovery | Corrective / Recovery | Revoke access, isolate devices, restrict sharing, preserve evidence, recover services, and tune controls through approved response. | Security Operations / Incident Commander |

## Procedure Catalog

| Procedure ID | Procedure | Trigger | Accountable owner | Required evidence |
|---|---|---|---|---|
| `PROC-01` | Classification and ownership review | New repository, new business process, material content change, or scheduled review | Business Data Owner | Classification decision, owner, purpose, authorized audience, review date |
| `PROC-02` | Label and DLP policy lifecycle | New or changed protection requirement | Information Security | Design approval, simulation results, tuning decision, exception list, enforcement approval |
| `PROC-03` | Access and privilege review | Scheduled review, role change, elevated access request, or anomalous access | Identity and Access Management | Role assignment, approval, activation, access-review result, removal evidence |
| `PROC-04` | Data exfiltration investigation | High-risk DLP, endpoint, insider-risk, or Defender alert | Security Operations | Alert, activity timeline, preserved logs, triage decision, containment record |
| `PROC-05` | Legal preservation and eDiscovery | Legal, regulatory, investigation, or litigation preservation request | Legal / eDiscovery | Matter authorization, custodians, hold scope, searches, review actions, release approval |
| `PROC-06` | Retention and disposition review | Scheduled disposition, retention change, or hold conflict | Records Management | Approved schedule, hold check, reviewer decision, disposition evidence |
| `PROC-07` | Exception management | Requested deviation from an approved data control | Enterprise Risk / Data Owner | Business justification, risk assessment, compensating controls, approval, expiry |
| `PROC-08` | Incident containment and recovery | Confirmed or reasonably suspected material data event | Incident Commander | Containment actions, access changes, communications, recovery validation, lessons learned |
| `PROC-09` | Control assurance and reporting | Monthly, quarterly, or annual governance cycle | Data Governance Council | Coverage metrics, exceptions, false positives, incidents, overdue reviews, remediation plan |

## Risk Control Matrix

| Risk ID | Risk scenario | Data scope | Inherent risk | Controls | Control type | Procedure | Control owner | Business owner | Monitoring source | Evidence produced | Review frequency | Residual risk |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| `RISK-01` | Sensitive customer records are shared externally with an unauthorized recipient. | `DATA-03`, `DATA-04` | Critical | `CTRL-01`, `CTRL-02`, `CTRL-03`, `CTRL-06`, `CTRL-09`, `CTRL-14` | Governance / Preventive / Detective | `PROC-02`, `PROC-04` | Information Security | Customer Operations Data Owner | DLP alerts, Activity Explorer, audit log, sharing activity | Policy match, label state, recipient, user justification, triage record | Monthly policy review; immediate alert triage | Medium |
| `RISK-02` | A departing financial advisor attempts to export a large collection of customer records. | `DATA-03`, `DATA-04` | Critical | `CTRL-03`, `CTRL-05`, `CTRL-06`, `CTRL-07`, `CTRL-09`, `CTRL-10`, `CTRL-11`, `CTRL-15` | Preventive / Detective / Corrective / Recovery | `PROC-04`, `PROC-08` | Security Operations | Wealth Management Data Owner | Endpoint DLP, Insider Risk, Defender, Entra sign-in logs, audit log | Alert timeline, device/user activity, file events, access changes, containment record | Continuous monitoring; quarterly scenario review | Medium |
| `RISK-03` | A compromised account accesses confidential financial information from an unmanaged or high-risk device. | `DATA-03`, `DATA-04` | Critical | `CTRL-03`, `CTRL-05`, `CTRL-09`, `CTRL-11`, `CTRL-15` | Preventive / Detective / Corrective / Recovery | `PROC-03`, `PROC-08` | Identity and Access Management | Relevant Business Data Owner | Entra sign-in risk, Conditional Access, Intune, Defender | Authentication result, device state, risk signals, session revocation, investigation record | Continuous; quarterly access-control review | Medium |
| `RISK-04` | Excessive or standing administrative privileges allow unauthorized policy changes or evidence access. | `DATA-02`, `DATA-03`, `DATA-04` | High | `CTRL-03`, `CTRL-04`, `CTRL-09`, `CTRL-13` | Governance / Preventive / Detective | `PROC-03`, `PROC-07` | Identity and Access Management | Executive Risk Sponsor | PIM, Entra audit logs, Purview audit, access reviews | Eligibility, activation, approval, role change, review result | Monthly privileged-role review | Low to Medium |
| `RISK-05` | A DLP or label policy blocks legitimate financial operations because it was enabled without simulation and tuning. | `DATA-02`, `DATA-03`, `DATA-04` | High | `CTRL-01`, `CTRL-06`, `CTRL-13`, `CTRL-14` | Governance / Preventive / Corrective | `PROC-02`, `PROC-07` | Information Security | Affected Business Process Owner | DLP simulation, user overrides, help-desk cases, policy reports | Simulation results, false-positive analysis, approval, exception record | Before enforcement; monthly after change | Medium |
| `RISK-06` | Records are deleted before an approved retention period or legal preservation obligation ends. | `DATA-02`, `DATA-03`, `DATA-04` | Critical | `CTRL-08`, `CTRL-09`, `CTRL-12`, `CTRL-13` | Governance / Preventive / Detective / Recovery | `PROC-05`, `PROC-06` | Records Management | Record-owning Business Unit | Retention reports, disposition review, eDiscovery case activity, audit log | Schedule, hold status, disposition decision, deletion or preservation evidence | Quarterly; before every disposition action | Low to Medium |
| `RISK-07` | Information is retained beyond business or legal need, increasing privacy, discovery, and breach exposure. | `DATA-02`, `DATA-03`, `DATA-04` | High | `CTRL-01`, `CTRL-08`, `CTRL-09`, `CTRL-13` | Governance / Preventive / Detective / Corrective | `PROC-01`, `PROC-06`, `PROC-09` | Records Management | Business Data Owner | Retention coverage, disposition backlog, storage and governance reports | Schedule approval, overdue disposition report, remediation record | Quarterly | Medium |
| `RISK-08` | Employees misclassify information or use unjustified overrides, weakening protection. | `DATA-02`, `DATA-03`, `DATA-04` | High | `CTRL-01`, `CTRL-02`, `CTRL-06`, `CTRL-09`, `CTRL-14` | Governance / Preventive / Detective | `PROC-01`, `PROC-02`, `PROC-09` | Data Governance Council | Business Data Owner | Label analytics, DLP overrides, policy tips, training completion | Label changes, override reason, coaching or remediation action | Monthly trend review; annual training review | Medium |
| `RISK-09` | A third-party application or service principal gains excessive access to sensitive Microsoft 365 data. | `DATA-03`, `DATA-04` | Critical | `CTRL-03`, `CTRL-04`, `CTRL-09`, `CTRL-11`, `CTRL-13` | Governance / Preventive / Detective / Corrective | `PROC-03`, `PROC-07`, `PROC-08` | Cloud Security / Identity and Access Management | Application Business Owner | Entra application audit, OAuth grants, Defender, audit logs | Permission grant, owner, approval, credential status, remediation action | Monthly high-risk app review; quarterly full review | Medium |
| `RISK-10` | Investigation access or evidence handling exposes employee or customer information beyond authorized need. | `DATA-03`, `DATA-04` | Critical | `CTRL-03`, `CTRL-04`, `CTRL-09`, `CTRL-10`, `CTRL-12`, `CTRL-13` | Governance / Preventive / Detective | `PROC-04`, `PROC-05`, `PROC-07` | Legal / Insider Risk Program Owner | Executive Risk Sponsor | Case audit, role assignments, search and export logs | Case authorization, role activation, search/export record, access review | Per case; quarterly role review | Low to Medium |
| `RISK-11` | Unmanaged endpoints or removable media are used to copy restricted records outside approved channels. | `DATA-03`, `DATA-04` | Critical | `CTRL-03`, `CTRL-05`, `CTRL-07`, `CTRL-09`, `CTRL-11`, `CTRL-15` | Preventive / Detective / Corrective / Recovery | `PROC-04`, `PROC-08` | Endpoint Management / Security Operations | Relevant Business Data Owner | Endpoint DLP, Intune, Defender for Endpoint, audit log | Device state, copy event, file classification, user context, containment evidence | Continuous; monthly policy health review | Medium |
| `RISK-12` | Governance controls drift because policies, exceptions, ownership, and telemetry are not reviewed. | `DATA-01` through `DATA-04` | High | `CTRL-01`, `CTRL-04`, `CTRL-09`, `CTRL-13` | Governance / Preventive / Detective / Corrective | `PROC-01`, `PROC-03`, `PROC-07`, `PROC-09` | Data Governance Council | Executive Risk Sponsor | Governance dashboard, policy inventory, access reviews, exception register | Coverage report, overdue owner review, expired exception, remediation plan | Monthly operational; quarterly governance council | Medium |

## Defense-in-Depth View

| Layer | Representative controls | Risk-reduction contribution |
|---|---|---|
| Governance | `CTRL-01`, `CTRL-08`, `CTRL-13` | Defines ownership, classification, retention, acceptable risk, and exceptions. |
| Identity | `CTRL-03`, `CTRL-04` | Reduces unauthorized and excessive access before data controls are evaluated. |
| Device and application | `CTRL-05`, `CTRL-07` | Limits where sensitive content can be accessed, synchronized, copied, or uploaded. |
| Data | `CTRL-02`, `CTRL-06`, `CTRL-08` | Applies classification-aware protection, DLP, retention, and disposition. |
| Detection | `CTRL-09`, `CTRL-10`, `CTRL-11` | Correlates behavior, policy matches, identity risk, endpoint activity, and administrative changes. |
| Investigation and response | `CTRL-12`, `CTRL-15` | Preserves evidence, contains exposure, supports legal review, and drives recovery and tuning. |

## Control Lifecycle

```text
Business and data-owner requirement
        -> Risk and impact assessment
        -> Control design and ownership
        -> Simulation or limited pilot
        -> Evidence review and tuning
        -> Authorized enforcement
        -> Continuous monitoring
        -> Exception and incident feedback
        -> Periodic assurance and redesign
```

## Governance Constraints

- Technical administrators implement approved requirements but do not independently determine legal obligations or risk acceptance.
- Policy presence is not evidence of operating effectiveness.
- Restrictive controls require simulation, business-process testing, exception planning, and rollback readiness.
- Insider-risk and eDiscovery access require privacy safeguards, role separation, auditability, and authorized purpose.
- Retention and sensitivity are related but independent governance dimensions.
- Every exception must have an owner, compensating control, expiration, monitoring requirement, and approval record.
- Residual risk must be accepted by an authorized business or risk owner, not inferred from product configuration.

## Target Assurance Measures

These measures define future reporting needs and are not claimed results:

- Percentage of `DATA-03` and `DATA-04` repositories mapped to an accountable owner
- Percentage of critical risks with tested preventive and detective controls
- DLP false-positive and justified-override rates by workload
- Percentage of privileged roles reviewed and activated through PIM
- Percentage of exceptions reviewed before expiration
- Median time from high-severity alert creation to triage
- Percentage of disposition events checked against active holds
- Percentage of control deficiencies with an approved remediation owner and due date
