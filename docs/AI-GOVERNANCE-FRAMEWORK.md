# AI Governance Framework

| Field | Value |
|---|---|
| Version | 1.1.1 |
| Type | Public reference framework for enterprise AI governance |
| Last updated | 2026-04-29 |
| Author and maintainer | Muhammad Elsaiedy |
| Contact | aigovernance@ccoe.io |
| Audience | Platform engineering, application security, application development, ML/AI engineering, product, risk, compliance, AI and CISO practitioners adapting this framework for their organisation |
| Applies to | All AI systems designed, deployed, operated, embedded, or consumed by an adopting organisation, including third-party SaaS features that use AI server-side on the organisation's data |

## About this framework

This is a public reference framework for governing generative and agentic AI systems in an enterprise context. It is written to be adapted by any AI or CISO practitioner working on AI governance in their organisation, regardless of sector, cloud stack, or regulatory posture. It is deliberately engineering-first: its spine is a layered control model, a four-seam threat model, a data-governance discipline, and a lifecycle. External standards (NIST, ISO, OWASP, supervisory guidance) anchor the framework in Appendix D; they do not structure it, because the controls must stand on their own before they are mapped to anyone else's taxonomy.

**This framework is technical and governance reference material. It is not legal, regulatory, audit, or licensing advice.** Adopters apply it in conjunction with their own legal, privacy, risk, internal-audit, and supervisory functions. Where the framework cites jurisdictional regimes or named obligations, those references are illustrative; current applicability and timing are determined by the adopter's legal desk, not by this document.

The framework is normative where controls can be made concrete today and honest where they cannot. Section 20 is an explicit register of the edges — controls the framework asserts that current industry primitives do not fully support, and disciplines that are deferred to later maturity stages. A governance document that pretends its gaps are closed is worse than one that names them, and the register is the primary input to the scoped workstreams that keep the framework living.

### Adoption note

This framework is reference material, not an approved control policy for any specific organisation. Before operational use, an adopter assigns a document owner, an attestation executive, and a Risk Committee; tailors the default bounds (Appendix B), the registration schema (Appendix A), and the control-test matrix (Appendix G) to its own environment; and applies a legal-desk review covering supervisory scope, data-protection regime, and any sector-specific obligations. Throughout the document, "the organisation" refers to whatever entity is adopting the framework.

**Risk Committee** as used here means the adopting organisation's designated AI-risk approval body, sized appropriately to its regulatory context. It may be a formal board-level risk committee, an executive AI-risk council, a security council, a single executive risk owner, or any equivalent governance forum that holds approval authority for Tier 1 AI systems and exception decisions. Smaller organisations are expected to consolidate the role; larger or regulated organisations are expected to formalise it.

Normative language follows RFC 2119: **MUST** / **MUST NOT** are requirements, **SHOULD** / **SHOULD NOT** are strong recommendations, **MAY** is permissive. In this public reference version, normative language defines the requirements that bind an adopter that has chosen to implement this framework as policy, not legal obligations imposed by this document on any reader. Adopters apply normative statements to themselves after local tailoring; the framework itself does not bind any party.

### How to cite

> AI Governance Framework (2026), v1.1.1, by Muhammad Elsaiedy. Reference framework for generative and agentic AI governance. Published under CC BY-SA 4.0.

**Short form:** Elsaiedy, M. (2026). *AI Governance Framework*, v1.1.1. CC BY-SA 4.0.

**Errata, contributions, and adoption questions:** aigovernance@ccoe.io

This document is published under Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0). Adaptations preserve attribution to the author and the share-alike obligation per the licence terms. Adopters separately determine the licensing and confidentiality posture of any internal policy or implementation derived from this text — for example, a private internal policy may be circulated within the adopting organisation while the source framework remains under CC BY-SA 4.0.

---

## 1. Purpose and scope

### 1.1 Purpose

The framework defines the controls, artefacts, and accountabilities required to operate AI systems safely, measurably, and at enterprise scale. It governs first-party AI systems the organisation builds, third-party models the organisation invokes, AI features embedded inside SaaS products the organisation consumes, coding agents operating against the organisation's code and infrastructure, and agent-to-agent interactions both within and across trust boundaries.

### 1.2 Scope

**In scope:**
- First-party AI systems the organisation builds (agents, runtime-context-retrieval applications including RAG and live-source patterns, fine-tuned models, embeddings-driven retrieval).
- Third-party foundation models the organisation invokes via hosted inference services, and open-weight models the organisation hosts itself.
- AI features embedded in SaaS products the organisation pays for (productivity copilots, customer-relationship and service-desk assistants, collaboration assistants, analytics copilots, and equivalents).
- Coding agents and AI assistants used by engineers against the organisation's code and infrastructure.
- Agent-to-agent interactions, including agents calling agents within and across trust boundaries.

**Out of scope:**
- Classical machine-learning models where generative or agentic AI is not involved. These remain governed by the organisation's existing model-risk-management standard. This framework governs GenAI and agentic AI specifically; the two regimes are complementary and a system using both is governed by whichever controls are more stringent for each component.
- Vendor-internal AI that has no access to the organisation's data and produces no output the organisation consumes. These are treated as ordinary third-party software.

### 1.3 Prohibited uses

The organisation does not build, deploy, or materially enable AI systems whose design falls into any of the following classes. These prohibitions apply regardless of jurisdiction and regardless of whether a specific regulation would require them.

- Systems that use subliminal, manipulative, or deceptive techniques to materially distort a person's behaviour against their interests.
- Systems that exploit vulnerabilities of specific groups (age, disability, socioeconomic) in a way that materially distorts behaviour.
- Social scoring of natural persons based on general-purpose behaviour or personality inference used for detrimental or disproportionate treatment.
- Real-time remote biometric identification of natural persons in publicly accessible spaces, outside narrowly scoped security or law-enforcement use cases with a lawful basis.
- Untargeted scraping of facial images or biometric data from the internet or CCTV to build or expand recognition databases.
- Emotion inference in the workplace or in education, outside safety or medical necessity with a lawful basis.
- Predictive policing of individuals based solely on personality traits, characteristics, or profiling.
- Inferring protected characteristics (race, political opinion, trade-union membership, religious or philosophical beliefs, sex life, sexual orientation) from biometric data for the purpose of classifying individuals.

Proposals that touch any of these classes are refused at intake. A deployed system later discovered to fall into one of these classes is withdrawn and the incident reported through the AI-specific incident catalogue (§12).

The prohibited-use list is policy-owned — jointly maintained by the adopting organisation's legal, privacy, risk, and AI-governance functions. Any proposed exception requires a legal determination of lawful basis, Risk Committee approval, and executive sign-off. No exception is self-approved by a product or engineering team. A narrowly scoped exception for one prohibited class (e.g. security or fraud-prevention use of identity verification) does not extend to any other class.

---

## 2. Design principles

Eight principles every section of this framework reduces to. A proposed control that cannot be justified against one of them is probably cosmetic.

1. **Defence in depth.** Two or three layers in the request path, plus posture management, plus observability, plus shadow-AI discovery. No single layer stops an adversary; no single product covers the request path end-to-end.
2. **The control is durable; the product is interchangeable.** Name capabilities, not vendors. A framework tied to a specific product is obsolete on the next procurement cycle.
3. **Registered or it does not exist.** An AI system absent from the registry is treated as unknown risk and subject to removal. This is the single most load-bearing rule in the framework.
4. **Bounds are the blast-radius cap.** Every agent runs inside explicit wall-clock, step, tool-call, token, cost, and action bounds. Defaults apply unless an exception is granted by a named authority.
5. **Identity and attribution are first-class.** Every AI action resolves to a human principal, an agent workload identity, a model version, and a tool set — all at invocation time, all in the log.
6. **Untrusted input is untrusted everywhere; authorisation is the source of truth.** User prompts, retrieved content, tool-call results, memory reads, and agent-to-agent messages are all untrusted. Retrieval must preserve source-system authorisation at query time, not rely on index-time filtering.
7. **Honest gaps beat false comfort.** Where a control class does not work, the framework says so. Section 20 is the register. Pretending a gap is closed is how organisations end up surprised.
8. **Least-Agency.** Avoid unnecessary autonomy. Deploying agentic behaviour where a deterministic automation, workflow, or API call would do expands the attack surface without adding value. Least-Agency is the complement to Least-Privilege: Least-Privilege constrains what an action can touch; Least-Agency constrains whether an agent should take the action at all.

---

## 3. Control model — the layers

The organisation's AI deployment spans twelve control layers. No single vendor's stack covers them all. Every layer has an owner, a primary implementation, and a stated gap.

| # | Layer | What it controls | Examples of capability |
|---|---|---|---|
| 1 | **Inference** | Model selection, routing, tenancy, data residency | Hosted foundation models via Bedrock, Azure OpenAI, Vertex AI; self-hosted open-weight serving (vLLM, TGI, SGLang) |
| 2 | **Gateway / traffic governance** | Key management, rate and token and cost budgets, guardrail routing, PII redaction, per-team quotas, auditable logging of every call | Cloud-native API gateways with WAF; AI-specific gateways (LiteLLM, Portkey, Kong AI Gateway, Cloudflare AI Gateway) |
| 3 | **Guardrails / content safety** | Prompt-attack detection, PII, denied topics, grounding checks, output filtering | Bedrock Guardrails, Azure Content Safety, Google Model Armor; specialist (Lakera, Protect AI, NeMo Guardrails, Cisco AI Defense, Palo Alto AI Runtime Security, Invariant Guardrails) |
| 4 | **Agent runtime** | Session isolation, workload identity, memory, tool mediation, sandboxed code and browser execution | Bedrock AgentCore, Azure AI Foundry Agent Service, Vertex AI Agent Builder; LangGraph, CrewAI, AutoGen, Anthropic Claude Agent SDK, OpenAI Agents SDK, AWS Strands, Pydantic-AI, Temporal for orchestration |
| 5 | **Tool / MCP layer** | Tool-definition pinning and signing, drift detection, allow-listing, per-tool auth scopes | MCP gateways, signed-manifest registries, tool-scan tooling, custom allow-lists |
| 6 | **Evaluation** | Pre-deployment quality, safety, and regression testing; context-retrieval quality (relevance, freshness, authorisation correctness, provenance, faithfulness); online evaluation | Bedrock Model Evaluation, Azure AI Evaluation, Vertex Eval; Braintrust, Arize Phoenix/AX, Galileo, Patronus, Promptfoo, DeepEval, Ragas, Garak, PyRIT |
| 7 | **LLM observability** | Prompt and response archive, decision-evidence trace, tool-call graph, cost attribution | Cloud-native monitoring; Langfuse, Arize Phoenix, Datadog LLM Observability, LangSmith, Helicone, Braintrust |
| 8 | **AI-SPM / posture** | Inventory of AI assets, exposure, toxic combinations, data lineage | Wiz AI-SPM, Orca, Prisma AIRS, Apiiro, Endor Labs |
| 9 | **Runtime protection** | Inline detection, tool-graph anomaly, blocking | Cisco AI Defense, Palo Alto AI Runtime Security, Lakera inline, Protect AI Recon |
| 10 | **Shadow-AI discovery** | Unsanctioned-AI detection, enumeration of SaaS-embedded features | Harmonic, Nudge Security, Grip Security, CASB telemetry (Netskope, Zscaler) |
| 11 | **Identity** | Agent workload identity, delegation, agent-to-agent | SPIFFE/SPIRE, Entra Agent ID, Okta Agent Identity, MCP authorisation (Protected Resource Metadata, Resource Indicators, Client ID Metadata Documents) |
| 12 | **Data governance / knowledge governance** | Data classification, purpose limitation, context-source approval (indexed and live), authorisation-aware retrieval, embedding lineage, retention, deletion, consent | Native DLP, Microsoft Purview and equivalent information-protection labels; Immuta, BigID, Securiti, OneTrust; vector-store ACL partitioning |

**Vendor and product names in the table above are illustrative examples only.** They are neither endorsements nor approved suppliers; they are not evidence that a named product satisfies the associated control. Implementation validation is the adopting organisation's responsibility. The control is durable; the product is interchangeable.

Incidents almost never come from a missing layer. They come from the seam between two layers — the guardrail fired, the runtime logged it, the observability platform archived it, and AI-SPM did not know the agent existed. The seam discipline in §4 governs that problem.

**Policy-as-code.** Layer configuration — model allow-lists, tool allow-lists, approved data classes, bounds, guardrail configurations, approved MCP endpoints, logging fields, kill-switch routes — SHOULD be represented as code in a versioned repository, with the same review and promotion discipline as application code. Spreadsheet-driven policy becomes unmaintained policy.

---

## 4. The four seams

Every AI interaction crosses four control seams. The framework organises its controls against these seams because attackers do.

| Seam | The question it answers | Primary controls |
|---|---|---|
| **Identity** | Who is acting, on whose behalf, with what delegation? | §8 Identity, delegation, and secrets |
| **Data-flow** | What content is entering the model, from where, at what trust level, and was the requester authorised to see it? | §9 Data governance and knowledge controls; §14 Prompt injection, poisoning, content integrity |
| **Decision** | What decision evidence exists? Can the organisation reconstruct the inputs, retrieved context, policies, model outputs, tool calls, approvals, and state transitions that led to the action? | §13 Observability and evidence |
| **Action** | What did the model actually do, with what blast radius, and could it have been stopped or compensated? | §10 Runtime bounds and action safety; §12 Incident response, kill-switch, rollback |

