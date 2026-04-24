# From Principles to Architecture

## How Each Foundation Principle Becomes a Technical Constraint

This document is the bridge. For each principle in `../PRINCIPLES.md` and each non-negotiable in `../docs/07-non-negotiables.md`, we translate the value commitment into concrete architectural requirements.

When an engineer asks *"why does our system work this way?"*, the answer is here.

---

## Principle 1 — Non-Sectarian Unity

### Principle text (summary)
Welcome Muslims across the full mainstream Islamic scholarly tradition; never privilege one school as "the" Muslim position when plurality exists.

### Architectural implications

- **Corpus structure** must support multi-tradition sourcing as a first-class concern. Schema includes `tradition` and `school` fields at the citation level, not as a hack retrofitted later.
- **Retrieval** must be able to pull multi-school results by default. Single-school filtering is opt-in at the query or institution level, never silently applied.
- **Ranking** must not systematically demote one school's sources relative to another. Equal weighting by default; deviations from equal weighting are disclosed in user interface.
- **Answer generation** must represent plural views when sources genuinely disagree. Prompts instruct LLMs to surface disagreement rather than smooth it over.
- **Per-institution filtering** is a configuration, not a data-level bias. A Shafi'i madrasa can configure "Shafi'i-first"; the underlying system still holds all schools equally.

### Implementation traceability

See: `03-data-architecture.md#citation-schema`, `basira/02-rag-pipeline.md#multi-school-retrieval`.

---

## Principle 2 — Radical Transparency

### Principle text (summary)
All material decisions happen in public; dissent is documented; no private deliberation of material consequence.

### Architectural implications

- **Source code** for Layer 1 and Layer 2 components (standards, developer tooling) is public from day one — no private dev phase that later gets "cleaned up" for release.
- **Architecture Decision Records (ADRs)** are published alongside the code, including rejected alternatives and dissenting opinions.
- **Observability** of our own operations is published: transparency reports, incident reports, governance decisions, funding disclosures.
- **Product opacity is disclosed.** When Basira's fine-tuned production model is proprietary, that fact is disclosed, along with what it does and how users can verify its behavior.
- **No undisclosed telemetry.** Any data collection that happens in production is in the privacy policy. If it's not in the policy, it does not happen.

### Implementation traceability

See: `02-tech-stack-decisions.md#observability`, `../PRIVACY.md`.

---

## Principle 3 — Human Dignity Absolutism

### Principle text (summary)
Never build or sell technology that harms innocents.

### Architectural implications

- **No targeting, tracking, or profiling capabilities** in production systems, even if theoretically valuable for analytics.
- **No "export all user interactions" features** for third parties, even for paying customers.
- **Rate limiting, abuse detection, and access controls** designed to catch misuse patterns, not to build dossiers.
- **Refusal behavior in the LLM layer:** prompts are designed to refuse requests that would harm users (e.g. helping an abuser locate a domestic-violence target).
- **Deployment controls:** production deployments require sign-off on non-negotiable compliance; no one-click path from "internal tool" to "weapons customer."

### Implementation traceability

See: `10-security-architecture.md#abuse-prevention`, `basira/02-rag-pipeline.md#refusal-policy`.

---

## Principle 4 — Knowledge as Trust

### Principle text (summary)
Default to open for standards, tooling, and research. Proprietary only for production business logic. Fortress for user data.

### Architectural implications

- **Repository layout** enforces the layer distinction:
  - Public repos under this GitHub org: Layer 1 standards, Layer 2 tooling.
  - Private (or subtree-restricted) repos: Layer 3 production systems.
  - Production databases with user data: behind multiple layers of access control, never dumped.
- **Build artifacts** (models, indices, released datasets) are published in Layer 1 and 2 forms when they would strengthen the ecosystem.
- **Data releases** exclude anything user-identifiable by construction, not by anonymization (which fails in practice).
- **Forkability is designed in.** Anyone should be able to clone our open components and build on them without needing us.

### Implementation traceability

See: `../docs/03-open-vs-closed.md`, `02-tech-stack-decisions.md#licensing`.

---

## Principle 5 — Sincerity of Intent

### Principle text (summary)
Work serves the Ummah and humanity; individual livelihood is legitimate, individual aggrandizement is not the goal.

### Architectural implications

Not a primarily technical principle, but has one technical expression:

- **No "founder-worship" features** — no first-class credit given to any single contributor in product UI beyond standard open-source attribution.
- **Credit is distributed** in commits, changelogs, and public acknowledgments across all contributors. The system does not amplify one name at the expense of others.

---

## Principle 6 — Excellence

### Principle text (summary)
Build as if Allah is watching — because He is. Poor quality is not acceptable.

### Architectural implications

- **Testing strategy:**
  - Unit tests for all business logic.
  - Integration tests for external boundaries.
  - **Evaluation harness** for scholarly content correctness, run on every change.
  - Adversarial tests for prompt injection, bias, and scholarly accuracy.
- **Code review required** before merge to main.
- **Security review** for anything touching authentication, user data, or cryptography.
- **Accessibility baseline** (WCAG AA) for all user-facing interfaces.
- **Performance budgets** set and enforced — no ship-first-optimize-later drift.
- **Documentation** alongside code, such that a new contributor 6 months later can understand it.

### Implementation traceability

