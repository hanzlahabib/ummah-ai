# Basira — Detailed Roadmap

> Audit-grade. Specific tasks. Effort estimates. Acceptance criteria. Risks.
> No fluff. Anyone can verify progress against this document.

---

## Status snapshot

| Item | Value |
|---|---|
| Phase | 0 (foundation done; v0.1 build not yet started) |
| Last update | 2026-04 |
| Repo | https://github.com/hanzlahabib/ummah-ai |
| Code commits to date | 0 |
| Active contributors | 1 (founder) |
| Paid staff | 0 |
| Verifying scholars | 0 (recruitment pending Council formation) |
| Council members | 0 |
| Funding secured | $0 |
| Funding committed (verbal) | $0 |
| Legal entity | Not yet formed |

---

## How to audit this roadmap

Anyone — contributor, scholar, journalist, regulator — can verify progress by checking:

1. **Repo state:** Each task in this roadmap maps to one or more PRs / commits. Track via labels.
2. **Issue tracker:** Each Epic has a corresponding GitHub issue with task checkboxes.
3. **Public benchmarks:** Quality claims are backed by published evaluation results in `evals/` directory (when code exists).
4. **Quarterly progress reports:** Published at end of each quarter, aligned with this roadmap.
5. **Transparency report:** Annual, includes spending vs. budget per Epic.

If a claim is made that contradicts what is in the repo, flag it as an issue.

---

## v0.1 Alpha — Build Scope (target: 6 calendar months from start)

**Goal:** Working invite-only Islamic research copilot covering Quran + 6 Sunni hadith + 4 Shia hadith collections, with citation-grounded chat in Arabic, Urdu, English. 50 alpha users.

**Total scope estimate:** 60-80 person-weeks. With 2-3 person team: 24-32 calendar weeks.

---

### Epic A1 — Project Setup & Infrastructure

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| A1.1 | Set up monorepo (pnpm workspaces) | 0.5pw | — | Repo runs `pnpm i && pnpm build` cleanly; CI passes |
| A1.2 | Configure CI (GitHub Actions): lint, type-check, test, secret scan | 0.5pw | A1.1 | All checks green on a sample PR |
| A1.3 | Provision Hetzner Cloud account, single VPS for development | 0.25pw | — | SSH access working; firewall configured |
| A1.4 | Set up Postgres 16 + pgvector extension on dev VPS | 0.5pw | A1.3 | DB accessible; pgvector functions usable |
| A1.5 | Set up Infisical (self-hosted) for secrets | 0.5pw | A1.3 | All env vars retrievable via SDK |
| A1.6 | Domain registration (`ummah-ai.org` or alternative) + DNS via Cloudflare | 0.25pw | — | Domain resolves; SSL via Let's Encrypt |
| A1.7 | Email infrastructure (transactional via Postmark / Resend) | 0.5pw | A1.6 | Verified sending; SPF/DKIM/DMARC pass |
| A1.8 | Production deploy pipeline (Docker + GitHub Actions → Hetzner) | 1pw | A1.1, A1.3 | Push-to-deploy works end-to-end |
| A1.9 | Basic observability (Grafana + Loki + Prometheus self-hosted) | 1pw | A1.3 | Dashboards visible; logs queryable |
| A1.10 | Backup automation (encrypted Postgres backups to second region) | 0.5pw | A1.4 | Daily backup verified; restore tested |
| **Total** | | **5.5pw** | | |

**Risks for A1:**
- Hetzner outage during setup → mitigation: pick stable VPS class; don't depend on novel features.
- pgvector version compatibility → mitigation: pin Postgres version; document upgrade path.

---

