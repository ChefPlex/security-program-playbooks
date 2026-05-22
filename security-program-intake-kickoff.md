# Security Program Intake and Kickoff Guide

Security programs fail for the same reasons any program fails - unclear scope, missing owners, dependencies nobody mapped, and compliance requirements that showed up too late to build into the design. The only thing different about security programs is that the blast radius when something goes wrong tends to be larger and more visible.

This guide covers how to stand up a security program correctly from the first conversation to the first steering committee update. Most of the work happens before anyone writes a line of code.

---

## Step 1: Intake

Before a security program gets a TPM assigned and a charter written, someone has to define what it actually is. That sounds obvious. It is frequently skipped.

### Questions to answer at intake

**What is the problem?**
Not the solution - the problem. "We need to implement MFA" is a solution. "40% of our high-privilege accounts have no second factor and three of our peer companies have been breached through credential theft in the last six months" is a problem. Start with the problem.

**What is the regulatory or compliance driver, if any?**
Security programs often have external forcing functions - a compliance deadline, an audit finding, a regulatory change, a contractual obligation. Identify these at intake. They will drive timeline constraints that are non-negotiable, and they need to be built into the plan from day one, not retrofitted later.

**What is the risk if we do not do this?**
Be specific. "Increased risk" is not useful. "A successful credential stuffing attack against our administrative accounts could result in unauthorized access to customer data across all tenants" is useful. Quantifying the risk - even roughly - helps with prioritization and resource justification.

**Who owns the problem?**
There is a difference between the executive sponsor, the product owner, and the engineering owner. All three need to be identified before a program starts. A security program with no clear business owner will drift.

**What does done look like?**
Before work starts, define what success means. Coverage percentages, audit pass/fail, vulnerability closure rates, certification achievement - whatever the measure is, agree on it upfront.

### Intake checklist

| Item | Owner | Done? |
|------|-------|-------|
| Problem statement documented | Requesting team | |
| Regulatory / compliance driver identified | TPM + GRC | |
| Risk of inaction quantified | Requesting team | |
| Executive sponsor named | Leadership | |
| Product / program owner named | Leadership | |
| Initial scope boundaries defined | PO + TPM | |
| Program added to tracking system | TPM | |
| TPM assigned | TPM Lead | |

---

## Step 2: Compliance and Security Review Triage

This is the step most programs get wrong. Compliance and security reviews take time - sometimes weeks, sometimes months. Starting them late is one of the most common reasons security programs miss their deadlines.

At program intake, triage which reviews apply. You do not need to complete them yet. You need to know they are coming so you can sequence them into the plan.

### Common review types for security programs

| Review Type | When to Initiate | What It Covers |
|-------------|-----------------|----------------|
| Compliance Assessment | At intake | Determines which regulatory frameworks apply (SOX, PCI, HIPAA, FedRAMP, SOC 2, etc.) and what controls are required |
| Security / Architecture Review | During Definition & Planning | Threat model the proposed solution; identify security requirements before build begins |
| Privacy / Legal Review | During Definition & Planning | Required for any program touching customer data, AI/ML systems, or new data collection |
| Penetration Testing | Pre-production | Required for internet-facing services or significant changes to existing ones |
| Vendor / Third-Party Review | Before vendor work begins | Security and procurement review for any buy decision |
| Change Advisory Board (CAB) | Before production changes | Required for infrastructure changes in regulated environments |

### The rule

If a review could block your go-live date, initiate it at least two phases before you need the result. Waiting until you need it is not a plan.

---

## Step 3: Program Structure for Security Work

Security programs often span multiple engineering teams, multiple compliance domains, and multiple stakeholder organizations simultaneously. The structure you set up at the beginning determines whether you can manage that complexity or just react to it.

### Core roles on a security program

| Role | Responsibilities |
|------|----------------|
| Executive Sponsor | Investment decisions, escalation authority, executive alignment |
| Product / Program Owner | Business requirements, Definition of Done, prioritization |
| Technical Program Manager | Program structure, delivery tracking, risk management, stakeholder communications |
| Engineering Lead(s) | Technical design, implementation, engineering team coordination |
| Security Architect | Threat modeling, architectural review, security requirements |
| GRC / Compliance | Regulatory requirements, audit coordination, control evidence |
| Legal / Privacy | Privacy review, contractual obligations, policy updates |

