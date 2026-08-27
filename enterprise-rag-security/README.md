# Enterprise RAG Security

The security workstream for a Retrieval-Augmented Generation program.

A RAG system answers questions using company data, on behalf of a specific person, from a corpus that person has partial access to. That combination is why it needs its own security treatment rather than a standard application review.

| Document | What It Is |
|---|---|
| [RAG Security Playbook](rag-security-playbook.md) | Trust boundary, the required control set, the four failure modes that actually happen, and the sign-off checklist for general availability. |
| [Prompt Injection Threat Model](prompt-injection-threat-model.md) | Direct and indirect injection, a test corpus you can run in CI, layered mitigations, and red team scope. |

## The Rule

> The system can never retrieve information the requesting user could not have accessed directly.

Filtering happens before content reaches the model, never after. Once unauthorized content is in the context window it has influenced the answer, and the model can be talked into revealing what it was given.

## Why These Incidents Are Found Late

The four failure modes in the playbook share a property. A retrieval quality bug produces a bad answer, and a user reports it. An access-control bug produces a good answer, delivered confidently, to someone who should never have seen it.

Nothing in the user experience signals a problem. Nobody files a ticket. These get found in audits, which is why the escalation trigger on every one of them is a single occurrence rather than a threshold.

## Program Context

The full program these sit inside, with phases, workstreams, milestones, and templates:

**[Enterprise RAG Program](https://github.com/ChefPlex/tpm-templates/tree/main/enterprise-rag-program)**

## Related In This Repo

- [Security Program Intake and Kickoff](../security-program-intake-kickoff.md)
- [Compliance Framework Reference](../compliance-framework-reference.md)
- [Security TPM Role Guide](../security-tpm-role-guide.md)
- [Security Incident Response Template](../security-incident-response-template.md)
- [Vulnerability Remediation Runbook](../vulnerability-remediation-runbook.md)
