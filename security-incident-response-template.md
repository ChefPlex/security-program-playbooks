# Security Incident Response Template

A framework for responding to technology security incidents. Six phases, in order. Each phase has a clear purpose, specific tasks, and a defined handoff to the next phase.

This document is a starting point, not a policy. Adapt the structure, the severity levels, and the role names to match your organization. The sequence is what matters - the instinct to skip phases under pressure is exactly what makes incidents worse.

Written for TPMs and incident leads who need to organize a response fast, communicate clearly under ambiguity, and make sure nothing important falls through.

---

## The Six Phases

1. Incident Identification and Initial Level Setting
2. Incident Response Team Notification
3. Incident Response Team Takes Lead
4. Data Exposure Investigated and Identified
5. Fix the Issue and Communicate
6. Follow-Up

The pages that follow detail each phase. The visual overview is in Appendix A.

---

## Severity Levels

Set severity at identification. Review and adjust as facts emerge - the initial level is your best guess under incomplete information, not a commitment.

| Level | Scope | Example |
|-------|-------|---------|
| **Sev1 / Critical** | All systems or applications affected, or potential for broad impact | Active intrusion across multiple systems, confirmed PII breach at scale |
| **Sev2+ / High** | Contained to one system or application but with potential to spread | Confirmed unauthorized access to one service with adjacent risk |
| **Sev2 / Medium** | Contained to one system or application | Isolated unauthorized access, single system compromise |
| **Sev3 / Low** | Contained to a portion or sub-component of one system | Minor anomaly, no confirmed data access, no spread risk |

When in doubt, set the level higher. A false positive at Sev1 costs an hour. A missed Sev1 costs much more.

---

## Data Classification

Know what data is at risk before you decide how to respond and who to notify.

**Public**
Information that is already a matter of public record or knowledge. Exposure has no legal or regulatory consequence.

**Private Individual Information (PII)**
Name combined with any of: Social Security Number, government ID number, financial account number, health or medical information. Exposure triggers legal notification obligations in most jurisdictions. Legal review required immediately.

**Company Individual Information**
Information whose exposure - regardless of law or regulation - would damage the organization's reputation or operations. Log-in credentials, email addresses, internal contact information, employee records.

**Confidential**
Information not to be released outside the company: financial data, proprietary data, trade secrets, intellectual property, strategic plans.

The most sensitive category of data involved determines the response level. When uncertain, assume the higher classification.

---

## Phase 1: Incident Identification and Initial Level Setting

**Purpose:** Confirm something happened, characterize it well enough to start the response, and notify the right people fast.

The instinct to fully understand an incident before notifying is understandable and wrong. Notify early with incomplete information. Correct as facts emerge.

### What Happened

Identify which category of incident this is:

- Equipment loss or theft (hardware, backup media, portable devices)
- Exposure or loss of employee or customer PII
- Exposure or loss of private or confidential company data
- Attempted or successful unauthorized access to systems or data
- Unauthorized use of systems for processing or storage
- Unwanted disruption or denial of service
- Changes to system hardware, firmware, or software without authorization
- Insertion of malware or other malicious code
- Other - document and proceed

### Who Noticed It

- Customer support or operations personnel
- Customer report
- Automated intrusion detection or monitoring
- System or network administrator
- Security team member
- Business partner
- External source (researcher, law enforcement, media)

### Initial Level Setting Tasks

1.1 Identify what happened and which category it falls into

1.2 Assess scope using these questions:
- Is it affecting one system or application, part of one, or multiple?
- What equipment or media is missing, if any?
- Do we know what data was on affected systems?
- When did this begin, or when was it first reported?
- Has the affected portion of the system been isolated?
- Have logs and data from the affected system been preserved?

1.3 Assign the initial severity level

1.4 Document everything from this point forward. These records support the investigation, any legal action, and the lessons learned review.

---

## Phase 2: Incident Response Team Notification

**Purpose:** Get the right people informed and moving. Speed matters here more than completeness.

Notify the Incident Response Team as soon as an incident is identified and initially scoped. Do not wait for confirmation of all facts. If the incident turns out to be a false positive, a quick all-clear is a better outcome than a delayed response to a real one.

