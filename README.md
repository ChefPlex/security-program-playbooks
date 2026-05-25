# security-program-playbooks

Frameworks, guides, and reference material for Technical Program Managers running security programs. Built from experience driving encryption, PKI, vulnerability remediation, and compliance programs at enterprise scale.

Security programs are not just technical programs with a security label on them. The compliance requirements are real, the blast radius when something goes wrong is larger, and the organizational surface area is usually bigger than anyone expects. These playbooks are designed to help TPMs navigate that complexity without having to figure it all out from scratch.

This is for the part of security programs where the technical risk, compliance date, and org chart all become the same problem.

---

## What Is Here

### Program Execution

| Playbook | What It Is |
|----------|-----------|
| [Security Program Intake and Kickoff Guide](security-program-intake-kickoff.md) | How to stand up a security program correctly - from intake through first steering committee. Covers compliance triage, dependency mapping, role definition, status reporting, and vulnerability remediation programs specifically. |
| [Security TPM Role Guide](security-tpm-role-guide.md) | What the security TPM job actually requires, where it differs from general TPM work, and how to be effective in it. Useful for TPMs new to security programs and for engineering teams trying to understand what to expect from their TPM. |
| [Security Incident Response Template](security-incident-response-template.md) | A six-phase incident response framework: identification, notification, investigation, containment, remediation and communication, and follow-up. Includes severity levels, data classification taxonomy, evidence handling, and notification decision tree. Sanitized from real incident response work. Adapt the role names and severity tiers to your organization. |

### Reference Material

| Document | What It Is |
|----------|-----------|
| [Compliance Framework Reference](compliance-framework-reference.md) | Plain-English summaries of SOX, PCI-DSS v4.0.1, HIPAA, FedRAMP, SOC 2, GDPR, and NIST CSF - what each framework requires, who it applies to, and what it means for a TPM running a program in that environment. Includes current version tracking and review dates. |

### Examples

| File | What It Shows |
|------|--------------|
| [Sample Security Program Intake](examples/sample-security-intake.md) | A weak intake vs. a complete one, with field-by-field explanation of why the difference matters. |
| [Sample Incident Escalation](examples/sample-incident-escalation.md) | A weak Sev1 escalation vs. a complete one, with explanation of what each element does and why timing matters more than completeness. |

---

## What Is Coming

- Vulnerability remediation program runbook
- Encryption program framework

---

## How to Use These

**Starting a new security program:** Start with the Intake and Kickoff Guide. Work through the intake checklist before the charter. The compliance triage section is the one most people skip - do not skip it.

**New to security TPM work:** Read the Role Guide first. It explains what the job requires and where security programs differ from general TPM work. Then read the Intake Guide to understand how to structure programs in this domain.

**Running an incident:** The Incident Response Template gives you the sequence and the key questions for each phase. The sample escalation shows what good communication looks like under pressure. Read both before you need them, not during.

**Running a vulnerability remediation effort:** The Intake Guide has a dedicated section on vulnerability remediation programs. The full Vulnerability Remediation Runbook is coming.

---

## Where This Breaks

This is not a substitute for security engineering, GRC, legal, privacy, or incident command. It is here to help the TPM ask better questions, sequence the work, and keep the mess visible.

It breaks when someone treats the playbook as the control instead of doing the work that proves the control is real. Documentation of a program is not the same as a running program.

---

## A Note on Domain Knowledge

You do not need to be a security engineer to be an effective security TPM. You need to be technical enough to understand what the team is building, fluent enough in compliance to know when a regulatory requirement changes the plan, and organized enough to manage complexity across teams that do not naturally talk to each other.

These playbooks assume that baseline and try to build on it - not replace the security engineers and GRC specialists, but give the TPM the context to work alongside them effectively.

---

## Contributing

If you have a framework, runbook, or reference document that has worked in practice for security programs - open a PR or file an issue. The bar is that it has to come from real experience, be generic enough to apply outside one company, and be documented well enough to use without asking questions.

---

## Final Note

Security programs punish late discovery. These notes are meant to surface the awkward questions early, while there is still time to make useful decisions.

---

*Built from experience running platform security, encryption, PKI, and compliance programs at enterprise scale. Maintained by [Eric White](https://www.linkedin.com/in/edwhite) | [ChefPlex](https://github.com/ChefPlex)*