The layered model in §3 answers *where* controls sit. The seam model answers *what* they must preserve. Every control in this framework is justified by one of the four seams.

The framework does not assume access to faithful model chain-of-thought. "Decision evidence" means the inputs, retrieved context, policy outcomes, tool calls, approvals, and state transitions actually captured. Where a platform exposes a planning trace, that is additional evidence, not primary evidence.

---

## 5. Risk tiering

Every AI system is assigned a tier at registration. Tier drives required controls, review cadence, and attestation authority.

| Tier | Definition |
|---|---|
| **Tier 1** | Customer-facing; regulated-decision-relevant; payments-adjacent; or with access to restricted or personal data at scale. Example classes: customer-facing chatbots, adverse-action drafting, KYC or AML scoring support, credit-decision augmentation. |
| **Tier 2** | Internal-facing but operating on restricted data, or producing artefacts that influence business decisions. Example classes: research assistants on restricted documents, internal retrieval-augmented or live-retrieval systems over confidential content, agent-drafted analyses. |
| **Tier 3** | Internal productivity, general-knowledge, or constrained to public or internal-low data classes. Example classes: coding agents on development repositories with no production reach, meeting summarisers, general productivity copilots. |

**Tier escalation is automatic** on any of:

- Data class in scope elevated to restricted or personal.
- Tool set gains a write-scope tool or destructive capability.
- User population expands beyond the originally scoped group.
- Integration with a system of record.
- For coding agents: access to infrastructure-as-code repositories, ability to modify CI/CD pipeline definitions, access to secrets or deployment variables, ability to produce customer-facing code, or any write path to production. Tier 3 status for a coding agent requires all of these to be absent.

Tier is reassessed within the sprint in which any triggering change is made.

---

## 6. AI system lifecycle

Every AI system moves through the same stages. The framework attaches controls at each stage and governance artefacts at each boundary.

| Stage | Required artefacts | Accountable role |
|---|---|---|
| **Intake** | Registry entry (Appendix A), risk tier, proposed tool set, data classification, initial threat model, AI System Card | Product owner, with application-security review |
| **Development** | Evaluation plan, guardrail configuration, identity model, bounds profile, kill-switch design, Context Corpus Card (if retrieval is used), Tool Risk Card per registered tool | Engineering lead |
| **Pre-deployment** | Evaluation results, red-team report, independent validation (Tier 1), sign-off per §18 | Application security reviewer (Tier 2 and Tier 3); Independent validator and Risk Committee (Tier 1) |
| **Deployment** | Gateway-mediated routing, observability wired, runtime bounds applied, incident runbook linked | Platform engineering |
| **Operation** | Drift monitoring, cost tracking, periodic evaluation, scheduled revalidation, exception tracking | Engineering lead, with application-security support |
| **Decommissioning** | Registry entry closed, credentials revoked, memory and data disposed per retention policy, artefacts archived | Engineering lead |

No stage may be skipped. A Tier 3 system moves through every stage with lighter artefacts; a Tier 1 system with heavier ones.

### 6.1 Governance artefacts

Three human-readable artefacts accompany every registered system. They are governance documents, not technical configurations; they are written so that an auditor, a data owner, or a newly onboarded engineer can understand the system without reading the code.

- **AI System Card** — purpose, users, model and provider, data classes in scope, known limitations, prohibited uses, fallback path, monitoring signals, required user disclosures, owner, risk tier, evaluation summary.
- **Context Corpus Card** — governance artefact for any runtime context source used by an AI system, covering vector indexes, hybrid search, live source-system APIs, long-context document loading, memory stores, and SaaS-grounded retrieval. Records: source systems, data owner, classification, ACL enforcement model, retrieval mode (indexed / live / memory / hybrid), refresh cadence or query-time policy, chunking and embedding strategy where applicable, deletion procedure, poisoning controls, evaluation dataset reference.
- **Tool Risk Card** — tool owner, read/write scope, auth method, blast radius, idempotency, rollback procedure, approval mode, rate limit, logging fields.

Prompts, tool definitions, routing templates, guardrail policies, and retrieval templates MUST be versioned and promoted through the same change-control path as application code. A silent prompt edit is a material change.

### 6.2 User disclosure

Any Tier 1 system that makes or materially influences decisions affecting a natural person (customer, employee, counterparty, applicant) MUST present a user-facing disclosure of AI involvement at the point of decision or first interaction. The disclosure MUST include the nature of the AI involvement, a means to request human review where feasible, and a contact for complaints. Disclosure text is recorded in the System Card.

Where a jurisdiction or regulation imposes a stricter transparency requirement, that requirement controls.

### 6.3 Secure AI SDLC

AI systems follow secure software-development-lifecycle controls, adapted to AI-specific artefacts. Minimum controls at every tier:

- Threat modelling at design time, revisited on material change.
- Dependency scanning, container or image signing where self-hosted inference or agent runtime is used, and IaC scanning for the supporting infrastructure.
- Secrets scanning of code, configs, prompts, tool definitions, and evaluation traces.
- Versioning of prompts, tool definitions, retrieval templates, guardrail policies, embedding models, and chunking strategies — promoted through the same review and change-control path as application code.
- Evaluation-dataset versioning and access control (§11.2).
- AI Bill of Materials (AIBOM) generated where feasible, covering models, datasets, prompts, tools, and agent personas. CycloneDX is the suggested interchange format (Appendix D).
- Release approval and rollback planning tied to §11.5 model-change discipline and §12.5 rollback primitives.

---

## 7. Registration and inventory

Per §2 Principle 3 ("Registered or it does not exist"), an AI system absent from the registry is treated as unauthorised and is subject to removal.

**Three populations are registered:**

1. Agents and AI systems the organisation builds, registered via the platform intake process.
2. MCP servers and external tools the organisation's agents call, captured at first-trust with a cryptographic hash of the tool-definition set. Drift generates an alert.
3. AI features embedded in SaaS products, discovered through vendor admin consoles, CASB telemetry, and shadow-AI discovery tooling, and recorded centrally.

**Schema** — Appendix A.

**Discovery obligations:**

- Platform engineering runs a quarterly reconciliation of first-party agents against cloud inventory (AI-SPM output).
- Procurement flags any SaaS contract renewal or purchase where the vendor has added AI features, so the registry reflects them before go-live.
- IT and endpoint management feed shadow-AI discovery signals into the registry at least monthly.

**Registry hygiene:**

- Entries MUST be reviewed at the tier-appropriate cadence (§11).
- Entries MUST be closed within 30 days of decommissioning.
- Orphaned entries (no active owner) are flagged monthly and escalated at 90 days.

**Exception governance:**

- All exceptions to framework requirements are recorded in the registry with a named residual-risk owner, a justification, compensating controls, and an **expiry date**. Permanent exceptions are prohibited.
- Renewal of an expired exception requires evidence that the compensating controls operated effectively during the prior period.
- The exception backlog (count of active exceptions, expiry status, overdue renewals) is reported to the Risk Committee quarterly.

---

## 8. Identity, delegation, and secrets

Three overlapping questions on the Identity seam.

### 8.1 Agent workload identity

Every registered agent has three resolvable identity records:

- **Deployment identity** — the long-lived identity attached to the registered system (SPIFFE ID, Entra Agent ID, Okta Agent Identity, or equivalent).
- **Runtime / session identity** — the short-lived credential bound to a specific invocation or session, issued at runtime with explicit TTL and audience.
- **Initiating-human binding** — the human principal on whose behalf the agent is currently acting, captured at session start and propagated into every downstream call.

Logs and audit trails MUST distinguish all three.

Implementation options for the deployment identity include a vendor-neutral SPIFFE/SPIRE identity (recommended for multi-cloud or multi-runtime agents), an IdP-native agent identity where the agent lives inside that identity fabric, or a runtime-issued workload identity where the agent is bound to a single runtime.

