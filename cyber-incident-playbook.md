# Cyber Incident Management Playbook
*Draws on SANS LDR553 and CIMTK (the Cyber Incident Management Toolkit); the six-phase structure is my own synthesis. Incident Commander perspective.*

---

## Part 1: First-Hour Actions (Detect & Classify to the first exec brief)

Use this section from the moment of notification until your first exec briefing. Goal: move from noise to a defensible, communicable position within ~60 minutes.

### 1. Triage & Classify (0-15 min)
**Action:** Confirm the report is real, identify the reporting channel, and assign initial severity.

**Work product: Incident Classification Record**
| Field | Entry |
|---|---|
| Time of notification | |
| Source (email, alert, 3rd party, attacker contact) | |
| Initial category (malware / extortion / BEC / DDoS / insider / unknown) | |
| Initial severity (Low/Med/High/Critical) | |
| Classified by | |
| Confidence in classification | Low / Med / High |

**Decision rule:** classify on *evidence available now*, not on worst-case speculation - but don't downgrade severity just because evidence is incomplete. Attacker-contact + potential data exfiltration defaults to **High**, escalating to **Critical** on confirmed customer/regulated data or confirmed lateral movement.

### 2. Establish Command (0-10 min, parallel)
**Action:** Name the Incident Commander (IC), confirm decision authority, and open the incident record/channel.

**Work product: IC Appointment Note** - one line: who is IC, who is deputy, what channel is single source of truth.

### 3. Orient & Scope (15-35 min)
**Action:** Pull together what's actually known. Don't investigate - orient.

**Work product: Known Facts / Assumptions / Unknowns (KFAU)**
| Known Facts | Assumptions | Unknowns |
|---|---|---|
| Verified, evidenced items only | Reasonable but unverified beliefs | Explicit open questions driving next tasks |

**Work product: Scope Statement**
- Systems/data believed affected (name them)
- Attack surfaces in play (e.g., internal server, endpoint, backup/lateral path, third-party/hosting)
- Explicitly out of scope for now (and why)
- One-line scope summary suitable for repeating verbatim in the exec brief

### 4. Set Objectives & Commander's Intent (35-45 min)
**Action:** Convert scope into 3-5 prioritized objectives, then compress into intent.

**Work product: Commander's Intent** (2-3 sentences, exec-ready)
> "Our priority is to [contain/verify/protect] a [severity]-severity incident involving [scope], focusing on [top 2-3 priorities], while maintaining [business continuity constraint]."

**Work product: Prioritized Objectives List** - ranked, each phrased as an outcome ("determine if attacker retains access") not a task ("review logs").

### 5. Set Battle Rhythm (45-50 min)
**Action:** Lock the cadence so the team isn't renegotiating meeting times mid-crisis.

**Work product: Battle Rhythm** (adapt times to your org/timezone)
| Time | Activity |
|---|---|
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

These six phases are the Incident Commander's command cycle. The [Security Incident Response Template](https://github.com/ChefPlex/security-program-playbooks/blob/main/security-incident-response-template.md) has a different six: the response steps a TPM or incident lead runs, from identification and team notification through the data-exposure investigation to follow-up. Same incident, different seat.

### Phase 1 - Detect & Classify
| | |
|---|---|
| **Objective** | Confirm incident is real; assign initial severity and category |
| **IC actions** | Validate source, appoint self/deputy as IC, open incident record |
| **Deliverables** | Incident Classification Record, IC Appointment Note |
| **Exit criteria** | Severity assigned, IC named, channel live |

### Phase 2 - Orient & Scope
| | |
|---|---|
| **Objective** | Establish what's known vs assumed; bound the problem |
| **IC actions** | Build KFAU, define in/out of scope, identify attack surfaces and linked systems, preserve evidence before containment changes it, bring in legal early so privilege decisions are made up front |
| **Deliverables** | KFAU table, Scope Statement, Systems/Users Impacted tracker (CIMTK grid) |
| **Exit criteria** | Scope statement exists and is defensible to execs and legal |