### Who Gets Notified

Adapt this list to your organization. At minimum:

| Function | Role in Response |
|----------|----------------|
| Information Security / CSIRT | Leads technical investigation |
| Legal | Determines notification obligations, reviews all external communications |
| HR | Involved when employees are affected or implicated |
| Communications / PR | Owns external messaging when required |
| Executive sponsor | Informed for Sev1 and Sev2+ events |
| Risk Management | Reviews insurance and contract implications |

For Sev1 and Sev2+ incidents, schedule a call or in-person meeting within 24 hours of notification.

### Notification Tasks

2.1 Compile all current information from Phase 1 before notifying

2.2 If this is a theft or physical loss incident, notify Facilities as well

2.3 Send initial notification to the incident response team. Include:
- General description of the incident type
- Whether the incident is ongoing or stopped
- Current severity level assessment
- Status of system isolation
- Status of log and evidence preservation

2.4 Be prepared to give a full verbal briefing at the first response team meeting

2.5 The response team will convene - in person or by call - as soon as possible after notification

2.6 Designate a point person to review contracts, insurance policies, and service agreements that may be affected

2.7 Designate a point person to coordinate with regulators, law enforcement, and card associations if applicable

2.8 Determine whether and when to notify insurance carriers and law enforcement

---

## Phase 3: Incident Response Team Takes Lead

**Purpose:** Determine the true nature and severity of the incident and establish the response strategy.

*Note: The Incident Response Team (IRT) is the functional equivalent of what NIST SP 800-61 and CERT standards call the CSIRT - Computer Security Incident Response Team.*

### Severity Assessment Questions

Work through these before committing to a response strategy:

3.1 Is the incident confirmed, or is this a potential false positive?

3.2 Is the incident still in progress?

3.3 What data or property is at risk, and how critical is it?

3.4 What is the business impact if the attack succeeds - minimal, serious, or critical?

3.5 Which systems are targeted? Where are they located physically and on the network?

3.6 Is the incident inside the trusted network?

3.7 How urgent is the response?

3.8 Can the incident be quickly contained?

3.9 Will the response alert the attacker, and does that matter?

3.10 What type of incident is this - intrusion, malware, abuse, physical theft, denial of service, other?

3.11 If equipment was lost or stolen, can the data be wiped remotely?

3.12 Has a report been made - or should one be made - to law enforcement, insurance carriers, or card associations?

### Incident Categorization

Categorize into the highest applicable level:

**Category 1 - Threat to sensitive data**
- Confidential
- Private individual (PII)
- Company individual information
- Public

**Category 2 - Threat to systems**
- Active intrusion
- Malware or worm
- Denial of service
- Unauthorized system use
- Physical theft or loss

Create an incident ticket. The ticket is the record of record from this point forward.

---

## Phase 4: Data Exposure Investigated and Identified

**Purpose:** Understand exactly what happened, what was accessed or affected, and establish the full scope before beginning remediation.

Do not skip containment to get to remediation faster. Remediating an uncontained incident is a losing exercise.

### Containing the Damage

4.1 Isolate the affected portion of the system. This may include:
- Disconnecting from external sources
- Taking affected services offline
- Blocking specific access paths
- Preventing lateral spread to adjacent systems

4.2 Preserve all logs, reports, and data associated with the affected systems. Image affected portions before any modification. The state of the system at the time of the incident is evidence - protect it before doing anything else.

4.3 Determine the points of access used

4.4 Determine whether data was extracted from, or uploaded to, the affected system

4.5 Block further access via those points

4.6 For lost or stolen equipment:
- Determine what data was on the equipment
- Remotely wipe data if possible
- Document or recreate what was on the equipment
- Notify appropriate law enforcement

4.7 Obtain copies of all reports and investigative documents from any involved vendors or service providers

4.8 Obtain copies of all insurance contracts covering involved vendors

### Determining Scope

Work through these to understand what actually happened:

- Confirm the attack point of entry
- Determine intent - was this targeted or opportunistic?
- Identify all systems that were compromised
- Identify files that were accessed and their sensitivity classification
- Assess whether the nature of the attack differs from the initial assessment
- Periodically report findings to the response team throughout the investigation

