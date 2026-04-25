# Shahid — Detailed Roadmap

> Audit-grade. Specific tasks. Effort estimates. Acceptance criteria. Risks.
> No fluff. Auditable by journalists, lawyers, and human-rights reviewers.

---

## Status snapshot

| Item | Value |
|---|---|
| Phase | Pre-implementation (sketch only) |
| Last update | 2026-04 |
| Funding secured | $0 |
| Funding committed | $0 |
| Partner orgs (verbal interest) | 0 (outreach pending) |
| Cases archived | 0 |
| Active contributors | 0 |
| Council members holding Shahid lead role | 0 |

Implementation begins Phase 2 of the Initiative (after Basira MVP demonstrates revenue path), per `../../docs/05-strategy.md`.

---

## How to audit this roadmap

1. **Repo state:** Once code begins, each task corresponds to issues/PRs.
2. **Cases archived:** Public count visible at any time; methodology documented.
3. **Partnership MoUs:** Public list of partner organizations.
4. **Quarterly Pattern Reports:** Published on schedule once v1.0 live.
5. **Annual transparency report:** Spending vs. budget, governance decisions, incidents.

---

## v0.1 — Methodology Phase (target: 6 months from Phase-2 start)

**Goal:** Documented methodology + 3 pilot partnerships + 10-20 case studies in internal archive. No public archive yet.

