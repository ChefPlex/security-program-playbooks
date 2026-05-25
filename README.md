# security-program-playbooks

Guides, templates, and reference material for Technical Program Managers running security programs.

This repo focuses on the TPM layer of security work: intake, compliance triage, evidence planning, dependency mapping, stakeholder alignment, reporting, and cross-team execution.

It is not a security engineering handbook. It is not a GRC policy library. It is not a complete incident response manual.

The goal is to help TPMs structure the messy middle of security programs: who owns the work, what evidence is needed, what risk is real, what decision is blocked, and what needs to happen next.

Built from experience driving encryption, PKI, vulnerability remediation, and compliance programs at enterprise scale.

## What Is Here

### Program Execution

| Guide | What It Is |
|---|---|
| [Security Program Intake and Kickoff Guide](security-program-intake-kickoff.md) | A practical intake structure for standing up security programs. Covers problem definition, compliance triage, stakeholders, dependencies, roles, evidence needs, and operating rhythm. |
| [Security TPM Role Guide](security-tpm-role-guide.md) | A guide to what security TPM work actually requires, where it differs from general TPM work, and how TPMs can work effectively with engineering, security, GRC, legal, and leadership teams. |

### Reference Material

| Document | What It Is |
|---|---|
| [Compliance Framework Reference](compliance-framework-reference.md) | Plain-English summaries of common compliance frameworks and what they usually mean for a TPM running security, infrastructure, or evidence-driven programs. |

### Examples

| Example | What It Shows |
|---|---|
| [Sample Security Intake](examples/sample-security-intake.md) | A concrete example of how to frame a security program intake with scope, stakeholders, evidence needs, and execution risks. |

## Current Scope

This repo is currently strongest for:

- Starting a new security program
- Clarifying the TPM role in security work
- Translating compliance pressure into executable program structure
- Identifying evidence needs before an audit or review becomes urgent
- Mapping stakeholders, dependencies, risks, owners, and escalation paths

This repo is not yet a full runbook library.

The next useful additions should be concrete operating examples and runbooks, not more high-level framing.

## What Is Coming

- Vulnerability remediation runbook
- Compliance evidence collection playbook

### Program Execution

| Playbook | What It Will Be |
|----------|----------------|
| Vulnerability Remediation Runbook | Step-by-step operational guide for running a vulnerability remediation program - triage, owner assignment, MTTR tracking, and reporting to audit and security teams. |
| Compliance Program Playbook | How to run a compliance program (SOX, PCI, HIPAA, FedRAMP) from initial assessment through audit close. Control mapping, evidence collection, and how to work effectively with GRC. |
| Security Incident Response: TPM Role | What the TPM does during a security incident - when to engage, how to track response actions, and how to manage the post-mortem and remediation program that follows. |
| Encryption Program Framework | How to scope, plan, and execute an encryption modernization program across a large distributed system. Coverage tracking, service prioritization, and how to manage the long tail. |

### Reference Material

| Document | What It Is |
|----------|-----------|
| [Compliance Framework Reference](compliance-framework-reference.md) | Plain-English summaries of SOX, PCI-DSS, HIPAA, FedRAMP, SOC 2, GDPR, and NIST CSF - what each framework requires, who it applies to, and what it means for a TPM running a program in that environment. |

---

## How to Use These

**Starting a new security program:** Start with the Intake and Kickoff Guide. Work through the intake checklist before the charter is written. The compliance triage section is the one most people skip - do not skip it.

**New to security TPM work:** Read the Role Guide first. It explains what the job requires and where security programs differ from general TPM work. Then read the Intake Guide to understand how to structure programs in this domain.

**Running a vulnerability remediation effort:** The Intake Guide has a dedicated section on vulnerability remediation programs. The full Vulnerability Remediation Runbook is coming.

---

## A Note on Domain Knowledge

You do not need to be a security engineer to be an effective security TPM. You need to be technical enough to understand what the team is building, fluent enough in compliance to know when a regulatory constraint changes the plan, and organized enough to manage complexity across teams that do not naturally talk to each other.

These playbooks assume that baseline and try to build on it - not replace the security engineers and GRC specialists, but give the TPM the context to work alongside them effectively.

---

## Contributing

If you have a framework, runbook, or reference document that has worked in practice for security programs - open a PR or file an issue. The bar is that it has to come from real experience, be generic enough to apply outside one company, and be documented well enough to be useful without asking you questions.

---

*Built from experience running platform security, encryption, PKI, and compliance programs at enterprise scale. Maintained by [Eric White](https://www.linkedin.com/in/edwhite) | [ChefPlex](https://github.com/ChefPlex)*