### Phase 3 - Plan & Direct
| | |
|---|---|
| **Objective** | Convert scope into objectives and direction; stand up the team |
| **IC actions** | Set objectives, write Commander's Intent, set Battle Rhythm, assign workstreams (technical, legal, comms, HR) |
| **Deliverables** | Commander's Intent, Prioritized Objectives, Battle Rhythm, PIR (Priority Intelligence Requirements) list |
| **Exit criteria** | Team knows what to do without micromanagement |

### Phase 4 - Communicate
| | |
|---|---|
| **Objective** | Control the narrative internally and externally before others control it for you |
| **IC actions** | Brief execs on cadence, coordinate legal/comms/PR, prepare disclosure-scenario messaging |
| **Deliverables** | Exec Briefing Notes, Press Statement Tracker, Email Comms Tracker, LE Briefing form (if law enforcement engaged), Notification Clocks log |
| **Exit criteria** | Holding statement ready; internal and external messaging aligned; no unapproved statements have gone out |

**Notification clocks:** *Do we need to tell customers?* is often decided by a clock that is already running. Log each one in the Decision Log: which regime applies, who decides, when the clock started, when it expires. Common ones:
- **SEC Form 8-K Item 1.05** (US public companies): generally due four business days after the company determines the incident is material. Name who makes the materiality call. [SEC release 2023-139](https://www.sec.gov/newsroom/press-releases/2023-139)
- **GDPR Article 33**: notify the supervisory authority without undue delay and, where feasible, within 72 hours of becoming aware of a personal data breach. [Art. 33](https://gdpr-info.eu/art-33-gdpr/)
- **US state breach laws, sector rules and contracts**: deadlines and triggers vary. Legal owns the list.

**Key discipline:** say enough to be credible, not everything you don't yet know. Avoid absolutes ("no risk," "fully secure," "nothing to worry about") - they age badly and are the first thing quoted back at you.

### Phase 5 - Counter-Compromise & Remediate
| | |
|---|---|
| **Objective** | Evict the attacker, close attack paths, remediate affected data/systems |
| **IC actions** | Direct containment/eradication, track systems/users impacted, track data remediation, validate no persistence remains |
| **Deliverables** | Counter-Compromise Activities tracker, CC Systems and Users Impacted tracker, CC Data Remediation tracker, Attacker Intelligence tracker |
| **Exit criteria** | Attacker access confirmed removed; remediation actions closed or scheduled |

### Phase 6 - Sustain & Exercise
| | |
|---|---|
| **Objective** | Return to normal operations; capture lessons; harden for next time |
| **IC actions** | Run Post-Incident Review, update playbooks, schedule follow-up exercises |
| **Deliverables** | Post-Incident Review report, updated Battle Rhythm/playbook, Exercise Roadmap |
| **Exit criteria** | Post-Incident Review complete, actions assigned, next exercise scheduled |

---

## Reference: Terminology
- **IC** - Incident Commander: coordinates response, priorities, decisions, communications
- **KFAU** - Known Facts / Assumptions / Unknowns
- **PIR** - Priority Intelligence Requirements: the questions the response most needs answered. Not the Post-Incident Review, which this playbook spells out in full
- **SITREP** - Situation Report: what happened, impact, actions, risks, next steps
- **Battle Rhythm** - scheduled cadence of meetings/updates/decisions during the incident
- **Decision Authority** - who is empowered to approve ransom, disclosure, shutdown decisions
- **Single Source of Truth** - the one channel/doc treated as authoritative incident record

## Reference: Standing Decision Rules
- Don't rush to pay ransom/extortion demands - payment doesn't guarantee an outcome or deletion. Check sanctions exposure and involve legal and law enforcement before any payment discussion (US: [OFAC ransomware advisory, September 2021](https://ofac.treasury.gov/media/912981/download))
- Default to consequence-based decisions: frame every technical fact in terms of data loss, downtime, or reputational exposure before it reaches execs
- Every ransom/legal/disclosure decision gets logged with rationale - assume it will be reviewed later
- Scope statements and exec briefs should be conservative and defensible, not comprehensive - you'll need to walk back overclaims, rarely underclaims
