# Basira — MVP Scope

## What Ships in Version 1.0

This document defines the scope of the first Basira release. Everything outside this scope is explicitly deferred.

The MVP is deliberately narrow. A narrow, excellent MVP is better than a wide, mediocre one — especially for a product whose trust depends on scholarly integrity.

---

## Scope Summary

```
┌─────────────────────────────────────────────┐
│ Version 1.0 (MVP)                           │
│                                             │
│ IN:                                         │
│   • Core corpus: Quran + 6 major Sunni     │
│     hadith collections + 4 major Shia      │
│     collections + 2-3 major tafseer works  │
│   • 3 languages: Arabic, Urdu, English     │
│   • Web app (responsive)                    │
│   • Free tier + Plus tier                  │
│   • Individual accounts only                │
│   • Citation-grounded responses             │
│   • Multi-school / multi-tradition default  │
│   • Scholar-verified core annotations       │
│   • Basic scholarly evaluation suite        │
│                                             │
│ OUT (deferred to later versions):           │
│   • Institutional tier / admin console      │
│   • Mobile native apps                      │
│   • Additional languages (Phase 2)          │
│   • Complete fiqh corpus (start with        │
│     focused subset)                         │
│   • Complete tafseer library (start with    │
│     most-used works)                        │
│   • Rapid-response current events module    │
│   • API for third-party integrations        │
│   • Offline mode                            │
│   • Voice interface                         │
│   • Image-based search (manuscript photos)  │
└─────────────────────────────────────────────┘
```

---

## Corpus Scope (MVP)

### Quran — complete
- Canonical Arabic text (Hafs 'an 'Asim reading).
- Translations in Arabic (modern exegetical), Urdu (2-3 major editions), English (3-4 major editions).
- [DEFERRED] Full variant qira'at — Phase 1.x.

### Hadith — Sunni collections (all 6 + Muwatta)
- Sahih al-Bukhari (complete).
- Sahih Muslim (complete).
- Sunan Abu Dawud.
- Jami' at-Tirmidhi.
- Sunan an-Nasa'i.
- Sunan Ibn Majah.
- Muwatta Malik.
- With gradings where source provides.

### Hadith — Shia collections (all 4 + Nahj al-Balagha)
- Al-Kafi.
- Man La Yahduruhu al-Faqih.
- Tahdhib al-Ahkam.
- Al-Istibsar.
- Nahj al-Balagha.

### Hadith — Ibadi
- [TENTATIVE] Musnad ar-Rabi' bin Habib — if clean digitization available.
- If not ready for MVP: deferred to Phase 1.x; clearly noted as a current gap.

### Tafseer — starter set
- **Sunni tafseer**: Tafseer Ibn Kathir (most widely-referenced).
- **Shia tafseer**: Tafseer al-Mizan by al-Tabatabai (widely-referenced Shia work).
- **Additional**: at least one tafseer by a contemporary widely-respected scholar (TBD during Phase 0).

More tafseer works added in Phase 1.x as scholar-review bandwidth allows.

### Fiqh — MVP deliberately light
Full fiqh corpora are large and complex. MVP scope:

- A **focused subset** covering the most common practical questions (ibadat — worship; basic muamalat — transactions).
- Representation from each major school (Hanafi, Maliki, Shafi'i, Hanbali, Ja'fari).
- [DEFERRED] comprehensive fiqh library to Phase 2.

### Contemporary fatwa
- [DEFERRED] to Phase 1.x. MVP refers users to established fatwa authorities for contemporary questions rather than attempting to aggregate.

---

## Features — In MVP

### Semantic search
- Query in Arabic, Urdu, or English.
- Returns relevant passages from the corpus with full citations.
- Multi-tradition by default; filters available.

### Citation-grounded chat
- Conversational Q&A.
- Every scholarly claim cited.
- Uncertainty surfaced when sources thin.
- Refusal for fatwa-issuing and out-of-scope requests.

### Multi-school representation
- When asking about topics where schools differ, all major positions surfaced.
- Configurable per-user "school-first" view (stored preference).

### User accounts
- Anonymous use (no account required for free tier basic queries).
- Pseudonymous accounts (email + passkey).
- Settings: language preferences, school preference, theme.

### Saved libraries
- Users can bookmark passages.
- Users can save conversation sessions (opt-in; stored per user).
- Export to markdown / PDF.

### High-threat mode
- Opt-in setting.
- Server-side query logging disabled.
- Client-side storage only.
- Reduced session metadata.

### Citation viewer
- Any citation in any response is clickable.
- Opens the primary source in context with surrounding passages.
- Shows translation alternatives.
- Shows scholar verification metadata.