Every agent identity appears in the non-human-identity access review at least quarterly for Tier 1 and Tier 2, and at least annually for Tier 3 (or on the cadence of the organisation's general NHI policy, whichever is more frequent). An agent identity without an owner, an expiry, a registry link, or a last-seen timestamp is a control finding. The review aligns to OWASP Non-Human Identities Top 10 (2025); see Appendix D.

**SaaS-embedded exception.** For SaaS-embedded AI where the vendor does not expose all three identity records, the missing identity attributes are recorded as a vendor-control limitation in the SaaS AI feature matrix (§15.1) and folded into the supplier review (§17). The NHI review for such systems covers whatever identity surface the vendor does expose (seat-level consumption, admin activation, service-principal grants, audit-log actor fields).

**Agent-card and A2A registry integrity.** Where agents discover peers via well-known agent cards (for example `/.well-known/agent.json`) or an A2A registry, host agents MUST verify the peer's cryptographic identity and capability descriptor before routing any task to it. Forged agent cards in open discovery directories are a documented attack class. Trust-on-first-use against an unauthenticated registry is prohibited for Tier 1.

### 8.2 Delegation chain

When a human asks an agent to do something, and that agent invokes another agent or a tool, the chain is:

`human principal → agent identity (deployment → session) → sub-agent / tool identity → target system`

**Principle (normative):** privileges scope down at each hop. A sub-agent receives the minimum scope needed for the task, not the inherited scope of the parent. Privilege propagation is an exception that MUST be justified and recorded.

**Current primitives:**

| Primitive | Standardisation | Where it works | Where it does not |
|---|---|---|---|
| OAuth 2.0 token-exchange (RFC 8693) for on-behalf-of flows, used within the OAuth 2.1 framing adopted by the MCP authorisation specification | RFC complete | Inside a single identity fabric (for example Microsoft Entra within the Entra boundary) | Cross-vendor on-behalf-of for agent chains is not a shipped capability |
| MCP authorisation (Protected Resource Metadata, Resource Indicators, Client ID Metadata Documents; Dynamic Client Registration as fallback) | Specified in MCP | Initial tool authentication | Does not solve multi-hop delegation |
| Proof-carrying tokens (audience-restricted, short-TTL JWTs) | Building blocks exist | Within a single trust zone | No cross-vendor standard for propagating delegation proof |
| SPIFFE/SPIRE workload identity | Mature OSS | Within the organisation's own infrastructure | Does not cross into SaaS or vendor-managed runtimes |
| IdP-native agent identity (Entra Agent ID, Okta Agent Identity) | Preview or early-release | Their respective identity fabrics | Do not interoperate across IdPs |

**Stance:**

- Within a trust zone the organisation owns, mTLS or SPIFFE workload identity between agents is required.
- Across trust boundaries, bearer tokens with short TTL, explicit audience claims (Resource Indicator), and registry-recorded delegation are required. Shared long-lived API keys are prohibited; if a tool integration only supports static keys, the integration MUST be fronted by a gateway that issues short-lived credentials to the agent and translates to the static key at the egress.
- MCP clients MUST use Resource Indicators and MUST reject authorisation servers that do not support PKCE/S256 and exact redirect-URI validation where applicable.
- Multi-hop delegation proof — cryptographically binding "this specific human authorised this specific sub-agent to call this specific tool" across vendor boundaries — is an aspiration, not a current requirement. The registry records the delegation chain even where it is not cryptographically proven.
- **Workflow-authorisation drift (time-of-check/time-of-use).** Long-running workflows whose authorisation is checked only at the start can execute actions after the user's rights have been reduced or revoked. For Tier 1 and Tier 2, re-authorisation MUST occur on context switch, on privilege-elevation step, and at any action whose blast radius exceeds the invocation's bounds. Tokens bound to a workflow step MUST have a TTL shorter than the expected step duration.

### 8.3 Secrets

An agent's blast radius is the union of its tool set, its data access, and the credentials in its environment.

- No static long-lived production credentials in any agent environment. Static credentials are permitted only for non-production.
- Production access uses short-lived STS AssumeRole sessions or Vault-issued dynamic secrets, bound to the agent session lifecycle.
- Rotation and revocation are tied to session end; credentials MUST NOT outlive the session.
- Credential material MUST NOT be written to agent long-term memory, prompt caches, observability traces, or evaluation traces. This is a redaction requirement enforced at the observability layer.

An agent holding static production-write credentials is a P1 finding regardless of tier.

---

## 9. Data governance and knowledge controls

The data-flow seam begins before the model is invoked. What the system is authorised to read, under what purpose, and how that authorisation survives transformation into embeddings and retrieval is a control surface of its own. Most enterprise retrieval incidents are not model failures; they are retrieval-authorisation failures.

### 9.1 Classification and purpose limitation

- Every AI system declares the data classes it is authorised to process (Public, Internal, Confidential, Restricted, Personal) at registration, for both input and output. Changes require tier reassessment.
- Purpose limitation is declared: personal data processed under a stated lawful basis (consent, contract, legitimate interest, or equivalent) is not reused for a different purpose without a fresh basis.
- AI systems MUST NOT aggregate data classes above their registered ceiling at runtime. An Internal-classified retrieval application MUST NOT retrieve Restricted documents even if indexed alongside.

### 9.2 Context-source approval and lineage

The framework governs all runtime context retrieval, not only classical vector-RAG. "Context source" means any data source an AI system retrieves from at runtime: indexed corpora (vector, hybrid), live source-system APIs reached via tools or MCP, long-context document loading, agent memory stores, and SaaS-grounded retrieval.

- Every context source has a declared data owner and a Context Corpus Card (§6.1).
- The source is used only where its owner has authorised AI processing at the system's classification level.
- Where retrieval is indexed, embeddings retain provenance metadata linking each chunk back to its source document, source system, and effective classification at ingestion time.
- Where retrieval is live (§9.8), returned objects carry the same provenance: source system, object identifier, effective classification at query time, and the authorisation decision applied.
- Poisoning controls apply at ingestion for indexed sources and at query-time sanitisation for live sources (same primitives as retrieved-content guardrailing in §14). Tier 1 ingestion pipelines and high-risk live sources (external-authored, user-generated, unverified) SHOULD require a second-model or rule-based allow/deny.

### 9.3 Authorisation-aware retrieval

This is the critical control and the one most often missing.

- Retrieval MUST enforce source-system authorisation at query time, not only at index time. Index-time filtering is insufficient unless the index is strictly partitioned so that every partition corresponds to a single identical authorisation boundary.
- Per-document or per-chunk ACLs MUST be preserved, resolved against the current requesting principal, and validated before retrieved content is supplied to the model.
- Source-system ACL changes MUST propagate to the retrieval layer as close to real-time as the source system exposes: near-real-time for Tier 1 where the source supports it, daily for Tier 2, weekly for Tier 3. The propagation cadence is recorded in the Context Corpus Card.
- Retrieval logs MUST include the requesting principal, the query, every candidate document ID and chunk ID considered, and the authorisation decision applied. This is the evidence base for investigating "how did the model answer from a document that user should not have seen."

### 9.4 Instruction and data separation

The model MUST be told, and the runtime MUST enforce where possible, that retrieved content is data, not instruction.

- Retrieved content MUST NOT be concatenated into the same channel as system or developer instructions. Where the model API supports it, retrieved content is placed in a distinct role (tool result, document, or equivalent) rather than a user or system prompt.
- Spotlighting (delimiting, encoding, or tagging untrusted content) SHOULD be used for high-sensitivity retrieval.
- Structured-output constrained decoding SHOULD be used where the task permits.

### 9.5 Output handling

Model outputs used to drive deterministic systems (SQL, shell, code, browser actions, workflow decisions, policy rulings) MUST be validated by deterministic code before execution. Output handling is a downstream injection surface; "the model would not say that" is not a control.

### 9.6 Canary tokens

Seeded canary tokens SHOULD be used in sensitive corpora, tools, or agent memory. Any model output, tool argument, or outbound call containing a canary token triggers a Sev 3 incident review and blast-radius investigation. Canaries convert silent exfiltration into a loud signal.

### 9.7 Retention, deletion, and erasure

- Retention is declared per context source and per memory store, aligned to records-management classification and any applicable regulatory retention requirement.
- Deletion MUST propagate to derived structures: embeddings, chunk metadata, prompt caches, fine-tuning deltas, observability traces, and memory snapshots. Erasure is a best-effort control until the supporting primitives mature; the Context Corpus Card declares the current erasure guarantee.
- Agent long-term memory MUST support PII and secret redaction on write, and namespaced erasure on request.

### 9.8 Live source-system retrieval

Where an AI system retrieves context directly from a source system at runtime (via an MCP tool, a native SDK, or a service API) rather than from an index, the same controls as indexed retrieval apply, adapted to the live pattern:

- **Source-system authorisation at query time** — the requesting principal's permissions on the source system gate what is returned. The retrieval layer does not substitute its own authorisation for the source system's.
- **Returned-object provenance** — source system, object identifier, effective classification, and the authorisation decision applied are recorded with the retrieval log.
- **Query logging** — queries, parameters, and result identifiers are logged sufficient to reconstruct what the model was given.
- **Tool-output sanitisation** — per §14.2. Live results are untrusted input and MUST be sanitised before being returned to the model.
- **Classification labelling** — the classification of returned objects propagates to the model's context and to the observability trace, so downstream controls (guardrails, output handling, retention) can apply it.
- **Retention and replay** — live-retrieval evidence is retained on the same schedule as indexed-retrieval evidence (§13.3) and is sufficient to support replay (§13.6).

**Preference.** Direct source-system retrieval SHOULD be preferred over replicated indexes where authorisation, deletion, freshness, or legal-hold requirements cannot be reliably preserved in the replica. Replicating a source system's data into an index inherits every one of that source's authorisation and retention obligations; where the replica cannot honour them, the live pattern is safer.

---

## 10. Runtime bounds and action safety

Every registered agent operates inside explicit bounds. Defaults apply unless an exception is granted.

### 10.1 Bound categories

- Wall-clock (synchronous and asynchronous).
- Tool invocations per task.
- Concurrent sessions per agent identity.
- Reasoning and recursion depth.
- Token budget per session.
- Cost budget per session.
- **Action-scope bounds:** records touched per action, rows modified per run, external recipients per message, funds moved or committed per session, permission changes per session, files written per task, memory writes per session, retry count, tool-error-rate stop threshold.
- Human-in-the-loop gate on write-scope tools (§10.3).

Default values are in Appendix B. The Appendix B values are organisational starter defaults, not evidence-based universal limits. Each system calibrates its bounds against task class, blast radius, latency SLO, cost SLO, and incident-response capability. Enforcement is layered: runtime (wall-clock, concurrency), framework (step and recursion), gateway (token and cost, interrupting), observability (after-the-fact alarm).

### 10.2 Exception handling

- Exceptions are requested via the registry (§7).
- Exceptions within 2× default are auto-approved with logging; exceptions above 2× escalate to the exception authority (§18).
- Exceptions above the hard ceiling (2× default in all dimensions, or any dimension past its absolute ceiling) escalate to the Risk Committee.
- Exception-grant SLA: 5 working days for Tier 2 and Tier 3; 10 working days for Tier 1.
- Every exception has an expiry date and a named residual-risk owner (§7).

### 10.3 Human-in-the-loop on write

Any tool that mutates state in a production system requires an out-of-band human approval per invocation for Tier 1, per session for Tier 2. Tier 3 MAY batch approvals on declared dev-only systems.

**HITL at scale.** Per-invocation human approval collapses into rubber-stamping above human cognitive throughput. As a starter threshold, *high frequency* means sustained write-tool invocation rates above 60 per minute per approver, or above the rate at which the approver has been measured to make non-rubber-stamp decisions on a representative sample (whichever is lower). Adopters set their own threshold per system tier and record it in the System Card. Where an agent design exceeds the threshold, the system MUST use one of the following patterns instead of reflexive per-invocation approval:

- **Batched approval** — group writes into a single approval unit (transaction, ticket, pull request) with one human decision per batch.
- **Risk-scored gating** — low-risk writes (idempotent, rate-bounded, bounded blast radius) proceed; high-risk writes (destructive, irreversible, privileged) require per-invocation approval. The risk score is documented and auditable.
- **Shadow or dry-run mode** — the agent proposes writes but does not execute; approved batches execute via a separate operator action.
- **Redesign to remove the write** — many high-frequency "writes" are better handled as events emitted to a downstream consumer that applies its own controls.

Adaptive approval strategies beyond these patterns are not yet standardised.

### 10.4 No direct destructive tool (Tier 1)

Tier 1 agents MUST NOT directly invoke irreversible destructive actions (schema-destructive database operations, irreversible financial commitments, bulk deletions, IAM revocations affecting production, production infrastructure teardown). Such actions are expressed as a proposed transaction — a ticket, a pull request, a signed change request, a dry-run report — executed by a separate controlled system with its own authorisation check.

### 10.5 Transaction safety and Intent Gate

For any write-scope tool whose failure or retry can cause double-application (duplicate payment, duplicate message, duplicate record):

- Tool definitions MUST declare idempotency semantics (idempotency key, natural key, or "not idempotent").
- Where not idempotent, the tool MUST be wrapped to enforce at-most-once or compensated execution.
- Compensating actions MUST be defined before go-live for every destructive or irreversible tool a Tier 1 agent can invoke.

**Intent Gate (Policy Enforcement Point, SHOULD for Tier 1 and Tier 2).** For high-impact tool calls, treat the planner's output as untrusted and interpose a Policy Enforcement Point between planning and execution. The Intent Gate validates: (a) the fully-qualified tool name and version (defence against tool-alias collision and typosquatting), (b) the argument schema, (c) the declared intent (subject, audience, purpose, session) against the active workflow, (d) rate and cost budgets, and (e) any bound signed intent envelope where the system uses one. Actions that fail any check fail closed and surface to human review.

---

## 11. Evaluation and model-change

### 11.1 Pre-deployment evaluation

Every registered system has an evaluation suite with a named owner, a published methodology, and a documented acceptance threshold. At minimum:

- **Capability regression** — task suite against the incumbent, pairwise LLM-as-judge with human spot-check. The human spot-check covers at least 5% of the LLM-judged sample; Tier 1 systems with people-impacting outputs may require a higher human-spot-check fraction.
- **Safety regression** — adversarial corpus for prompt injection, jailbreak, PII leakage, tool misuse. Bypass rate tracked as a metric.
- **Latency and cost** — p50 and p95 latency, cost per task on representative traffic.
- **Context-retrieval evaluation** — where runtime context retrieval is involved (indexed or live; see §9), score separately: retrieval relevance, authorisation correctness (did retrieval honour source-system ACLs for the requesting principal?), freshness, provenance completeness, and generation faithfulness to the retrieved evidence.
- **Agentic task reliability** — for agents, task-completion rate on an internal agent-task suite. Static knowledge benchmarks (MMLU, HELM) do not predict agent reliability and are not sufficient evidence.
- **Schema compliance** — for systems using structured outputs or tool calls, schema-adherence rate.
- **Outcome analysis** — for Tier 1 people-impacting systems: error-severity weighting, disparate-impact check where relevant, human-override rate analysis.

Every metric has a documented threshold below which deployment is blocked. Thresholds are recorded before the evaluation runs.

**Independence.** Tier 1 evaluation is reviewed by a validator independent of the engineering team that built the system. Independence is organisational, not just personal.

### 11.2 Evaluation dataset governance

Evaluation datasets are themselves sensitive and attackable.

- Datasets are versioned and access-controlled.
- Golden test sets are held out from training, fine-tuning, prompt-development, and online traffic. Where a golden set must be referenced during development (for ad-hoc spot checks), it is read-once via an audit-logged path; bulk programmatic access is prohibited. Tier 1 Outcome-analysis golden sets SHOULD be third-party held, escrowed, or held by an internal function organisationally separate from the building team where the validator-independence requirement (§11.1) makes building-team custody insufficient.
- Datasets containing production data are handled at the classification of that production data.
- **Dataset, threshold, and judge-model drift** are tracked separately. The evaluation owner monitors dataset drift (input-distribution shift on the eval corpus), threshold drift (silent acceptance-threshold relaxation across releases), and judge-model drift (LLM-as-judge calibration shift between judge versions). All three are reported at the §11.5 revalidation cadence.

### 11.3 Red-team cadence

- Tier 1 — continuous automated red-teaming (Garak, PyRIT, Promptfoo, equivalents) plus quarterly human-led engagement (internal team or external).
- Tier 2 — continuous automated; human-led on material change.
- Tier 3 — automated red-team suite at release and on material change; continuous automated testing where centrally supported. Centralised red-team tooling for every Tier 3 productivity assistant is often impractical, so release-gated testing is the baseline expectation.

### 11.4 Online evaluation

Pre-deployment evaluation is necessary but not sufficient. Tier 1 and Tier 2 systems run online evaluation:

- **Sampled live evaluation** — a bounded random sample of production invocations is scored by the regression suite (and by humans for Tier 1) on a declared cadence.
- **Drift detection** — statistical drift on input distribution, output distribution, and metric scores, with alert thresholds.
- **Incident-triggered regression** — any Sev 1 or Sev 2 incident (§12) triggers the full regression suite before the system returns to normal operation.

**LLM-as-judge caveat.** LLM-as-judge is useful for scale but is not independent ground truth. It MUST be calibrated against human evaluation on a known sample, monitored for drift, and never used as sole evidence for Tier 1 safety.

### 11.5 Model-change standard

A change in underlying model — including version bumps the vendor performs silently — is a material event.

- The resolved foundation-model identifier is captured on every invocation as a log attribute, and the registry compares the resolved identifier on each invocation against the last observed resolved identifier for that registry entry. Any change emits an **Alert-for-Revalidation** to the engineering lead and application security, and suspends the system from promotion to Tier 1 traffic until revalidated. This per-invocation resolved-identifier check is the load-bearing control; alias-level pinning (e.g. an inference profile or `…-latest` alias) is acceptable only when paired with it. Where the provider does not expose a resolved identifier, the limitation is recorded in the registry and the system's concentration exposure (§17) is flagged.
- On any model version change, the evaluation suite (§11.1) runs. Tier 1 and Tier 2 require sign-off before the new version is promoted; Tier 3 is logged.
- A change in the system prompt, tool definitions, retrieval template, guardrail policy, embedding model, chunking strategy, or context source (indexed or live) is equally a material event and goes through the same evaluation and promotion discipline.
- Revalidation cadence: Tier 1 quarterly, Tier 2 semi-annually, Tier 3 annually or on material change.

### 11.6 New-model adoption

A new foundation model entering the organisation (first production use) triggers Risk Committee review regardless of the tier of the first consuming system.

---

## 12. Incident response, kill-switch, rollback

### 12.1 Kill primitives

Every Tier 1 and Tier 2 agent has all four kill primitives:

1. **Gateway-level** — revoke the API key or route. Fastest and most portable.
2. **Runtime-level** — terminate the session, pod, or microVM; revoke the workload's session token.
3. **Tool-level** — remove the tool from the allow-list; revoke the MCP or OAuth token.
4. **Data-access-level** — rotate or revoke the credential or role the agent's workload assumes.

A single named "stop this agent" procedure exists for each Tier 1 and Tier 2 agent, with a named owner and a documented MTTR target. For Tier 1 the target is 15 minutes from alert to full stop; organisations new to AI operations should plan for a 30–60 minute initial capability, compressing over time as the procedure matures. The target is validated by live-fire drill at least annually; drift from target is a control finding.

### 12.2 AI-specific incident classes

The incident catalogue explicitly includes:

- Confirmed data exfiltration via model output or tool call.
- Prompt or response archive breach.
- Embedding or vector-store exfiltration.
- Unauthorised SaaS AI feature enablement.
- Vendor silent model change (detected via the Alert-for-Revalidation control in §11.5).
- Tool-definition drift without approval.
- Memory poisoning (detected by content drift or canary).
- Agent identity compromise or session-token theft.
- Agent runaway loop or cost blow-out.
- Unauthorised external communication by an agent.
- Canary-token leakage.
- User demonstrably harmed by a hallucinated operational instruction.
- Regulated record generated incorrectly by an AI system.
- Deployment of a system later found to fall within the prohibited-uses list (§1.3).

### 12.3 Severity matrix

| Severity | Trigger |
|---|---|
| **Sev 1** | Confirmed data exfiltration, privileged-action compromise, cross-tenant breach, destructive irreversible action, material financial harm, safety harm, regulatory harm, or adverse action taken against a natural person on the basis of incorrect AI output |
| **Sev 2** | Bounded data leak, runaway cost (>10× budget in a single session), confirmed prompt-injection compromise without exfiltration, tool-poisoning indicator, silent vendor model change detected after production exposure |
| **Sev 3** | Guardrail bypass without impact, evaluation score regression in production, injection attempt blocked, suspected tool drift, canary leak |
| **Sev 4** | Anomalous behaviour requiring investigation but no immediate action |

**Triage note.** Prompt-injection compromise and aggressive hallucination leave nearly identical log shapes. Triage is forensic (decision-evidence trace, tool-call graph, retrieved-content diff), not signature-based. Observability (§13) enables this.

### 12.4 Containment decision tree

For Sev 1 and Sev 2, the on-call sequence is:

1. Stop the model route at the gateway.
2. Disable the tool or tools implicated in the incident.
3. Revoke the agent's workload session identity.
4. Freeze the agent's long-term memory namespace.
5. Freeze the context source for the affected classification (revoke the index, suspend the live-retrieval tool, or both as appropriate).
6. Preserve the decision-evidence traces and logs; apply legal hold.
7. Notify the data owner, privacy and legal, and the Risk Committee duty officer.
8. Run a blast-radius query: which users, sessions, tools, documents, and downstream systems were touched in the window?

### 12.5 Rollback

- **Model version regression** — pinned inference profile with previous-version alias kept available; the swap is a configuration change, not a redeploy.
- **Prompt or tool-set regression** — versioned deployment; blue-green or canary for Tier 1 and Tier 2.
- **Memory poisoning** — long-term memory can be cleared by namespace without nuking the agent's entire state. Memory stores MUST support namespaced erasure.

### 12.6 Shared-supply-chain incidents

A compromised MCP server or a poisoned vendor model is a supply-chain incident. Blast radius is every agent that trusted the supplier. A pre-defined severance runbook MUST exist for each registered external dependency at Tier 1 and Tier 2.

### 12.7 External notification

AI incidents frequently trigger external notification obligations that run on legally mandated timelines. The incident runbook MUST identify which incident classes trigger which notification categories, with named owners and timeline targets. The categories below are illustrative, not legal advice; adopters MUST map each category to the regimes that apply to their own jurisdictions, sector, and listing status, and the legal desk owns that determination.

Notification categories the runbook covers where applicable:

- **Personal-data breach notification** to the supervisory authority and, where the breach is likely to result in high risk to natural persons, to affected individuals. AI-driven exfiltration of personal data is a breach like any other.
- **Material cybersecurity incident disclosure** for publicly listed issuers and for regulated financial-services entities operating under supervisory regimes that require post-incident disclosure on a defined timeline.
- **Significant ICT-incident reporting** under sectoral cyber-resilience and operational-resilience regimes (financial-services operational-resilience law, network-and-information-systems law, critical-infrastructure regimes).
- **User-disclosure obligation** where a user was subject to an adverse action based on incorrect AI output. The disclosure and remediation path declared under §6.2 is invoked.
- **Sectoral or supervisory-relationship notification** as required by the adopter's primary regulator (banking supervisor, insurance regulator, healthcare authority, telecommunications authority, or equivalent).

Specific named regimes that an adopter is likely to need to map against — including GDPR, applicable national and state privacy laws, securities-disclosure rules, NIS2, DORA, HIPAA, and sector equivalents — are catalogued in **Appendix D under "Personal-data and incident-notification regimes"**, with indicative timelines for legal-desk reference. Current applicability of each regime to the adopter is a legal determination, not a framework determination.

External notification is a legal-desk-led activity. The engineering response (containment, preservation of evidence per §12.4) runs in parallel and MUST NOT be blocked by the notification process.

---

## 13. Observability and evidence

Observability for AI agents has five slices. No single tool covers all of them; assume multi-vendor.

| Slice | Question it answers | Primary layer |
|---|---|---|
| **Prompt and response archive** | What did the model see and say? | LLM observability, invocation logs |
| **Decision-evidence trace** | What inputs, retrieved context, policies, tool calls, approvals, and state transitions led to the action? | LLM observability with agent-trace support, framework checkpoints |
| **Tool-call graph** | Which tools fired, in what order, with what arguments, against what data? | Runtime protection, specialist tools |
| **Posture and inventory** | What agents exist and what can they touch? | AI-SPM |
| **Runtime anomaly** | Is something wrong right now? | Runtime protection and SIEM correlation |

The framework does not rely on model chain-of-thought as audit evidence. "Decision-evidence trace" means the inputs, retrieved context, policies, model outputs, tool calls, approvals, and state transitions actually captured. Where a platform exposes a planning trace, that is additional evidence, not primary evidence.

### 13.1 Minimum evidence per invocation

For every Tier 1 or Tier 2 agent invocation, the logged record includes:

- Human principal ID, agent deployment identity, agent session identity, session ID.
- Resolved foundation-model identifier (not just the alias).
- Prompt, response, and full tool-call sequence with arguments and results (see §13.2 for privacy-aware logging).
- Guardrail configuration ID and any guardrail-trigger events.
- Retrieved-context identifiers (document IDs, chunk IDs) and the authorisation decision for each.
- Bounds state (budgets consumed, steps taken).
- Applied exception references, if any.

### 13.2 Privacy-aware logging

Full-fidelity logging is required unless prohibited by law, legal privilege, data-minimisation requirement, or regulated-record classification. Where full logging is restricted, the system MUST log redacted payloads plus cryptographic digests, metadata, retrieval IDs, policy decisions, and approval records sufficient for investigation. Redaction rules are declared in the registry.

Full prompt and response archives containing regulated or personal data are themselves a data risk. They inherit the classification of the highest-class data that can appear in them, and are protected accordingly.

### 13.3 Retention

Retention is determined by records-management classification and regulatory need.

- Invocations forming part of a regulated business record: per the applicable record-retention schedule (commonly seven years in regulated financial services).
- Invocations whose content is not a regulated record: least-privilege retention approved by legal and privacy, typically 90 to 180 days for Tier 1 and Tier 2 forensic value, shorter for Tier 3.

The system's data owner sets the actual retention value at registration. Longer-than-necessary retention of AI evidence logs is a data risk, not a safety margin.

### 13.4 Evidence integrity

Observability logs are the sole forensic record for many classes of AI incident. For Tier 1:

- Logs are written to append-only or tamper-evident storage.
- Log access is audited; privileged access to the evidence store is reviewed monthly.
- Integrity-break detection (missing invocation IDs, gaps in sequence, modified records) generates an alert.

### 13.5 Semantic conventions

OpenTelemetry GenAI semantic conventions are the framework's preferred interchange format. As of writing, model-span attributes in the `gen_ai.*` namespace are stabilising while agent-span and tool-span attributes are still moving out of experimental status. Production exporters MUST NOT assume cross-vendor compatibility today and SHOULD normalise at the observability platform rather than at the source until the agent and tool conventions stabilise.

### 13.6 Replay

Tier 1 agents support deterministic replay of a frozen session state against an alternate model version or configuration, for incident investigation and regression analysis. This is a framework capability (checkpointer, session-state persistence, equivalent) that MUST be enabled — it is typically off by default.

### 13.7 AI outputs as business records

AI-generated outputs that inform regulated decisions, customer communications, adverse actions, legal positions, operational approvals, or financial commitments may themselves constitute business records and MUST be handled under the organisation's records-management, legal-hold, and e-discovery obligations.

- The System Card declares whether a system's outputs are treated as business records and under which retention schedule.
- Where outputs are records, the record MUST include: the input context (to the extent privacy and privilege permit), the resolved model identifier, the retrieved context identifiers, the human approval record where applicable, and the final output as issued.
- Records-management retention takes precedence over §13.3 default retention where the two diverge.
- Legal-hold requests propagate to the output record, the supporting decision-evidence trace, and any underlying context-source objects referenced at the time of the decision.

---

## 14. Prompt injection, poisoning, and content integrity

The data-flow seam at the model boundary. Three live, unmitigated attack classes.

### 14.1 Indirect prompt injection via retrieved content

Retrieved documents, tool-call results, calendar invites, ticket comments, email bodies, and document metadata are all untrusted input. Guardrails that inspect only the user turn and model output do not see this class of attack.

**Controls:**

- Retrieved content MUST be routed through a guardrail layer as untrusted input. On most first-party guardrail implementations this is an explicit "treat as user input" flag that is not the default.
- Tool-output sanitisation MUST strip control tokens, zero-width characters, hidden Unicode tag characters (U+E0000 block), and HTML from any tool response before reintroduction to the model.
- Instruction and data separation per §9.4 MUST be applied.
- Spotlighting (delimiting or encoding untrusted segments) SHOULD be used for high-sensitivity retrieval.

### 14.2 Tool and MCP poisoning

Malicious or compromised MCP servers can embed instructions in tool descriptions that are read by the model but not displayed by most clients. Tool definitions may also change silently after approval.

**Controls:**

- All MCP endpoints are registered (§7).
- Tool definitions are hashed on first-trust; any change generates an alert and blocks use until re-reviewed.
- Tool outputs are untrusted input: they MUST be schema-validated before being passed to the model and separately validated before being used to drive downstream action (§9.5).
- Signed tool manifests SHOULD be preferred where the registry supports them.
- Outbound MCP traffic MUST be mediated by the gateway; direct agent-to-internet MCP is prohibited.
- AI components (models, tools, prompts, agent personas, datasets) SHOULD be inventoried via a CycloneDX AIBOM; see Appendix D.

### 14.3 Memory and vector-store poisoning

Long-term agent memory, vector stores, and retrieval ingestion pipelines are attack surfaces. Poisoning a document before embedding can cause an agent to retrieve an injected instruction weeks later.

**Controls:**

- Ingestion pipelines apply content scanning before embedding.
- Long-term agent memory supports PII and secret redaction on write, and namespaced erasure on request.
- Vector stores record source-document provenance per chunk.
- Tier 1 memory writes SHOULD require a second-model review or a rule-based allow/deny.
- Canary tokens (§9.6) SHOULD be seeded in sensitive corpora.

### 14.4 Defence-in-depth rationale

The following are known not to work as standalone controls:

- System prompts alone do not defend against injection.
- Keyword blocking ("ignore previous instructions") is trivially bypassed.
- Naive small-model classifiers have high bypass rates on adversarial corpora.
- Single-layer guardrails, first-party or specialist, are not sufficient on their own.

Published red-team and adversarial-evaluation work consistently shows that single-layer guardrails are bypassable under adaptive attack, especially where attackers can iterate, obfuscate instructions, or exploit indirect prompt-injection paths. This is not a criticism of any single product; it is the operational reason the framework requires two or three layers in the request path, plus out-of-band human-in-the-loop on irreversible action. A single-layer posture is non-compliant with this framework on its evidence alone.

---

## 15. SaaS-embedded AI

The hardest class, because the model call happens inside the vendor's cloud. The organisation has no runtime control over it.

**What can be done contractually:**

- Data Processing Agreements cover AI processing, training prohibition on customer data, sub-processor disclosure including AI model providers, audit rights, and log-export SLAs.
- Indemnity language covers IP and hallucination harm where vendor commitments exist.
- Contracts preserve the right to disable AI features and the notification timeline for sub-processor changes.

**What can be done technically:**

- Tenant isolation verification (vendor attestation plus periodic test).
- Sensitivity-label enforcement at the data layer (information-protection labels and field-level security).
- Conditional access and vendor admin controls.
- CASB or DLP at egress: inspect what leaves the environment toward the vendor (prevent sensitive documents being ground into vendor context) and what returns from the vendor to the endpoint. CASB and DLP do not see the vendor-internal model call; it happens server-side. Do not treat CASB as a runtime control.
- Seat-level consumption monitoring.

**What cannot be done:**

- Run the organisation's guardrails over the vendor's model call.
- Reconstruct the full grounding chain cryptographically.
- Enforce the organisation's runtime bounds on the vendor's agent.
- Replay a vendor-internal agent action.

**Governing posture:**

- SaaS AI is reviewed at the feature level, not only the vendor level. A vendor's "AI capabilities" is not a single toggle; each feature has its own data scope, admin control, and logging.
- SaaS AI features are disabled in admin consoles where the vendor permits tenant control, until reviewed and registered (§7). Where a vendor ships a feature on-by-default and does not expose a tenant-level disable, the feature is treated as an existing deployment pending vendor-risk review, with compensating controls (classification-label enforcement, conditional access, seat restriction) applied until the review completes.
- Enabling features by business units without review, where a tenant-level disable exists and has been deliberately flipped, is a policy violation, not an accidental configuration.
- Log-export SLAs deliver vendor audit output to the SIEM on a schedule commensurate with tier — daily for Tier 1, weekly for Tier 2.
- Sub-processor chains are reviewed annually; vendor AI-provider changes trigger re-review.

### 15.1 SaaS AI feature matrix

For each registered SaaS AI feature, the registry records:

| Field | Notes |
|---|---|
| Feature name | Vendor-published name |
| Default state | On or off in the vendor tenancy |
| Data it can access | Classification ceiling |
| Admin control | Who can enable, at what scope |
| Logging available | Field-level, session-level, or none |
| Retention setting | Vendor-tenant retention controlled by the organisation where possible |
| Training / product-improvement use | Whether the vendor uses the organisation's data for model improvement |
| Sub-processor path | Which model provider, under what DPA |
| Customer-managed-key support | Yes, no, or partial |
| Data residency | Region commitment |
| Disable procedure | How to turn it off; runbook link |

---

## 16. Coding agents

Coding agents write code, install dependencies, invoke CI, and touch production-adjacent systems. They inherit everything else in this framework; this section adds the specifics.

- **Credentials.** No production write credentials in any coding-agent environment. Read-only production access is a case-by-case approval; write access is development or preview environments only; session-bound where possible.
- **Default permission mode.** Read-only or dry-run. Write permissions require explicit per-session elevation, logged against the human principal.
- **"Dangerous" flags.** Any agent flag that disables per-action approval is an organisational policy setting, not a per-user choice, and requires approval-authority sign-off.
- **SCM provenance.** Commits made by a coding agent carry an agent-identity trailer bound to the initiating human principal. PRs authored by an agent are identifiable as such.
- **Review gate.** AI-authored changes require human review from a non-agent principal before merge. Auto-merge is disabled for AI-authored PRs. An agent cannot satisfy two-person review, cannot approve its own PR, and cannot bypass CODEOWNER requirements on sensitive paths.
- **Sensitive paths require a security reviewer.** AI-authored changes to authentication, authorisation, cryptography, IAM, network policy, payment paths, trading paths, or security-control code require a security reviewer in addition to normal review.
- **Dependency and IaC discipline.** Coding agents MUST NOT autonomously install dependencies outside the lockfile; MUST NOT modify CI/CD workflow files without CODEOWNER review; MUST NOT bypass secret scanning. AI-authored changes MUST pass SAST, dependency scan, IaC scan, and tests before merge.
- **Tool and MCP allow-list.** Coding agents see development and CI MCP servers and documentation sources. Production database MCPs, production observability MCPs, and payment, HR, and customer-data MCPs are prohibited.
- **Sandbox for untrusted code execution.** Agent-initiated code execution (code interpreter, browser, shell) runs in a sandboxed environment with no production network reach and no production credentials.
- **Model version pinning.** The coding-agent model is a registered model (§11.5). Silent vendor-driven upgrades are not permitted; the pinning is at the client configuration level.
- **Audit surface.** The agent's tool-call trace and model-invocation log is ingested into LLM observability and SIEM. Traces may contain source code and secrets; redaction rules apply.
- **Adoption at scale is a deployment, not a personal choice.** Organisational coding-agent adoption beyond a declared pilot cohort is subject to the same registration, tier assessment, and controls as any other AI deployment.
- **Blast-radius test.** Every coding-agent deployment includes a tabletop: if this agent were compromised or prompt-injected right now, what is the worst 15-minute outcome? An answer that touches IaC, CI/CD, secrets, or production is automatic Tier 1 or Tier 2.

---

## 17. Supplier and concentration risk

Single-vendor concentration for AI inference is a governance problem distinct from security.

**Recognised concentration exposures:**

- **Inference concentration** — one hyperscaler provides foundation-model access for the majority of production AI workload.
- **Model-family concentration** — one model family underpins the majority of critical tasks.
- **Observability concentration** — one LLM-observability vendor holds all prompt and response archives.
- **MCP concentration** — one vendor's MCP infrastructure brokers the majority of tool calls.

**Concentration-failure triggers** (the supplier-risk review covers all of these, not only vendor insolvency):

- Regional outage.
- Vendor policy change (safety, acceptable-use, rate, price).
- Model deprecation or forced migration.
- Price shock.
- Safety-policy drift causing behaviour regression on the organisation's workload.
- Logging or export-API removal.
- Sanctions or export-control constraints.
- IP indemnity variance.
- Model behaviour regression after a vendor safety-tuning cycle.
- Contractual term changes forcing data-handling reconfiguration.

**Required mitigations:**

- **Inference.** Every Tier 1 system has a documented alternative inference path — a named second provider, a named substitute model, an evaluation delta, known feature gaps, a documented estimate of switching cost, data-residency impact, operational runbook owner, and a last-rehearsal status. The requirement is that the path is known, the cost is known, and no vendor in the stack has a failure mode with an unknown blast radius.
- **Architectural portability (SHOULD).** Tier 1 systems are built with a clear separation between the inference provider and the application logic — an adapter or hexagonal pattern — so that provider substitution is a bounded change. Implementation patterns that satisfy this include a model-routing gateway (LiteLLM, Portkey, Cloudflare AI Gateway) or a hand-rolled provider adapter; the framework names capabilities, not products, and any pattern that lets the application code remain unchanged across a provider swap satisfies the requirement. Vendor-internal routing primitives (for example Bedrock cross-region inference profiles) help with intra-vendor model and region substitution but do not by themselves satisfy provider-portability — they are a complement to, not a replacement for, the adapter boundary.
- **Model family.** Tier 1 evaluation suites include at least one alternative model family as a reference, so a forced swap is not a cold start.
- **Observability.** Prompt and response archives are exportable in a non-proprietary format (JSON Lines at minimum) to a second storage destination. Vendor lock-in of audit evidence is unacceptable.
- **MCP.** Critical tools have a failover path — direct SDK, alternative MCP provider, or manual procedure — documented in the registry entry.

A portability path is only as good as its last rehearsal. Periodic portability exercise — at minimum a table-top, ideally a real staging-environment cutover — is the operationalisation of this section and is tracked against each Tier 1 system's Last Rehearsal Date.

**Supplier review.** Every Tier 1 AI supplier (inference, guardrails, LLM observability, MCP gateway, agent runtime) is reviewed annually on: concentration risk, financial health, security posture, certifications, incident history, roadmap alignment, policy and pricing and IP-indemnity stability. Critical suppliers MUST have a substitution plan in the registry.

---

## 18. Roles, responsibilities, AI literacy, and attestation

| Role | Responsibility |
|---|---|
| **Product owner** | Accountable for the AI system's purpose, risk tier, and registry entry accuracy. Signs intake. |
| **Engineering lead** | Accountable for the implementation, evaluation, observability wiring, and operation. Signs deployment. |
| **Application security reviewer** | Reviews threat model, guardrail configuration, tool allow-list, identity model, and secrets. Signs pre-deployment for Tier 2 and Tier 3. |
| **Independent validator (Tier 1)** | Reviews evaluation methodology and results, drift monitoring, and validation evidence, applying model-risk-management discipline by analogy. Signs pre-deployment for Tier 1. |
| **Data owner** | Accountable for data classification, context-source approval (indexed and live), retention, and erasure. Signs the Context Corpus Card. |
| **Platform engineering lead** | Operates the gateway, runtime, observability, and identity infrastructure. Accountable for framework-level SLOs. |
| **Bounds and exception authority** | Grants or denies bounds and framework exceptions within published SLAs. Default: platform engineering lead and application security reviewer, jointly, for exceptions within 2× default; Risk Committee above. Accountable for exception-expiry tracking and renewal discipline. |
| **Risk Committee** | Approves new-model adoption, Tier 1 deployment, exceptions above hard ceiling, and post-incident remediation plans. |
| **Attestation executive** | Named executive who attests to the framework's operating effectiveness annually. Typically a CIO, CTO, or CISO with accountability joint among them. The framework does not mandate title, only that the name is on the registry. |

### 18.1 AI literacy

Every engineer building or operating AI systems completes the organisation's AI-literacy and safe-use training at onboarding and annually thereafter. Users of Tier 1 systems receive context-appropriate briefing on the system's capabilities, limits, and escalation path before first authorised use.

The training covers: the framework's principles and prohibited uses; common failure modes (hallucination, prompt injection, tool misuse); the escalation path for suspected incidents; the user-disclosure obligations for systems that affect natural persons.

### 18.2 Attestation cadence

- Tier 1 systems: annual attestation of effective operation by the attestation executive.
- Framework itself: annual attestation of adoption and effectiveness.
- Breaks in attestation (registry gaps, bounds violations without exceptions, overdue revalidations, expired exceptions without renewal) are reported to the Risk Committee quarterly.

---

## 19. Framework review, revision, and cost

### 19.1 Cadence

- **Six-month review** until the framework is declared stable (target: two consecutive reviews with no structural changes).
- **Annual review** thereafter.
- **Event-driven revision** on: material incident; significant change to the AI capability landscape (new attack class, new protocol standard, major vendor change); regulatory or supervisory change affecting scope; or working-group request with two-member support.

Revisions are version-controlled; prior versions are retained for audit. Every review explicitly revisits §20, because the known-limits register is where reality diverges from intention. Every scheduled review also refreshes vendor and product examples (notably §3, §15, and Appendix D), removing discontinued, renamed, materially changed, or merged products so that examples do not silently age the framework.

### 19.2 Cost of controls

The controls in this framework have real cost. Observability retention, evaluation infrastructure, human-in-the-loop headcount, red-team engagement, and redundancy for portability all consume budget. Tier 1 operation in particular is not a marginal add-on to a normal engineering team. Budgets and headcount allocations for controls are tracked alongside the systems they protect, and the attestation executive sees both together. A control that cannot be resourced is an exception under §7, not an unstated gap.

---

## 20. Known limits and open questions

The framework explicitly names where it asserts controls that current industry primitives do not fully support, and what the working group is treating as a later-maturity discipline. This register is reviewed at every framework cadence and is the primary input to scoped workstreams.

A control is credible only if it is honest about its own limits.

### 20.1 Cross-vendor agent identity and delegation proof

**The framework's position (§8.2).** Privileges scope down at each hop of a human → agent → sub-agent → tool chain, with the delegation recorded.

**What current primitives support.** OAuth 2.0 token-exchange (RFC 8693) is standardised — and is the token-exchange primitive used within the OAuth 2.1 framing adopted by the MCP authorisation specification — but production on-behalf-of across different model providers, MCP servers, and tool integrations is fragmentary. Proof-carrying tokens that cryptographically bind "this specific human authorised this specific sub-agent to call this specific tool" across vendor boundaries are not a shipped capability in 2026.

**Current operating practice.** Record the delegation chain in the registry; require short-lived audience-bounded tokens across trust boundaries; prohibit shared long-lived API keys; front static-key integrations via a gateway that issues short-lived credentials inward.

**Monitored for maturation.** A2A protocol work, the MCP authorisation ecosystem's handling of multi-hop delegation, and cross-vendor IdP-native agent identity.

### 20.2 Adaptive HITL at agent speed

**The framework's position (§10.3).** Human-in-the-loop gate on write-scope tools; batched, risk-scored, shadow-mode, or redesign patterns for high-frequency writes.

**What is not solved.** A general-purpose adaptive-approval standard — sampling-based, risk-scored, fatigue-resistant — that a working group can deploy without rolling its own scoring. Sits within OWASP Agentic Top 10 ASI09 Human-Agent Trust Exploitation, alongside anthropomorphic trust, authority bias, and fake-explainability cases.

**Current operating practice.** The four patterns in §10.3. Designs demanding high-frequency production writes document which pattern they use and why it is not equivalent to rubber-stamping.

**Monitored for maturation.** Adaptive-approval tooling in LLM observability and runtime-protection products; plan-divergence detection; adaptive trust calibration; peer-firm patterns.

### 20.3 Portability rehearsal

**The framework's position (§17).** Every Tier 1 system has a documented alternative inference path, with known switching cost and timeline.

**Later maturity.** Periodic exercise of that path — table-top first, then a real staging-environment cutover — as a scheduled activity per Tier 1 system, with a tracked Last Rehearsal Date. The initial position is: document the path now; the rehearsal cadence matures as working-group capacity allows.

**Monitored for maturation.** Whether the organisation can resource one rehearsal per Tier 1 system by the first annual review; whether adapter-pattern separation becomes table stakes across Tier 1 builds.

### 20.4 Memory erasure and right-to-erasure audit

**The framework's position (§9.7, §12.5, §14.3).** Agent long-term memory supports namespaced erasure; memory writes apply redaction; deletion propagates to embeddings, caches, and observability traces.

**What current primitives support.** Most agent-memory products expose an erasure API; few provide a tamper-evident audit that erasure has propagated to all derived structures.

**Current operating practice.** Treat erasure as a best-effort control with manual verification. Limit long-term memory to cases where the value clearly exceeds the erasure-audit burden.

**Monitored for maturation.** Cryptographic-erasure patterns (key destruction over encrypted memory), tamper-evident audit for vector stores, and GDPR Article 17 enforcement trends.

### 20.5 Cryptographic provenance of retrieved content

**The framework's position (§9.2, §14.3).** Vector stores record source-document provenance per chunk.

**Later maturity.** End-to-end cryptographic chain-of-custody from source document → chunk → embedding → retrieval → answer. Signing on ingestion, signed manifests on retrieval, and verifiable citations in the response are research-level.

**Current operating practice.** Source-document ID and chunk ID recorded per retrieval; citations surfaced where the application supports it; no cryptographic binding.

**Monitored for maturation.** C2PA adoption for enterprise documents, signed-retrieval patterns, and whether any evaluation platform commits to end-to-end provenance as a product feature.

### 20.6 SaaS-embedded model-call observability

**The framework's position (§15).** Register SaaS-embedded AI features at feature level; consume vendor audit logs; apply CASB or DLP at the egress.

**What cannot be done.** See the vendor's internal model call. Vendor-internal traffic does not traverse the organisation's network; no CASB, DLP, or guardrail of the organisation's is in that path. Vendor audit logs are what they are.

**Current operating practice.** Demand log-export SLAs in contract; monitor seat-level consumption; enforce features-off-by-default; treat the SaaS AI call as an opaque black box whose control is contractual.

**Monitored for maturation.** Whether any major SaaS vendor ships a field-level AI-access audit that passes an auditor's test; whether sub-processor transparency improves.

### 20.7 Kill-switch MTTR realism

**The framework's position (§12.1).** 15-minute MTTR from alert to full stop for Tier 1 agents, with all four kill primitives.

**What is genuinely hard.** Real-world MTTR depends on alert latency, on-call routing, gateway-revocation propagation, session-token revocation propagation across runtimes, and the coordination of tool-level and data-access-level revocations. First-time drills frequently exceed the target; organisations new to AI operations should plan for a 30–60 minute initial capability, compressing over time.

**Current operating practice.** Document the MTTR target; validate by annual live-fire drill; treat any drill-to-target gap as a finding with a remediation plan.

**Monitored for maturation.** Platform-level "agent kill" primitives maturing to the point where a single operator action achieves all four revocations.

### 20.8 Tool-definition signing

**The framework's position (§14.2).** Tool definitions hashed on first-trust; change generates alert; signed manifests SHOULD be preferred.

**Later maturity.** A universally-adopted signed-manifest standard for MCP and vendor tool registries. Multiple efforts are moving in this direction; none is yet mature.

**Current operating practice.** First-trust hash per tool definition; drift alerting; gateway-mediated outbound MCP.

### 20.9 Prompt-injection detection at production false-positive rates

**The framework's position (§14.1).** Retrieved content treated as untrusted; guardrail layered; tool-output sanitised.

**What is not solved.** Inline detection of steady-state prompt injection at production volume with the false-positive rate a regulator would call "prevented." Every layered defence reduces the risk; none closes it. Defence-in-depth plus human-in-the-loop on irreversible action is the framework's answer; a perfect detector is not available.

**Current operating practice.** Layered guardrails (§3 layers 2–3), structural mitigations (spotlighting, structured outputs, sanitisation), instruction-and-data separation, HITL on writes.

### 20.10 Multi-agent emergent failure modes

**The framework's position (§8).** Agent identity and delegation discipline.

**What no production framework detects.** Malicious or compromised agents diverging from intended behaviour (OWASP Agentic Top 10 ASI10) and single-fault propagation across agents into system-wide impact (ASI08). Emergent collusion, sycophancy cascades, goal drift, and reward hacking across agent interactions fall here. No inline control catches this class fully; the mitigations are architectural (avoid uncontrolled multi-agent loops; require human-in-the-loop at composition seams), evaluative (evaluation suites include multi-agent traces), and attestation-based (signed behavioural manifests declaring expected tools and goals, validated by the orchestrator before each action; watchdog agents that monitor peers for deviation).

**Current operating practice.** Avoid composed multi-agent architectures where a single-agent design will do — the Least-Agency principle (§2.8) applied to composition. Any multi-agent composition is a material design decision and requires Tier 1 review regardless of the individual agents' tiers. Behavioural-manifest attestation and watchdog-agent patterns are monitored for maturation rather than required today.

### 20.11 Evaluation does not prove safety; LLM-as-judge is not ground truth

Passing an evaluation suite provides evidence against tested scenarios; it does not prove the absence of unsafe behaviour. LLM-as-judge scales evaluation but requires calibration against human judgement, drift monitoring, and spot-check sampling. Neither should carry sole evidentiary weight for Tier 1 safety.

**Current operating practice.** Acceptance thresholds set before evaluation runs; human spot-check baseline for Tier 1; LLM-as-judge calibrated against a human-labelled reference set and re-calibrated on model change.

### 20.12 Redaction is lossy; retrieval authorisation drifts; memory creates records

- PII and secret redaction in prompts, traces, and memory is imperfect; systems must assume sensitive data may enter logs despite redaction.
- Source-system ACLs change faster than indexes. Authorisation-aware retrieval (§9.3) requires continuous reconciliation, which is operationally non-trivial.
- Long-term agent memory can create regulated records and right-to-erasure exposure that did not exist before the memory was introduced.

**Current operating practice.** Redaction combined with classification and access controls on log stores (§13.2); declared ACL-reconciliation cadence in the Context Corpus Card; long-term memory use is a registered design decision, not a default.

### 20.13 Vendor model behaviour is not fully controllable

Even pinned models may change behaviour due to backend infrastructure, safety layers, routing, or provider-side mitigations unless contractually fixed and technically observable. A pinned alias is not the same as a frozen model.

**Current operating practice.** Resolved-model-identifier capture (§11.5); Alert-for-Revalidation on identifier change; online evaluation (§11.4) catches behavioural drift that version-pinning cannot.

### 20.14 Register maintenance

Discovering AI features inside SaaS products the moment a vendor turns them on, keeping the registry current with business-unit-initiated feature toggles, and avoiding registry rot where entries outlive the systems they describe are all genuinely hard.

**Current operating practice.** Procurement hook at contract renewal; shadow-AI discovery monthly; quarterly reconciliation; orphan-entry escalation at 90 days. These mechanisms detect sprawl with delay, not in real time.

### 20.15 Post-quantum readiness of agent-signing primitives

The framework's signing and attestation patterns — signed tool manifests (§14.2), signed intent envelopes (§10.5), workload-identity certificates (§8.1), and behavioural manifests (§20.10) — assume classical public-key cryptography (RSA, ECDSA, Ed25519). Post-quantum migration for these primitives is not yet a solved problem at scale in agent tooling.

**Current operating practice.** Adopt crypto-agile key-management where the signing stack permits; track NIST PQC standardisation (ML-KEM, ML-DSA, SLH-DSA) and the major PKI vendors' PQ readiness; prefer signing schemes and libraries that are on a stated PQ-migration path; treat long-lived signed artefacts (for example a signed tool manifest cached for years) as the highest-priority migration targets.

---

# Appendices

## Appendix A — Registration schema

Every registered AI system records the following. Fields marked M are mandatory at registration; fields marked O are required before deployment.

| Field | M / O | Notes |
|---|---|---|
| System name | M | Unique, human-readable |
| Registry ID | M | System-assigned |
| Owner (human, named) | M | Product owner |
| Engineering lead | M | Accountable engineer |
| Data owner | M | Accountable for data classification and corpus |
| Business purpose | M | One paragraph |
| Risk tier | M | 1, 2, or 3 |
| User population | M | Customer, internal business unit, public, restricted group |
| Geography / jurisdiction | M | Where users are; where data is processed |
| Legal basis for personal data | O | Consent, contract, legitimate interest, or other (where personal data is in scope) |
| Data classification (input) | M | Public, Internal, Confidential, Restricted, Personal |
| Data classification (output) | M | Same scale |
| Source data systems | M | Systems the AI reads from |
| Model(s) used | M | Provider, family, version, pinning mechanism |
| Inference path | M | Gateway route, direct SDK, vendor-embedded, etc. |
| Tools / MCP endpoints | M | Named; each with tool-definition hash if MCP |
| External network access (yes/no) | M | Can the agent reach the internet |
| External communication capability (yes/no) | M | Can the agent send messages, email, or post to external systems |
| Write capability (yes/no) | M | Can the agent mutate state |
| Destructive action capability (yes/no) | M | Can the agent perform irreversible actions (subject to §10.4) |
| Guardrail configuration ID | O | Reference to the applied guardrail policy |
| Runtime bounds profile | O | Reference to Appendix B default or named exception |
| Agent workload identity (deployment / session) | O | SPIFFE ID or equivalent |
| Delegation model | O | How human → agent → tool identity chain works |
| Secrets posture | O | Static, dynamic, or session-bound; where held |
| AI System Card | O | Link |
| Context Corpus Card | O | Required if retrieval is used |
| Tool Risk Card(s) | O | One per registered tool |
| Observability destination | O | LLM-observability project, SIEM index |
| Redaction rules | O | What is redacted, where |
| Evaluation suite reference | O | Link to evaluation plan and last result |
| Acceptance thresholds | O | Per-metric thresholds for deployment |
| Kill-switch runbook | O | Link |
| Failover / portability plan | O | Required for Tier 1; substitute model, evaluation delta, switching cost, runbook owner, Last Rehearsal Date |
| Last validation date | O | Updated on revalidation |
| Next revalidation due | O | Per §11.5 cadence |
| Last access review (NHI) | O | Quarterly for Tier 1 and Tier 2 |
| Active exceptions | O | Each with expiry date and residual-risk owner |
| User disclosure required | O | Yes or no; where and how |
| Business continuity fallback | O | What happens if the AI is unavailable |
| Last incident or tabletop | O | Date and summary |
| Attestation status | O | Current attestation executive sign-off |

## Appendix B — Default bounds

Normative defaults. Applied automatically at registration. Exceptions per §10.2. These are organisational starter defaults, not evidence-based universal limits. Each system calibrates against its own task class, blast radius, latency SLO, cost SLO, and incident-response capability.

| Bound | Default | Hard ceiling | Enforcement layer |
|---|---|---|---|
| Wall-clock (sync) | 5 min | 15 min | Runtime |
| Wall-clock (async) | 30 min | 2 h | Runtime |
| Tool calls per task | 50 | 200 | Framework and gateway |
| Concurrent sessions per agent identity | 10 | 50 | Runtime |
| Reasoning / recursion depth | 15 | 40 | Framework |
| Token budget per session | 200K | 1M | Gateway and LLM observability |
| Cost budget per session (dev) | $10 | $50 | Gateway (interrupting) |
| Cost budget per session (prod) | Tiered, set at registration | — | Gateway (interrupting) |
| Records touched per action | 100 | 1,000 | Tool definition |
| External recipients per message action | 1 | 10 | Tool definition |
| Funds moved or committed per session | Set at registration | — | Tool definition and gateway |
| Memory writes per session | 50 | 200 | Agent runtime |
| Retry count per tool | 3 | 10 | Framework |
| Tool error-rate stop threshold | 25% over 10 consecutive calls | — | Framework |
| HITL gate on write-scope | Required | Not waivable for Tier 1 | Tool definition and gateway |
| Exception-grant SLA | 5 working days | 10 for Tier 1 | Procedural |

The 200K-token-per-session default is intentionally conservative relative to current frontier-model context windows (several 2026 models support ≥1M-token context). Long-context document loading, large-corpus retrieval, and multi-document reasoning workloads will routinely exceed it; calibrate against the system's actual task class and re-tier where the workload demands it. The hard ceiling tracks current frontier capacity rather than recommended use — a single invocation that fills the ceiling is itself a design signal worth registering.

## Appendix C — Threat model reference

### OWASP Top 10 for LLM Applications 2025 (GenAI Security Project version)

This framework references the OWASP GenAI Security Project 2025 taxonomy. Older OWASP project pages use different numbering; where this document references an LLM-nn identifier, it refers to the 2025 list below.

| ID | Threat |
|---|---|
| LLM01 | Prompt Injection |
| LLM02 | Sensitive Information Disclosure |
| LLM03 | Supply Chain |
| LLM04 | Data and Model Poisoning |
| LLM05 | Improper Output Handling |
| LLM06 | Excessive Agency |
| LLM07 | System Prompt Leakage |
| LLM08 | Vector and Embedding Weaknesses |
| LLM09 | Misinformation |
| LLM10 | Unbounded Consumption |

### OWASP Top 10 for Agentic Applications 2026 (primary agentic reference)

Published December 2025 by the OWASP GenAI Security Project, Agentic Security Initiative. Introduces the Least-Agency principle (see §2.8) and is scored via the OWASP AI Vulnerability Scoring System (AIVSS).

The ASI-to-T&M and ASI-to-AIVSS mappings below are taken directly from the OWASP Top 10 for Agentic Applications 2026 PDF, Appendix A (OWASP Agentic AI Security Mapping Matrix). They are the framework's published mappings, not an internal interpretation. Where this document paraphrases or compresses OWASP wording, the authoritative OWASP source controls. The three taxonomies (ASI, T&M, AIVSS) overlap but were not designed as one-to-one peers — a single ASI risk often maps to several T-numbers, and AIVSS scoring covers infrastructure-CVE dimensions that ASI deliberately does not. Adopters should treat the mapping as a navigation aid across taxonomies, not a coverage equivalence.

| ID | Threat | Mapped Threats & Mitigations (v1.1) | AIVSS Core Risk |
|---|---|---|---|
| ASI01 | Agent Goal Hijack | T6 Goal Manipulation · T7 Misaligned & Deceptive Behaviours | Agent Goal & Instruction Manipulation |
| ASI02 | Tool Misuse and Exploitation | T2 Tool Misuse · T4 Resource Overload · T16 Insecure Inter-Agent Protocol Abuse | Agentic AI Tool Misuse |
| ASI03 | Identity and Privilege Abuse | T3 Privilege Compromise | Agent Access Control Violation |
| ASI04 | Agentic Supply Chain Vulnerabilities | T17 Supply Chain Compromise · T2 · T11 · T12 · T13 · T16 | Agent Supply Chain & Dependency Attacks |
| ASI05 | Unexpected Code Execution | T11 Unexpected RCE and Code Attacks | Insecure Agent Critical Systems Interaction |
| ASI06 | Memory & Context Poisoning | T1 Memory Poisoning · T4 Memory Overload · T6 Broken Goals · T12 Shared Memory Poisoning | Memory Use & Contextual Awareness |
| ASI07 | Insecure Inter-Agent Communication | T12 Agent Communication Poisoning · T16 Insecure Inter-Agent Protocol Abuse | Agent Memory & Context Manipulation [^asi07] |
| ASI08 | Cascading Failures | T5 Cascading Hallucination Attacks · T8 Repudiation & Untraceability | Agent Cascading Failures |
| ASI09 | Human-Agent Trust Exploitation | T7 Misaligned & Deceptive Behaviours · T8 Repudiation & Untraceability · T10 Overwhelming Human in the Loop | Agent Untraceability / Human Manipulation |
| ASI10 | Rogue Agents | T13 Rogue Agents in Multi-Agent Systems · T14 Human Attacks on Multi-Agent Systems · T15 Human Manipulation | Behavioural Integrity · Operational Security · Compliance Violations |

[^asi07]: The ASI07 → "Agent Memory & Context Manipulation" mapping reproduces the OWASP Top 10 for Agentic Applications 2026 PDF, Appendix A (OWASP Agentic AI Security Mapping Matrix), exactly as published. Adopters and reviewers may reasonably observe that this AIVSS Core Risk reads as more aligned with ASI06 (Memory & Context Poisoning) than with ASI07 (Insecure Inter-Agent Communication); the framework reproduces the OWASP source rather than re-mapping it. Where the apparent semantic mismatch matters in practice, treat ASI07 as covering inter-agent protocol and channel-integrity concerns and consult §8.1 (agent-card and A2A integrity), §14.2 (tool and MCP poisoning), and §17 (concentration and severance runbooks).

### OWASP Agentic AI — Threats and Mitigations v1.1 (secondary reference)

Retained for depth. T1–T17 provide a finer-grained threat decomposition than the Top 10.

| ID | Threat |
|---|---|
| T1 | Memory Poisoning |
| T2 | Tool Misuse |
| T3 | Privilege Compromise |
| T4 | Resource Overload / Memory Overload |
| T5 | Cascading Hallucination Attacks |
| T6 | Intent Breaking & Goal Manipulation (Broken Goals) |
| T7 | Misaligned & Deceptive Behaviours |
| T8 | Repudiation & Untraceability |
| T9 | Identity Spoofing & Impersonation |
| T10 | Overwhelming Human-in-the-Loop |
| T11 | Unexpected RCE and Code Attacks |
| T12 | Agent Communication Poisoning / Shared Memory Poisoning |
| T13 | Rogue Agents in Multi-Agent Systems |
| T14 | Human Attacks on Multi-Agent Systems |
| T15 | Human Manipulation |
| T16 | Insecure Inter-Agent Protocol Abuse |
| T17 | Supply Chain Compromise |

## Appendix D — External anchoring

This framework is internally consistent on its own terms. The references below show how it maps to external standards, not how it derives from them.

| Standard | Relevance |
|---|---|
| **NIST AI RMF 1.0** (Jan 2023) | Govern, Map, Measure, Manage functions; the framework's control design follows this shape. |
| **NIST AI 600-1** (Generative AI Profile, Jul 2024) | GenAI-specific risk categories; evaluation and measurement guidance. |
| **NIST AI 100-2e2025** (Adversarial ML taxonomy) | Threat-model input. |
| **ISO/IEC 42001:2023** (AI Management Systems) | Annex A controls are the closest external equivalent to this framework's operational controls; adopt as certification target if the organisation pursues one. Provides the management-system wrapper (policy, roles, resources, competence, awareness, communication, documentation, operation, performance, improvement) that complements the technical controls here. |
| **U.S. banking model-risk guidance** (Fed SR 26-2 / OCC Bulletin 2026-13, joint OCC/Fed/FDIC, 17 April 2026 — superseding SR 11-7 and OCC Bulletin 2011-12) | Relevant by analogy for governance, validation independence, ongoing monitoring, vendor oversight, and outcomes analysis. The 2026 revised interagency guidance is principles-based and **expressly excludes generative AI and agentic AI from its scope** (per a footnote describing such models as "novel and rapidly evolving"). Agencies have signalled forthcoming separate work — including an anticipated request for information on banks' use of AI — to address GenAI and agentic AI specifically. The organisation applies MRM-style discipline to Tier 1 GenAI and agentic systems as an internal risk-control standard and tracks future agency guidance. Legal-desk confirmation of current supervisory position is an implementation prerequisite for any regulated-entity adoption. |
| **OCC Bulletin 2023-17** | Third-party risk management; applies to vendor-supplied models and SaaS-embedded AI. |
| **OWASP LLM Top 10 (2025 GenAI Security Project version)** | Threat-class reference; Appendix C. |
| **OWASP Top 10 for Agentic Applications 2026** (Dec 2025) | Primary agentic threat reference (ASI01–ASI10); Appendix C. Introduces Least-Agency (§2.8). |
| **OWASP Agentic AI Threats and Mitigations v1.1** | Secondary agentic threat reference (T1–T17); Appendix C. |
| **OWASP Non-Human Identities Top 10 (2025)** | Non-human-identity lifecycle risks (offboarding, secret leakage, overprivileged NHIs, long-lived secrets, human use of NHIs). Anchors the quarterly NHI access review in §8.1. |
| **OWASP AI Vulnerability Scoring System (AIVSS)** | Scoring and prioritisation framework used by the Agentic Top 10. Applied to Tier 1 evaluation findings and post-incident scoring. |
| **OWASP CycloneDX and AIBOM** | Bill-of-Materials standard for AI components. The framework's supply-chain and drift controls (§14.2, §17) align with CycloneDX AIBOM expectations; adopt as the interchange format where possible. |
| **EU AI Act** | Entered into force August 2024; prohibited-practice and AI-literacy obligations from February 2025; general-purpose-AI obligations from August 2025; full high-risk applicability August 2026; product-integrated high-risk extensions to August 2027. Applies case-by-case based on provider or deployer role, EU market placement, EU users or affected persons, high-risk classification, prohibited practices, GPAI obligations, and AI-literacy obligations. Legal review required for any system used in or affecting the EU. |
| **MCP authorisation specification** | Protected Resource Metadata, Resource Indicators, Client ID Metadata Documents; Dynamic Client Registration retained as fallback. |
| **Personal-data and incident-notification regimes** *(illustrative — applicability is a legal determination)* | Examples adopters may need to map against include GDPR Article 33 (notification to supervisory authority within 72 hours of awareness) and Article 34 (notification to affected individuals where high risk to rights and freedoms); UK GDPR; applicable U.S. state and national privacy laws; SEC Item 1.05 of Form 8-K (four business days from materiality determination for material cybersecurity incidents); NIS2 (24-hour early-warning, 72-hour notification, one-month final report for significant incidents); DORA (incident-classification and reporting timelines for in-scope financial entities); HIPAA breach notification; sectoral and supervisor-specific reporting obligations. Cited timelines reflect published rules at the time of writing and are subject to regulator change; specific applicability and current timing are determined by the legal desk. |

## Appendix E — Glossary

| Term | Definition |
|---|---|
| **A2A** | Agent-to-agent communication |
| **Agent** | A system that autonomously plans multi-step actions using a large language model and invokes tools to execute them |
| **AIBOM** | AI Bill of Materials — inventory of AI components (models, datasets, prompts, tools) in a system |
| **AI System Card** | Human-readable governance artefact per §6.1 |
| **AIVSS** | AI Vulnerability Scoring System (OWASP) |
| **AI-SPM** | AI Security Posture Management — inventory and exposure of AI assets |
| **Bounds** | Runtime limits (wall-clock, steps, tool calls, tokens, cost, action-scope) applied to an agent |
| **Canary token** | Seeded content used to detect exfiltration (§9.6) |
| **Data-flow seam** | The control boundary governing what content enters the model, from where, at what trust level, and under what authorisation |
| **Decision seam** | The control boundary governing reconstructable decision evidence |
| **Decision-evidence trace** | The inputs, retrieved context, policies, outputs, tool calls, approvals, and state transitions captured for an invocation |
| **Guardrail** | An inline policy layer that inspects prompts, retrieved content, or outputs |
| **HITL** | Human-in-the-loop |
| **Identity seam** | The control boundary governing who acts, on whose behalf, with what delegation |
| **Intent Gate** | Policy Enforcement Point between planning and execution for high-impact tool calls (§10.5) |
| **Least-Agency** | Design principle to avoid unnecessary autonomy (§2.8) |
| **LLM observability** | Prompt, response, trace, and cost tooling for LLM-based systems |
| **MCP** | Model Context Protocol |
| **MRM** | Model Risk Management |
| **NHI** | Non-human identity |
| **Principal** | The human on whose behalf an agent acts |
| **RAG** | Retrieval-augmented generation — one implementation pattern of runtime context retrieval, typically using indexed vector stores. The framework's retrieval controls (§9) cover RAG and all other runtime-context-retrieval patterns |
| **Context Corpus Card** | Human-readable governance artefact for any runtime context source (indexed corpus, live source-system API, memory store, SaaS-grounded retrieval) per §6.1 |
| **Context source** | Any data source an AI system retrieves from at runtime: indexed corpora, live source-system APIs, long-context document loading, agent memory stores, SaaS-grounded retrieval |
| **Runtime context retrieval** | The retrieval step at runtime that supplies context to a model, whether from an index, a live API, memory, or a SaaS-grounded source. Generalises "RAG" to cover non-indexed patterns (§9, §9.8) |
| **Seam** | A control boundary between layers where incidents typically originate |
| **Tier** | The risk classification assigned to an AI system at registration |
| **Tool definition** | The schema and description by which a model discovers and invokes a tool |
| **Tool Risk Card** | Human-readable governance artefact for a registered tool per §6.1 |
| **Workload identity** | Non-human identity assigned to a specific agent or service |

## Appendix F — Document history

| Version | Date | Summary |
|---|---|---|
| 1.0 | 2026-04-24 | Initial reference framework published for adoption. |
| 1.1 | 2026-04-24 | Reframed as public reference framework (licence, adoption note, citation). Renamed RAG Corpus Card → Context Corpus Card; broadened retrieval controls to runtime context retrieval including live source-system APIs (§9.8). Added secure AI SDLC consolidation (§6.3), SaaS exception to three-identity-record requirement (§8.1), prohibited-use policy-ownership note (§1.3), vendor-example disclaimer (§3), tightened Sev 1 harm language, softened Tier 3 red-team cadence, broadened retrieval evaluation metrics. Added §12.7 External notification (GDPR, SEC, NIS2, DORA obligations), §13.7 AI outputs as business records, §20.15 post-quantum readiness. Added Appendix G Control test matrix for auditability. |
| 1.1.1 | 2026-04-29 | Editorial polish and publication-readiness pass for public release. Added author and maintainer attribution to the front-matter table and the "How to cite" block (Muhammad Elsaiedy, elsaiedy@ccoe.io) to satisfy CC BY-SA 4.0 attribution requirements. **Editorial:** added top-level "not legal, regulatory, audit, or licensing advice" statement; definitive licence wording (CC BY-SA 4.0, no "recommended" hedge); generic Risk Committee definition for adopters of varying size; clarified that RFC 2119 normative language binds adopters who implement this framework as policy, not the framework's readers; §12.7 restructured (notification *categories* in body text; named regimes — GDPR, SEC, NIS2, DORA, HIPAA — moved to Appendix D as illustrative with a single legal-disclaimer line); §19.1 adds vendor- and product-example refresh at each scheduled review; Appendix C provenance language moved above the OWASP Agentic 2026 mapping table with a coverage-drift caveat (ASI / T&M / AIVSS taxonomies are not one-to-one peers); Appendix G expanded with Owner, Frequency, and Evidence-artefact columns for each control test; Layer-3 §3 Evaluation row updated from "RAG retrieval quality" to "context-retrieval quality (relevance, freshness, authorisation correctness, provenance, faithfulness)" for taxonomy consistency; §9.3 residual "Corpus Card" reference normalised to "Context Corpus Card". **Factual fixes:** §8.2 / §20.1 corrected "OAuth 2.1 token-exchange (RFC 8693)" → "OAuth 2.0 token-exchange (RFC 8693), used within the OAuth 2.1 framing adopted by the MCP authorisation specification" (RFC 8693 is OAuth 2.0 Token Exchange); Appendix D U.S. banking MRM row rewritten to reflect the joint OCC/Fed/FDIC issuance of 17 April 2026 (Fed SR 26-2 / OCC Bulletin 2026-13) which supersedes SR 11-7 and OCC Bulletin 2011-12, and which expressly excludes generative AI and agentic AI from its scope. **Product-name accuracy:** "Vertex Agent Builder" → "Vertex AI Agent Builder"; "Anthropic claude-agent-sdk" → "Anthropic Claude Agent SDK"; added OpenAI Agents SDK and AWS Strands as peers; "Nudge" / "Grip" → "Nudge Security" / "Grip Security". **Internal consistency:** §6 Pre-deployment row names Application Security Reviewer (Tier 2/3) alongside Independent validator + Risk Committee (Tier 1); §8.1 NHI access-review cadence explicit for Tier 3 (annual or organisation-wide NHI policy, whichever is more frequent); §11.5 model-change paragraph reordered to lead with the per-invocation resolved-identifier check, alias-level pinning explicitly secondary; §12.7 cross-link to Appendix D notification regimes strengthened. **Calibration:** §10.3 "high tool-call frequency" given a numeric starter threshold (≥60 write-tool invocations per minute per approver, or below the empirically measured non-rubber-stamp rate, whichever is lower); §11.1 "5% sample" clarified as the human-spot-check fraction of LLM-judged samples; §11.2 golden-set protection sharpened from "where avoidable" to read-once / audit-logged path / bulk programmatic access prohibited; Tier 1 Outcome-analysis golden sets held third-party, escrowed, or by an internal function organisationally separate from the building team where the validator-independence requirement makes building-team custody insufficient; eval drift split into dataset / threshold / judge-model drift. **Slop trim:** §2.1 "Defence in depth is the architecture, not a slogan" reframed as plain statement; §4 "is cosmetic" duplicate of §2 removed; §7 "Registration is the spine" cross-referenced to §2 Principle 3 instead of restated; §17 "Portability is a living control, not a document" replaced with "A portability path is only as good as its last rehearsal." **Appendix C:** ASI07 row footnoted to acknowledge the OWASP-published mapping ("Agent Memory & Context Manipulation") reproduces the OWASP source verbatim despite the apparent semantic mismatch with inter-agent communication. **Appendix B:** added a calibration note on token-budget defaults vs current frontier-model context windows (≥1M-token). **§13.5:** OTel "preferred interchange format" wording sharpened to distinguish stabilising model-span attributes from still-experimental agent- and tool-span attributes. **§17:** named the adapter-pattern implementation primitives (model-routing gateway — LiteLLM, Portkey, Cloudflare AI Gateway — or a hand-rolled provider adapter); noted that vendor-internal routing primitives such as Bedrock cross-region inference profiles help intra-vendor model and region substitution but do not by themselves satisfy provider-portability. |

---

## Appendix G — Control test matrix

A starter set of operational tests for each major control. Adopters extend this with their own tests, calibrate thresholds to their environment, name specific role-holders to the Owner column, and integrate the tests into the release pipeline, internal-audit programme, and attestation evidence.

Every test is expressed as: a control being tested, a test method, the expected observable, the pass criterion, the role responsible for executing it, the cadence at which it is executed, and the evidence artefact retained. Owner names role-types — adopters substitute named functions or individuals from their own organisation. Frequency is the minimum cadence; adopters may increase it.

| Control | Test method | Expected observable | Pass criterion | Owner (role) | Frequency | Evidence artefact |
|---|---|---|---|---|---|---|
| **Registration completeness (§7)** | Sample 5% of production AI invocations from gateway and observability logs over a rolling 30-day window; for each distinct system, verify a registry entry exists. | Registry entry with matching system identifier, current owner, and in-scope tier. | 100% of sampled invocations trace to a registered system; unmatched invocations open a Sev 3 finding. | Platform engineering (registry custodian) | Monthly | Sampling report with invocation IDs and registry-match status, retained in the registry-evidence log |
| **Query-time ACL retrieval (§9.3)** | Issue a retrieval query as a synthetic test principal who lacks source-system permission on a known document; observe the retrieval log and the model response. | Retrieval log shows the document was considered and denied; model output contains no information from the document. | No leakage from the denied document; authorisation decision recorded in the retrieval log with the principal identity. | Application security, supported by data governance | Per release of any retrieval component; quarterly otherwise | Synthetic-test result record and the corresponding retrieval-log query, retained 12 months |
| **Live source-system retrieval provenance (§9.8)** | Issue a live-API retrieval via a registered tool; inspect the decision-evidence trace. | Trace contains source system, object identifier, effective classification, requesting principal, and authorisation decision. | All five fields present for every live-retrieval invocation in Tier 1 and Tier 2. | Application security | Continuous via observability assertions; per-release sample for each new live-retrieval tool | Decision-evidence trace samples retained 90 days; assertion-failure alerts retained 12 months |
| **Tool-definition drift (§14.2)** | Modify a registered MCP tool's schema, description, or allow-listed arguments in a staging environment; attempt to invoke via a Tier 2 agent. | The gateway or registry blocks the call; an alert is raised; the registry marks the tool as "drift-pending-review". | Block, alert, and registry state all occur within the published SLA; auto-merge into the allow-list is not possible. | Platform engineering (gateway and registry owner) | Continuous (CI gate on every tool change) plus quarterly synthetic drill | Drift-detection alert log and quarterly drill report |
| **MCP authorisation (§8.2)** | Attempt MCP access from a client that does not provide a valid Resource Indicator or valid PKCE exchange. | Authorisation server rejects the request with a specific error class; client logs the rejection. | Rejection is verifiable in both server-side and client-side logs; redirect URI validation is exact. | Identity engineering / IAM | Per release of any MCP-facing component; annual full review | Authorisation-server reject log with synthetic-test correlation IDs |
| **Kill-switch live-fire drill (§12.1)** | In a non-production environment that mirrors production identity and gateway topology, execute the "stop this agent" procedure against a test Tier 1 agent; time each of the four kill primitives. | All four kill primitives complete; tool calls from the agent fail after revocation; session token ceases to authenticate. | MTTR at or under the Tier 1 target; any drill overrun logged as a finding with remediation plan; drill is repeated within the correction cycle. | SRE and incident-response lead, jointly | Annual minimum per Tier 1 system, plus after any material change to the action-execution path | Drill report (timing, observed results, remediation actions); retained for the life of the system |
| **Model-change Alert-for-Revalidation (§11.5)** | In staging, force a change in the resolved model identifier (swap inference profile alias to a different model). | Registry emits Alert-for-Revalidation; promotion to Tier 1 traffic is suspended in the gateway or deployment pipeline. | Alert is emitted and promotion is blocked without manual operator action; unblocking requires the Tier-1 model-change sign-off. | ML engineering and model-validation | Per Tier 1 model deployment, plus monthly automated alias-resolution check | Alert log and the corresponding sign-off ticket |
| **Prompt-redaction and evidence integrity (§13.2, §13.4)** | Inject a synthetic secret (for example a fake API key of known format) into a test invocation's prompt; verify the logged record. | Synthetic secret is redacted or replaced with a digest in the logged record; full-fidelity record exists in the restricted-access store if required. | The secret does not appear in cleartext in any archive accessible to non-privileged log readers; integrity-break detection fires on any log record with a missing invocation ID. | Observability platform owner, supported by data privacy | Continuous via canary-token seeding plus quarterly synthetic test | Canary-token detection report; log integrity-hash report; redaction-failure alerts |
| **HITL gate on write (§10.3)** | Attempt a write-scope tool invocation from a Tier 1 agent without the out-of-band approval record. | The gateway or tool-definition layer blocks the call; an audit record is created for the blocked attempt. | Block occurs before any mutation; no write reaches the target system; approval-bypass attempts are visible in the audit trail. | Application security | Per release of write-scope tooling; monthly synthetic | Synthetic-test record and audit-log entries for blocked attempts |
| **No direct destructive tool (§10.4)** | Attempt to register a Tier 1 agent with a tool whose capability card declares an irreversible destructive operation. | Registration validator rejects the tool binding; suggests the proposed-transaction pattern. | No Tier 1 registration exists with a direct destructive tool; any legacy exception has an expiry and a residual-risk owner. | Risk function and platform engineering, jointly | Continuous on registration; quarterly registry audit | Registry validator report and tool-binding audit |
| **Coding-agent CODEOWNER gate (§16)** | Submit a pull request authored by a coding agent that modifies a CI/CD workflow file without CODEOWNER review. | CI blocks the merge; the PR status shows the CODEOWNER requirement unmet. | Merge is not possible without a non-agent CODEOWNER approval; self-approval by the agent is rejected. | Application security and developer experience, jointly | Continuous (every PR) | CI block log and PR review records |
| **Coding-agent sensitive-path gate (§16)** | Submit an AI-authored PR modifying authentication, cryptography, or IAM code without a security reviewer. | CI or branch-protection blocks the merge; the PR is routed to a security reviewer. | Merge requires an explicit security reviewer; the agent's identity trailer does not satisfy the security-reviewer role. | Application security | Continuous (every PR touching sensitive paths) | Branch-protection log and security-review records |
| **SaaS AI feature discovery (§15)** | Enable a registered SaaS product's AI feature in a test tenant without creating a registry entry. | Shadow-AI discovery or the SaaS admin-audit pipeline detects the activation within the declared cadence; the registry records a discovery event. | Detection within the stated Tier cadence (daily for Tier 1, weekly for Tier 2); escalation to platform engineering within SLA. | Platform engineering and SaaS-governance lead | Continuous via shadow-AI discovery; quarterly reconciliation against the SaaS portfolio | Discovery-pipeline log and the registry discovery-event entries |
| **Exception expiry (§7)** | Query the registry for active exceptions; verify expiry dates and renewal evidence. | Every exception has an expiry date, a residual-risk owner, and (for renewals) evidence of compensating-control operation during the prior period. | No permanent exceptions; no exception past its expiry without either renewal or closure. | Risk Committee secretariat | Monthly registry sweep; quarterly committee review | Exception-register report and committee minutes |
| **User disclosure (§6.2)** | For a sampled Tier 1 decision-affecting flow, inspect the user-facing output at the point of decision. | Disclosure of AI involvement is visible; a path to human review is offered; contact for complaints is present. | Disclosure matches the text declared in the System Card; any drift is a control finding. | Product, supported by risk and legal | Per release of any user-facing change to a Tier 1 system; quarterly UX audit | Sampled UI captures and the matching System Card snapshot |
| **External notification readiness (§12.7)** | Tabletop a Sev 1 AI incident with personal-data exfiltration; execute the notification workflow without sending. | The legal-desk-led workflow identifies applicable regimes (per Appendix D), produces a draft notification, and times the internal decision cycle. | Draft notification is ready within the regulatory timeline net of external counsel review; contact lists are current; escalation to the attestation executive occurs in the prescribed window. | Legal desk and incident-response lead, jointly | Annual tabletop minimum; after any material change to incident-response or to the regimes catalogued in Appendix D | Tabletop report, draft notification, and the workflow-timing record |
| **Live-fire portability exercise (§17, §20.3)** | For one Tier 1 system per year, execute a documented alternative-inference-path cutover in a staging environment. | The substitute model is invoked; the evaluation delta is measured; the operational runbook is exercised end-to-end. | Cutover completes; evaluation delta is within the declared acceptable range; the Last Rehearsal Date is updated in the registry. | SRE and ML engineering, jointly | Annual per Tier 1 system | Cutover runbook execution report, evaluation-delta record, and the registry Last Rehearsal Date update |
