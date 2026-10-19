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

The permission-aware retrieval step is the one that gets designed last and should be designed first. Retrofitting it means reindexing, and sometimes means changing the vector store. Apply the filter inside the retrieval query, before ranking. Filtering a top-K result afterward leaves the user with whatever authorized scraps survived, which produces the over-filtering failure tested below.

**Structured data takes a second path, and the rule holds on it too.** Many answers combine documents with records queried live from a CRM, ERP, or data warehouse through SQL or an API ([how the two paths fit together](https://github.com/ChefPlex/ai-automations/blob/main/ai-programs/enterprise-rag-program/rag-architecture-primer.md#structured-and-unstructured-information)). That query must run with the requesting user's authority (row-level security, a user-scoped or delegated token), not the integration's service account. A service account that can read every row turns the structured path into the leak the document filter was built to stop, and the document-side test matrix won't catch it.

Running as the user is necessary and not sufficient. The structured path has its own ways around it:

- **Shared caches.** A response or semantic cache keyed on the question but not on the identity serves one user's rows to the next.
- **Objects that run with their owner's rights.** Definer-rights views, materialized extracts, and some semantic-model layers return rows the querying user could not select directly. Check what the query actually touches, not just who issued it.
- **Inference across rows.** Aggregates and differencing ("total for the team, minus total for everyone but Pat") can reveal values no single permitted row shows. Same problem as inference across documents, below.
- **Oversharing.** Row-level policy inherits whatever the source grants, exactly like document ACLs.
- **Model-written SQL.** When the model drafts the query, constrain it: a read-only role, a governed semantic layer or parameterized templates where possible, validation before execution, and row, time, and cost limits.

Some SaaS APIs offer no delegated or per-user token. Where a service account is genuinely unavoidable, record it as an exception with an owner, enforce the user's entitlements in the application layer before and after the call, narrow the account to the minimum it needs, and put that path in the permission test matrix. An exception written down and tested is a control. A "never" that is quietly ignored is not.

## Two Failures That Look Alike

Both end with the model saying something it shouldn't. They are different failures with different controls, and conflating them leads to fixing the wrong one.

| | Authorization leakage | Indirect prompt injection |
|:---|:---|:---|
| What happens | The system retrieves or exposes content the requester is not entitled to | The requester is entitled to the content, and the content contains text that tries to steer the model or agent |
| Access control | Broken, misconfigured, or inherited from an overshared source | Working exactly as designed |
| Where it is stopped | Permission filtering at retrieval, on both paths, before the model | Stopped by egress control, capability separation, and deterministic authorization of every action. Bounded, not stopped, by the permission filter: the injection can only reach what the user could |
| Example | A user asks about compensation and gets a chunk from an HR folder they can't open | A user asks for a summary of a shared doc, and a sentence in it tells the model to email the summary elsewhere |

They also compound. An injection that reaches a tool running on a service account, rather than as the user, becomes authorization leakage as well: the confused deputy in the threat model.

The working rule for the second one:

> Treat retrieved content as untrusted data even when the user is authorized to read it.

Authorization answers "may this person see this." It says nothing about whether the text is safe to act on. A document the user is fully entitled to can still carry an attacker's instructions, because the attacker only needed write access to something that got indexed. Detail and test cases: [Prompt Injection Threat Model](prompt-injection-threat-model.md).

## The Model Is Never the Security Boundary

The model can propose a query, a tool call, an email, a record update, or a workflow step. It cannot be the thing that decides whether that action is allowed, because anything in its context, including an injected sentence, can influence what it proposes.

> Agents propose; deterministic systems dispose.

In practice that means every action the model proposes passes through enforcement points outside the model. The first five below are deterministic: they do not read natural language and cannot be talked out of a decision. Approval is deterministic only in what triggers it and what it binds to; the judgment is a person's. Output validation is not deterministic at all. Both are layered on top, never relied on alone.

| Enforcement point | What it decides | Deterministic |
|:---|:---|:---|
| Authorization service or policy engine | Whether this user may perform this action on this resource | Yes |
| IAM, scoped OAuth tokens, capability tokens | What the credential behind the call can physically reach | Yes |
| Database access controls, row-level security | Which rows a structured query returns | Yes |
| API or tool gateway, with session data-flow rules | Which tools exist, with which parameter constraints, enforced in code outside the tool schema; which combinations are denied in one session (for example, read restricted data then send anything outbound) | Yes |
| Egress proxy or allowlist | Where data may go: rendered URLs, images, outbound calls. The load-bearing control against injection-driven exfiltration | Yes |
| Approval workflow | Whether a human confirms a consequential, rule-triggered action, shown raw (see [Human Approval](prompt-injection-threat-model.md#human-approval)). A person reads natural language, so the trigger and the binding to the exact action must be deterministic even though the judgment is not | Trigger and binding only |
| Output validation (DLP, PII, content classifiers) | Whether the response or outbound payload looks like it should leave | No: an alarm, as in the threat model's Model layer |

Separating system instructions, application instructions, the user's request, and retrieved content into distinct, labelled parts of the prompt is good architecture: it reduces ambiguity and makes injection harder. It is not enforcement. Natural-language data can still influence the model however it is labelled, so instruction separation is defense in depth, never the control the system relies on.

## A Blocked Action Is a Signal

When an enforcement point denies something, the control worked. The denial is still evidence, and it has to be logged and looked at.

Be precise about what is observable. Permission filtering inside the retrieval query, and row-level security, work by never returning what the user can't see. They are silent by design, so "a retrieval denied by an ACL" is usually not an event that exists. Don't build detection on it. What does exist:

- a request outside the token's scope, or a denial from the policy engine or tool gateway
- an attempted tool call the user or agent is not authorized for
- an explicit request for a specific document, record, or ID outside the user's permissions
- a query whose authorized result set is empty where the question clearly expected an answer, repeated (boundary probing)
- known injection patterns in retrieved content or input
- a request from one agent to another that the design doesn't expect
- a request to change the session's scope, role, or identity, or to act as another user (what "privilege escalation" means in this system)
- an outbound destination blocked by the egress allowlist
- a response or outbound payload flagged by output validation

Do not expose, even in logs readable by the user, how many results were filtered out. A count of hidden hits is itself a side channel.

A single denial has several possible causes, and the response differs for each: malicious prompt injection, buggy orchestration, a model error, a policy misconfiguration, or a user asking for something they genuinely aren't entitled to. Only the log tells them apart. A system that blocks silently has prevention and no detection, and the first sign of a sustained injection campaign will be the one attempt that got through.

| Stage | What it means here |
|:---|:---|
| Prevent | Retrieval filtering, run-as-user queries, deterministic authorization of actions, egress allowlist |
| Detect | Denials, scope violations, injection signatures, and boundary probing logged with identity, retrieved document IDs, and the proposed action, and alerted on patterns. These logs hold sensitive arguments and possibly the exfiltration payload itself, so they get their own access control and retention period |
| Respond | A runbook that distinguishes attack from bug, traces a denial back to the document that triggered it, pulls the poisoned document from the index, purges affected caches and memory, and revokes tokens issued to the session ([Security Incident Response Template](../security-incident-response-template.md)) |

## Required Controls

| Control | Notes specific to RAG |
|---|---|
| SSO | Standard. The identity flowing into retrieval must be the real end user, never a service account, except a recorded and tested exception on the structured path (see above). |
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
| Structured queries run as the user | SQL and API calls made for the user use row-level security or a user-scoped token, not the integration's service account; unavoidable service accounts are recorded exceptions with application-layer enforcement. Caches are keyed by identity. Included in the permission test matrix. |
| Chunk metadata and provenance | Every chunk carries its source, author, ingestion date, classification, access-control attributes, and effective date. The access attributes are what the permission filter reads; classification drives handling and retention; the effective date keeps a superseded policy from answering. A check confirms each cited chunk supports the claim it is cited for. The full set of fields: the [primer's knowledge card](https://github.com/ChefPlex/ai-automations/blob/main/ai-programs/enterprise-rag-program/rag-architecture-primer.md#a-chunk-is-a-knowledge-card). |
| Security telemetry on denials | Every observable denial (policy engine, tool gateway, token scope, egress, explicit out-of-permission requests) is logged with identity and context, and alerted on patterns. See [A Blocked Action Is a Signal](#a-blocked-action-is-a-signal). |
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
- [ ] Structured (live-query) path verified in the same matrix: queries run as the user, caches keyed by identity, any service-account exception recorded and tested
- [ ] Every action the model can propose is authorized by a deterministic enforcement point, not by the model or the prompt
- [ ] Observable denials (policy, gateway, scope, egress) are logged and alerted on, in logs with their own access control
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

- [Enterprise RAG Program Playbook](https://github.com/ChefPlex/ai-automations/tree/main/ai-programs/enterprise-rag-program) - the full program this workstream sits inside
- [Security Program Intake and Kickoff](../security-program-intake-kickoff.md)
- [Compliance Framework Reference](../compliance-framework-reference.md)
- [Security TPM Role Guide](../security-tpm-role-guide.md)
