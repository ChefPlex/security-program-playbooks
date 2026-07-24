# Compliance Program Playbook

Compliance programs - SOX, PCI-DSS, HIPAA, FedRAMP, SOC 2, and similar - have a structure that looks unfamiliar to TPMs coming from feature delivery work. The deliverable is not a shipped feature. It is a body of evidence that satisfies an external auditor that a set of controls operate as designed. This playbook covers how to run that kind of program from initial assessment through audit close.

For plain-English summaries of what each framework actually requires, see the [Compliance Framework Reference](compliance-framework-reference.md). This document covers how to run the program itself, not what any single framework says.

---

## Step 1: Initial Assessment

Before a compliance program has a charter, someone has to determine what it is actually on the hook for. That is usually a joint effort between the TPM and GRC (Governance, Risk, and Compliance), not something either owns alone.

### Questions to answer at assessment

**Which framework, and why now?**
A new framework requirement usually has a trigger - a new customer contract, a market expansion, a regulatory change, a prior audit finding. Know the trigger. It usually sets the real deadline, which is rarely negotiable the way a feature deadline is.

**What is already in place?**
Most organizations are not starting from zero. Existing controls from other frameworks often satisfy part of a new one. A gap assessment against the specific framework's control list - not a general security review - is the right starting point.

**Who is the framework owner on the GRC side?**
Every framework needs a named GRC owner who understands its specific control language and audit expectations. The TPM runs the program. GRC owns the framework interpretation. Conflating these roles is a common failure mode.

**What is the audit type and cadence?**
Some frameworks are point-in-time assessments (a FedRAMP ATO). Others are continuous with periodic audits (SOC 2 Type II, ongoing PCI compliance). This changes how the program is structured - a point-in-time push looks different from a program that has to sustain evidence collection indefinitely.

### Assessment checklist

| Item | Owner | Done? |
|------|-------|-------|
| Framework and trigger identified | GRC | |
| Gap assessment against control list completed | GRC + Security | |
| GRC framework owner named | GRC Lead | |
| Audit type and cadence confirmed | GRC | |
| Hard deadline (if any) documented | GRC + Legal | |
| TPM assigned | TPM Lead | |

---

## Step 2: Control Mapping

Frameworks are expressed as control requirements, not engineering tasks. The core work of standing up the program is translating each control into something a team can actually build or operate.

### Control mapping process

1. **List every required control** from the framework, using GRC's interpretation as the source of truth.
2. **Map each control to an existing capability, a gap, or a partial gap.** Be specific - "access control" as a category is not useful; "quarterly access reviews for production database access" is.
3. **Assign an engineering or process owner per gap.** Same principle as vulnerability remediation - ownership at the level of a specific person, not a team name.
4. **Identify controls that are process, not technical.** Some controls are satisfied by a documented procedure and evidence that it is followed, not by an engineering build. Do not assign these to engineering by default.

### Control tracking table

| Control ID | Requirement | Current State | Owner | Target Date | Evidence Type |
|-----------|-------------|---------------|-------|-------------|---------------|
| Example | Encryption of data at rest | Partial - 60% of services | Eng Lead | Q3 | Config scan + attestation |
| Example | Quarterly access review | Not in place | IT Ops | Q2 | Review log + sign-off |

A control mapping spreadsheet with fifty rows and no owner column is a document, not a program.

---

## Step 3: Evidence Collection

Evidence is what turns "we do this" into something an auditor will accept. Most compliance programs that run late are late because evidence collection was treated as an afterthought instead of a workstream with its own owner and deadlines.

### What counts as evidence

Varies by framework and control, but generally falls into a few categories:

- **Configuration evidence** - screenshots, exports, or automated scans showing a system is configured as required
- **Log evidence** - records showing a control operated over time (access logs, review logs, approval logs)
- **Attestation** - a named individual formally confirming a process was followed
- **Policy documentation** - the written policy that a control is meant to enforce

### Evidence discipline

Decide the evidence standard for each control before the audit window opens, not during it. Auditors reject evidence that is inconsistent, undated, or missing a clear chain from the control requirement to the artifact. Build evidence collection into the operational rhythm of the teams doing the work - quarterly access reviews should produce their own evidence automatically, not require a scramble two weeks before the audit.

### Common evidence failure modes

- Evidence exists but is not dated, so it cannot prove the control operated during the audit period
- Evidence shows the control was configured once but not that it was checked or maintained
- Evidence is scattered across individual inboxes and chat threads instead of a central, auditable location

---

## Step 4: Working With GRC

GRC and the TPM function are often confused for each other, which causes friction. They are complementary, not overlapping.

| Function | Owns |
|----------|------|
| GRC | Framework interpretation, control requirements, audit relationship, risk acceptance authority |
| TPM | Program structure, cross-team coordination, delivery tracking, escalation, stakeholder communication |

**What GRC needs from the TPM:** a reliable view of where each control stands, early warning when a control owner is behind, and a single point of coordination instead of chasing a dozen engineering teams individually.

**What the TPM needs from GRC:** authoritative interpretation of ambiguous control language, and a clear answer on what evidence will actually satisfy the auditor - the TPM should not be guessing at compliance interpretation.

---

## Step 5: Pre-Audit Readiness

### Readiness review, before the auditor shows up

Run an internal readiness review at least one full cycle before the real audit, especially for a first-time framework. Treat it exactly like the real audit: pull evidence, walk through control by control, and find gaps while there is still time to close them.

### Readiness checklist

| Item | Owner |
|------|-------|
| Every control has current, dated evidence | Control owners |
| Evidence is centrally located and accessible to the audit team | TPM |
| Any open gaps have a remediation plan with a date before the audit | Control owners |
| Key personnel are briefed on what auditors typically ask | GRC |
| Escalation path defined for anything discovered during the audit itself | TPM + GRC |

---

## Step 6: During the Audit

**Route all auditor requests through a single point of contact.** Usually GRC, sometimes the TPM. Individual engineers responding directly to ad hoc auditor requests leads to inconsistent answers and scope creep in what the auditor decides to look at.

**Track every request and every response.** A simple log - what was asked, who answered, what was provided, when - prevents the same request from being answered twice with different information.

**Do not guess.** If a control owner does not know the answer to an auditor's question, get back to them with a confirmed answer rather than speculating in the room.

---

## Step 7: Audit Findings and Close-Out

Findings from the audit itself are handled differently depending on severity and whether they threaten certification.

### Finding response

| Finding Type | Response |
|-------------|----------|
| Minor - does not threaten certification | Remediation plan with date, tracked to close |
| Major - threatens certification | Immediate escalation to executive sponsor, expedited remediation |
| Observation - not a formal finding but noted | Document response, no formal remediation required unless recurring |

### Close-out

Certification or audit sign-off is not the end of the program if the framework requires ongoing compliance (as most do). At close-out:

- Document what evidence collection needs to become steady-state operations rather than a program activity
- Transfer control ownership from the program to whoever will maintain it long-term
- Set the calendar for the next audit cycle now, not when it is six weeks away

A compliance program that ends at certification and has no owner for the next cycle will be starting from scratch again next year.

---

*Version 1.0. Propose changes via pull request.*
