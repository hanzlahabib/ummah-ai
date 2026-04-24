# System Overview

## The Initiative as a Whole — Projects, Shared Infrastructure, and Boundaries

This document provides the big-picture view. Each project will have its own architecture documents for depth; this one shows how they fit together.

---

## The Four Projects and What They Share

```
                     ┌────────────────────────────────────────┐
                     │         THE UMMAH AI INITIATIVE        │
                     │    (legal entity + governance layer)   │
                     └────────────────────────────────────────┘
                                       │
         ┌─────────────────┬───────────┴──────────┬─────────────────┐
         ▼                 ▼                      ▼                 ▼
   ┌──────────┐     ┌──────────┐          ┌──────────┐       ┌──────────┐
   │ BASIRA   │     │ SHAHID   │          │  HISN    │       │  MIZAN   │
   │ research │     │ archive  │          │ defense  │       │ audit    │
   │ copilot  │     │ witness  │          │ toolkit  │       │ platform │
   └─────┬────┘     └─────┬────┘          └─────┬────┘       └─────┬────┘
         │                │                     │                  │
         └────────────────┴──────────┬──────────┴──────────────────┘
                                     │
                                     ▼
                   ┌─────────────────────────────────┐
                   │    SHARED INFRASTRUCTURE        │
                   │                                 │
                   │  • Arabic/Urdu NLP libraries    │ ← Layer 1/2 (open)
                   │  • Evaluation frameworks        │
                   │  • Content provenance (C2PA)    │
                   │  • Islamic corpora (public)     │
                   │  • Identity/auth                │ ← Layer 3 (proprietary)
                   │  • Secrets management           │
                   │  • Observability                │
                   │  • Sovereign hosting backbone   │
                   └─────────────────────────────────┘
```

### What each project owns

- **Basira** owns: Islamic content corpora (curated), scholar verification workflow, RAG infrastructure for scholarly Q&A, user-facing web and mobile apps, B2B integrations.
- **Shahid** owns: submission intake, cryptographic provenance chain, archival storage (redundant, multi-jurisdiction), search and research interfaces.
- **Hisn** owns: educational content, defensive tooling (deepfake detection, OPSEC guides, secure-comms primers), crisis coordination protocols.
- **Mizan** owns: audit methodology, reproducible measurement scripts, periodic reports dashboard, platform engagement workflow.

### What they share

- **NLP and retrieval infrastructure** (tokenizers, embedders, reranking pipelines) — especially Arabic-specific tooling.
- **Evaluation frameworks** — how do we measure correctness, bias, safety.
- **Identity and auth** (where projects expose authenticated user interfaces).
- **Observability** — logging, metrics, alerting — with careful separation for Layer 4 data.
- **Sovereign hosting backbone** — the core infrastructure on which all projects run.
- **Publication and community tooling** — repos, issue trackers, documentation sites.

---

## High-Level Data Flow (Basira, as illustrative example)

```
┌──────────┐      ┌────────────────┐      ┌───────────────┐
│  User    │─────▶│  Edge / CDN    │─────▶│  Next.js App  │
│ (web/app)│      │  (Cloudflare?) │      │  (Vercel /    │
└──────────┘      └────────────────┘      │   Hetzner)    │
                                          └───────┬───────┘
                                                  │
                                                  ▼
                                          ┌──────────────┐
                                          │  API Layer   │
                                          │  (Node.js /  │
                                          │   tRPC)      │
                                          └───┬──────┬───┘
                                              │      │
                         ┌────────────────────┘      └────────────────────┐
                         ▼                                                ▼
                ┌────────────────┐                              ┌──────────────────┐
                │  RAG Service   │                              │  User Data       │
                │  (Python /     │                              │  (Postgres       │
                │   FastAPI)     │                              │   on Hetzner,    │
                │                │                              │   EU jurisdiction)│
                │  • Retrieval   │                              │                  │
                │  • Rerank      │                              │   Layer 4 —      │
                │  • Generate    │                              │   encrypted,     │
                │  • Cite        │                              │   access-logged  │
                └───┬────────┬───┘                              └──────────────────┘
                    │        │
                    ▼        ▼
          ┌────────────┐ ┌────────────┐
          │   Vector   │ │    LLM     │
          │  DB        │ │  (API:     │
          │ (Qdrant /  │ │   Claude / │
          │  pgvector) │ │   Groq;    │
          │            │ │   later:   │
          │ Corpora =  │ │   self-    │
          │ Layer 2    │ │   hosted)  │
          │ public     │ └────────────┘
          └────────────┘
```

### Data-classification color coding

- **Public corpora** (Quran, classical hadith, tafseer) → Layer 2, public.
- **Curated/verified content** (our scholar-reviewed annotations) → Layer 3, proprietary.
- **User queries, accounts, subscriptions** → Layer 4, fortress.
- **Third-party API calls** (LLM providers) → handled with data-minimization; no long-term retention by providers where contractually achievable.

---

## Cross-Project Policies

### Authentication

- Shared identity layer across projects where authentication is needed.
- User choice of identity:
  - Anonymous (no account).
  - Pseudonymous (email + passkey, no real-name requirement).
  - Institutional (tied to verified institution membership).
