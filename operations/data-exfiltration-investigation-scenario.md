# Data Exfiltration Investigation Scenario

## Scenario Title

**Departing Financial Advisor Attempts to Export Customer Records**

## Purpose

This tabletop scenario demonstrates how the target-state governance architecture coordinates detection, evidence preservation, containment, legal escalation, investigation, recovery, and control improvement.

It is not a record of an actual incident. No alert volume, investigation result, response time, affected customer count, or control outcome is claimed.

Source artifacts:

- [Data Classification and Governance Taxonomy](../governance/data-classification-governance-taxonomy.md)
- [Risk Control Matrix](../governance/risk-control-matrix.md)
- [Reference Architecture](../architecture/reference-architecture.md)

## Scenario Context

A financial advisor has submitted notice of resignation. During the notice period, the advisor attempts to copy a large collection of customer records from an approved Microsoft 365 repository to an unapproved external destination.

The records may include customer contact details, account information, financial profiles, correspondence, and advisory notes. The scenario assumes that the information is classified as `DATA-03` Confidential or `DATA-04` Highly Confidential / Restricted according to the approved taxonomy.

## Traceability

| Traceability element | References |
|---|---|
| Data classifications | `DATA-03`, `DATA-04` |
| Primary risk | `RISK-02` - departing advisor exports customer records |
| Related risks | `RISK-01`, `RISK-03`, `RISK-10`, `RISK-11`, `RISK-12` |
| Preventive controls | `CTRL-02`, `CTRL-03`, `CTRL-05`, `CTRL-06`, `CTRL-07` |
| Detective controls | `CTRL-09`, `CTRL-10`, `CTRL-11` |
| Governance and preservation | `CTRL-01`, `CTRL-12`, `CTRL-13` |
| Response and recovery | `CTRL-15` |
| Primary procedures | `PROC-04`, `PROC-08` |
| Supporting procedures | `PROC-03`, `PROC-05`, `PROC-07`, `PROC-09` |

## Tabletop Assumptions

- The organization has an approved offboarding process, but resignation alone is not treated as proof of malicious intent.
- Customer records have accountable data owners and an approved classification.
- Relevant Microsoft 365 audit sources are enabled and accessible to authorized investigators.
- DLP, Endpoint DLP, Insider Risk Management, Defender, Entra ID, Intune, and eDiscovery capabilities depend on licensing and implementation status.
- Employment, privacy, surveillance, legal-hold, and notification decisions are made by authorized stakeholders, not by technical administrators acting alone.
- Investigation access follows least privilege, purpose limitation, role separation, and audit requirements.

## Trigger Conditions

The workflow may begin when one or more approved detection sources identify activity such as:

- A DLP policy match involving `DATA-03` or `DATA-04` content.
- A large or unusual download, copy, synchronization, upload, print, or removable-media event.
- An attempt to send customer records to a personal email address or unapproved external domain.
- An Endpoint DLP event involving a restricted egress channel.
- A Defender alert that adds device or session risk context.
- An Insider Risk alert created under an approved policy and privacy framework.
- A business, compliance, or manager report requiring validation.

A single signal does not establish intent. Triage must validate context before the organization reaches an incident conclusion.

## Investigation Workflow

```text
Potential exfiltration signal
        |
Detection validation and severity triage
        |
Preserve volatile and authoritative evidence
        |
Apply proportionate containment when justified
        |
Notify legal, privacy, HR, and business owners by threshold
        |
Conduct authorized investigation and scope analysis
        |
Decide recovery, notification, and control-remediation actions
        |
Document lessons learned and reassess residual risk
```

## Stage 1 - Detection

**Objective:** Establish whether the signal is credible, in scope, and sufficiently material for investigation.

| Activity | Decision or evidence |
|---|---|
| Record the originating alert | Alert identifier, source, timestamp, policy and rule, severity, user, device, workload |
| Identify the data involved | Classification, record owner, repository, file count or aggregation risk where available |
| Identify the attempted action | Download, copy, email, upload, removable media, print, sharing change, or application access |
| Preserve original context | Raw alert and source links without modifying the underlying content |
| Check duplicate or known activity | Existing case, approved migration, business process, testing activity, or known false positive |

**Controls and procedures:** `CTRL-06`, `CTRL-07`, `CTRL-09`, `CTRL-10`, `CTRL-11`, `PROC-04`.

