# AI Tooling in Regulated Environments

**One rule overrides the adoption sequence: no regulated data reaches any tool before the agreement
covering it is signed and the tool is configured for it.**

General AI tool sequencing is covered in the
[AI Tool Decision Matrix](https://github.com/ChefPlex/ai-automations/blob/main/frameworks/ai-tool-decision-matrix.md).
That guidance is tool-agnostic and assumes ordinary business data. This document is the overlay for
environments handling PHI (Protected Health Information), and it changes the order.

---

## The rule that outranks the phases

**Confirm agreement availability before adoption, not after.**

A BAA (Business Associate Agreement) or equivalent data processing agreement must be in place, and
the tool configured under it, before any regulated data flows. This is not a phase. It is a gate
that sits in front of every phase.

⚠️ **A free tier is not automatically a compliant tier.** Many vendors offer a BAA on enterprise
plans and not on starter or consumer plans, and the product is otherwise identical. The same tool,
same interface, same login, and a completely different contractual position. **Check the plan, not
the vendor.**

Most enterprise-tier AI, security scanning, compliance automation and source control vendors offer
BAAs. Many workflow automation connectors, consumer tools and free tiers do not.

---

## What changes from the standard sequence

### Compliance automation moves from phase 5 to phase 1

In the general sequence, evidence automation is a late "evaluate." In a regulated environment it is
close to first, for a commercial reason rather than a compliance one: **audit readiness gates
enterprise sales.** A healthcare buyer asks for the SOC 2 report during procurement, not after. If
the answer is "in progress," the deal slows down at the exact moment it was ready to close.

Treat evidence collection as revenue infrastructure and it stops being a cost centre argument.

### Workflow automation needs a connector review

Workflow tools route data between systems, which makes them the least obvious and most likely place
for regulated data to end up somewhere it should not be. **Review each connector's data handling
and agreement posture before automating any flow that could carry regulated data.**

The failure here is rarely the main tool. It is the third integration in a five-step automation that
nobody audited because the automation was "just moving a status field."

### Maintain a data classification line

Map each tool to the **highest data class it may touch**: public, internal, confidential, PHI. Tools
without a signed agreement are capped at non-PHI use.

**Enforced, not assumed.** A classification that exists only in a spreadsheet is a description of
intent. It becomes a control when something actually prevents the crossing, and when someone would
find out if it were crossed.

---

## The human gate on ambiguity

**When it is unclear whether a payload contains regulated data, a human reviews it before it reaches
an AI or any third party.**

This connects directly to the execution tier model. In a regulated environment, **any activity whose
input might contain PHI cannot sit at tier 5**, regardless of how mechanical the work is, because
tier 5 removes the per-item human gate and the per-item judgment is exactly what is required.

The practical shape:

| Situation | Tier ceiling |
|---|---|
| Input definitely contains no regulated data | No constraint |
| Input may contain regulated data, classification is automatic and reliable | Tier 4, gate is real |
| Input may contain regulated data, classification requires judgment | Tier 3 or below |
| Output is a determination about a patient or a member | Tier 1 or 2 |

**A "maybe" is a human review.** The cost of reviewing a payload that turned out to be clean is a
few minutes. The cost of the reverse is a notification obligation.

---

## Checklist before adopting any AI tool in a regulated environment

1. **Is a BAA available on the plan we are actually buying?** Not the enterprise tier we might buy
   later. The one on the invoice.
2. **Is it signed and countersigned?** Available is not the same as executed.
3. **Is the tool configured for it?** Some vendors require specific settings, a specific
   deployment region, or zero-retention mode before their agreement applies. **Default settings are
   commonly not the compliant settings.**
4. **What is the highest data class this tool may touch?** Write it down and put it where the
   people using the tool will see it.
5. **What connectors does it reach through?** Each one inherits the question.
6. **Who finds out if regulated data goes somewhere it should not?** If the answer is nobody, the
   control does not exist yet.
7. **Is training or retention on our data disabled where required?** Verify against the vendor's
   current documentation, and record the date you checked.

---

## A note on scope

**This is a program management document, not legal advice.** It covers how to sequence and gate
tool adoption so a compliance review is straightforward rather than alarming. Determining which
agreements are required, and whether a specific configuration satisfies them, belongs with counsel
and your compliance function.

The value here is in **not arriving at that conversation late.** A tool already carrying regulated
data under no agreement is a remediation project. The same tool, gated before adoption, is a
five-minute approval.
