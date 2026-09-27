# Enterprise RAG Security

The security workstream for a Retrieval-Augmented Generation program.

A RAG system answers questions using company data, on behalf of a specific person, from a corpus that person has partial access to. That combination is why it needs its own security treatment rather than a standard application review.

| Document | What It Is |
|---|---|
| [RAG Security Playbook](rag-security-playbook.md) | Trust boundary, the required control set, the common failure modes, and the sign-off checklist for general availability. |
| [Prompt Injection Threat Model](prompt-injection-threat-model.md) | Direct and indirect injection, memory and tool poisoning, a test corpus you can run in CI, layered mitigations, and red team scope. |

## The Rule

> The system can never retrieve information the requesting user could not have accessed directly.

Filtering happens before content reaches the model, never after. Once unauthorized content is in the context window it has influenced the answer, and the model can be talked into revealing what it was given.

The rule bounds what can leak. It does not stop data the user is entitled to from being sent out by an injected instruction. That takes egress control and capability separation, covered in the threat model.

## Why These Incidents Are Found Late

The failure modes in the playbook share a property. A retrieval quality bug produces a bad answer, and a user reports it. An access-control bug produces a good answer, delivered confidently, to someone who should never have seen it.

Nothing in the user experience signals a problem. Nobody files a ticket. These get found in audits, which is why the escalation trigger on every one of them is a single occurrence rather than a threshold.

## Framework Mapping

The threat model cites these by ID where each threat appears.

| Framework | Entries used here |
|---|---|
| [OWASP Top 10 for LLM Applications 2025](https://genai.owasp.org/llm-top-10/) | LLM01 Prompt Injection, LLM02 Sensitive Information Disclosure, LLM06 Excessive Agency, LLM07 System Prompt Leakage, LLM08 Vector and Embedding Weaknesses, LLM10 Unbounded Consumption |
| [OWASP Top 10 for Agentic Applications](https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/) | ASI01 Agent Goal Hijack, ASI02 Tool Misuse, ASI03 Identity & Privilege Abuse, ASI04 Agentic Supply Chain Vulnerabilities, ASI06 Memory & Context Poisoning |
| [MITRE ATLAS](https://atlas.mitre.org/) | LLM Prompt Injection (AML.T0051), RAG Poisoning (AML.T0070), False RAG Entry Injection (AML.T0071), AI Agent Context Poisoning (AML.T0080), AI Agent Tool Poisoning (AML.T0110), AI Supply Chain Rug Pull (AML.T0109), LLM Response Rendering (AML.T0077), Exfiltration via AI Agent Tool Invocation (AML.T0086), Extract LLM System Prompt (AML.T0056), Cost Harvesting (AML.T0034). Case study: EchoLeak (AML.CS0059). |
| [NIST AI 600-1](https://doi.org/10.6028/NIST.AI.600-1) | Artificial Intelligence Risk Management Framework: Generative Artificial Intelligence Profile (July 2024), the GenAI companion to the AI RMF. Use it to place this workstream inside an organization's AI risk program. |

## Program Context

The full program these sit inside, with phases, workstreams, milestones, and templates:

**[Enterprise RAG Program](https://github.com/ChefPlex/tpm-templates/tree/main/enterprise-rag-program)**

## Related In This Repo

- [Security Program Intake and Kickoff](../security-program-intake-kickoff.md)
- [Compliance Framework Reference](../compliance-framework-reference.md)
- [Security TPM Role Guide](../security-tpm-role-guide.md)
- [Security Incident Response Template](../security-incident-response-template.md)
- [Vulnerability Remediation Runbook](../vulnerability-remediation-runbook.md)