---

## Phase 5: Fix the Issue and Communicate

**Purpose:** Remediate affected systems, restore security, and communicate appropriately to internal and external stakeholders.

### Restoring the System

Before touching anything, confirm the affected portion is isolated and evidence is preserved.

During remediation, the IRT should:

- Determine whether unauthorized hardware has been attached to the network
- Examine privileged groups for unauthorized entries
- Search for security assessment or exploitation software left by the attacker
- Look for unauthorized processes set to run at startup
- Search for gaps or deletions in system logs
- Review intrusion detection logs for timeline, attack method, and scope
- Examine logs for: unusual connections, authentication failures, activity outside normal hours, permission changes, elevated privileges
- Compare systems to baseline file and system integrity checks
- Search for sensitive data that may have been moved or staged for later retrieval

### Remediation Tasks

5.1 Follow the applicable response procedure for the incident type:
- Active intrusion
- Inactive intrusion
- Malware or worm
- System failure
- System abuse
- Property theft
- Denial of service (web or database)

If no applicable procedure exists, document what was done and establish a procedure after the fact.

5.2 Use forensic techniques to determine how the incident occurred: review system logs, look for log gaps, review intrusion detection logs, interview witnesses. Only authorized personnel should conduct interviews or examine evidence.

5.3 Recommend changes to prevent recurrence

5.4 Implement changes upon approval

5.5 Restore affected systems to a clean state:
- Re-install from scratch and restore from backups if necessary. Preserve evidence first.
- Force password changes if credentials may have been compromised
- Harden the system by disabling unused services
- Apply all outstanding patches
- Confirm real-time security monitoring and intrusion detection is running
- Confirm the system is logging the correct events at the correct level
- Test security before returning the system to production

5.6 Evidence preservation - throughout the entire process:
- Copy logs, email, and all documentable communication
- Image affected systems before any modification
- Maintain a list of witnesses
- Retain evidence as long as prosecution is possible, and beyond in case of appeal

### Reporting and Communication

**Report to the response team first.** Schedule a meeting to review findings and the status of remediation before any broader communication goes out.

**Communication decision tree:**

| Audience | Review Required Before Sending |
|----------|-------------------------------|
| Broader internal group beyond response team | Legal review |
| Employees | HR and Legal review |
| External non-company audience | Legal review |
| General public or customer community | Legal and Communications review |

**If Private Individual Information (PII) was compromised:**

5.2.1 Determine which individuals were affected

5.2.2 Assemble contact information for affected individuals

5.2.3 Determine notification requirements under applicable state, federal, and international law (GDPR 72-hour notification clock starts at confirmed breach, not at resolution)

5.2.4 Determine whether notice to credit reporting agencies, law enforcement, or card associations is required

5.2.5 Determine whether you have sufficient contact information to notify all affected individuals; if not, determine what substituted notification options are available

5.2.6 Consider establishing a dedicated hotline for affected customers with a prepared script

5.2.7 Consider offering credit monitoring assistance

5.2.8 Legal prepares all formal notices

---

## Phase 6: Follow-Up

**Purpose:** Close the loop, document the full response, assess costs, and make sure the incident teaches the organization something useful.

The follow-up phase is the one most organizations skip or rush. It is also the one that determines whether the next incident goes better or worse.

### Meetings

The response team should convene after the investigation and communication phases are complete to review:
- Whether all appropriate response steps were taken
- Whether the investigation and reporting were adequate
- What additional steps are needed to prevent recurrence
- Review of the final response with senior management

### Documentation

Document the following before memory fades:

6.1 How the incident was discovered

6.2 Category of the incident

6.3 How the incident occurred - entry point, method, timeline

6.4 Where the attack originated - IP addresses, source, attribution if available

6.5 What the response plan was

6.6 What was actually done in response

6.7 Whether the response was effective

6.8 Cost assessment - include both direct and indirect costs:
- Labor costs for investigation, reinstallation, data recovery
- System downtime costs: lost productivity, lost revenue, hardware replacement
- Legal costs
- Costs of repairing physical security measures
- Reputational and customer trust damage
- Loss of competitive position from release of proprietary information