### Epic A2 — Core Corpus Ingestion: Quran

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| A2.1 | Acquire Tanzil Quran data (XML + JSON) under documented license | 0.5pw | — | Local copy with provenance recorded |
| A2.2 | Define Quran schema (per `../03-data-architecture.md`) | 0.5pw | A1.4 | Migration applied; types in code |
| A2.3 | Ingestion script: Tanzil XML → Postgres rows | 1pw | A2.1, A2.2 | 6,236 verses inserted; content-hash matches source |
| A2.4 | Translation ingestion: Yusuf Ali, Pickthall, Sahih International (English) | 0.5pw | A2.3 | 3 translations linked to verses |
| A2.5 | Translation ingestion: Maulana Maududi, Tafheem-ul-Quran (Urdu) — 2 editions | 0.5pw | A2.3 | 2 Urdu translations linked |
| A2.6 | Arabic normalization profile (hamza, tatweel, diacritics) | 1pw | — | Documented profile; test cases pass |
| A2.7 | Embedding generation (OpenAI text-embedding-3-large) | 0.5pw | A2.3, A2.6 | All 6,236 verses embedded; pgvector index built |
| A2.8 | Verification: hash check vs. Tanzil source on each ingestion | 0.25pw | A2.3 | Automated check in CI; mismatch fails build |
| **Total** | | **4.75pw** | | |

**Risks for A2:**
- Tanzil license changes → mitigation: archive current snapshot under `corpus/quran/source/`; document version.
- Embedding model deprecated → mitigation: ADR-006 defines migration path to BGE-M3.

---

### Epic A3 — Core Corpus Ingestion: Hadith Sunni (6 + Muwatta)

