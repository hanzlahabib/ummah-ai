# Mizan — Detailed Roadmap

> Audit-grade. Specific tasks. Effort estimates. Acceptance criteria. Risks.
> Auditable by digital-rights researchers, regulators, journalists.

---

## Status snapshot

| Item | Value |
|---|---|
| Phase | Pre-implementation (sketch only) |
| Last update | 2026-04 |
| Funding secured | $0 |
| Active researchers | 0 |
| Reports published | 0 |

Implementation begins late Phase 2 / early Phase 3 of the Initiative, after Basira and Shahid demonstrate sustainability.

---

## Mission scope

Mizan = **independent, reproducible audits** of how major platforms handle Muslim content, accounts, and issues. Quantitative findings; published methodology; press- and regulator-ready outputs.

Mizan is NOT:

- An advocacy group (we publish data, others advocate).
- A litigation firm (we provide data; partners litigate).
- A platform-bashing exercise (when platforms behave well, that's part of the report).

---

## How to audit this roadmap

1. **Reports:** Each report has a public methodology + reproducible script + dataset.
2. **Reproducibility:** Independent third-party can re-run analyses on the same data and get the same results.
3. **Pre-registration:** Hypotheses and methods registered before data collection (where applicable), reducing p-hacking.
4. **Annual transparency report:** Spending, partnerships, regulatory engagement.

---

## v0.1 — Methodology + First Report (target: 6 months from start)

**Goal:** Documented methodology + 1 published report on a single platform / single issue.

### Epic M1 — Methodology Development

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| M1.1 | Survey existing methodologies (AlgorithmWatch, Center for Countering Digital Hate, 7amleh) | 1pw | — | Documented; reusable approaches identified |
| M1.2 | Define metrics: content moderation disparity (e.g., shadow ban rates by language) | 1pw | M1.1 | Operationalized; reproducible |
| M1.3 | Define metrics: hashtag suppression | 1pw | M1.1 | Same |
| M1.4 | Define metrics: account suspension by demographic indicator | 1pw | M1.1 | Same |
| M1.5 | Statistical methods (sample size, confidence intervals, statistical power) | 1pw | — | Standard methodology; pre-registration support |
| M1.6 | Pre-registration template (lock methodology before data collection) | 0.5pw | M1.5 | Template ready |
| M1.7 | Methodology peer review by 2+ academic statisticians + 2+ digital rights researchers | (external) | M1.1-M1.6 | Reviewed; revisions integrated |
| M1.8 | Published methodology document (Layer 1 open) | 0.5pw | M1.7 | Permalinked |
| **Total** | | **5pw + external review** | | |

---

### Epic M2 — Measurement Infrastructure

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| M2.1 | Platform API clients (where APIs exist: Meta Graph, X API, YouTube Data, TikTok Research API) | 4pw | — | Authenticated, rate-limited, error-handled |
| M2.2 | Scraping framework for non-API data (where Terms of Service permit research) | 3pw | (legal) | Legal review complete; framework operational |
| M2.3 | Comparative test harness (paired content varying by language / demographic) | 2pw | M2.1, M2.2 | Sends paired posts; tracks outcomes |
| M2.4 | Data warehouse (Postgres) for collected metrics | 1pw | — | Schema flexible per metric type |
| M2.5 | Reproducibility infrastructure (Jupyter notebooks; pinned environments) | 1pw | M2.4 | Notebooks + Docker images for re-running analyses |
| M2.6 | Privacy: data collection respects platform users' privacy; no scraping of private content | 1pw | M2.2 | Documented privacy policy for our research |
| **Total** | | **12pw + legal** | | |

---

### Epic M3 — First Investigation: Pilot Study

Pick one platform + one issue for the pilot. Recommended: Meta + Palestinian-content moderation disparity (high-signal, well-precedented by BSR audit, 7amleh data).

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| M3.1 | Pre-register hypotheses + methodology (publish in advance) | 0.5pw | M1.6 | Public registry entry |
| M3.2 | Build test corpus (paired Arabic / Hebrew / English content, controlled for keyword and topic) | 1.5pw | — | 100+ paired posts; review by Arabic-speaker partner |
| M3.3 | Execute paired posts via test accounts | 1pw | M2.3, M3.2 | Posts sent; outcomes tracked over 30+ days |
| M3.4 | Data collection (visibility, reach, engagement, suspensions) | 1pw | M3.3 | Data complete; integrity hash recorded |
| M3.5 | Statistical analysis | 2pw | M3.4 | Reproducible notebook; results with confidence intervals |
| M3.6 | Report writing (technical + executive summary) | 2pw | M3.5 | Press-ready + policy-ready formats |
| M3.7 | Pre-publication review by partner researchers | 0.5pw | M3.6 | Review complete; revisions integrated |
| M3.8 | Publication (own site + paper repository like SSRN) | 0.5pw | M3.7 | Published; DOI assigned |
| M3.9 | Press outreach (5-10 news organizations) | 1pw | M3.8 | Coverage tracked |
| M3.10 | Platform engagement (notify Meta of findings before publication; per coordinated-disclosure norms) | 0.5pw | M3.7 | Notification documented; response (if any) included in report |
| **Total** | | **10.5pw** | | |

---

### v0.1 Total Effort Summary

| Epic | Effort (pw) |
|---|---|
| M1 Methodology | 5 + external |
| M2 Infrastructure | 12 + legal |
| M3 Pilot study | 10.5 |
| **Subtotal** | **27.5pw + external + legal** |
| Buffer (15%) | 4pw |
| **Total** | **~32pw + external review + legal review** |

**Team scenarios:**

| Team | Calendar weeks |
|---|---|
| 1 FT data scientist + 1 PT engineer + 1 PT writer + legal | 18-22 weeks |
| 2 FT (data science + engineering) | 16 weeks |

**v0.1 budget (rough):**

| Item | Cost |
|---|---|
| Infrastructure (servers, data storage) | $1,000 |
| Methodology peer review honoraria (4 reviewers × $1,000) | $4,000 |
| Legal review (TOS, scraping risk) | $5,000 |
| Data scientist (1 FT × 5 mo) | $30,000-50,000 |
| Engineer (PT × 5 mo) | $15,000-25,000 |
| Writer / editor | $5,000-10,000 |
| Press outreach + dissemination | $2,000 |
| Misc (tools, conferences) | $3,000 |
| **Total** | **$65,000-100,000** |

---

### v0.1 Exit Criteria

- [ ] Published methodology (peer-reviewed)
- [ ] Pre-registered + executed pilot study
- [ ] Published report with reproducible analysis
- [ ] Picked up by 3+ news outlets
- [ ] Platform notified per coordinated-disclosure
- [ ] Reproducibility verified by 1+ independent researcher

---

## v0.5 — Quarterly Report Cadence (target: 12 months after v0.1)

**Goal:** Quarterly reports across multiple platforms; established research partnerships.

### Epics (summary)

| Epic | Description | Effort |
|---|---|---|
| B-M1 | Expand to 3 platforms (Meta + X + TikTok) | 6pw |
| B-M2 | Standardized quarterly report format | 3pw |
| B-M3 | Public dashboard (real-time-ish metrics) | 6pw |
| B-M4 | Academic partnerships (2-3 universities for replication) | 4pw |
| B-M5 | Researcher API (let academics access aggregated data) | 4pw |
| B-M6 | Methodology v2 (incorporating learnings) | 3pw |
| B-M7 | Onboard 2-3 partner research orgs (AlgorithmWatch coordination) | (negotiation) |
| **Total** | | **26pw + negotiation** |

### v0.5 Exit Criteria

- [ ] 4 quarterly reports published
- [ ] Cross-platform analysis live
- [ ] 2-3 academic partnerships formal
- [ ] Public dashboard accessible
- [ ] Cited in 5+ academic papers / regulatory submissions

---

## v1.0 — Public Platform (target: 6 months after v0.5)

**Goal:** Public platform with reports + dashboard + researcher access. First regulator engagement (EU DSA, UK Online Safety Act, etc.).

### Epics (summary)

| Epic | Description | Effort |
|---|---|---|
| L-M1 | Public report archive with citation tools | 4pw |
| L-M2 | Live dashboard (selected metrics updated weekly) | 6pw |
| L-M3 | Regulator engagement workflow (EU DSA submissions, etc.) | 4pw |
| L-M4 | Multi-language outputs (key reports in Arabic, Urdu, English minimum) | 3pw |
| L-M5 | Annual cross-platform synthesis report | 3pw |
| L-M6 | Press kit + briefing infrastructure | 2pw |
| **Total** | | **22pw** |

### v1.0 Exit Criteria

- [ ] 8+ quarterly reports + 1 annual synthesis published
- [ ] Engaged with 2+ regulators
- [ ] Reports cited in 1+ regulatory action
- [ ] Multi-language report distribution
- [ ] Annual budget $1M+ secured

---

## v2.0 — Coalition (Y3+)

| Epic | Description | Effort |
|---|---|---|
| C-M1 | Cross-platform longitudinal study (multi-year tracking) | 6pw / yr |
| C-M2 | Coalition with AlgorithmWatch / 7amleh / CCDH for joint reports | (negotiation) |
| C-M3 | Whistleblower channel (encrypted intake for platform insiders) | 3pw |
| C-M4 | Litigation support service (data preparation for lawsuits) | (operational) |
| **Total** | | **9pw + ongoing** |

---

## Risk Register (Mizan-specific)

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Platform changes API access (cuts off measurement capability) | High | High | Maintain multiple measurement methods; coalition-share data when one method dies |
| Legal threat from platforms (TOS enforcement, computer fraud claims) | Medium | High | Legal counsel from start; research-safe-harbor jurisdictions; methodology that respects TOS |
| Methodology criticized as biased | Medium | Moderate | Pre-registration; peer review; transparent statistics; consistent methodology across platforms |
| Failure to find statistically significant effects (null results) | Medium | Low | Publish null results too; that's good science |
| Platform "fixes" issue right before report (claim no problem) | Medium | Low | Document the change; before/after framing strengthens story |
| Funding tied to specific findings (corruption pressure) | Low | Severe | Refuse funders demanding specific outcomes (per `FINANCIAL_MODEL.md`); diversify funding |
| Bad-faith counter-research from platforms | Medium | Low | Publish open data; let third parties replicate |

---

## Funding model (Mizan-specific)

Mizan funded primarily via:

- **Foundation grants** — democracy / digital rights (Open Society, Ford, MacArthur, Knight, Mozilla, Reset).
- **Academic partnership budgets** — universities co-fund specific research.
- **Press syndication** — major outlets license exclusive findings (priced fairly; never exclusive enough to delay public release).
- **Regulator-paid research** — EU Commission, UK Ofcom, similar bodies pay for specific studies under their accountability mandates.
- **Basira mission allocation** — share of Initiative's waqf-allocation pool.

Target annual budget v0.5: **$500K-1.5M**. v1.0+: **$1.5M-3M**.

Refused funding (per `FINANCIAL_MODEL.md`):

- Anything from platforms being audited (clear conflict).
- Anything tied to specific findings.
- Anything with editorial control.

---

## Open Questions

1. **First platform target** — Meta is most-precedented; X has more accessible data; TikTok is least studied. Council vote.
2. **Test-account ethics** — using research test accounts is standard but contentious; specific protocol per platform.
3. **Whistleblower intake** — when and how to launch (post-v1.0 likely; legal complexity high).
4. **Scaling vs. depth** — broad coverage of many issues vs. deep coverage of fewer? Council guidance.

---

## Cross-references

- Project sketch: `01-overview.md`
- Disclosure framework: `../../docs/03-open-vs-closed.md`
- Threat model: `../../docs/08-project-threat-model.md`
- Financial model: `../../FINANCIAL_MODEL.md`
- Council: `../../COMMITTEE.md`
