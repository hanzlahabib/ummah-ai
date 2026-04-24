# Tech Stack Decisions

## Architecture Decision Records (ADRs)

Every material technology choice in this document is recorded as an ADR with: **Context**, **Options Considered**, **Decision**, **Consequences**, and **Status**.

Status meanings: `[DECIDED]`, `[TENTATIVE]`, `[OPEN]`, `[DEFERRED]`, `[SUPERSEDED-BY-#N]`.

---

## ADR-001: Primary Application Language (TypeScript)

**Status:** [DECIDED]
**Date:** 2026-04

### Context

The founding contributors are strongest in TypeScript/Node.js. Solo / small team must optimize for shipped code, not theoretical best fit.

### Options Considered

1. **TypeScript + Node.js** — founder's strength; huge ecosystem; excellent Next.js integration.
2. **Python** — best ML ecosystem; weaker web backend than Node.
3. **Go** — excellent performance, but smaller AI ecosystem and slower iteration for solo founder.
4. **Rust** — strong safety/perf, long ramp-up cost for team; over-engineered for MVP.

### Decision

**TypeScript** as the primary application language. Server-side runs on **Node.js 24 LTS** or **Bun** (see ADR-002). Client-side React + Next.js App Router.

**Python microservices are permitted** only where genuinely necessary: embedding models, classical Arabic NLP, offline corpus processing. These are isolated services, not the main application.

### Consequences

- Solo founder ships faster.
- Wide contributor pool.
- Some ML tasks require crossing a language boundary — acceptable cost.
- If we later need heavy Python data pipelines, they live as separate services.

---

## ADR-002: Runtime (Node.js 24 LTS, Bun evaluation)

**Status:** [TENTATIVE]
**Date:** 2026-04

### Context

Server runtime choice affects performance, ecosystem compatibility, and operations.

### Options Considered

1. **Node.js 24 LTS** — default, universally compatible, boring-but-reliable.
2. **Bun** — faster startup, better DX, but younger ecosystem.
3. **Deno** — secure by default, but smaller adoption.

### Decision

**Node.js 24 LTS** for production services at least through MVP. Bun evaluated for dev tooling and possibly specific hot paths later.

### Consequences

- Well-trodden path; most tooling "just works."
- No surprise compatibility issues with established packages.
- Revisit after MVP if Bun ecosystem has matured.

---

## ADR-003: Frontend Framework (Next.js 16 App Router)

**Status:** [DECIDED]
**Date:** 2026-04

### Context

We need a framework for user-facing web apps (Basira primarily, with consumer + institutional UIs).

### Options Considered

1. **Next.js 16 App Router** — React Server Components, strong SEO, mature ecosystem, Vercel support but not locked to Vercel.
2. **SvelteKit** — smaller bundle, less boilerplate, but smaller ecosystem.
3. **Remix** — great DX, but smaller adoption curve than Next.
4. **Plain React SPA** — loses SEO, loses SSR.

### Decision

**Next.js 16 App Router**.

- Deployed on Vercel for MVP (stateless frontend — no Layer 4 data there).
- Must remain self-hostable (Docker + Node.js) for sovereignty.

### Consequences

- Strong SSR/SSG, good SEO, fast time-to-content.
- App Router patterns well-documented.
- Cache Components (Next 16) enable granular `use cache` control for scholarly content.
- Vercel deployment is convenient but not required — self-host path stays supported.

---

## ADR-004: Database (Postgres + pgvector)

**Status:** [DECIDED]
**Date:** 2026-04

### Context

We need a database that handles: relational data (users, subscriptions, institutions), full-text search (Arabic/Urdu), vector similarity (embeddings), JSON (scholar annotations, citations), and reliable backup.

### Options Considered

1. **Postgres + pgvector** — one system handling all needs, excellent tooling, self-hostable.
2. **Postgres + separate Qdrant/Weaviate** — specialized vector DB; extra operational complexity.
3. **MongoDB + Atlas Vector Search** — flexible schema but less operational confidence.
4. **DynamoDB + Pinecone** — managed but vendor-locked and not portable.

### Decision

