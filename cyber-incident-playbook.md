# Cyber Incident Management Playbook
*Draws on SANS LDR553 and CIMTK (the Cyber Incident Management Toolkit); the six-phase structure is my own synthesis, from the Incident Commander's seat. The response team's tasks, the severity scale, the data classes, the reporting clocks and the incident ticket come from an incident response template built from incident response work across enterprise security programs. The two used to be separate documents; they are one here, and where they disagree it is called out in [Where the Two Sources Differ](#where-the-two-sources-differ).*

This is a starting point, not a policy. Adapt the structure, the severity levels and the role names to your organization. The sequence is what matters: the instinct to skip steps under pressure is exactly what makes incidents worse.

---

## Part 1: First-Hour Actions (Detect & Classify to the first exec brief)

Use this section from the moment of notification until your first exec briefing. Goal: move from noise to a defensible, communicable position within ~60 minutes.

The instinct to fully understand an incident before notifying is understandable and wrong. Notify early with incomplete information. Correct as facts emerge.

### 1. Triage & Classify (0-15 min)
**Action:** Confirm the report is real, identify the reporting channel, and assign initial severity.

**Work product: Incident Classification Record**
| Field | Entry |
|:---|:---|
| Time of notification | |
| Source (email, alert, 3rd party, attacker contact) | |
| Initial category (see [incident categories](#incident-categories)) | |
| Initial severity (see [severity levels](#severity-levels)) | |
| Classified by | |
| Confidence in classification | Low / Med / High |

**Decision rule:** classify on *evidence available now*, not on worst-case speculation - but don't downgrade severity just because evidence is incomplete. Attacker-contact + potential data exfiltration defaults to **High**, escalating to **Critical** on confirmed customer/regulated data or confirmed lateral movement. When in doubt, set the level higher. A false positive at the top level costs an hour. A missed one costs much more.

### 2. Establish Command and Notify the Response Team (0-10 min, parallel)
**Action:** Name the Incident Commander (IC), confirm decision authority, open the incident record/channel, and notify the response team.

**Work product: IC Appointment Note** - one line: who is IC, who is deputy, what channel is single source of truth.

Notify the response team as soon as the incident is identified and initially scoped. Do not wait for confirmation of all facts. If it turns out to be a false positive, a quick all-clear is a better outcome than a delayed response to a real one. **For Critical (Sev1), open a bridge within one hour of notification, at any hour. For High (Sev2+), convene within 24 hours.** Who gets notified is in [Reference](#who-gets-notified).

**Use an out-of-band channel.** If email, chat, or the identity provider may be compromised, the attacker may be reading the response. Agree the backup channel (a separate chat workspace, a phone bridge, personal devices) before an incident, and move to it whenever a core collaboration or identity system is in scope.

**The initial notification includes:** the general description of the incident type · whether it is ongoing or stopped · the current severity · the status of system isolation · the status of log and evidence preservation. Be ready to give a full verbal briefing at the first response team meeting.

### 3. Orient & Scope (15-35 min)
**Action:** Pull together what's actually known. Don't investigate - orient.

**Work product: Known Facts / Assumptions / Unknowns (KFAU)**
| Known Facts | Assumptions | Unknowns |
|:---|:---|:---|
| Verified, evidenced items only | Reasonable but unverified beliefs | Explicit open questions driving next tasks |

Questions that fill it in:
- Is it affecting one system or application, part of one, or multiple?
- What equipment or media is missing, if any?
- Do we know what data was on affected systems, and its [classification](#data-classification)?
- When did this begin, or when was it first reported?
- Has the affected portion of the system been isolated?
- Have logs and data from the affected system been preserved?

**Work product: Scope Statement**
- Systems/data believed affected (name them)
- Attack surfaces in play (e.g., internal server, endpoint, backup/lateral path, third-party/hosting)
- Explicitly out of scope for now (and why)
- One-line scope summary suitable for repeating verbatim in the exec brief

Document everything from this point forward. These records support the investigation, any legal action, and the lessons learned review.

### 4. Set Objectives & Commander's Intent (35-45 min)
**Action:** Convert scope into 3-5 prioritized objectives, then compress into intent.

**Work product: Commander's Intent** (2-3 sentences, exec-ready)
> "Our priority is to [contain/verify/protect] a [severity]-severity incident involving [scope], focusing on [top 2-3 priorities], while maintaining [business continuity constraint]."

**Work product: Prioritized Objectives List** - ranked, each phrased as an outcome ("determine if attacker retains access") not a task ("review logs").

### 5. Set Battle Rhythm (45-50 min)
**Action:** Lock the cadence so the team isn't renegotiating meeting times mid-crisis.

**Work product: Battle Rhythm** (adapt times to your org/timezone)
| Time | Activity |
|:---|:---|
| Stand-up | IM/IR sync - carryover + today's priorities |
| Focus block | Uninterrupted forensics/response work - no meetings |
| Collaboration block | Cross-team working session |
| IM<->IR sync | Strategy and findings exchange |
| Exec update prep | IC drafts brief |
| Exec update | Delivered, actions/questions captured |
| Handover | Notes for next shift/tomorrow |

### 6. Anticipate & Prepare the Exec Brief (50-60 min)
**Action:** Pre-empt the four questions execs always ask: *How bad is this? Are we breached? Do we need to tell customers? Are we still operational?*

**Work product: Exec Briefing Notes (3x5 format)** - 3 bullets max per section:
- **Situation** (what happened, current severity)
- **Impact** (business terms: data/downtime/reputation - not logs)
- **Actions in progress**
- **Decisions needed from execs now**
- **Next update time**

**Work product: Action Log** - owner, task, due, status.
**Work product: Decision Log** - decision, who made it, when, rationale (critical for ransom/legal/disclosure calls - these get scrutinized later).

---

## Part 2: Full Six-Phase Incident Management Lifecycle

Each phase gives the IC's command work, then the response team's tasks inside it. Same incident, both seats.

### Phase 1 - Detect & Classify
| | |
|:---|:---|
| **Objective** | Confirm incident is real; assign initial severity and category |
| **IC actions** | Validate source, appoint self/deputy as IC, open incident record, notify the response team |
| **Deliverables** | Incident Classification Record, IC Appointment Note, initial notification |
| **Exit criteria** | Severity assigned, IC named, channel live, response team notified |

**Response team tasks**
- Identify what happened and which [category](#incident-categories) it falls into, and [who noticed it](#who-noticed-it).
- Assign the initial [severity](#severity-levels). It is the best guess under incomplete information, not a commitment; review and adjust as facts emerge.
- Compile everything known before notifying, and send the initial notification.
- If this is a theft or physical loss, notify Facilities as well.

### Phase 2 - Orient & Scope
| | |
|:---|:---|
| **Objective** | Establish what's known vs assumed; bound the problem |
| **IC actions** | Build KFAU, define in/out of scope, identify attack surfaces and linked systems, preserve evidence before containment changes it, bring in legal early so privilege decisions are made up front |
| **Deliverables** | KFAU table, Scope Statement, Systems/Users Impacted tracker (CIMTK grid), incident ticket |
| **Exit criteria** | Scope statement exists and is defensible to execs and legal |

**Response team tasks**
- Work through the assessment questions before committing to a response strategy:
  - Is the incident confirmed, or is this a potential false positive?
  - Is the incident still in progress?
  - What data or property is at risk, and how critical is it?
  - What is the business impact if the attack succeeds - minimal, serious, or critical?
  - Which systems are targeted? Where are they located physically and on the network?
  - Is the incident inside the trusted network?
  - How urgent is the response? Can the incident be quickly contained?
  - Will the response alert the attacker, and does that matter?
  - What type of incident is this - intrusion, malware, abuse, physical theft, denial of service, other?
  - If equipment was lost or stolen, can the data be wiped remotely?
  - Has a report been made - or should one be made - to law enforcement, insurance carriers, or card associations?
- Categorize into the highest applicable level: **Category 1, threat to sensitive data** (Confidential, PII, Company Individual, Public) or **Category 2, threat to systems** (active intrusion, malware or worm, denial of service, unauthorized system use, physical theft or loss).
- **Open the incident ticket.** The ticket is the record of record from this point forward. Minimum fields are in [Reference](#incident-ticket).
- **Ask Legal about privilege.** Legal decides whether outside forensic firms are engaged through counsel, and how findings are documented, so that privilege is preserved where it can be. Settle this before the forensic work starts, not after the report exists.
- Designate a point person to review contracts, insurance policies, and service agreements that may be affected, and one to coordinate with regulators, law enforcement, and card associations if applicable. Decide whether and when to notify insurance carriers and law enforcement.
- **Determine the scope:** confirm the point of entry · determine intent (targeted or opportunistic) · identify every compromised system · identify the files accessed and their classification · check whether the attack differs from the initial assessment · report findings to the response team periodically throughout.
- **Contain the damage, evidence first.** Preserve all logs, reports, and data associated with the affected systems, and image affected portions before any modification. The state of the system at the time of the incident is evidence - protect it before doing anything else. Then isolate the affected portion: disconnect from external sources, take affected services offline, block specific access paths, prevent lateral spread. Determine the points of access used and whether data was extracted from or uploaded to the system, and block further access through those points. Do not skip containment to get to remediation faster; remediating an uncontained incident is a losing exercise. See [where the two sources differ](#where-the-two-sources-differ) on when containment happens.
- **For lost or stolen equipment:** determine what data was on it, remotely wipe if possible, document or recreate what was on it, and notify law enforcement.
- Obtain copies of all reports and investigative documents from any involved vendors or service providers, and of the insurance contracts covering them.

### Phase 3 - Plan & Direct
| | |
|:---|:---|
| **Objective** | Convert scope into objectives and direction; stand up the team |
| **IC actions** | Set objectives, write Commander's Intent, set Battle Rhythm, assign workstreams (technical, legal, comms, HR) |
| **Deliverables** | Commander's Intent, Prioritized Objectives, Battle Rhythm, PIR (Priority Intelligence Requirements) list |
| **Exit criteria** | Team knows what to do without micromanagement |

**Response team tasks**
- Pick the response procedure for the incident type: active intrusion, inactive intrusion, malware or worm, system failure, system abuse, property theft, denial of service (web or database). If no applicable procedure exists, document what was done and establish a procedure after the fact.
- Use forensic techniques to determine how the incident occurred: review system logs, look for log gaps, review intrusion detection logs, interview witnesses. Only authorized personnel should conduct interviews or examine evidence.

### Phase 4 - Communicate
| | |
|:---|:---|
| **Objective** | Control the narrative internally and externally before others control it for you |
| **IC actions** | Brief execs on cadence, coordinate legal/comms/PR, prepare disclosure-scenario messaging |
| **Deliverables** | Exec Briefing Notes, Press Statement Tracker, Email Comms Tracker, LE Briefing form (if law enforcement engaged), Notification Clocks log |
| **Exit criteria** | Holding statement ready; internal and external messaging aligned; no unapproved statements have gone out |

**Notification clocks:** *Do we need to tell customers?* is often decided by a clock that is already running, and several start at awareness or at a determination, not at confirmation. Legal owns the determination; the IC makes sure the clocks are known on day one. Log each one in the Decision Log: which regime applies, who decides, when the clock started, when it expires. The common ones are in [Regulatory Reporting Clocks](#regulatory-reporting-clocks).

**Key discipline:** say enough to be credible, not everything you don't yet know. Avoid absolutes ("no risk," "fully secure," "nothing to worry about") - they age badly and are the first thing quoted back at you.

**Response team tasks**
- **Report to the response team first.** Review findings and the status of remediation before any broader communication goes out.
- **Get the right review before anything is sent:**

| Audience | Review Required Before Sending |
|:---|:---|
| Broader internal group beyond the response team | Legal review |
| Employees | HR and Legal review |
| External non-company audience | Legal review |
| General public or customer community | Legal and Communications review |

- **Keep the investigation confidential within the response team.** Discussion outside that group - especially if the incident may have been internally generated - can alert the attacker, compromise the investigation, or create legal exposure.
- **If personally identifiable information (PII) was compromised:**
  - Determine which individuals were affected, and assemble their contact information.
  - Determine notification requirements under applicable state, federal, and international law. The GDPR 72-hour clock starts when the organization becomes aware of the breach, not at confirmation of full scope and not at resolution.
  - Determine whether notice to credit reporting agencies, law enforcement, or card associations is required.
  - Determine whether you have contact information for everyone affected; if not, what substitute notification options are available.
  - Consider a dedicated hotline for affected customers with a prepared script, and credit monitoring assistance.
  - Legal prepares all formal notices.

### Phase 5 - Counter-Compromise & Remediate
| | |
|:---|:---|
| **Objective** | Evict the attacker, close attack paths, remediate affected data/systems |
| **IC actions** | Direct containment/eradication, track systems/users impacted, track data remediation, validate no persistence remains |
| **Deliverables** | Counter-Compromise Activities tracker, CC Systems and Users Impacted tracker, CC Data Remediation tracker, Attacker Intelligence tracker |
| **Exit criteria** | Attacker access confirmed removed; remediation actions closed or scheduled |

**Response team tasks**
- Before touching anything, confirm the affected portion is isolated and evidence is preserved.
- **Hunt for what the attacker left:** unauthorized hardware attached to the network · unauthorized entries in privileged groups · security assessment or exploitation software · unauthorized processes set to run at startup · gaps or deletions in system logs · in intrusion detection logs, the timeline, method and scope · in system logs, unusual connections, authentication failures, activity outside normal hours, permission changes, elevated privileges · differences from baseline file and system integrity checks · sensitive data moved or staged for later retrieval.
- Recommend changes to prevent recurrence, and implement them on approval.
- **Restore affected systems to a clean state:** re-install from scratch and restore from backups if necessary, evidence first · force password changes if credentials may have been compromised · harden the system by disabling unused services · apply all outstanding patches · confirm real-time monitoring and intrusion detection are running · confirm the system logs the correct events at the correct level · test security before returning the system to production.
- If the security review called for it, run a penetration test before the service goes back into production.
- **Preserve evidence throughout:** copy logs, email and all documentable communication · image affected systems before any modification · keep a list of witnesses · retain evidence as long as prosecution is possible, and beyond in case of appeal.

### Phase 6 - Sustain & Exercise
| | |
|:---|:---|
| **Objective** | Return to normal operations; capture lessons; harden for next time |
| **IC actions** | Run Post-Incident Review, update playbooks, schedule follow-up exercises |
| **Deliverables** | Post-Incident Review report, updated Battle Rhythm/playbook, Exercise Roadmap |
| **Exit criteria** | Post-Incident Review complete, actions assigned, next exercise scheduled |

The follow-up phase is the one most organizations skip or rush. It is also the one that determines whether the next incident goes better or worse.

**Response team tasks**
- **Meet** after the investigation and communication are complete to review whether all appropriate response steps were taken, whether the investigation and reporting were adequate, what more is needed to prevent recurrence, and the final response with senior management.
- **Document, before memory fades:** how the incident was discovered · its category · how it occurred (entry point, method, timeline) · where the attack originated (IP addresses, source, attribution if available) · what the response plan was · what was actually done · whether the response was effective.
- **Assess the cost**, direct and indirect: labor for investigation, reinstallation and data recovery · downtime (lost productivity, lost revenue, hardware replacement) · legal costs · repairing physical security · reputational and customer trust damage · loss of competitive position from release of proprietary information.
- **Work through the lessons learned questions before closing:** Could an additional policy have prevented this? Was a procedure not followed, and what would ensure it is followed in future? Was the response appropriate, and how could it be improved? Were all appropriate parties informed in time? Were the procedures detailed enough for the situation? Have changes been made to prevent a similar incident? Should security policies or insurance policies be updated? What should change about this plan?
- **Update the plan.** Review the response end to end, find what worked and what did not, and update the plan before the next incident. The value of this exercise degrades the longer you wait.
- **Test it.** Between incidents, run a tabletop with the [Incident Tabletop Exercise Kit](incident-tabletop-exercise-kit.md). After an incident, have an outside security firm test the system for intrusion and other vulnerabilities, unannounced. Testing the technical security and the operation of this plan at the same time is more efficient than treating them as separate exercises.

---

## Where the Two Sources Differ

The SANS-based command structure and the enterprise response template agree on almost everything. Where they do not, both views are kept here so you can choose deliberately.

| Topic | SANS / Incident Commander view | Enterprise response template view | What to do |
|:---|:---|:---|:---|
| **Severity scale** | Low / Medium / High / Critical, driven by evidence and data impact: attacker contact plus possible exfiltration defaults to High, confirmed customer or regulated data or lateral movement makes it Critical | Sev3 / Sev2 / Sev2+ / Sev1, driven by blast radius: a sub-component, one system, one system that could spread, all systems | They map roughly Low = Sev3, Medium = Sev2, High = Sev2+, Critical = Sev1, but they measure different things. Pick one scale for your organization. When the two would give different answers, use the higher |
| **When containment happens** | Evidence is preserved in Orient & Scope; containment and eradication are directed together in Counter-Compromise & Remediate | Containment sits inside the data-exposure investigation, before remediation starts: "do not skip containment to get to remediation faster" | Both agree on evidence first. Isolate as soon as evidence is preserved, during scoping, if waiting would let the damage spread; leave eviction and eradication for Phase 5. Weigh whether containing will alert the attacker |
| **Who leads** | An Incident Commander commands: priorities, decisions, communications | The response team (the CSIRT, Computer Security Incident Response Team) "takes lead" on the investigation | Name both. The IC owns the command decisions and the exec line; the response team lead owns the technical investigation |
| **The record** | The incident record or channel is the single source of truth | The incident ticket is the record of record from the moment the team takes lead | Use the ticket as the record and the channel as the live working space, and say so in the IC Appointment Note |
| **Phase names** | Detect & Classify · Orient & Scope · Plan & Direct · Communicate · Counter-Compromise & Remediate · Sustain & Exercise | Identification & initial level setting · Team notification · Team takes lead · Data exposure investigated · Fix and communicate · Follow-up | This playbook uses the SANS names. The template's steps are folded into them: identification and notification into Phase 1, taking the lead and the investigation into Phase 2, fix into Phase 5, communicate into Phase 4, follow-up into Phase 6 |

---

## Reference

### Severity Levels

Set severity at identification. Review and adjust as facts emerge - the initial level is your best guess under incomplete information, not a commitment.

| Level | Scope | Example | Convene |
|:---|:---|:---|:---|
| **Critical / Sev1** | All systems or applications affected, or potential for broad impact | Active intrusion across multiple systems, confirmed PII breach at scale | Bridge within 1 hour, any hour |
| **High / Sev2+** | Contained to one system or application but with potential to spread | Confirmed unauthorized access to one service with adjacent risk | Within 24 hours |
| **Medium / Sev2** | Contained to one system or application | Isolated unauthorized access, single system compromise | |
| **Low / Sev3** | Contained to a portion or sub-component of one system | Minor anomaly, no confirmed data access, no spread risk | |

### Incident Categories

- Equipment loss or theft (hardware, backup media, portable devices)
- Exposure or loss of employee or customer PII
- Exposure or loss of private or confidential company data
- Attempted or successful unauthorized access to systems or data
- Unauthorized use of systems for processing or storage
- Unwanted disruption or denial of service
- Changes to system hardware, firmware, or software without authorization
- Insertion of malware or other malicious code
- Ransomware or data extortion (encryption, theft with a threat to publish, or both)
- Third-party SaaS compromise, including stolen OAuth tokens or integration credentials used to reach your data through a vendor
- Identity or cloud control plane compromise (identity provider, SSO, cloud admin accounts, CI/CD)
- Business email compromise or impersonation, including deepfake voice or video used to request payments or access
- AI system incident: prompt injection, sensitive data leaking through an assistant, or an AI agent taking actions it should not
- Insider
- Other - document and proceed

### Who Noticed It

Customer support or operations personnel · a customer report · automated intrusion detection or monitoring · a system or network administrator · a security team member · a business partner · an external source (researcher, law enforcement, media).

### Data Classification

Know what data is at risk before you decide how to respond and who to notify. The most sensitive category involved determines the response level. When uncertain, assume the higher classification.

- **Public** - Information that is already a matter of public record or knowledge. Exposure has no legal or regulatory consequence.
- **Personally Identifiable Information (PII)** - Name combined with any of: Social Security Number, government ID number, financial account number, health or medical information. Also a username or email address combined with a password or security question and answer that would permit access to an online account. Under California's breach law (Cal. Civ. Code 1798.82(h)(2)) that alone is notifiable personal information; check other states with Legal. Exposure triggers legal notification obligations in most jurisdictions. Legal review required immediately.
- **Company Individual Information** - Information whose exposure, regardless of law or regulation, would damage the organization's reputation or operations: email addresses on their own, internal contact information, employee records.
- **Confidential** - Information not to be released outside the company: financial data, proprietary data, trade secrets, intellectual property, strategic plans.

### Who Gets Notified

Adapt this list to your organization. At minimum:

| Function | Role in Response |
|:---|:---|
| Information Security / CSIRT | Leads technical investigation |
| Legal | Determines notification obligations, reviews all external communications |
| HR | Involved when employees are affected or implicated |
| Communications / PR | Owns external messaging when required |
| Executive sponsor | Informed for Critical and High (Sev1 and Sev2+) events |
| Risk Management | Reviews insurance and contract implications |

### Regulatory Reporting Clocks

Legal owns the determination. The IC's job is to make sure the clocks are known on day one, because several start at awareness or at a determination, not at confirmation. Checked against the linked sources in September 2026:

| Regime | Who it covers | Clock | Source |
|:---|:---|:---|:---|
| SEC Form 8-K Item 1.05 | US public companies | Generally 4 business days after determining an incident is material. Name who makes the materiality call | [SEC 2023-139](https://www.sec.gov/newsroom/press-releases/2023-139) |
| GDPR Art. 33 | Controllers of EU personal data | Supervisory authority within 72 hours of becoming aware, where feasible | [GDPR Art. 33](https://gdpr-info.eu/art-33-gdpr/) |
| NIS2 | EU essential and important entities | Early warning 24 hours from becoming aware; notification 72 hours; final report one month later | [Commission NIS2 FAQ](https://digital-strategy.ec.europa.eu/en/faqs/directive-measures-high-common-level-cybersecurity-across-union-nis2-directive-faqs) |
| DORA | EU financial entities | Initial notification 4 hours after classifying as major and no later than 24 hours after becoming aware of the incident; intermediate 72 hours; final one month | [EBA](https://www.eba.europa.eu/activities/single-rulebook/regulatory-activities/operational-resilience/joint-technical-standards-major-incident-reporting) |
| EU Cyber Resilience Act | Manufacturers of digital products sold in the EU, from September 11, 2026 | Actively exploited vulnerability or severe incident: early warning 24 hours; notification 72 hours; final report 14 days after a fix (vulnerability) or one month (incident) | [Commission CRA reporting](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting) |
| HIPAA Breach Notification | Covered entities and business associates | Individuals without unreasonable delay, no later than 60 days after discovery. HHS at the same time for 500 or more; annually for fewer than 500. Prominent media outlets on the same 60-day clock when more than 500 residents of one state or jurisdiction are affected | [45 CFR 164.404](https://www.law.cornell.edu/cfr/text/45/164.404), [164.406](https://www.law.cornell.edu/cfr/text/45/164.406), [164.408](https://www.law.cornell.edu/cfr/text/45/164.408) |
| NYDFS 23 NYCRR 500.17 | NYDFS-regulated entities | 72 hours after determining a cybersecurity incident occurred; extortion payment notice within 24 hours, with a written explanation within 30 days | [23 NYCRR 500.17](https://www.law.cornell.edu/regulations/new-york/23-NYCRR-500.17) |
| CIRCIA | US critical infrastructure | Not yet in effect: the final rule is still pending. The statute sets 72 hours for covered incidents and 24 hours for ransom payments once it is | [CISA CIRCIA](https://www.cisa.gov/topics/cyber-threats-and-advisories/information-sharing/cyber-incident-reporting-critical-infrastructure-act-2022-circia) |

US state breach laws, contracts, and card brand rules add their own clocks. This table is a starting list, not the full set.

### How This Maps to NIST

[NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final) (April 2025) superseded Rev. 2 and recasts incident response as a Community Profile of the NIST Cybersecurity Framework (CSF) 2.0. This playbook covers the response itself. Preparation lives in Govern, Identify, and Protect, and belongs in your program, not in the middle of an incident.

| Phase in this playbook | CSF 2.0 function |
|:---|:---|
| 1. Detect & Classify | Detect |
| 2. Orient & Scope (including containment) | Respond |
| 3. Plan & Direct | Respond |
| 4. Communicate | Respond |
| 5. Counter-Compromise & Remediate | Respond, then Recover |
| 6. Sustain & Exercise | Recover, and lessons learned feed back into preparation |

### Incident Ticket

Every incident has a ticket from Phase 2 onward. It extends the Incident Classification Record from the first hour. Minimum fields:

| Field | Entry |
|:---|:---|
| Incident ID | |
| Date/Time Opened | |
| Reported By | |
| Severity Level | |
| Incident Type | |
| Data Classification | |
| Systems Affected | |
| Incident Status | Open / Contained / Resolved |
| IC and Response Team Lead | |
| Legal Contact | |
| Date/Time Contained | |
| Date/Time Resolved | |
| Date/Time Closed | |
| Brief Description | |

### Terminology
- **IC** - Incident Commander: coordinates response, priorities, decisions, communications
- **CSIRT** - Computer Security Incident Response Team: the response team that leads the technical investigation
- **KFAU** - Known Facts / Assumptions / Unknowns
- **PIR** - Priority Intelligence Requirements: the questions the response most needs answered. Not the Post-Incident Review, which this playbook spells out in full
- **SITREP** - Situation Report: what happened, impact, actions, risks, next steps
- **Battle Rhythm** - scheduled cadence of meetings/updates/decisions during the incident
- **Decision Authority** - who is empowered to approve ransom, disclosure, shutdown decisions
- **Single Source of Truth** - the one channel/doc treated as authoritative incident record

### Standing Decision Rules
- **Notify early, in both directions.** Notify the response team with what you know. Notify external parties - customers, regulators - on the timeline required by law, not when you feel ready.
- **Treat a suspected incident as real until you can confirm otherwise.** A false positive that was treated seriously is a training exercise. A real incident treated as a false positive is a crisis.
- **Image first. Fix second. Always.** The single most common mistake in incident response is modifying systems before preserving evidence.
- **Legal is in the room from the start if PII is in scope.** The GDPR clock starts at awareness, not when every detail is confirmed and not at resolution.
- Don't rush to pay ransom/extortion demands - payment doesn't guarantee an outcome or deletion. Check sanctions exposure and involve legal and law enforcement before any payment discussion (US: [OFAC ransomware advisory, September 2021](https://ofac.treasury.gov/media/912981/download))
- Default to consequence-based decisions: frame every technical fact in terms of data loss, downtime, or reputational exposure before it reaches execs
- Every ransom/legal/disclosure decision gets logged with rationale - assume it will be reviewed later
- Scope statements and exec briefs should be conservative and defensible, not comprehensive - you'll need to walk back overclaims, rarely underclaims

### Worked Example

[Sample Security Incident Escalation](examples/sample-incident-escalation.md): a weak first escalation next to a complete one, and why timing matters more than completeness.

---

*Last reviewed: September 2026. Verify notification requirements against current law - GDPR, NIS2, DORA, the EU CRA, SEC rules, HIPAA, NYDFS, CCPA, state breach notification statutes, and card association requirements change, and CIRCIA is not yet final. This is a framework, not legal advice. Maintained by [Eric White](https://github.com/ChefPlex) | [ChefPlex](https://github.com/ChefPlex)*
