# Mizan — Architectural Sketch

## مِيزَان — Scale of Justice

Independent audit platform documenting how major technology platforms treat Muslim content, Muslim users, and Muslim-relevant issues. Detailed architecture deferred to Phase 2+; this document captures the direction.

---

## Status

**Current phase**: design sketch only. Implementation overlaps with late Phase 2 / early Phase 3.

---

## Mission Recap

Per `../../docs/04-projects.md`:

Conduct and publish independent, reproducible audits of platform behavior — Meta, TikTok, X, YouTube, Google, and others — specifically on how they handle Muslim content, accounts, and issues. Use empirical findings to pressure platforms toward fairer behavior and to provide the evidentiary basis for policy advocacy and legal action.

---

## Core Components

### 1. Audit methodologies (published as Layer 1)

Reproducible, documented methods for measuring:

- **Content moderation disparities** — comparable content's treatment across Arabic vs. Hebrew, Urdu vs. Hindi, etc.
- **Account suspension patterns** — rates, causes, appeal outcomes for Muslim-associated accounts.
- **Hashtag suppression** — quantitative measurement of discovery-surface visibility.
- **Algorithmic amplification disparities** — how similar content performs across identity categories.
- **Ad-library analysis** — which political / ideological actors advertise, how, targeting whom.
- **Platform policy inconsistency** — documented application of rules differently to different content.

### 2. Measurement infrastructure

- **Scripts and tools** that execute audits at scale.
- **Dashboards** showing current state and trends.
- **Data pipelines** for periodic re-measurement.
- **Archival** of findings so historical patterns can be revisited.

### 3. Reports

- **Quarterly reports** per platform.
- **Annual cross-platform synthesis.**
- **Special investigations** for acute events (e.g. war coverage suppression, specific policy rollouts).
- **Press-ready formats** for journalist use.
- **Policy-ready formats** for regulators (EU DSA, UK Online Safety Act, etc.).

### 4. Engagement

- **Formal channels** with platforms (trust and safety teams, policy leads).
- **Regulator engagement** where jurisdictions have accountability mechanisms.
- **Academic collaboration** — methodologies usable by university researchers.
- **Media partnerships** for reach.

---

## High-Level Architecture Sketch

```
┌─────────────────────────────────────────────────────────┐
│  MEASUREMENT INFRASTRUCTURE                             │
│   • Platform API clients (where available)              │
│   • Scraping tooling (where APIs inadequate)            │
│   • Comparison test suites                              │
│   • Reproducible measurement scripts                    │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│  DATA WAREHOUSE                                         │
│   • Raw measurement data                                │
│   • Aggregated metrics                                  │
│   • Historical time series                              │
│   • Separation: public data vs. source-identified       │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│  ANALYSIS LAYER                                         │
│   • Statistical analysis                                │
│   • Anomaly detection                                   │
│   • Cross-platform comparison                           │
│   • Historical trend tracking                           │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│  PUBLICATION LAYER                                      │
│   • Public reports                                      │
│   • Interactive dashboards                              │
│   • Researcher API (for academic use)                   │
│   • Regulator data access (credentialed)                │
│   • Journalist portal                                   │
└─────────────────────────────────────────────────────────┘
```

---

## Design Principles Specific to Mizan

### Rigor over reach

- Every claim backed by data.
- Statistical methods documented.
- Confidence intervals published, not just point estimates.
- Peer-reviewable methodology — other researchers can reproduce.

### Neutrality

- Not "anti-platform." Anti-injustice.
- When platforms behave well, that's part of the report.
- Improvement over time documented alongside persistent issues.
- Platforms given opportunity to comment before major findings published (coordinated disclosure for research).

### Reproducibility

- Methods published.
- Scripts open-sourced where legally possible (some may require platform terms-of-service consideration).
- Data samples archived for independent verification.

### Source protection

- Whistleblower channels exist.
- Source identities protected (Layer 4).
- Internal platform leaks handled with journalistic standards.

---

## Data Model Sketch

### Audit event

```typescript
type AuditEvent = {
  id: string;
  platform: string;                // meta / tiktok / x / youtube / ...
  methodology_id: string;          // link to methodology
  test_type: string;               // comparative / observational / survey / ...
  ran_at: string;
  executed_by: string;             // contributor / partner
  raw_observations: any[];         // platform-specific
  metrics: Record<string, number | string>;
  samples: Sample[];
  confidence: number;              // 0..1
  flags: string[];
  reproducibility: {
    code_version: string;
    data_snapshot_hash: string;
  };
};
```

### Finding

A higher-level conclusion drawn from one or more audit events:

```typescript
type Finding = {
  id: string;
  title: string;
  platforms: string[];
  period: { start: string; end: string };
  summary: string;
  evidence: string[];              // links to audit events
  scale: "individual" | "systematic" | "intermittent";
  severity: "low" | "medium" | "high" | "critical";
  platform_response?: PlatformResponse;
  follow_up: FollowUpAction[];
  publication_status: "draft" | "coordinated-disclosure" | "published";
};
```

---

## Partnership Strategy

Like Hisn, Mizan works heavily through partnerships:

- **AlgorithmWatch** (Europe) — established methodology; coordinate where overlap.
- **7amleh** — Palestinian-specific platform issues.
- **Center for Countering Digital Hate** — similar methodologies.
- **Academic partners** — statistical rigor, methodology peer review.
- **Investigative journalism outlets** — distribution of findings.
- **Civil society coalitions** — joint action where findings warrant.

---

## Tech Stack (Tentative)

- **Python** for data analysis and statistical work (appropriate ecosystem).
- **Jupyter / notebooks** for reproducible analysis.
- **Postgres** for metric storage.
- **Dashboarding** — Grafana or Metabase.
- **Web publication** — static site (Astro / Next.js).
- **API** for programmatic access — read-only, versioned.

Different from Basira's stack because data/analysis work has different ecosystem fit than user-facing AI product.

---

## Legal Considerations

Platform auditing sits in legally contested space:

- **Terms of service** — some measurement methods conflict with platform TOS. Legal counsel guides what is and isn't acceptable.
- **Computer fraud laws** (US CFAA, UK equivalents) — research-safe harbor varies by jurisdiction. Hosting and operation choice matters.
- **Data protection** — even audit data can have personal data in it. Protect accordingly.

Mizan therefore requires legal counsel engaged throughout, in addition to general initiative legal support.

---

## Relationship to Other Projects

- **Shahid** provides documented incidents; Mizan measures the systems that produced them.
- **Hisn** uses Mizan's findings to advise users on platform choice ("platform X currently suppresses content like yours; consider platform Y").
- **Basira**'s stance on specific platforms as sources may be informed by Mizan findings.

---

## Guiding Principles Specific to Mizan

- **Data wins.** Intuition is a hypothesis; data is evidence.
- **Replicable or it doesn't count.** A finding no one else can reproduce is suggestion, not proof.
- **Platforms are not monolithic.** Findings granular enough to credit improvement.
- **Regulators and platforms are both audiences.** Format findings for both.
- **Users are the ultimate audience.** Findings should translate to better decisions for the people affected.

---

## Cross-References

- Project description: `../../docs/04-projects.md#project-4--mizan`
- Disclosure framework: `../../docs/03-open-vs-closed.md#mizan-algorithmic-accountability-auditor`

---

Full architecture documented when Phase 2+ implementation begins.