### Epic S1 — Methodology Documentation

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| S1.1 | Define case schema (per-incident structured record) | 1pw | — | Schema covers: incident type, geography, date, victims, evidence types, sources, corroborations |
| S1.2 | Define evidence-quality grades (Tier-1 multi-corroborated → Tier-3 single-source) | 0.5pw | — | Grading rubric public; reviewable |
| S1.3 | Define provenance chain requirements (C2PA where applicable; chain-of-custody for documents) | 1pw | — | Doc covers media, documents, witness statements separately |
| S1.4 | Define witness/source-protection protocol | 1pw | — | Threat-model-aware protocol; reviewed by 2 HR-org partners |
| S1.5 | Submission acceptance criteria (what we accept, what we don't) | 0.5pw | S1.1, S1.2 | Documented; reviewable |
| S1.6 | Methodology peer review by 2+ established HR organizations | (external) | S1.1-S1.5 | Written feedback; revisions integrated |
| S1.7 | Final methodology document published | 0.25pw | S1.6 | Layer-1 open content; permalinked |
| **Total** | | **4.25pw + external review** | | |

---

### Epic S2 — Infrastructure Setup

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| S2.1 | Provision archive primary site (Hetzner EU) | 0.5pw | — | Server live; SSH+key auth |
| S2.2 | Provision replica site #1 (different jurisdiction; consider Iceland, Switzerland, or Malaysia) | 1pw | — | Server live; replication tested |
| S2.3 | Provision replica site #2 (third jurisdiction) | 1pw | — | Server live; replication tested |
| S2.4 | Postgres + append-only log table (cryptographic hash chain) | 1.5pw | S2.1 | Append-only enforcement at schema level; hash chain validates |
| S2.5 | Object storage for media artifacts (encrypted at rest) | 1pw | S2.1 | Self-hosted MinIO or equivalent; encryption keys managed via Infisical |
| S2.6 | Replication automation (primary → replicas) | 1pw | S2.1-S2.3 | Daily replication; integrity verified; lag <24h |
| S2.7 | Tor hidden service for intake | 0.5pw | S2.1 | `.onion` address live; intake form accessible |
| S2.8 | Backup integrity verification (daily) | 0.5pw | S2.4-S2.5 | Automated check; alert on hash mismatch |
| **Total** | | **7pw** | | |

---

### Epic S3 — Cryptographic Provenance

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| S3.1 | C2PA library integration (sign manifests; verify on submission) | 2pw | S2.5 | Manifests sign-and-verify on test artifacts |
| S3.2 | Internal signing keys (HSM or hardware-backed) | 1pw | S2.5 | Keys provisioned; rotation documented |
| S3.3 | Per-case hash chain (each case appended to log; previous hash included) | 1pw | S2.4 | Chain verifies; tamper detection works |
| S3.4 | Public timestamping (OpenTimestamps anchor) | 1pw | S3.3 | Anchored to Bitcoin; verification reproducible |
| S3.5 | Verification tools (third party can verify chain integrity from public data) | 1pw | S3.1-S3.4 | CLI tool published; reproducible |
| **Total** | | **6pw** | | |

---

### Epic S4 — Submission Intake

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| S4.1 | Web intake form (over Tor + clearnet) | 1.5pw | S2.7, S2.4 | Form accepts: case description, evidence files, optional witness ID |
| S4.2 | Encrypted intake (PGP option for high-sensitivity) | 1pw | S4.1 | Submitter can encrypt to Shahid public key; submission stored encrypted until processing |
| S4.3 | Anonymous intake (zero-knowledge for highest sensitivity) | 2pw | S4.2 | Submitter encrypts to a key only the case-handling partner holds; Shahid never sees plaintext |
| S4.4 | Intake triage UI (for partner organizations only) | 1pw | S4.1 | Partners can review pending submissions; assign |
| S4.5 | Submission rate limiting + spam filtering | 0.5pw | S4.1 | Reasonable limits; bot-spam filtered |
| **Total** | | **6pw** | | |

---

### Epic S5 — Verification Workflow

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| S5.1 | Verification queue (cases awaiting review) | 1pw | S4.4 | Queue visible to authorized verifiers |
| S5.2 | Corroboration tooling (link cases to other cases / external sources) | 1.5pw | S2.4 | Cross-references queryable |
| S5.3 | Decision recording (verified / requires-more / rejected, with reasoning) | 1pw | S5.1 | Decisions persist with verifier identity (could be partner-org rep) |
| S5.4 | Verification grading per S1.2 rubric | 0.5pw | S5.3 | Grade attached to each case |
| S5.5 | Source-protection check (no submission published without source-protection review) | 1pw | S5.3 | Required gate; documented in case record |
| **Total** | | **5pw** | | |

---

### Epic S6 — Pilot Partnerships

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| S6.1 | Identify and approach 5-10 candidate partner orgs (HRW, 7amleh, +972, Forensic Architecture, Bellingcat, Access Now, Article 19, Reporters Without Borders, etc.) | 3pw | — | Initial outreach to all; responses tracked |
| S6.2 | MoU template (legally reviewed; mission-aligned) | 1pw | (legal) | Template approved by counsel |
| S6.3 | Sign 3 pilot partnerships | (negotiation) | S6.1, S6.2 | 3 signed MoUs; partners onboarded |
| S6.4 | Partner training (use of intake, verification tools, security practices) | 1pw | S6.3 | Each partner has trained operators |
| S6.5 | First 10-20 cases sourced from pilot partners | (operational) | S6.3, S2.4-S5.5 | 10-20 cases in archive with full provenance |
| **Total** | | **5pw + negotiation** | | |

---

### v0.1 Total Effort Summary

| Epic | Effort (pw) |
|---|---|
| S1 Methodology | 4.25 |
| S2 Infrastructure | 7 |
| S3 Cryptographic provenance | 6 |
| S4 Submission intake | 6 |
| S5 Verification workflow | 5 |
| S6 Pilot partnerships | 5 + negotiation |
| **Subtotal** | **33pw + negotiation** |
| Buffer (15%) | 5pw |
| **Total** | **~38pw + negotiation time** |

**Team scenarios:**

| Team | Calendar weeks |
|---|---|
| 1 person FT + partner-org collaboration | 38 weeks (~9 mo) |
| 2 FT + partner collaboration | 19 weeks (~5 mo) |

**v0.1 budget (rough):**

| Item | Cost |
|---|---|
| 3 servers (primary + 2 replicas, 6 mo) | $1,500 |
| Object storage (initial) | $300 |
| Domain + Tor | $100 |
| HSM / hardware keys | $1,000 |
| External methodology peer-review honoraria (2 reviewers) | $4,000 |
| Legal counsel (entity, MoUs, evidence standards review) | $8,000 |
| Engineer salaries (1 FT × 9 mo, depending on funding model) | $50,000-90,000 |
| Investigative lead (part-time) | $20,000-40,000 |
| Misc (security audits, conferences, travel for partner meetings) | $10,000 |
| **Total** | **$95,000-155,000** |

Funded primarily via:
- Mission-allocation from Basira (Phase 2 onward).
- Foundation grants (Open Society, Ford, MacArthur, Luminate, Omidyar, Muslim philanthropic waqfs).

---

### v0.1 Exit Criteria (audit checklist)

- [ ] Methodology document published; peer-reviewed by 2+ HR orgs
- [ ] 3 partner MoUs signed
- [ ] Infrastructure: primary + 2 replica sites operational; replication verified
- [ ] Cryptographic chain operational; 3rd-party verification tool published
- [ ] Tor + clearnet intake live; anonymous-mode tested
- [ ] 10-20 cases in archive with full provenance
- [ ] Source-protection protocol audited by HR-org partner

---

## v0.5 — Closed Pilot (target: 12 months after v0.1)

**Goal:** Internal archive of 100-500 cases. First publicly-released case. Verification workflow mature.

### Epics (summary)

| Epic | Description | Effort |
|---|---|---|
| B-S1 | Internal search + browse for partner orgs | 4pw |
| B-S2 | Citation export (BibTeX, legal-citation formats) | 2pw |
| B-S3 | Case-detail UI for case investigators | 3pw |
| B-S4 | Multi-source corroboration view | 3pw |
| B-S5 | First publicly-released case (with partner co-publication) | 1pw + coordination |
| B-S6 | Onboard 2-3 additional partners | (negotiation) |
| B-S7 | Annual external security review | (external) |
| B-S8 | Source-protection review board (independent) | 2pw setup |
| **Total** | | **15pw + ext + negotiation** |

### v0.5 Exit Criteria

- [ ] 100-500 cases archived
- [ ] 5+ partner orgs producing active submissions
- [ ] First major news story citing Shahid as source
- [ ] Legal panel review confirms court-admissibility in at least one jurisdiction
- [ ] Funding runway secured for v1.0 (12+ months)

---

## v1.0 — Public Launch (target: 6 months after v0.5)

**Goal:** Public search + research API. First quarterly Pattern Report. 1,000+ cases.

### Epics (summary)

| Epic | Description | Effort |
|---|---|---|
| L-S1 | Public search & browse UI | 6pw |
| L-S2 | Case-detail public pages | 4pw |
| L-S3 | Researcher API (credentialed; rate-limited) | 5pw |
| L-S4 | Citation tools (one-click "cite this case") | 1pw |
| L-S5 | First Pattern Report (quarterly) — analysis tooling + report template | 4pw |
| L-S6 | Permalink + DOI assignment for major cases | 2pw |
| L-S7 | Multilingual case display (Arabic, Urdu, English minimum) | 4pw |
| L-S8 | Docs site explaining methodology, how to use, how to contribute | 2pw |
| **Total** | | **28pw** |

### v1.0 Exit Criteria

- [ ] 1,000+ verified cases public
- [ ] Cited in 5+ major news publications
- [ ] Cited in 1+ academic paper
- [ ] Referenced in 1+ legal filing
- [ ] 10+ partner organizations
- [ ] First Pattern Report published

---

## v2.0 — Institutional (Y3+)

| Epic | Description | Effort |
|---|---|---|
| I-S1 | ICC / UN tribunal pipeline (chain-of-custody hardening) | 8pw |
| I-S2 | Legal filing support service (paralegal team) | (operational) |
| I-S3 | Cross-jurisdictional mirror network (3+ regions, automatic) | 6pw |
| I-S4 | Bellingcat / Forensic Architecture data integration | 4pw |
| I-S5 | Researcher dashboard for academics | 4pw |
| **Total** | | **22pw + operational** |

### v2.0 Exit Criteria

- [ ] 10,000+ cases
- [ ] Referenced in ICC-level / UN proceeding
- [ ] Methodology cited in academic literature
- [ ] Multi-jurisdiction mirror network live

---

## v3.0 — Permanence (Y5+)

Permanent archive infrastructure designed to outlast operating organization.

(High-level only at this stage.)

---

## Risk Register (Shahid-specific)

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| State-level legal demand to remove specific case | High | High | Multi-jurisdiction mirrors; counsel-mediated response; transparent challenge |
| State-level technical attack on infrastructure | Medium | Severe | Geographic redundancy; offline backups; cryptographic integrity prevents silent tampering |
| Source compromise (submitter identified) | Medium | Critical | Zero-knowledge intake; minimal metadata; strict source-protection protocol |
| False / fabricated submission | Medium | High | Rigorous corroboration; multiple verifiers; grading rubric makes uncertainty visible |
| Misuse to target individuals | Low | High | Access controls on protected index; bulk-query review |
| Funding instability | High | Severe | Multiple streams: Basira mission allocation + grants + waqf; never one source >40% |
| Politicization accusations | High | Moderate | Neutral methodology; consistent application; transparent process |
| Partner org defection / capture | Medium | Moderate | MoU includes mission-alignment clauses; multi-partner redundancy |

---

## Open Questions

1. **Replica jurisdictions** — final selection (Iceland? Switzerland? Malaysia? Estonia? Costa Rica?). Council vote.
2. **Anchoring chain** — Bitcoin via OpenTimestamps, or alternative? Trade-off: established vs. coin-volatile.
3. **Researcher API gating** — pure-credentialed, or vouching-based? Risk of opening to bad-faith requesters.
4. **Legal-filing support pricing** — free for HR orgs; what about for-profit law firms? Council decision.

---

## Cross-references

- Project sketch: `01-overview.md`
- Threat model: `../../docs/08-project-threat-model.md`
- Disclosure framework: `../../docs/03-open-vs-closed.md`
- Council structure: `../../COMMITTEE.md`
- Financial model: `../../FINANCIAL_MODEL.md`