### Subscription management
- Free tier, Plus tier.
- Stripe integration.
- Clear pricing.
- Self-service cancellation.

---

## Features — Out (Deferred to Future Versions)

### Institutional tier
- Admin console for institutions.
- Per-institution school configuration.
- User management.
- Invoicing and procurement flows.
- [Target: Version 1.1 or 1.2]

### Mobile native apps
- MVP is responsive web only.
- iOS + Android native apps in Phase 2.

### Additional languages
- Bahasa Indonesia, Malay, Bengali, Turkish, Persian in Phase 1.x.
- Hausa, Swahili, French, Spanish later.

### API for third-party developers
- Public API for other Muslim developers to build on Basira.
- [Target: Version 1.3+] after MVP stability demonstrated.

### Voice interface
- Audio input and output.
- Arabic Quranic recitation integration.
- [Target: Phase 2]

### Manuscript / image search
- Search using an image of a classical manuscript page.
- OCR for Arabic manuscripts.
- [Target: Phase 3]

---

## Success Criteria for MVP Launch

Before Basira 1.0 is considered launched (not "shipped" — we can ship earlier and iterate in public):

### Functional

- [ ] Core corpus ingested and indexed per `../03-data-architecture.md`.
- [ ] Scholar verification process operational with at least 10 active scholars across required traditions.
- [ ] Evaluation suite passing defined thresholds (correctness, hallucination rate, bias, refusal).
- [ ] Three-language UI with full translation coverage.
- [ ] Responsive web app functional on desktop and mobile browsers.

### Operational

- [ ] Observability stack operational.
- [ ] Backup and disaster recovery procedures tested.
- [ ] Incident response playbooks in place.
- [ ] Security review complete (internal at minimum; external audit preferred).

### Compliance & Trust

- [ ] Privacy policy published and implemented.
- [ ] Terms of service published.
- [ ] Transparency report template in place.
- [ ] Scholar verification transparent (scholars listed, process documented).
- [ ] Evaluation benchmarks published (Layer 1 open).

### Commercial

- [ ] Pricing model finalized.
- [ ] Billing infrastructure operational.
- [ ] Free tier sustainable within infra budget.
- [ ] First 3-5 institutional conversations started for pilots.

### Governance

- [ ] Founding governance documents signed (foundation docs).
- [ ] Legal entity established (or clear path to establishment).
- [ ] Scholar advisory council convened.
- [ ] Waqf-style revenue commitment in governing documents.

---

## Known Gaps at Launch (Acknowledged, Not Hidden)

We will ship with known gaps. Transparency about them is itself a feature:

- Ibadi hadith coverage may be limited depending on digitization availability.
- Fiqh coverage is intentionally narrow at MVP — the commitment is to expand.
- Some less-common classical works are not yet ingested.
- Latency for complex cross-school queries may be higher than target at MVP scale.
- Scholar verification throughput is the primary bottleneck for corpus growth.

A public "known gaps" list is published and updated.

---

## Timeline

Aspirational rough targets. Subject to revision as reality lands:

- **Phase 0 (Foundation)**: 2026 Q2 (now).
- **Phase 1 build**: 2026 Q3 - Q4.
- **Alpha (private)**: 2026 Q4 - 2027 Q1.
- **Beta (limited public)**: 2027 Q1 - Q2.
- **MVP launch (v1.0)**: 2027 Q2 - Q3.
- **Sustained operation / Phase 2 planning**: 2027 H2+.

These targets are not commitments. They are planning scaffolds that will be adjusted as the real work reveals real timelines.

---

## What Could Cause Scope Reduction

Things that could force tightening the MVP further:

- Insufficient scholar-review throughput.
- Insufficient funding to sustain infrastructure at target scale.
- Technical challenges with classical Arabic NLP beyond MVP timeline.
- Legal / jurisdictional complications with hosting or incorporation.

In any of these cases, we prefer shipping a smaller excellent product to shipping a larger flawed one. Scope reduction is documented openly; arbitrary deadline pressure is not a reason to compromise principles.

---

## What Could Cause Scope Expansion

Things that could (but should not) tempt us to expand:

- Investor pressure for more features.
- Competitor moves.
- Sudden mass interest.

None of these are justifications to ship more than we can do well. Discipline here is mission-protective.

---

## Cross-References

- Product overview: `01-overview.md`
- RAG pipeline: `02-rag-pipeline.md`
- Scholar verification: `03-scholar-verification.md`
- System overview: `../01-system-overview.md`
- Strategy: `../../docs/05-strategy.md`