### Lessons Learned

Work through these questions before closing the incident:

- Could an additional policy have prevented this?
- Was a procedure not followed that enabled the incident? What would ensure it is followed in the future?
- Was the incident response appropriate? How could it be improved?
- Were all appropriate parties informed in a timely manner?
- Were the response procedures sufficiently detailed to cover the situation?
- Have changes been made to prevent re-infection or a similar incident?
- Should any security policies be updated?
- Should insurance policies be updated?
- What should change about this plan?

### Policy and Plan Updates

After documentation is complete, review the response process end-to-end. Find what worked, find what did not, and update the plan before the next incident. The value of this exercise degrades the longer you wait.

### Periodic Security Testing

After an incident, engage an outside security firm to test the system for intrusion or other vulnerabilities. Do this on an unannounced basis. Testing both the technical security and the operation of this response plan at the same time is more efficient than treating them as separate exercises.

---

## Appendix A: Visual Task Flow

```
Security Incident Identified
         ↓
Initial Severity Level Set by Person Reporting
  Sev1 - All systems / broad impact
  Sev2+ - One system, potential to spread
  Sev2 - One system, contained
  Sev3 - Sub-component of one system
         ↓
Severity Level Reviewed and Confirmed
         ↓
Incident Response Team Notified
(Sev1 and Sev2+: call or meeting within 24 hours)
         ↓
IRT Takes Lead on Investigation
         ↓
Data Type and Exposure Identified ──────────────────────────────┐
  Public                                                         │
  Private Individual (PII)                                       │
  Company Individual                                            IRT Reports to Response Team
  Confidential                                                   │
         ↓                                                       │
IRT Reports to Response Team ←──────────────────────────────────┘
         ↓
Additional Notification Required?
  ├── Internal only → Legal review → Message sent
  ├── External → Legal + Communications review → Message sent
  └── No notification required
         ↓                              ↓
Appropriate Engineering /        Code/System Rollback
Operations Teams Notified        Possible or Necessary?
                                         ↓
                                 Incident Resolved?
                                  ├── Yes → Continue
                                  └── No → Scope problem
                                           Define resources and timeline
                                           Resolve issue
                                           QA and rollout
                                           Incident resolved?
                                                  ↓ Yes
         ↓
Review and Document Incident and Response for Lessons Learned
```

---

## Appendix B: Incident Ticket Template

Every incident should have a ticket from Phase 3 onward. Minimum fields:

| Field | Entry |
|-------|-------|
| Incident ID | |
| Date/Time Opened | |
| Reported By | |
| Severity Level | |
| Incident Type | |
| Data Classification | |
| Systems Affected | |
| Incident Status | Open / Contained / Resolved |
| IRT Lead | |
| Legal Contact | |
| Date/Time Contained | |
| Date/Time Resolved | |
| Date/Time Closed | |
| Brief Description | |

---

## Notes on This Template

**On notification timing:** The instinct to wait until you have full information before notifying is wrong in both directions. Notify the response team early with what you know. Notify external parties - customers, regulators - on the timeline required by law, not when you feel ready.

**On GDPR:** If any affected individuals are EU residents, the 72-hour notification clock to the relevant supervisory authority starts at the point of confirmed breach, not at resolution. Legal needs to be in the room from Phase 2 if PII is in scope.

**On false positives:** Act on a suspected incident as if it is real until you can confirm otherwise. A false positive that was treated seriously is a training exercise. A real incident treated as a false positive is a crisis.

**On evidence:** The single most common mistake in incident response is modifying systems before preserving evidence. Image first. Fix second. Always.

**On communication during the incident:** Keep the investigation confidential within the response team. Discussion outside that group - especially if the incident may have been internally generated - can alert the attacker, compromise the investigation, or create legal exposure.

---

*Template version 1.0. Built from incident response work across enterprise security programs. Maintained by [Eric White](https://github.com/ChefPlex) | [ChefPlex](https://github.com/ChefPlex)*

*Last reviewed: May 2026. Verify notification requirements against current law - GDPR, CCPA, state breach notification statutes, and card association requirements change. This is a framework, not legal advice.*
