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
- Turning broad security concerns into a program someone can actually run

This repo is not yet a full runbook library.

The next useful additions should be concrete operating examples and runbooks, not more high-level framing.

## Maturity

| Artifact | Status | Notes |
|---|---|---|
| Security Program Intake and Kickoff Guide | Ready | Good starting point for new security programs that need scope, ownership, evidence needs, and operating rhythm. |
| Security TPM Role Guide | Ready | Useful role clarification for TPMs working with security engineering, GRC, legal, product, and leadership. |
| Compliance Framework Reference | Reference | Useful orientation material. Should be reviewed periodically against primary sources. |
| Sample Security Intake | Ready | Shows the difference between vague intake and actionable program framing. |
| Vulnerability Remediation Runbook | Planned | Build next. |
| Compliance Evidence Collection Playbook | Planned | Build next. |

## What Is Coming

- Vulnerability remediation runbook
- Compliance evidence collection playbook

## How to Use These

### Starting a new security program

Start with the [Security Program Intake and Kickoff Guide](security-program-intake-kickoff.md).

Work through the intake checklist before the charter is written. The compliance triage section is the one most people skip. Do not skip it.

If the intake cannot name the system, data, control objective, owner, decision, evidence need, and risk of doing nothing, the program is not ready to execute. That is not failure. That is the first workstream.

### New to security TPM work

Read the [Security TPM Role Guide](security-tpm-role-guide.md) first.

Security TPM work is still TPM work, but the failure modes are different. Evidence matters. Regulatory language matters. Security architecture matters. So do ownership, escalation, and timing.

A good security TPM does not replace the security engineer or the GRC specialist. The TPM makes sure the right work is visible, sequenced, owned, and finished.

### Translating compliance pressure into execution

Use the [Compliance Framework Reference](compliance-framework-reference.md) to understand the shape of the compliance driver, then use the intake guide to translate that driver into program work.

A compliance requirement is not automatically an engineering plan. Someone still has to define:

- What systems are in scope
- What data is in scope
- What evidence is required
- Who owns the technical work
- Who owns the control interpretation
- What decision is blocked
- What date matters
- What risk materializes if nothing changes

That translation layer is where security TPMs earn trust.

### Running a vulnerability remediation effort

The intake guide includes the structure needed to start a vulnerability remediation program: scope, owners, severity, remediation path, exception handling, reporting cadence, and escalation triggers.

The full vulnerability remediation runbook is planned, but the intake guide is enough to avoid the most common failure: starting remediation work without a clear owner model or reporting rhythm.

## Where This Breaks

These guides break when they are treated as paperwork instead of operating tools.

A security intake that does not change what the team does next is not useful. A compliance reference that does not clarify evidence, ownership, or deadlines is trivia. A role guide that does not help a TPM know when to push, escalate, or get out of the way is not doing its job.

Security programs also break when the TPM tries to sound more technical than they are.

You do not need to be a security engineer to be an effective security TPM. You do need to be technical enough to understand what the team is building, fluent enough in compliance to know when a regulatory constraint changes the plan, and organized enough to manage complexity across teams that do not naturally talk to each other.

## A Note on Domain Knowledge

These playbooks assume baseline TPM skill and build from there.

They are meant to help TPMs work alongside security engineers, infrastructure teams, GRC, legal, product, support, and leadership. They do not replace the people who own technical design or control interpretation.

The TPM owns the operating system around the work:

- Intake
- Scope
- Ownership
- Cadence
- Dependency tracking
- Risk visibility
- Evidence planning
- Escalation
- Decision hygiene
- Closeout

That is enough work. Do it well.

## Related Repos

- [tpm-templates](https://github.com/ChefPlex/tpm-templates) - Program charters, RFC/ADR templates, RAID guides, communication plans, and lifecycle tools from real TPM work.
- [tpm-toolbox](https://github.com/ChefPlex/tpm-toolbox) - Lightweight TPM trackers, checklists, RAID logs, and AI-assisted workflows for program execution.
- [program-reporting-frameworks](https://github.com/ChefPlex/program-reporting-frameworks) - Status, steering committee, lessons-learned, and investment frameworks for honest program reporting.
- [ai-automations](https://github.com/ChefPlex/ai-automations) - AI-assisted TPM prompts, workflows, examples, and review checks for safer program artifacts.

## Contributing

If you have a framework, runbook, or reference document that has worked in practice for security programs, open a PR or file an issue.

The bar is that it has to come from real experience, be generic enough to apply outside one company, and be documented well enough to be useful without asking you questions.

Useful beats complete.

## Maintainer

Built from experience running platform security, encryption, PKI, and compliance programs at enterprise scale.

Maintained by [Eric White](https://github.com/ChefPlex).