## Stage 2 - Triage

**Objective:** Determine preliminary severity and the next authorized action without assuming intent.

The triage analyst evaluates:

- Whether the content is `DATA-03` or `DATA-04`.
- Volume and aggregation risk.
- Destination trust and external-sharing status.
- User role, approved business purpose, employment context, and prior exceptions.
- Device compliance, management, ownership, and Defender risk.
- Sign-in risk, location, session, authentication strength, and Conditional Access result.
- Whether DLP warned, blocked, allowed an override, or operated in simulation.
- Whether the user supplied a business justification.
- Whether related activity appears across Exchange, SharePoint, OneDrive, Teams, endpoints, or applications.

| Preliminary outcome | Next action |
|---|---|
| Benign or authorized activity | Document rationale, retain required evidence, tune the control if needed, and close through `PROC-04`. |
| Unclear activity | Escalate for additional evidence and business-owner validation. |
| Credible high-risk activity | Begin evidence preservation and proportionate containment under `PROC-08`. |
| Potential legal, privacy, or employee-relations matter | Notify authorized legal, privacy, and HR stakeholders before expanding investigative scope. |

## Stage 3 - Evidence Preservation

**Objective:** Preserve relevant evidence before it is changed, expires, or becomes unavailable.

Potential evidence sources include:

- DLP alert, rule, policy mode, matched activity, and user override
- Microsoft Purview Audit events
- Activity Explorer events
- Entra ID sign-in, risk, Conditional Access, and application activity
- Intune device compliance and ownership state
- Microsoft Defender device timeline and correlated incidents
- SharePoint, OneDrive, Exchange, and Teams activity
- Insider Risk case activity when authorized
- eDiscovery custodian, hold, search, review, and export audit records
- Help-desk, offboarding, access-request, and approved-exception records

Preservation requirements:

1. Record authoritative timestamps and time zones.
2. Preserve source identifiers, collection method, collector, and access history.
3. Avoid unnecessary duplication or export of customer content.
4. Restrict evidence to the approved case team.
5. Request a legal hold through `PROC-05` when authorized preservation is required.
6. Document evidence gaps, retention limits, and unavailable telemetry.

**Controls and procedures:** `CTRL-04`, `CTRL-09`, `CTRL-12`, `PROC-04`, `PROC-05`.

## Stage 4 - Containment

**Objective:** Reduce the likelihood or impact of further exposure while preserving business continuity and investigative integrity.

Containment is risk-based and authorized. Possible actions include:

- Revoke active sessions and require reauthentication.
- Temporarily restrict access to affected repositories.
- Remove external-sharing links or recipient access.
- Isolate or restrict a managed endpoint through the approved endpoint process.
- Block a destination, application, removable-media action, or risky transfer channel.
- Suspend privileged roles or application grants when implicated.
- Accelerate an approved offboarding access change.
- Preserve content before account or device actions alter evidence.

Containment must record:

- Decision-maker and approving authority
- Reason and supporting evidence
- Exact action and timestamp
- Scope and expected business impact
- Rollback or restoration criteria
- Legal, HR, privacy, and business-owner coordination where required

**Controls and procedures:** `CTRL-03`, `CTRL-05`, `CTRL-07`, `CTRL-11`, `CTRL-15`, `PROC-03`, `PROC-08`.

## Stage 5 - Legal, Privacy, HR, and Business Escalation

**Objective:** Ensure that investigation authority, employee considerations, customer impact, preservation, and notification decisions are handled by the correct stakeholders.

Escalation questions include:

- Does the activity create a legal preservation obligation?
- Is employee monitoring or investigation subject to privacy or labor requirements?
- Is customer, regulator, insurer, law-enforcement, or contractual notification potentially required?
- Does the investigation require expanded custodians, content searches, or review access?
- Who may approve deeper content inspection or evidence export?
- Does the data owner confirm that the transfer lacked an approved business purpose?
- Does an approved exception or contractual process apply?

Technical administrators provide evidence and implement approved actions. They do not independently determine employee intent, legal conclusions, reportability, or regulatory compliance.

## Stage 6 - Investigation

**Objective:** Establish scope, sequence, data exposure, control behavior, and required action while maintaining procedural fairness and evidence integrity.

### Investigation Questions