Collections: Bukhari, Muslim, Abu Dawud, Tirmidhi, Nasa'i, Ibn Majah, Muwatta Malik.

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| A3.1 | Survey legitimate sources (Sunnah.com API, openhadith.org, etc.); document licensing | 0.5pw | — | License audit complete; primary source chosen |
| A3.2 | Define hadith schema (book, chapter, hadith#, gradings, isnad) | 0.5pw | A1.4 | Migration applied |
| A3.3 | Ingestion: Sahih al-Bukhari (~7,500 hadiths) | 1pw | A3.1, A3.2 | All hadiths inserted; gradings preserved |
| A3.4 | Ingestion: Sahih Muslim (~7,500 hadiths) | 1pw | A3.1, A3.2 | Same |
| A3.5 | Ingestion: Sunan Abu Dawud (~5,000) | 0.75pw | | Same |
| A3.6 | Ingestion: Jami' at-Tirmidhi (~4,000) | 0.75pw | | Same |
| A3.7 | Ingestion: Sunan an-Nasa'i (~5,500) | 0.75pw | | Same |
| A3.8 | Ingestion: Sunan Ibn Majah (~4,400) | 0.75pw | | Same |
| A3.9 | Ingestion: Muwatta Malik (~1,800) | 0.5pw | | Same |
| A3.10 | English translations linked (where openly licensed) | 1pw | A3.3-A3.9 | At least 1 English translation per collection |
| A3.11 | Urdu translations linked where available | 1pw | | At least 3 collections have Urdu |
| A3.12 | Embedding generation for all hadiths | 1pw | A3.3-A3.9, A2.7 | ~36,000 hadiths embedded |
| A3.13 | Cross-reference graph (which hadith references which Quran verse) | 1.5pw | A2.3, A3.3-A3.9 | Bidirectional links queryable |
| **Total** | | **11.25pw** | | |

**Risks for A3:**
- Source data quality varies — manual spot-check required → mitigation: scholar review of random sample (Epic A6).
- Licensing changes mid-ingestion → mitigation: archive snapshots under provenance metadata.

---

### Epic A4 — Core Corpus Ingestion: Hadith Shia (4 + Nahj al-Balagha)

Collections: Al-Kafi, Man La Yahduruhu al-Faqih, Tahdhib al-Ahkam, Al-Istibsar, Nahj al-Balagha.

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| A4.1 | Source identification (al-islam.org, thaqalayn.net, hadith.al-shia.org) | 0.5pw | — | Sources documented; licensing verified |
| A4.2 | Schema mapping (Shia collections have different structure) | 0.5pw | A3.2 | Schema extends A3.2 cleanly |
| A4.3 | Ingestion: Al-Kafi (~16,000 narrations across 8 vols) | 1.5pw | A4.1, A4.2 | All narrations ingested |
| A4.4 | Ingestion: Man La Yahduruhu al-Faqih | 1pw | | Same |
| A4.5 | Ingestion: Tahdhib al-Ahkam | 1pw | | Same |
| A4.6 | Ingestion: Al-Istibsar | 0.75pw | | Same |
| A4.7 | Ingestion: Nahj al-Balagha | 0.5pw | | Same |
| A4.8 | English translations linked | 1pw | A4.3-A4.7 | At least 1 per major work |
| A4.9 | Urdu/Persian translations linked | 1pw | | Where available |
| A4.10 | Embedding generation | 1pw | A4.3-A4.7 | ~25,000 narrations embedded |
| **Total** | | **8.25pw** | | |

**Risks for A4:**
- Shia digital corpora are less complete than Sunni — gaps possible → mitigation: document gaps explicitly; scholar review identifies priorities.
- Some scholarly editions are publisher-restricted → mitigation: prefer public-domain editions; license modern editions if needed.

---

### Epic A5 — RAG Pipeline (Retrieval + Generation)

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| A5.1 | Hybrid retrieval (BM25 + dense vector) — Postgres `tsvector` + pgvector | 2pw | A2.7, A3.12, A4.10 | Returns top-50 candidates in <500ms p95 |
| A5.2 | Query understanding (language detect, intent, Arabic normalization) | 1pw | A2.6 | Routes correctly across 100-query test set |
| A5.3 | Cross-encoder reranking (BGE-reranker or equivalent) | 1pw | A5.1 | Improves top-10 relevance vs baseline by ≥15% on benchmark |
| A5.4 | Citation enforcement: every claim must reference retrieved chunk | 2pw | A5.3 | 0 hallucinated citations in 500-query benchmark |
| A5.5 | Multi-school answer template (when sources disagree across schools) | 1.5pw | A5.4 | Schools surfaced explicitly per `09-content-policy.md` |
| A5.6 | Refusal policy (no fatwas, no targeting, etc.) | 1pw | A5.4 | Refuses 100% of test set of disallowed prompts |
| A5.7 | Streaming response (SSE) | 0.5pw | A5.4 | First token <1s p95 |
| A5.8 | Provider abstraction (Claude primary, Groq fallback) | 1pw | — | Switches providers on failure within 2s |
| **Total** | | **10pw** | | |

**Risks for A5:**
- Cross-encoder for Arabic underperforms → mitigation: evaluate multiple models; budget time for fine-tuning if needed.
- LLM provider rate limits during peak → mitigation: provider abstraction; monitor and adjust.

---

### Epic A6 — Scholar Verification Workflow

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| A6.1 | Scholar onboarding portal (sign-in, role assignment) | 1pw | A1.7 | 5 scholars onboarded |
| A6.2 | Review queue UI: scholar sees content awaiting verification | 1.5pw | A6.1 | Queue displays correctly; can approve/reject/revise |
| A6.3 | Decision recording (verification record per `03-scholar-verification.md`) | 1pw | A6.2 | Records persist; queryable; auditable |
| A6.4 | Cross-school review routing (multi-tradition content → multiple scholars) | 1pw | A6.3 | Routing works per content metadata |
| A6.5 | Compensation tracking (per-review counts; for invoicing) | 0.5pw | A6.1 | Monthly report generates per scholar |
| A6.6 | Public verification badge (clickable on any verified content) | 1pw | A6.3 | UI shows scholar pseudonymous badge + credentials category |
| **Total** | | **6pw** | | |

**Risks for A6:**
- Insufficient scholars willing in alpha phase → mitigation: smaller corpus first; recruit aggressively in parallel.
- Scholar burnout from too much work → mitigation: paid retainers; cap reviews per month.

---

### Epic A7 — Web App (Next.js Frontend)

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| A7.1 | Next.js 16 App Router scaffold | 0.5pw | A1.1 | Runs locally; deploys to Vercel |
| A7.2 | Auth integration (Better Auth, passkeys + email magic-link) | 1.5pw | A1.4 | Sign-in / sign-up flows work |
| A7.3 | Chat UI (streaming responses, citation rendering) | 2pw | A5.7 | Citations clickable; streaming visible |
| A7.4 | Citation viewer (source-reader for Quran/hadith with surrounding context) | 2pw | A2.3, A3.3-A3.9, A4.3-A4.7 | RTL Arabic; translations side-by-side |
| A7.5 | i18n: Arabic, Urdu, English UI strings | 1pw | A7.1 | UI fully translated; RTL works |
| A7.6 | High-threat mode toggle | 0.5pw | A7.2 | Sessions ephemeral; no logs server-side |
| A7.7 | Settings (school preference, language, theme) | 0.5pw | A7.2 | Settings persist per user |
| A7.8 | Accessibility audit (WCAG AA baseline) | 1pw | A7.3-A7.7 | Lighthouse a11y ≥95 |
| **Total** | | **9pw** | | |

**Risks for A7:**
- RTL layout bugs in shared components → mitigation: test both directions throughout.
- Accessibility regressions on iteration → mitigation: a11y in CI.

---

### Epic A8 — Evaluation Suite

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| A8.1 | Correctness benchmark (200 questions, known answers + sources) | 2pw | A5.4 | Pass rate ≥85% on benchmark |
| A8.2 | Hallucination benchmark (50 trap questions for invented references) | 1pw | A5.4 | 0 hallucinated citations |
| A8.3 | Bias benchmark (50 multi-school questions) | 1pw | A5.5 | School representation within 10% balance |
| A8.4 | Refusal benchmark (50 disallowed prompts) | 0.5pw | A5.6 | Refuses 100% |
| A8.5 | Multilingual benchmark (run A8.1 in Arabic, Urdu, English) | 0.5pw | A8.1 | Quality drop <15% across languages |
| A8.6 | Automated evaluation in CI (run subset on every PR; full suite on main merge) | 1pw | A8.1-A8.5 | CI gates merges on evaluation results |
| A8.7 | Public benchmark scores published in repo | 0.25pw | A8.6 | `evals/results/` directory updated weekly |
| **Total** | | **6.25pw** | | |

**Risks for A8:**
- Benchmark gaming pressure as visible scores → mitigation: rotate held-out set; scholar adversarial review.

---

### Epic A9 — Security & Privacy

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| A9.1 | Secret scanning in CI (gitleaks) | 0.25pw | A1.2 | Prevents accidental commits |
| A9.2 | Rate limiting (per-user, per-IP) at API layer | 1pw | A7.2 | DOS attempts mitigated; legitimate users unaffected |
| A9.3 | Layer 4 encryption (column-level for queries, billing) | 1pw | A1.4 | Encrypted at rest; key in Infisical |
| A9.4 | Audit logging (admin access; sensitive operations) | 0.75pw | A1.4 | Logs queryable; retention defined |
| A9.5 | Penetration test (external) | (external) | — | Findings remediated before public alpha |
| A9.6 | Privacy policy implementation (data deletion, export) | 1pw | A1.4 | User can delete; export works |
| **Total** | | **4pw + external** | | |

**Risks for A9:**
- Pen-test findings critical → mitigation: budget 2pw cushion for fixes.

---

### Epic A10 — Alpha Launch

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| A10.1 | Invitation system (waitlist + invite codes) | 0.5pw | A7.2 | 50 invites sent and tracked |
| A10.2 | Feedback widget integrated everywhere | 0.5pw | A7.3 | Submissions arrive at Council Secretary inbox |
| A10.3 | Documentation site (/docs) explaining the alpha + how to give feedback | 0.5pw | — | Public docs live |
| A10.4 | Status page (public uptime monitoring) | 0.25pw | A1.9 | Visible to invited users |
| A10.5 | Onboarding email sequence | 0.25pw | A1.7 | Welcome + tutorial + feedback prompt |
| **Total** | | **2pw** | | |

---

### v0.1 Total Effort Summary

| Epic | Effort (pw) |
|---|---|
| A1 Infrastructure | 5.5 |
| A2 Quran ingestion | 4.75 |
| A3 Hadith Sunni | 11.25 |
| A4 Hadith Shia | 8.25 |
| A5 RAG pipeline | 10 |
| A6 Scholar verification | 6 |
| A7 Web app | 9 |
| A8 Evaluation suite | 6.25 |
| A9 Security & privacy | 4 |
| A10 Alpha launch | 2 |
| **Subtotal** | **66.5pw** |
| Buffer (15%) | 10pw |
| **Total** | **76.5pw** |

**Team scenarios:**

| Team | Calendar weeks |
|---|---|
| 1 person full-time | 76 weeks (1.5 yrs) |
| 2 people full-time | 38 weeks (~9 mo) |
| 3 people full-time | 26 weeks (~6 mo) |
| 1 FT + 2 part-time | 35 weeks (~8 mo) |

**v0.1 budget (rough):**

| Item | Cost |
|---|---|
| Hetzner infrastructure (6 mo) | $300 |
| Domain + DNS + email | $100 |
| LLM API budget (alpha-scale) | $500 |
| OpenAI embeddings (one-time + maintenance) | $200 |
| Scholar retainers (5 scholars × 6 mo × $500/mo) | $15,000 |
| External pen-test | $5,000 |
| Legal (entity formation + contracts) | $3,000 |
| Misc (tools, security keys, etc.) | $400 |
| **Buffer 20%** | $4,900 |
| **Total** | **~$29,400** |

If team is mostly volunteer + founder funded: $29K is the cash burn for v0.1.

---

### v0.1 Exit Criteria (audit checklist)

A reviewer should be able to verify each:

- [ ] All Epic A1-A10 tasks completed (visible in repo issue tracker)
- [ ] Quran corpus ingested with provenance hashes matching Tanzil
- [ ] 6 Sunni hadith collections + Muwatta ingested (~36,000 narrations)
- [ ] 4 Shia hadith collections + Nahj al-Balagha ingested
- [ ] 5+ verifying scholars onboarded; first 100 verified content units published
- [ ] Evaluation suite passing thresholds (correctness ≥85%, hallucination 0, bias balanced, refusal 100%)
- [ ] Web app live at `https://alpha.basira.ummah-ai.org` (or equivalent)
- [ ] 50 alpha users invited; 30+ have logged in at least 3 times
- [ ] External pen-test report received and findings remediated
- [ ] Privacy policy implemented; user data export works; deletion works
- [ ] First quarterly transparency entry published

If any checkbox is unchecked, v0.1 is not done.

---

## v0.5 Beta — Build Scope (target: 9 calendar months after v0.1)

**Goal:** Limited public beta with tafseer + fiqh subset + paid Plus tier + first institutional pilots. 1,000+ users.

### Epics (summary)

| Epic | Description | Effort |
|---|---|---|
| B1 | Tafseer ingestion: Ibn Kathir + al-Mizan + 1 contemporary | 8pw |
| B2 | Fiqh subset: ibadat across 5 schools (Hanafi, Maliki, Shafi'i, Hanbali, Ja'fari) | 12pw |
| B3 | School-filter configuration UX | 2pw |
| B4 | Saved libraries + multi-turn sessions | 3pw |
| B5 | Payment infrastructure (Stripe) | 3pw |
| B6 | Plus tier features (higher limits, export, priority) | 2pw |
| B7 | Institutional admin console v1 (user mgmt, school defaults) | 4pw |
| B8 | Institutional pilot onboarding (3-5 pilots) | 4pw |
| B9 | Public waitlist → tranched signups | 2pw |
| B10 | Evaluation suite v2 (expanded benchmarks) | 3pw |
| B11 | Self-hosted embedding migration evaluation (BGE-M3) | 4pw (research-only) |
| B12 | Mobile-responsive UX polish | 2pw |
| **Total** | | **49pw** |

### v0.5 Exit Criteria

- [ ] Tafseer + initial fiqh corpora live with scholar verification
- [ ] Paid Plus tier launched with regional pricing
- [ ] At least 3 paying institutional pilots signed
- [ ] $3-10K MRR
- [ ] 1,000+ active users
- [ ] 15+ verifying scholars
- [ ] Eval suite v2 results published
- [ ] First Council formed (5-7 members) per `COMMITTEE.md`

---

## v1.0 Public Launch — Build Scope (target: 6 calendar months after v0.5)

**Goal:** Full public launch. Free + Plus + Institutional tiers. 6 languages. 10,000+ users.

### Epics (summary)

| Epic | Description | Effort |
|---|---|---|
| L1 | Bengali, Bahasa, Turkish UI + content scaffolding | 6pw |
| L2 | Source-reader (full classical text reading UI) | 6pw |
| L3 | Family plan + parental controls | 3pw |
| L4 | Institutional admin console v2 (SSO, custom branding) | 6pw |
| L5 | Public benchmarks dashboard (live scores) | 2pw |
| L6 | First Annual Transparency Report published | 1pw |
| L7 | Waqf revenue commitment activated + first audit | (legal) |
| L8 | External security audit | (external) |
| L9 | Marketing site + onboarding redesign | 4pw |
| L10 | Performance optimization (latency, throughput) | 4pw |
| **Total** | | **32pw** |

### v1.0 Exit Criteria

- [ ] 6 languages live (Arabic, Urdu, English, Bengali, Bahasa, Turkish)
- [ ] Free + Plus + Institutional tiers operational
- [ ] 10,000+ MAU
- [ ] $20-50K MRR
- [ ] 25+ verifying scholars
- [ ] First Annual Transparency Report published
- [ ] External security audit passed; no critical findings
- [ ] Waqf 20% mission allocation legally binding + first quarterly transfer to Track B

---

## v1.5 Reach (calendar Y2-Y3)

| Epic | Description | Effort |
|---|---|---|
| R1 | iOS native app | 12pw |
| R2 | Android native app | 12pw |
| R3 | Voice interface (TTS for Arabic recitation; STT for queries) | 8pw |
| R4 | Offline mode (core corpus on-device) | 8pw |
| R5 | Swahili, Hausa, Persian | 6pw |
| R6 | Self-hosted embedding model deployed (BGE-M3) | 6pw |
| R7 | Second data center (APAC/MENA) | 6pw |
| R8 | Rapid-response current events module | 4pw |
| **Total** | | **62pw** |

### v1.5 Exit Criteria

- [ ] Mobile apps live in App Store + Play Store
- [ ] 9 languages
- [ ] 100,000+ users
- [ ] $500K-2M ARR
- [ ] 50+ verifying scholars
- [ ] Self-hosted embedding model in production
- [ ] Second data center serving 30%+ of traffic

---

## v2.0 Platform (calendar Y3-Y4)

Public Developer API; institutional admin v3; chaplaincy partnerships at scale.

| Epic | Description | Effort |
|---|---|---|
| P1 | Public Developer API (OAuth, rate limiting, usage billing) | 12pw |
| P2 | SDKs (TypeScript, Python) | 6pw |
| P3 | Marketplace (third-party integrations) | 8pw |
| P4 | White-label institutional offering | 8pw |
| P5 | National curriculum partnerships (Muslim-majority countries) | (negotiation) |
| **Total** | | **34pw** |

### v2.0 Exit Criteria

- [ ] Developer API live with documented SDKs
- [ ] 10+ third-party integrations using API
- [ ] $5-20M ARR
- [ ] Council fully formed (12 members)
- [ ] Board of Trustees established per Phase G2 governance

---

## v3.0 Sovereign (Y5+)

Self-hosted sovereign LLM; own compute backbone.

(High-level only at this stage; detailed planning in Phase 2 of the Initiative.)

---

## Open Questions / Decisions

These are unresolved as of 2026-04. Resolution required before relevant Epic begins.

1. **Embedding migration timing** — exactly when to switch from OpenAI to BGE-M3. Trigger: cost crosses threshold or quality matches.
2. **Institutional pricing currency** — fix in USD, or per-region? Pakistan/Indonesia institutions can't afford USD pricing.
3. **API rate limiting fairness** — exact limits per tier; revisit after first month of beta.
4. **Mobile app stack** — React Native vs native vs hybrid. Decide before R1 starts.
5. **Tafseer ordering after Ibn Kathir + al-Mizan** — which third (and subsequent)? Council vote.
6. **Fatwa councils integration** — which contemporary councils to ingest (Dar al-Ifta, European Council for Fatwa, etc.). Council vote.

---

## Risk Register (Basira-specific, project-level)

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Scholar bottleneck (cannot verify content fast enough) | High | Severe | Scholar recruitment in parallel from Phase 0; competitive retainers; cross-tradition redundancy |
| LLM provider rate-limit / contract issue | Medium | Moderate | Multi-provider abstraction (ADR-005); ability to switch in <2s |
| Hetzner outage | Low | Moderate | Multi-region backups; runbook tested |
| User data breach | Low | Severe | Encryption at rest + in transit; column-level for sensitive; pen-tests; bug bounty post-launch |
| Misrepresentation of Islamic content (scholarly accuracy) | Medium | Severe | Multi-stage scholar review; benchmark for hallucinations; rapid-fix process |
| Sectarian backlash on a specific feature | Medium | Moderate | Multi-school representation by default; content policy strict; advisory escalation |
| Capital insufficient for v0.1 build | Medium | Severe | Scope reduction first (smaller corpus before broader features); see budget alternatives below |
| Legal entity delay blocks scholar payments | Medium | Moderate | Founder personally fronts retainers via documented loan to entity until formed |
| Founder-key-person risk | Medium | Severe | Bus-factor protocol from Day 1 (`private/BUS_FACTOR.md`); Council formation by v0.5 |

---

## Reduced-scope alternatives if funding constrained

If full scope is unaffordable, reduced versions:

**Tier 1 (cheapest viable, ~$10K):**
- Quran only (no hadith)
- 1 language (Urdu or English alone)
- 1 scholar verifying
- Free tier only, no commercial yet
- Demonstrates principle; doesn't prove commercial viability

**Tier 2 (~$20K):**
- Quran + Bukhari + Muslim only
- Arabic + Urdu + English
- 3 scholars
- Free tier + small Plus tier
- Sufficient to prove commercial path

**Tier 3 (full v0.1, ~$30K):**
- As specified above

These tiers are documented so reduced scope is a deliberate choice, not a quiet quality slip.

---

## Cross-references

- Scholar verification process: `03-scholar-verification.md`
- RAG pipeline detail: `02-rag-pipeline.md`
- Data architecture: `../03-data-architecture.md`
- Tech stack ADRs: `../02-tech-stack-decisions.md`
- Security: `../10-security-architecture.md`
- Council structure: `../../COMMITTEE.md`
- Financial model: `../../FINANCIAL_MODEL.md`
- Main roadmap: `../../ROADMAP.md`

---

This document is updated whenever a task moves status. Current state is the truth; this document follows reality, not the other way around.