**Postgres 16+** as the primary database with **pgvector** extension for embeddings. Managed via **Drizzle ORM** (founder familiarity; type-safe; portable).

For very-large vector workloads later, **Qdrant** is the designated escape hatch. Not needed for MVP.

### Consequences

- Single system to operate; single backup strategy.
- Type-safe schema migrations via Drizzle.
- Self-hostable on Hetzner; no vendor lock-in.
- Performance is adequate for MVP scale (millions of chunks); revisit if we exceed this.

---

## ADR-005: LLM Provider Strategy

**Status:** [DECIDED] (MVP); [OPEN] for post-MVP sovereignty plan

### Context

Generating scholarly answers requires a high-quality LLM. Options: API providers (hosted) vs. self-hosted open weights.

### Options Considered

1. **API-first** (Anthropic Claude as primary, Groq/Together as fallback) — fast MVP; no infra burden; quality; zero-retention contracts available.
2. **Self-hosted open weights** (Llama 3.3 70B, Qwen 2.5, Jais, Fanar) — full sovereignty; high infra cost; narrower quality gap every month.
3. **Hybrid** — API for general reasoning, self-hosted for Arabic-specific tasks.

### Decision

**MVP:** API-first with **Anthropic Claude Sonnet 4.6** or **Claude Haiku 4.5** as primary reasoning model. **Groq** (Llama 3.3 / Qwen) as secondary for cost-sensitive workloads and fallback.

**Provider abstraction layer** — a thin internal interface — prevents lock-in and enables provider swap or addition without rewriting call sites.

**Zero-retention** contracts required with any LLM provider handling production user queries.

**Post-MVP (Phase 2+):** Evaluate self-hosted deployment of an open classical-Arabic-specialized model (Jais, Fanar, or a fine-tune we produce). Goal: route sensitive / sovereignty-critical requests to self-hosted; keep API for general capability.

### Consequences

- Fastest MVP path.
- Dependency on external providers — mitigated by abstraction and multi-provider strategy.
- Post-MVP sovereignty roadmap requires infra investment; budgeted in strategy Phase 3.

---

## ADR-006: Embeddings (Hybrid: OpenAI for launch, BGE-M3 roadmap)

**Status:** [DECIDED] (MVP); [TENTATIVE] on migration timeline

### Context

Arabic, especially classical Arabic, is poorly served by standard English-centric embedders. We need semantic search that works on Quran/Hadith/tafseer in Arabic and on user queries that arrive in Urdu/English/Arabic mix.

### Options Considered

1. **OpenAI `text-embedding-3-large`** — strong multilingual; easy; but third-party dependency.
2. **BGE-M3** — open-source, multilingual, excellent Arabic performance, self-hostable.
3. **Cohere embed-multilingual-v3** — good multilingual; vendor dependency.
4. **AraBERT / CAMeLBERT derivatives** — Arabic-specialized but weaker cross-lingual.

### Decision

**MVP:** Start with **OpenAI `text-embedding-3-large`** for speed of launch. Benchmark against BGE-M3 during Phase 1.

**Phase 2 target:** Migrate to **self-hosted BGE-M3** (or its then-best open successor). Embeddings for public Islamic corpora become a Layer 2 open release — freely distributable to benefit other Muslim AI developers.

### Consequences

- MVP launches quickly.
- Open roadmap exists for sovereignty-by-embedding.
- Re-embedding cost when migrating is manageable at MVP scale; grows expensive at scale → migrate before scale.

---

## ADR-007: Hosting (Tiered: Vercel edge, Hetzner EU core)

**Status:** [DECIDED]
**Date:** 2026-04

### Context

Hosting jurisdiction affects legal exposure (CLOUD Act, subpoenas), performance, and cost. Layer 4 data must not sit in jurisdictions where single-government compulsion is routine.

### Options Considered

Already analyzed in `01-system-overview.md` under Jurisdiction and Hosting Strategy.

### Decision