1. What information was accessed, selected, downloaded, copied, shared, or transmitted?
2. Which classification and retention requirements applied?
3. Did content leave an approved boundary, or was the action blocked?
4. Which identity, device, application, session, and destination were involved?
5. Was the activity isolated or part of a broader pattern?
6. Did an exception, approved workflow, or legitimate customer-service purpose exist?
7. Which preventive and detective controls operated, failed, or lacked coverage?
8. Were privileged access, application permissions, or unmanaged devices involved?
9. What evidence is unavailable, and how does that limit confidence?
10. What containment, preservation, notification, and remediation decisions are authorized?

### Investigation Boundaries

- Apply least privilege to investigators and reviewers.
- Separate security triage from legal conclusions and employment decisions.
- Limit searches to approved purpose, custodians, dates, locations, and data types.
- Audit searches, previews, exports, downloads, role activation, and case access.
- Avoid labeling the activity malicious until authorized review supports that conclusion.
- Record competing explanations and unresolved facts.

## Stage 7 - Recovery

**Objective:** Restore approved business operations and confirm that risk treatment is effective.

Potential recovery activities include:

- Restore access that was temporarily restricted when authorized.
- Complete approved offboarding and ownership transfer.
- Remove unauthorized links, copies, applications, or credentials where possible.
- Validate that affected repositories retain correct ownership and permissions.
- Confirm that required holds remain active before disposition or deletion.
- Tune DLP conditions, thresholds, exclusions, notifications, or enforcement mode.
- Correct device, identity, application, or logging gaps.
- Provide targeted user or administrator guidance.
- Track remediation owners and due dates through governance reporting.

Recovery does not imply that data can always be recalled or that all exposure can be eliminated. Residual risk must be documented and accepted by the authorized owner.

## Stage 8 - Lessons Learned

**Objective:** Convert the scenario into measurable improvements without overstating control effectiveness.

| Review area | Questions |
|---|---|
| Classification | Were the customer records correctly classified as `DATA-03` or `DATA-04`? |
| Ownership | Was an accountable business data owner available to validate permitted use? |
| Identity | Did authentication, Conditional Access, and privilege controls provide sufficient context and restriction? |
| Device | Was the endpoint managed, compliant, monitored, and covered by the required controls? |
| DLP | Did policy scope, conditions, mode, action, alerting, and override behavior match the risk objective? |
| Monitoring | Were audit events complete, timely, correlated, and retained long enough? |
| Investigation | Were roles, purpose, evidence handling, privacy safeguards, and legal authority clear? |
| Response | Were containment actions proportionate, authorized, reversible, and documented? |
| Governance | Did exceptions, offboarding, review frequency, and residual-risk ownership operate as designed? |

Outputs feed `PROC-02` policy tuning, `PROC-07` exception review, `PROC-08` recovery, and `PROC-09` control assurance.

## Tabletop Decision Log

This blank structure is intended for a future exercise. It is not execution evidence.

| Decision point | Available evidence | Options considered | Decision authority | Decision | Follow-up owner |
|---|---|---|---|---|---|
| Severity classification | To be collected | To be evaluated | Security Operations Lead | Pending exercise | Unassigned |
| Evidence preservation | To be collected | To be evaluated | Legal / eDiscovery | Pending exercise | Unassigned |
| Containment | To be collected | To be evaluated | Incident Commander | Pending exercise | Unassigned |
| Employee and privacy escalation | To be collected | To be evaluated | Legal / Privacy / HR | Pending exercise | Unassigned |
| Notification assessment | To be collected | To be evaluated | Authorized Legal and Executive Stakeholders | Pending exercise | Unassigned |
| Recovery and control tuning | To be collected | To be evaluated | Control Owners and Business Data Owner | Pending exercise | Unassigned |

## Target Evidence Package

If this scenario is exercised in a future phase, an evidence package should contain only authorized and sanitized material:

- Scenario scope and approvals
- Alert and telemetry inventory
- Timeline with authoritative sources
- Classification and data-owner decision
- Evidence-preservation record
- Containment and rollback decisions
- Legal, privacy, HR, and business escalation record
- Control gaps and remediation plan
- Residual-risk decision
- Exercise lessons learned

No synthetic alert, fabricated investigation result, or invented performance metric should be presented as implementation evidence.
