# Sample Security Program Intake

A security program that starts with a vague scope is a security program that will fight about scope for its entire life. This example shows what a weak intake looks like, what a better one looks like, and why the difference matters.

---

## Weak Intake

**We need to complete encryption work for compliance.**

### Why This Fails

No named regulation or control objective. No systems in scope. No data classification. No owner. No evidence requirement. No date. No risk of doing nothing. The engineering team has no idea what to build. The GRC team has no idea what evidence to collect. The TPM has no charter to write. This is not a program intake - it is a note that someone is anxious about compliance.

---

## Better Intake

| Field | Entry |
|-------|-------|
| Program | Customer Data Encryption Coverage |
| Driver | Compliance commitment and internal risk reduction |
| Systems in Scope | Customer profile service, billing export pipeline, legacy reporting database |
| Data in Scope | Customer PII and billing metadata |
| Required Outcome | Confirm encryption at rest and in transit for in-scope systems, document gaps, and produce evidence package for audit review |
| Primary Engineering Owner | Platform Security |
| TPM Owner | Eric |
| Required Decision | Whether the legacy reporting database is remediated, isolated, or formally exceptioned |
| Target Date | 2026-08-15 |
| Escalation Trigger | No remediation path for legacy reporting database by 2026-06-30 |

### Why This Works

The intake separates the compliance driver from the actual delivery work. It names systems, data, owners, dates, and the decision that can block the program. The engineering team knows what they are building. GRC knows what evidence to expect. The TPM has enough to write a charter. The legacy database decision is named explicitly because it is the risk that will surface eventually - better to name it at intake than to discover it six weeks before the audit.

---

## The Questions That Matter at Intake

Before a security program starts, these questions should have answers:

- What is the driver? Regulation, audit finding, risk reduction, or product dependency?
- What systems and data are in scope? Named specifically, not described generally.
- What does done look like? Specific outcome, not general improvement.
- Who owns the technical work?
- What decisions are needed before the program can proceed?
- What is the risk of doing nothing, and by when does that risk materialize?

If any of these are blank, that is the first workstream: filling them in.

---

*Part of the [security-program-playbooks](https://github.com/ChefPlex/security-program-playbooks) repo. See the [Security Program Intake and Kickoff Guide](../security-program-intake-kickoff.md) for the full framework.*
