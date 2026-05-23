# security-program-playbooks

Frameworks, guides, and reference material for Technical Program Managers running security programs. Built from experience driving encryption, PKI, vulnerability remediation, and compliance programs at enterprise scale.

Security programs are not just technical programs with a security label on them. The compliance requirements are real, the blast radius when something goes wrong is larger, and the organizational surface area is usually bigger than anyone expects. These playbooks are designed to help TPMs navigate that complexity without having to figure it all out from scratch.

---

## What Is Here

### Program Execution

| Playbook | What It Is |
|----------|-----------|
| [Security Program Intake and Kickoff Guide](security-program-intake-kickoff.md) | How to stand up a security program correctly - from intake through first steering committee. Covers compliance triage, dependency mapping, role definition, status reporting, and vulnerability remediation programs specifically. |
| [Security TPM Role Guide](security-tpm-role-guide.md) | What the security TPM job actually requires, where it differs from general TPM work, and how to be effective in it. Useful for TPMs new to security programs and for engineering teams trying to understand what to expect from their TPM. |

---

## What Is Coming

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