### The TPM's specific role in security programs

The TPM does not own the security domain knowledge - that belongs to the security architects, engineers, and GRC team. What the TPM owns is the program itself: the structure, the delivery, the dependencies, the escalations, and the communications.

That means being technical enough to understand what the engineering team is building and why, fluent enough in compliance to know when a regulatory constraint changes the plan, and organized enough to track all of it across teams that do not naturally talk to each other.

---

## Step 4: Dependency Mapping for Security Programs

Security programs touch more teams than most. A TLS modernization program affects every service that makes network calls. An encryption-at-rest program affects every service that writes to storage. A vulnerability remediation program may involve dozens of engineering teams across the organization.

Map dependencies before you start, not after you hit one.

### Dependency categories for security programs

**Engineering dependencies**
Which teams have to make changes to support this program? Do they have capacity? Is this work in their roadmap, or does it need to be negotiated?

**Platform / infrastructure dependencies**
What does this program depend on from the platform layer - certificate management, key management, identity, network configuration? Who owns those services?

**Compliance dependencies**
What evidence needs to be collected, and who is responsible for collecting it? What is the audit timeline?

**Vendor / third-party dependencies**
If the program involves a vendor, what are they responsible for delivering and by when? What is the escalation path if they slip?

**Regulatory / external dependencies**
Are there external deadlines - regulatory enforcement dates, contractual milestones, product launch dates - that constrain the schedule regardless of internal capacity?

---

## Step 5: Security Program Status Reporting

Security programs often have two reporting audiences with different needs: the engineering teams doing the work and the executives accountable for the risk.

### For engineering teams

Frequent, specific, operational. What is blocked, what is at risk, what decisions are needed this week. The RAID log is the primary tool here - review it every week, not just when something goes wrong.

### For executive stakeholders

Infrequent, high-signal, outcome-focused. Executives need to know whether the program is on track, what the current risk exposure is, and what - if anything - they need to do. They do not need sprint velocity numbers.

**A security executive update should answer three questions:**
1. Are we on track to achieve the security objective by the committed date?
2. What is the current risk exposure and how is it trending?
3. Is there anything that requires a decision or escalation at this level?

If the answer to question 3 is no, keep the update short. If the answer is yes, come with a recommendation, not just the problem.

### Status color for security programs

Use the same Red / Yellow / Green framework as any program, but be specific about what is driving the color:

- **Green** - Program is on track. Security objective will be achieved by committed date. No escalations needed.
- **Yellow** - Specific risk identified. Named mitigation in progress. May need executive support on [specific item].
- **Red** - Program is off track. Security objective is at risk by [specific date]. Recommend [specific action]. Need decision by [date].

Never go from Green to Red without a Yellow in between. If something went from fine to crisis without a Yellow, the reporting was wrong, not the program.

---

## Step 6: Vulnerability Remediation Programs

A specific type of security program worth calling out separately. Vulnerability remediation programs - fixing CVEs, closing audit findings, addressing penetration test results - have a different operational rhythm than build programs.

### What makes them different

- The scope is often not fully known at the start
- New findings can come in while existing ones are being remediated
- The "done" criteria is defined by the auditor or security team, not the engineering team
- Multiple engineering teams may be affected simultaneously with no prior coordination

### How to run them

**Triage first.** Not everything is equally urgent. Critical vulnerabilities with active exploits are not the same as medium-severity findings with no known exploit path. Define your severity framework upfront and apply it consistently.

**Assign owners at the service level.** Remediation programs fail when ownership is unclear. Every affected service should have a named engineering owner responsible for the fix, not just a team.

**Set MTTR targets by severity.** Mean Time to Remediate should be defined per severity level and tracked as the program's primary metric. This is what the security team and auditors will ask about.

**Track coverage, not just completion.** A vulnerability remediation program that closes 80% of findings is not 80% done - it depends on which 80%. Make sure your reporting shows coverage across services and severity levels, not just raw counts.

**Do not let perfect be the enemy of done.** A temporary workaround that reduces risk exposure while a permanent fix is engineered is a legitimate program output. Track it as such.

---

*Version 1.0. Propose changes via pull request.*
