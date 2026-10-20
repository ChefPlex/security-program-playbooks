# Security Program Intake and Kickoff Guide

**Everything a program needs is in the general templates. Start there:** the [Program Charter](https://github.com/ChefPlex/tpm-templates/blob/main/program-charter-template.md), the [Program Kickoff Checklist](https://github.com/ChefPlex/tpm-templates/blob/main/program-kickoff-checklist.md), the [Program Phases Playbook](https://github.com/ChefPlex/tpm-templates/blob/main/program-phases-playbook.md), the [RAID Log Guide](https://github.com/ChefPlex/tpm-templates/blob/main/raid-log-guide.md) and the [Program Status Reporting Framework](https://github.com/ChefPlex/program-reporting-frameworks/blob/main/program-status-reporting-framework.md). The [I need to…](https://github.com/ChefPlex/tpm-templates/blob/main/PORTFOLIO-GUIDE.md) guide has the reading path.

**This page is only what to consider on top of them when the program is a security program.** Security programs fail for the same reasons any program fails - unclear scope, missing owners, dependencies nobody mapped, and compliance requirements that showed up too late to build into the design. The difference is that the blast radius when something goes wrong tends to be larger and more visible.

---

## Things to Consider for a Security Program

### 1. Start with the problem, not the solution

"We need to implement MFA" is a solution. "40% of our high-privilege accounts have no second factor and three of our peer companies have been breached through credential theft in the last six months" is a problem. Start the charter with the problem.

Be specific about the risk of not doing it. "Increased risk" is not useful. "A successful credential stuffing attack against our administrative accounts could result in unauthorized access to customer data across all tenants" is useful. Quantifying the risk - even roughly - helps with prioritization and resource justification. A worked example is in the [sample security intake](examples/sample-security-intake.md).

### 2. Name the compliance driver and the reporting clocks at intake

Security programs often have external forcing functions - a compliance deadline, an audit finding, a regulatory change, a contractual obligation. Identify them at intake. They drive timeline constraints that are non-negotiable, and they need to be built into the plan from day one, not retrofitted later.

If the program touches systems covered by an incident reporting clock (SEC Form 8-K Item 1.05, NIS2, DORA, the EU Cyber Resilience Act, HIPAA breach notification, NYDFS Part 500), name them at intake too. They change what logging, detection, and escalation the program has to deliver. The [Cyber Incident Management Playbook](cyber-incident-playbook.md#regulatory-reporting-clocks) lists the clocks.

### 3. Triage the reviews at intake, and start them early

This is the step most programs get wrong. Compliance and security reviews take time - sometimes weeks, sometimes months. Starting them late is one of the most common reasons security programs miss their deadlines. At intake, triage which reviews apply. You do not need to complete them yet. You need to know they're coming so you can sequence them into the plan.

| Review Type | When to Initiate | What It Covers |
|:---|:---|:---|
| Compliance Assessment | At intake | Determines which regulatory frameworks apply (SOX, PCI, HIPAA, FedRAMP, SOC 2, etc.) and what controls are required |
| Security / Architecture Review | During Definition & Planning | Threat model the proposed solution; identify security requirements before build begins |
| Privacy / Legal Review | During Definition & Planning | Required for any program touching customer data, AI/ML systems, or new data collection |
| AI Risk Review | During Definition & Planning | Required when the program builds, buys, or embeds an AI system or model. Classifies the use against the [EU AI Act](https://ai-act-service-desk.ec.europa.eu/en/ai-act/timeline/timeline-implementation-eu-ai-act) risk tiers where it applies, and assesses it against the [NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework) or [ISO/IEC 42001](https://www.iso.org/standard/42001) if the organization has adopted one |
| Penetration Testing | Pre-production | Required for internet-facing services or significant changes to existing ones |
| Vendor / Third-Party Review | Before vendor work begins | Security and procurement review for any buy decision |
| Change Advisory Board (CAB) | Before production changes | Required for infrastructure changes in regulated environments |

**The rule:** if a review could block your go-live date, initiate it at least two phases before you need the result. Waiting until you need it is not a plan.

### 4. Name three owners, and add the security roles

There's a difference between the executive sponsor, the product owner, and the engineering owner. All three need to be identified before a program starts. A security program with no clear business owner will drift.

Security programs add roles the general RACI does not always have:

| Role | Responsibilities |
|:---|:---|
| Security Architect | Threat modeling, architectural review, security requirements |
| GRC / Compliance | Regulatory requirements, audit coordination, control evidence |
| Legal / Privacy | Privacy review, contractual obligations, policy updates |

The TPM doesn't own the security domain knowledge - that belongs to the security architects, engineers, and GRC team. What the TPM owns is the program itself. What that requires is covered in [The Security TPM Role](security-tpm-role-guide.md#what-a-security-tpm-owns).

### 5. Map the security dependencies

Security programs touch more teams than most. A TLS modernization program affects every service that makes network calls. An encryption-at-rest program affects every service that writes to storage. Map dependencies before you start, not after you hit one. Beyond the usual engineering and vendor dependencies, look for:

- **Platform services** the program relies on: certificate management, key management, identity, network configuration. Who owns them?
- **Compliance evidence:** what has to be collected, by whom, and on what audit timeline?
- **External deadlines:** regulatory enforcement dates and contractual milestones that hold regardless of internal capacity.

### 6. Report to two audiences, and say what drives the color

Security programs often have two reporting audiences: the engineering teams doing the work, who need what is blocked and what is decided this week, and the executives accountable for the risk. Use the [status reporting framework](https://github.com/ChefPlex/program-reporting-frameworks/blob/main/program-status-reporting-framework.md) for both.

**A security executive update should answer three questions:**
1. Are we on track to achieve the security objective by the committed date?
2. What is the current risk exposure and how is it trending?
3. Is there anything that requires a decision or escalation at this level?

If the answer to question 3 is no, keep the update short. If the answer is yes, come with a recommendation, not just the problem.

Use the colors as defined in the [Program Status Reporting Framework](https://github.com/ChefPlex/program-reporting-frameworks/blob/main/program-status-reporting-framework.md#status-color-definitions), the same as any program. What a security program adds is saying what drives the color: a **delivery risk** (a team is behind on its part) or a **security risk** (an exposure stays open until that part lands). The executive needs to know which, because the second one is risk the company is carrying today.

### 7. Vulnerability remediation runs differently

The scope is often not known at the start, new findings arrive while old ones are being fixed, and "done" is defined by the auditor or the security team. Use the [Vulnerability Remediation Runbook](vulnerability-remediation-runbook.md) rather than the general phases.

---

## Add These Rows to the Kickoff Checklist

| What happens | Artifact / outcome | Owner | Level |
|:---|:---|:---|:---|
| Identify the regulatory or compliance driver | Driver named in the charter | TPM + GRC | Must |
| Identify the reportable-incident regimes in scope | Regimes and their clocks listed | GRC + Legal | Must |
| Triage which security, privacy, AI and compliance reviews apply | Review list with start dates | TPM | Must |
| Name the security architect, GRC and legal / privacy contacts | Names in the RACI | TPM | Should |

---

*Version 1.2. Last reviewed October 2026. Propose changes via pull request.*

*Part of the [ChefPlex TPM portfolio](https://github.com/ChefPlex/tpm-templates/blob/main/PORTFOLIO-GUIDE.md). Maintained by [Eric White](https://www.linkedin.com/in/edwhite). Licensed under [CC BY 4.0](LICENSE).*