- For Shahid/Hisn high-risk users, additional "high-threat mode" accounts with reduced data retention and stronger defaults.

### Payment

- Subscription billing via a single shared provider (Stripe initially; may diversify).
- No card data touches our servers.
- Billing data is Layer 4 — protected accordingly.

### Observability

- Application metrics and logs collected for operational necessity.
- User-identifying fields scrubbed before retention.
- Aggregate metrics published in transparency reports.
- No third-party tracking, advertising SDKs, or analytics that send user behavior to external parties.

### Rate limiting and abuse prevention

- Per-account and per-IP rate limits, configured to deter abuse without building profiles.
- Bot and automation detection using off-the-shelf tooling (Cloudflare Bot Management or equivalent).
- Suspicious-activity reviews are privacy-preserving (looking at behavior patterns, not content).

---

## Jurisdiction and Hosting Strategy

### Three hosting tiers

1. **Edge / stateless** — Content delivery, static assets, stateless UI pages.
   - Provider: Cloudflare (CDN) + Vercel (Next.js edge rendering).
   - Jurisdiction implications: these hold no Layer 4 data; US CLOUD Act exposure is acceptable because there is no sensitive data to compel.

2. **Application / cacheable** — API routes, cache, ephemeral computation.
   - Provider: Hetzner (Germany / Finland) primary; possibly Scaleway (France) secondary.
   - Jurisdiction: EU. Strong GDPR protections. Favorable legal environment.

3. **Sensitive data / Layer 4** — User databases, activist data, source-protected content.
   - Provider: Hetzner EU.
   - For highest-threat content (certain Shahid/Hisn data): zero-knowledge encrypted, so provider jurisdiction is less critical — but still chosen carefully.

### Legal entity jurisdiction

[TENTATIVE] — to be decided in Phase 0 completion. Candidates:

- **UAE (DIFC)** — Gulf stability, good legal infrastructure for the mission, but local political considerations.
- **Malaysia (Labuan or mainland)** — Muslim-majority, good legal framework, emerging Islamic fintech hub.
- **Estonia** — e-residency, EU, strong digital-rights legal tradition, but distant from Muslim-world mission center of gravity.
- **Switzerland (non-EU)** — strong privacy, neutral, expensive.
- **UK** — common law, reasonable framework, but UK political climate around some mission-relevant topics is unstable.

Decision is a founding-phase governance item. Legal counsel engaged when resources allow.

---

## Deployment Topology (MVP)

```
┌──────────────────────────────────────────────────────────────┐
│                    INTERNET                                   │
└───────────────────────────┬──────────────────────────────────┘
                            │
                            ▼
              ┌─────────────────────────────┐
              │   Cloudflare (CDN/WAF)      │
              │   - SSL termination         │
              │   - Bot filtering           │
              └───────────────┬─────────────┘
                              │
                              ▼
              ┌─────────────────────────────┐
              │   Vercel (Next.js frontend) │
              │   - Static + SSR            │
              │   - No user data            │
              └───────────────┬─────────────┘
                              │
                              ▼ (API calls)
              ┌─────────────────────────────┐
              │   Hetzner (Germany)         │
              │   ┌─────────────────────┐   │
              │   │  API server         │   │
              │   │  (Node.js / Bun)    │   │
              │   └─────────┬───────────┘   │
              │             │               │
              │   ┌─────────▼───────────┐   │
              │   │  Postgres + pgvector│   │
              │   │  (user data,        │   │
              │   │   corpora indices)  │   │
              │   └─────────────────────┘   │
              │   ┌─────────────────────┐   │
              │   │  Object storage     │   │
              │   │  (public artifacts) │   │
              │   └─────────────────────┘   │
              └─────────────────────────────┘
                              │
                              ▼ (LLM API calls)
              ┌─────────────────────────────┐
              │   Claude / Groq / Together  │
              │   (external LLM providers;  │
              │    zero-retention config    │
              │    where available)         │
              └─────────────────────────────┘
```

**Later stages (Phase 2+):**
- Self-hosted models for classical Arabic work (reducing external API dependence).
- Multi-region hosting for resilience.
- Dedicated Shahid archive infrastructure (geographically redundant).

---

## What's Not Here (Yet)

- Specific load targets and scaling plans — need production data.
- Detailed disaster-recovery procedures — will be written alongside first production deployment.
- CI/CD pipeline specifics — part of the implementation phase.
- Specific monitoring dashboards and alert runbooks — part of operations.
- Sovereign LLM hosting plan — strategic, multi-year, depends on external developments.

These are `[DEFERRED]` not `[OPEN]` — we know we need them; we'll write them when we have the grounded experience to write them well.

---

## Cross-References

- Tech-stack decisions: `02-tech-stack-decisions.md`
- Data architecture: `03-data-architecture.md`
- Security architecture: `10-security-architecture.md`
- Basira deep dive: `basira/01-overview.md`

---

This overview is intentionally high-level. The substance lives in the focused documents. Return here when you need to remember how the pieces fit.