- **Edge / stateless** (UI, CDN, static assets): **Cloudflare + Vercel**. Contains no Layer 4 data.
- **Application core + user data (Layer 4)**: **Hetzner (Germany / Finland)** — EU jurisdiction, strong GDPR, favorable legal environment, excellent price-performance.
- **Object storage for public artifacts**: **Cloudflare R2** (S3-compatible) or **Hetzner object storage**.
- **Object storage for sensitive archives (Shahid)**: **Self-hosted on Hetzner** with geographic redundancy, client-side encryption where warranted.

### Consequences

- Commercial-grade hosting without US hyperscaler lock-in.
- EU legal environment generally protective of user data.
- Scale economics require managed Postgres eventually — Hetzner cloud + manual ops for MVP, evaluate managed options (e.g. Neon self-hosted, CrunchyData) later.

---

## ADR-008: Auth (Better Auth, self-hostable)

**Status:** [DECIDED]
**Date:** 2026-04

### Context

Authentication and session management must be secure, auditable, and not lock us into a vendor who could be compelled or fail.

### Options Considered

1. **Better Auth** — modern, self-hostable, TypeScript-first, no vendor lock-in.
2. **Clerk** — excellent DX, but third-party session storage; not sovereignty-aligned for Layer 4 concerns.
3. **Auth0 / Okta** — enterprise-heavy; vendor lock-in.
4. **Lucia / custom** — full control, higher implementation burden.

### Decision

**Better Auth** for the auth layer.

Key configuration:
- **Passkeys** (WebAuthn) as primary factor where possible; TOTP fallback; email-magic-link fallback.
- **Session data** stored in our own Postgres — never with a third party.
- **No social login** by default (privacy-preserving). Optional for users who prefer it, clearly labeled.
- **Account types:** Anonymous (no account), Pseudonymous (email + passkey), Institutional (tied to verified institution).

### Consequences

- Full control over session storage; no third-party retention.
- Passkey-first is stronger security than passwords.
- Some users unfamiliar with passkeys — graceful fallback exists.

---

## ADR-009: Payments (Stripe for MVP)

**Status:** [DECIDED]
**Date:** 2026-04

### Context

Commercial product requires subscription billing. Compliance and PCI scope matter.

### Options Considered

1. **Stripe** — market standard; good global coverage; transparent fees.
2. **Lemon Squeezy / Paddle** — merchant-of-record; handles tax globally; good for solo founders.
3. **Direct ACH / bank integration** — lower fees but regional and high compliance burden.

### Decision

**Stripe** for MVP. **Lemon Squeezy or Paddle evaluated** for merchant-of-record convenience if tax compliance becomes burdensome.

For Muslim-world customers, add **regional payment options** (Mada in KSA, various providers in Malaysia/Indonesia) as growth demands. This is a Phase 2 consideration.

### Consequences

- Card data never touches our servers.
- Billing data is Layer 4 — protected accordingly (encrypted, access-controlled).
- Stripe dependency acknowledged; abstraction layer keeps future swap feasible.

---

## ADR-010: Observability (Self-hosted minimum)

**Status:** [DECIDED]
**Date:** 2026-04

### Context

We need logs, metrics, and errors without sending user data to third-party telemetry providers.

### Options Considered

1. **Sentry Cloud** — excellent DX but sends error context to US-based service; problematic for Layer 4 exposure.
2. **Self-hosted Sentry** — full control; operational cost.
3. **Grafana + Loki + Prometheus + Tempo** — open-source, self-hostable, standard.
4. **Datadog / New Relic** — rich features, vendor lock-in, cost.

### Decision

**Self-hosted Grafana stack** (Grafana, Loki for logs, Prometheus/Mimir for metrics, Tempo for traces, Alertmanager for alerts) on Hetzner.

For error tracking: **Sentry self-hosted** or **GlitchTip** (lighter-weight Sentry-compatible alternative).

**Scrubbing pipeline** on log ingest removes user-identifying fields before retention.

### Consequences

- Operational burden of running the stack — acceptable at MVP scale.
- No user-data leakage to third-party telemetry.
- Transparency reports can be generated directly from our own aggregated logs.

---

## ADR-011: Secrets Management (Infisical)

**Status:** [TENTATIVE]
**Date:** 2026-04

### Context

Secrets (API keys, database passwords, signing keys) must be stored securely, rotatable, and auditable. Never in plain `.env` checked into git.