See: `02-tech-stack-decisions.md#quality-gates`, `basira/02-rag-pipeline.md#evaluation`.

---

## Principle 7 — Long-Term Horizon

### Principle text (summary)
Multi-generational project; no sacrificing principles for short-term wins.

### Architectural implications

- **Avoid lock-in.** We choose portable technologies over proprietary managed services where possible.
  - Postgres, not DynamoDB.
  - Standard Docker/OCI containers, not vendor-specific runtimes.
  - S3-compatible object storage, not vendor-specific blob stores.
  - Open model weights over API-only models, as option exists.
- **Durable data formats.** Corpora stored in formats that will remain readable decades from now — JSON, Parquet, plain text with well-known schemas.
- **Versioned schemas.** Breaking changes to data formats are explicit and migration-supported.
- **Documented dependencies.** Every dependency has a reason; casual adoption is avoided.
- **Sustainable complexity.** Prefer simple, boring technology over novel stacks that may not exist in 10 years.

---

## Principle 8 — Fork Rights

### Principle text (summary)
Anyone may fork the work and continue under the same principles.

### Architectural implications

- **Permissive licensing** of code (MIT default) makes forking legally unambiguous.
- **Self-contained components.** A fork should be able to spin up the system without our infrastructure. No hidden cloud dependencies that only we can operate.
- **Published deployment guides** so a fork can be stood up by a competent team in a reasonable time.
- **No "phone-home" behaviors** that depend on our servers for operation.
- **Data portability** — users and institutions can export their configurations and (for public content) their datasets.

---

## Non-Negotiables — Architectural Enforcement

### NN-1 (no harmful tech) & NN-2 (no harmful customers)

- **Feature-gating by deployment:** Any feature that could plausibly be misused (e.g. bulk export, detailed user analytics) is gated behind deployment-time configuration and not present in open-source distributions.
- **Sales/support process control:** Customer onboarding includes a compliance step; this is procedural as much as technical.

### NN-3 (no sectarian exclusion)

- Authentication does not collect sectarian identity as a required field. Optional self-identification is permitted for features that require it (e.g. madhhab-default for a user's saved preferences).

### NN-4 (no Layer 4 opening)

- **Access to production user data requires:**
  - Named human operator identity (no service accounts with broad access).
  - Logged access with reason.
  - Minimum time-bounded access (JIT access patterns).
  - Audit published in transparency reports (aggregated).
- **Zero-knowledge architectures** for user segments with severe threat models. This is built into the product, not added later.
- **Client-side encryption** for the most sensitive product categories (e.g. certain Shahid submissions, certain Hisn use cases).

### NN-5 (no private material decisions)

- **ADRs required** for material decisions; these live in the repo.
- **Incident records** published after coordinated-disclosure window.
- **Governance outcomes** written up publicly.

### NN-6 (no capture)

- **Multi-maintainer repository control** from day one.
- **Published funding disclosures.**
- **Technical checks on merge permissions:** no single-person silent commit to `main` on consequential paths.

### NN-7 (no suppression of dissent)

- **Public issue trackers** — no private bug trackers for material engineering decisions.
- **Dissent documented** in ADRs and issues.

### NN-8 (no betrayal of the vulnerable)

- **Threat-model-aware defaults.** Products serving activists, journalists, and dissidents default to the most protective configuration.
- **No dark patterns** in UX that reduce safety for convenience.
- **Explicit "high-threat mode"** options that maximize user protection.

### NN-9 (no falsehood disguised as scholarship)

- **Citation requirement in LLM output:** prompts enforce that every factual claim traces to a specific source. Hallucination is a quality bug, treated as critical.
- **Scholar verification workflow** as a hard gate for content that reaches production.
- **Evaluation suite** includes a scholarly-accuracy benchmark, run on every model change.

### NN-10 (no principle abandonment under pressure)

- **Configuration of non-negotiable checks** cannot be disabled by single-person action. Changes to these checks require governance review.

---

## Summary Table

| Principle / NN | Key Architectural Expression |
|---|---|
| P1 Non-sectarian | Multi-tradition schema, equal retrieval default |
| P2 Transparency | Public code Layer 1-2, ADRs, transparency reports |
| P3 Human dignity | No tracking features, refusal behavior in LLM |
| P4 Knowledge as trust | Repo layering by disclosure tier, open formats |
| P5 Sincerity | Distributed credit in product UI |
| P6 Excellence | Test + eval gates, code review, accessibility baseline |
| P7 Long horizon | Avoid lock-in, open formats, portable stack |
| P8 Fork rights | MIT code, self-contained deployments, no phone-home |
| NN-1/2 | Feature gating, compliance checks in sales |
| NN-3 | Optional not required sectarian metadata |
| NN-4 | Access controls, zero-knowledge, client-side encryption |
| NN-5 | ADRs, public incident reports |
| NN-6 | Multi-maintainer control, funding disclosures |
| NN-7 | Public trackers, ADR dissent recording |
| NN-8 | Protective defaults, high-threat mode |
| NN-9 | Citation enforcement, scholarly evaluation gates |
| NN-10 | Governance-level controls on NN-enforcement |

---

## Closing

Principles without architectural expression become decorative. Architecture without principled grounding becomes arbitrary.

This document exists so neither happens.

When architectural decisions feel disconnected from the mission, trace the principle. If you can't, raise it.
