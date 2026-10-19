# security-program-playbooks

Frameworks, guides, and reference material for Technical Program Managers running security programs. Built from experience driving encryption, PKI, vulnerability remediation, and compliance programs at enterprise scale.

Security programs carry real compliance requirements, a larger blast radius when something goes wrong, and an organizational surface area that is usually bigger than anyone expects. These playbooks help TPMs work through that without having to figure it all out from scratch.

This is for the part of security programs where the technical risk, compliance date, and org chart all become the same problem.

Companion to [infrastructure-program-playbooks](https://github.com/ChefPlex/infrastructure-program-playbooks), for data center, migration, directory and network programs.

---

## What Is Here

### Program Execution

| Playbook | What It Is |
|----------|-----------|
| [Security Program Intake and Kickoff Guide](security-program-intake-kickoff.md) | What to consider on top of the general charter, kickoff and phases when the program is a security program: problem before solution, the compliance driver and reporting clocks at intake, review triage with the two-phases rule, the security roles, security dependencies, and reporting delivery risk apart from security risk. Points to the general templates for everything else. |
| [Security TPM Role Guide](security-tpm-role-guide.md) | What the security TPM job actually requires, where it differs from general TPM work, and how to be effective in it. Useful for TPMs new to security programs and for engineering teams trying to understand what to expect from their TPM. |
| [Vulnerability Remediation Runbook](vulnerability-remediation-runbook.md) | How to run a vulnerability remediation program end to end: risk-based prioritization on exposure, known exploitation, exploit automation and impact (benchmarked to CISA BOD 26-04), SLA attainment by tier, ownership at the service level, tracking coverage instead of raw completion percentage, handling new findings mid-program, and reporting to security teams and auditors. |
| [Encryption Program Playbook](encryption-program-playbook.md) | How to run encryption modernization across a large distributed estate, in ten steps. Sizing the problem before remediating anything, why finding owners stalls programs more than finding services, coverage work versus currency work, the concentration trap that makes a schedule serial when the plan looks parallel, the standing escalation deal, certificate chain desync, and how to close honestly at diminishing returns rather than at 100 percent. Companion piece: [Lessons from Leading Large-Scale Encryption Programs](https://github.com/ChefPlex/learning-notes/blob/main/lessons-from-large-scale-encryption-programs.md) in learning-notes. |
| [Compliance Program Playbook](compliance-program-playbook.md) | How to run a compliance program - SOX, PCI, HIPAA, FedRAMP, SOC 2 - from initial assessment through audit close. Control mapping, evidence collection, working with GRC, pre-audit readiness, and handling audit findings. |
| [Required Control Rollout Playbook](required-control-rollout-playbook.md) | How to land a security control that every existing customer must adopt or be blocked. Sequencing by risk (highest-risk and free accounts first, largest accounts last), customer communication waves on a fixed lead time, warning mode before enforcement, exceptions with an expiry date, holding published enforcement dates, support load, and measuring adoption from system state. |
| [Cyber Incident Management Playbook](cyber-incident-playbook.md) | The whole incident in one place. First-hour actions from detection to the first executive brief, then six phases from Detect & Classify through Sustain & Exercise, each with the incident commander's work and the response team's tasks. Severity levels, data classes, who to notify, a table of regulatory reporting clocks, a mapping to NIST SP 800-61 Rev. 3, the incident ticket and the standing decision rules. Draws on SANS LDR553 and an enterprise response template, and says plainly where the two differ. Adapt the role names and severity tiers to your organization. |
| [Incident Tabletop Exercise Kit](incident-tabletop-exercise-kit.md) | A tabletop exercise to run beside the Cyber Incident Management Playbook: scope and testable objectives, roles, a ransomware scenario with timed injects, a facilitator script, the evidence package an auditor will ask for, and an after-action report format. |

### AI and Retrieval Systems

AI security coverage lives here, in the RAG folder and the AI tooling reference below. The other playbooks touch AI only where it changes their own work (AI risk review at intake, AI incident classes, AI in vulnerability discovery).

| Playbook | What It Is |
|----------|-----------|
| [Enterprise RAG Security](enterprise-rag-security/) | The security workstream for a system that answers questions using company data on behalf of a specific person. Trust boundary on both the document and structured-data paths, permission-aware retrieval, the control set, deterministic enforcement and denial telemetry, the six failure modes that actually happen, and a prompt injection threat model covering authority between agents, with a test corpus you can run in CI. The program these sit inside is in [ai-automations](https://github.com/ChefPlex/ai-automations/tree/main/ai-programs/enterprise-rag-program). |

### Reference Material

| Document | What It Is |
|----------|-----------|
| [Compliance Framework Reference](compliance-framework-reference.md) | Plain-English summaries of SOX, PCI-DSS v4.0.1, HIPAA, FedRAMP, SOC 2, GDPR, and NIST CSF, plus short entries for ISO/IEC 27001, SEC Form 8-K Item 1.05, NIS2, DORA, and the EU Cyber Resilience Act, AI Act and Digital Services Act - what each requires, who it applies to, and what it means for a TPM running a program in that environment. Carries a last-reviewed date, with sources linked for the dates and figures checked at that review. Frameworks move faster than any reference; confirm with GRC before relying on it. |
| [AI Tooling in Regulated Environments](ai-tooling-regulated-data.md) | Gating AI and DevOps tool adoption behind BAAs and a data classification line, and what changes from the standard adoption sequence when PHI is in scope. Covers why compliance automation moves from last to first, the connector review that workflow automation needs, and the tier ceiling on any activity whose input might contain regulated data. Overlay on the [AI Tool Decision Matrix](https://github.com/ChefPlex/ai-automations/blob/main/frameworks/ai-tool-decision-matrix.md) in ai-automations. |

### Examples

| File | What It Shows |
|------|--------------|
| [Sample Security Program Intake](examples/sample-security-intake.md) | A weak intake vs. a complete one, with field-by-field explanation of why the difference matters. |
| [Sample Incident Escalation](examples/sample-incident-escalation.md) | A weak Sev1 escalation vs. a complete one, with explanation of what each element does and why timing matters more than completeness. |

---

## What Is Coming

Nothing queued. The encryption program framework that sat here for a while is now the
[Encryption Program Playbook](encryption-program-playbook.md) above.

Things get added when they have been used on real work often enough to be worth writing down, not
because a list said they were coming.

---

## How to Use These

**Starting a new security program:** Start with the general [charter](https://github.com/ChefPlex/tpm-templates/blob/main/program-charter-template.md) and [kickoff checklist](https://github.com/ChefPlex/tpm-templates/blob/main/program-kickoff-checklist.md), with the Intake and Kickoff Guide beside them for what a security program adds. The review triage is the part most people skip - don't skip it.

**New to security TPM work:** Read the Role Guide first. It explains what the job requires and where security programs differ from general TPM work. Then read the Intake Guide for what to consider when a program is a security program.

**Running an incident:** The Cyber Incident Management Playbook gives the incident commander the first hour and the command work, and the response team the tasks and key questions for each phase. The sample escalation shows what good communication looks like under pressure. Read both before you need them, not during.

**Testing the incident plan:** Run the Tabletop Exercise Kit against the Cyber Incident Management Playbook at least once a year, and decide what goes in the evidence package before the session.

**Turning on a control every customer must adopt:** Start with the Required Control Rollout Playbook. Write the blocked-account message and the exception policy before the first announcement goes out.

**Running a vulnerability remediation effort:** Start with the Vulnerability Remediation Runbook - severity framework, ownership, and tracking are covered end to end.

**Running a compliance program:** Start with the Compliance Program Playbook for how to run the program itself - assessment, control mapping, evidence, audit. Use the Compliance Framework Reference alongside it for what the specific framework actually requires.

**Securing a RAG or AI retrieval system:** Start with the RAG Security Playbook for the trust boundary and control set, then the Prompt Injection Threat Model. Read the injection document before the architecture is settled, because the one control that does the real work is filtering at retrieval, and that's expensive to retrofit.

---

## Where This Breaks

This isn't a substitute for security engineering, GRC, legal, privacy, or incident command. It's here to help the TPM ask better questions, sequence the work, and keep the mess visible.

It breaks when someone treats the playbook as the control instead of doing the work that proves the control is real. Documentation of a program isn't the same as a running program.

---

## A Note on Domain Knowledge

You don't need to be a security engineer to be an effective security TPM. You need to be technical enough to understand what the team is building, fluent enough in compliance to know when a regulatory requirement changes the plan, and organized enough to manage complexity across teams that don't naturally talk to each other.

These playbooks assume that baseline and try to build on it - not replace the security engineers and GRC specialists, but give the TPM the context to work alongside them effectively.

---

## Contributing

If you have a framework, runbook, or reference document that has worked in practice for security programs - open a PR or file an issue. The bar is that it has to come from real experience, be generic enough to apply outside one company, and be documented well enough to use without asking questions.

---

## Final Note

Security programs punish late discovery. These notes are meant to surface the awkward questions early, while there's still time to make useful decisions.

---

*Built from experience running platform security, encryption, PKI, and compliance programs at enterprise scale. Maintained by [Eric White](https://www.linkedin.com/in/edwhite) | [ChefPlex](https://github.com/ChefPlex)*

## License

Copyright (c) 2026 Eric White. Licensed under [CC BY 4.0](LICENSE): use it, adapt it, put it to
work in your own program. Credit is the only condition.

---

**Part of the ChefPlex TPM portfolio** | Status: Active | [I need to… find the right document across all the repositories](https://github.com/ChefPlex/tpm-templates/blob/main/PORTFOLIO-GUIDE.md) | [All repositories](https://github.com/ChefPlex)