### Options Considered

1. **Infisical** — open-source, self-hostable, good DX.
2. **HashiCorp Vault** — industry standard but heavy for small team.
3. **Doppler** — nice SaaS; third-party dependency.
4. **AWS Secrets Manager** — US provider, lock-in.
5. **`.env` with good hygiene** — fragile at scale.

### Decision

**Infisical** (self-hosted on Hetzner for MVP). Evaluate HashiCorp Vault if scale demands.

Strict rule: **no plain-text secrets in git.** Pre-commit hook (`gitleaks` or equivalent) blocks accidental commits.

### Consequences

- Secrets centralized, auditable, rotatable.
- One more service to operate.
- Founding contributors must set up Infisical before writing code that needs secrets.

---

## ADR-012: Licensing Defaults

**Status:** [DECIDED]
**Date:** 2026-04

### Context

Per `../docs/03-open-vs-closed.md`, different layers need different licenses.

### Decision

- **Layer 1/2 code (public libraries, tooling, frameworks)**: **MIT** default. **Apache 2.0** where patent grants matter. **AGPL-3.0** for defensive tools where proprietary appropriation would undermine the mission.
- **Layer 3 code (production systems)**: Proprietary by default; no explicit license file in that repo.
- **Documentation**: **CC BY-SA 4.0** (as in `../LICENSE`).
- **Public datasets**: **CC BY-SA 4.0** or **CDLA-Sharing-1.0**, per dataset.

Each public repo must have an explicit `LICENSE` file. Each source file may carry `SPDX-License-Identifier`.

### Consequences

- Clear expectations for contributors and forks.
- MIT default maximizes ecosystem uptake.
- AGPL available for defensive tools that merit it (e.g. specific Hisn utilities).

---

## ADR-013: CI/CD (GitHub Actions for now)

**Status:** [TENTATIVE]
**Date:** 2026-04

### Context

Automated build, test, and deploy pipelines needed as soon as we ship code.

### Options Considered

1. **GitHub Actions** — where our code lives; free tier generous.
2. **Self-hosted Gitea + Drone** — full sovereignty; operational cost.
3. **Hosted alternatives** (CircleCI, Buster) — third-party cost + dependency.

### Decision

**GitHub Actions** for MVP. Self-hosted runners on Hetzner for builds that should not run on GitHub-controlled infra (e.g. production deploys that need secret access).

**Move plan:** If GitHub becomes hostile or compromised, we migrate to self-hosted Gitea + Woodpecker or similar. Keep CI workflows written in a portable style (not Actions-specific hacks) to reduce migration cost.

### Consequences

- Fast path to CI.
- GitHub dependency acknowledged.
- Escape hatch documented.

---

## ADR-014: Monorepo vs Multi-repo

**Status:** [DECIDED]
**Date:** 2026-04

### Context

Four projects + shared infrastructure — how to organize repositories?

### Options Considered

1. **Single monorepo** (`ummah-ai/ummah-ai`) — all docs, code, tooling.
2. **Foundation + per-project repos** — docs monorepo + separate code repos.
3. **Full multi-repo** — one repo per concern.

### Decision

**Hybrid:**

- **`ummah-ai/ummah-ai`** (this repo) — foundation documents and architecture. Public.
- **Per-project code repos** (to be created when code starts):
  - `ummah-ai/basira` (mostly public Layer 1/2 tooling + proprietary subtrees; see layer doc)
  - `ummah-ai/shahid`
  - `ummah-ai/hisn`
  - `ummah-ai/mizan`
- **Shared libraries repos** for Layer 2 tooling:
  - `ummah-ai/arabic-nlp`
  - `ummah-ai/islamic-corpora-schema`
  - `ummah-ai/evals`
  - etc.

Each project repo references the foundation repo for principles.

### Consequences

- Clean separation by concern.
- Public components have their own star count and issue trackers (helps adoption).
- Proprietary subtrees live within project repos, protected by access control.

---

## ADR-015: Package Manager (pnpm)

**Status:** [DECIDED]
**Date:** 2026-04

### Context

JavaScript package manager choice affects speed, disk use, and dependency graph integrity.

