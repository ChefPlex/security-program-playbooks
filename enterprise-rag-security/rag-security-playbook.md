# Enterprise RAG Security Playbook

The security workstream for a Retrieval-Augmented Generation program. Written for the security TPM or security engineer who owns sign-off on a system that answers questions using company data.

Make this a workstream from week one. Run as a review gate at week 18, it produces a system that works and can't ship, and a schedule nobody can recover.

## The Rule Everything Else Serves

> The system can never retrieve information the requesting user could not have accessed directly.

Everything below is in service of that sentence. If a control doesn't help hold it, it's hygiene rather than architecture.

The corollary matters just as much: **authorization filtering happens before content reaches the model.** Filtering the response afterward isn't a control. Once unauthorized content is in the context window, it has influenced the answer whether or not it appears in the output, and the model can be talked into revealing what it was given.

## Trust Boundary

```text
   USER
     |
     v
   SSO / IAM                     authenticate the human
     |
     v
   Authorization                 what is this user entitled to
     |
     v
   RAG Application
     |
     v
   Permission-aware Retrieval    filter HERE, before the model
     |
     v
   LLM                           only ever sees authorized content
     |
     +--> Tool Calls             run as the user, authorized per call
     |
     v
   Response Filtering            DLP, PII, safety
     |
     v
   Egress                        only allowlisted URLs, images, destinations
     |
     v
   Audit Log                     query, documents, tool calls, identity, outcome
```

Retrieved text and tool output are data, never instructions. The model will still read instructions in them, so every step after the model assumes it may have been steered.

The permission-aware retrieval step is the one that gets designed last and should be designed first. Retrofitting it means reindexing, and sometimes means changing the vector store.

## Required Controls

| Control | Notes specific to RAG |
|---|---|
| SSO | Standard. The identity flowing into retrieval must be the real end user, never a service account. |
| IAM | Service accounts that read source systems commonly have broader access than any individual user. That gap is where leakage lives. |
| RBAC / ABAC | Whichever the source systems use. The index has to be able to represent it. |
| Document-level ACLs | Per-document, resolvable at query time. Collection-level permissions are not sufficient for most enterprise corpora. |
| Encryption in transit | Standard. |
| Encryption at rest | Includes the vector store. Embeddings are derived from the source content and leak information about it. |
| Secrets management | Model provider keys, source system credentials. |
| Audit logging | Query, retrieved document IDs, requesting identity, and what was returned. Without retrieved document IDs an incident cannot be investigated. |
| Tenant isolation | If multi-tenant, at the index level rather than by query filter alone. |
| DLP | On the response path. |
| PII detection | On ingestion and on the response path. |
| Data classification | Before ingestion, never after. |
| Retention controls | Retrieval indexes are a copy of the corpus and inherit its retention obligations. |
| Deletion workflows | A document deleted at source must leave the index inside a defined window. |
| Model-provider data controls | What the provider retains, whether it trains on submissions, where it processes. Legal signs this. |
| Egress allowlist | The client fetches or renders only URLs and images on an allowlist. Tool calls that send data out go to named destinations only. |
| Chunk provenance | Every chunk carries its source, author, and ingestion date. A check confirms each cited chunk supports the claim it is cited for. |
| Agent and tool inventory | Every tool, connector, and MCP server the system can call, its permissions, its pinned version, and a named human owner. |

## Access-Control Failure Modes, and the Injection That Bypasses Them

These are the common failure modes. The first five are access-control failures. The sixth happens with access control working. They share a property worth naming: none of them looks like a failure to the user.

**Oversharing at source.** The source systems already grant more than anyone intended: a folder shared with the whole company, a site nobody locked down. The index inherits those permissions faithfully, and retrieval makes the overshare findable in one question. Filtering is working as designed. Review source permissions on the highest-sensitivity collections before ingestion.

**Service account over-permission.** The ingestion account can read everything, so everything is indexed, and per-user filtering is the only thing standing between a user and the whole corpus. When filtering has a bug, the failure is total rather than partial.

**Permission drift.** A user changes teams. Source system access updates immediately. The index doesn't, and keeps answering from documents they can no longer open. Test propagation and monitor the window.

**Deletion that doesn't propagate.** A document is deleted at source for a reason, often a legal one, and remains retrievable because deletion was never wired to the index.

**Embedding leakage.** Embeddings are derived from content. An unprotected vector store is a partial copy of the corpus, and it tends to be treated as infrastructure rather than as data.

**Injection-driven exfiltration.** A retrieved document carries instructions, the model follows them, and data the user was entitled to leaves through a rendered link, an image, or a tool call. This is the best-documented class in public: [EchoLeak](https://arxiv.org/abs/2509.10540) ([CVE-2025-32711](https://www.cve.org/CVERecord?id=CVE-2025-32711)) exfiltrated Microsoft 365 Copilot data this way with access control intact. Retrieval filtering limits what can leak. Only egress control and capability separation stop it leaking. See the [Prompt Injection Threat Model](prompt-injection-threat-model.md).

Every one of these delivers data, confidently and in a well-formed answer, to someone who should not have it. In the sixth, that someone is not even in the conversation. Nobody files a ticket, which is why they're found in audits rather than in support queues, and why the escalation trigger is a single occurrence rather than a threshold.

## Security Testing Scope

Prompt injection has its own document because it's its own threat model: [Prompt Injection Threat Model](prompt-injection-threat-model.md).

Beyond injection, test:

- Cross-user data leakage
- Sensitive information disclosure through inference across multiple authorized documents
- Malicious or poisoned documents inside the corpus
- Tool misuse, where the system can act rather than only answer, including poisoned tool descriptions and memory that persists across sessions
- Model-provider leakage
- Permission propagation, both grants and revocations
- Deletion propagation
- Over-filtering: when permission filtering removes the documents that answer a question, the system says it cannot answer. It never builds an answer from the partial set that is left.
- Citations: each cited chunk supports the claim, and false statements planted in the corpus are caught by provenance, since they carry no instruction for a scanner to find

The inference case is the subtle one. A user may be entitled to twenty documents individually while the synthesis of all twenty is something they were never meant to be able to assemble. Worth a conversation with whoever owns data governance, and worth deciding deliberately rather than discovering.

## Sign-Off

Security cannot approve general availability until:

- [ ] Identity flows end to end, real user identity reaches retrieval
- [ ] Permission-aware retrieval verified against a test matrix of users and documents
- [ ] Permission changes propagate within the agreed window, tested both directions
- [ ] Deletion propagates within the agreed window, tested
- [ ] Vector store encrypted and access-controlled as data, not as infrastructure
- [ ] Audit log captures query, retrieved document IDs, and identity
- [ ] Over-filtered queries refuse rather than answer from what is left, tested
- [ ] Egress allowlist enforced for rendered URLs, images, and outbound tool calls
- [ ] Agent and tool inventory complete, every entry with a named owner
- [ ] Direct and indirect injection suite passes, each case run repeatedly, against a corpus refreshed with adaptive attacks. A pass is a regression floor, not proof of resistance.
- [ ] Red team complete, findings closed or formally accepted with a named approver
- [ ] Residual injection risk accepted in writing by a named approver
- [ ] Model-provider data terms reviewed and accepted by legal
- [ ] Kill switch exists and has been tested

## Related

- [Enterprise RAG Program Playbook](https://github.com/ChefPlex/tpm-templates/tree/main/enterprise-rag-program) - the full program this workstream sits inside
- [Security Program Intake and Kickoff](../security-program-intake-kickoff.md)
- [Compliance Framework Reference](../compliance-framework-reference.md)
- [Security TPM Role Guide](../security-tpm-role-guide.md)