### Options Considered

1. **pnpm** — fast; disk-efficient; strict hoisting; founder's existing preference.
2. **npm** — default; slow; loose hoisting.
3. **Yarn (classic or berry)** — fine, but pnpm generally superior.
4. **Bun** — fastest; still young.

### Decision

**pnpm** for all projects.

### Consequences

- Consistent across the initiative.
- Strict hoisting catches phantom dependency issues early.

---

## ADR-016: Data Residency for Production User Data

**Status:** [DECIDED]
**Date:** 2026-04

### Context

Where user data physically sits determines which governments can compel its production.

### Decision

**Layer 4 data** (user accounts, queries, sensitive content) physically stored in **EU jurisdictions** only — Germany (Hetzner Falkenstein / Nuremberg) primary, Finland (Hetzner Helsinki) secondary.

Data does not transit through US-controlled infrastructure for storage (transit for delivery is acceptable if encrypted end-to-end).

### Consequences

- GDPR protections apply.
- US CLOUD Act does not reach this data through any of our providers.
- Performance implications for non-EU users acceptable at MVP scale; evaluate CDN or replicated read-only caches later.

---

## ADR-017: Quality Gates for Merge

**Status:** [DECIDED]
**Date:** 2026-04

### Context

Per Principle 6 (Excellence), code and content must meet a bar before reaching production.

### Decision

PR to `main` requires:

1. **Automated checks green:** tests pass, type checks pass, lint clean, no secret leakage detected.
2. **Code review by at least one maintainer** who did not author the PR.
3. **For LLM/content changes:** **evaluation suite passes** on the scholarly accuracy benchmark.
4. **For security-sensitive changes:** additional review by a designated security reviewer.
5. **No `[WIP]` or draft merges** — clean history.

`main` is protected; direct pushes disabled except via documented emergency-fix procedure requiring post-hoc review.

### Consequences

- Slower merge than a solo-author setup; worth the protection.
- Review bandwidth constraint — must scale maintainer pool as contributions grow.

---

## Summary Table

| ADR | Topic | Decision |
|---|---|---|
| 001 | Primary language | TypeScript |
| 002 | Runtime | Node.js 24 LTS |
| 003 | Frontend framework | Next.js 16 App Router |
| 004 | Database | Postgres + pgvector + Drizzle |
| 005 | LLM provider | Claude API + Groq (MVP); self-hosted later |
| 006 | Embeddings | OpenAI → BGE-M3 migration |
| 007 | Hosting | Vercel edge + Hetzner EU core |
| 008 | Auth | Better Auth self-hosted |
| 009 | Payments | Stripe |
| 010 | Observability | Self-hosted Grafana stack |
| 011 | Secrets | Infisical self-hosted |
| 012 | Licensing | MIT/Apache/AGPL per layer; CC BY-SA docs |
| 013 | CI/CD | GitHub Actions (migration plan documented) |
| 014 | Repo layout | Hybrid: foundation monorepo + per-project repos |
| 015 | Package manager | pnpm |
| 016 | Data residency | EU (Germany/Finland) only for Layer 4 |
| 017 | Quality gates | Auto checks + review + evals for LLM changes |

---

## Open Decisions

- **ADR-*: Legal entity jurisdiction** — [OPEN] pending counsel.
- **ADR-*: Dedicated Shahid archive infrastructure** — [DEFERRED] until Phase 2.
- **ADR-*: Self-hosted sovereign LLM** — [DEFERRED] to Phase 3.
- **ADR-*: Mobile app stack** — [OPEN]. Likely React Native for first cut; evaluate native later.

When these are decided, ADRs will be added and this table updated.

---

## Meta: How to Add an ADR

1. Identify the decision is material enough to warrant an ADR (rule of thumb: would changing this be expensive later?).
2. Number the ADR (next in sequence).
3. Fill in Context, Options, Decision, Consequences, Status.
4. Open a PR.
5. Discussion happens in the PR.
6. Merge when consensus exists and maintainer approves.

If a decision later supersedes an earlier ADR, the earlier one is marked `[SUPERSEDED-BY-#N]` and remains in the record. Decisions are append-only; we keep history.
