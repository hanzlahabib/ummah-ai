# Hisn — Detailed Roadmap

> Audit-grade. Specific tasks. Effort estimates. Acceptance criteria. Risks.
> No fluff. Auditable by digital-rights organizations and at-risk users.

---

## Status snapshot

| Item | Value |
|---|---|
| Phase | Pre-implementation (sketch only) |
| Last update | 2026-04 |
| Funding secured | $0 |
| Active contributors | 0 |
| Partner orgs (verbal interest) | 0 |

Implementation begins Phase 2-3 of the Initiative, after Basira MVP and Shahid v0.1 are operational.

---

## How to audit this roadmap

1. **Public content delivery:** Educational guides countable; translations countable.
2. **Tool downloads:** Browser extension installs from official stores; verifiable.
3. **Detection accuracy:** Public benchmark scores for deepfake detection.
4. **Partnership MoUs:** Public list.
5. **Annual transparency report:** Documented users helped (anonymized aggregate), tools shipped.

---

## Mission scope (constraint)

Hisn is **not** a replacement for established digital-security organizations. It is:

- **Translator** — bringing existing best practices into Muslim languages (Urdu, Arabic, Bahasa, Bengali, Turkish, Persian, etc.).
- **Localizer** — contextualizing OPSEC for Muslim threat models (state surveillance of Muslim minorities; deepfake harassment of Muslim women; activist protection in MENA / South Asia).
- **Tool packager** — taking proven open-source security tools and making them accessible.
- **Coordinator** — connecting acutely-threatened individuals with established specialist organizations (Access Now, EFF, Tactical Tech).

Hisn is NOT:

- A first-response service for digital emergencies.
- An elite security firm.
- A research lab producing novel cryptography.

This scope discipline keeps Hisn deliverable.

---

## v0.1 — Educational Foundation (target: 6 months from start)

**Goal:** Public site with translated, contextualized digital-security education in 3 languages. No tools yet beyond pointers to existing ones.

### Epic H1 — Content Production: Threat Models

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| H1.1 | Threat model: Muslim diaspora professional (US/UK/EU) | 1pw | — | Documented; reviewed by 2 partner orgs |
| H1.2 | Threat model: Activist in MENA / South Asia | 1pw | — | Same |
| H1.3 | Threat model: Muslim woman facing deepfake harassment | 1pw | — | Same; reviewed by women's-org partner |
| H1.4 | Threat model: Journalist covering Muslim-majority conflicts | 1pw | — | Same |
| H1.5 | Threat model: Convert with hostile family situation | 0.5pw | — | Same |
| H1.6 | Threat model: Madrasa student / scholar in surveilled jurisdiction | 0.5pw | — | Same |
| **Total** | | **5pw** | | |

---

### Epic H2 — Content Production: How-To Guides (English first)

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| H2.1 | Device security baseline (full-disk encryption, password manager, 2FA, updates) | 1.5pw | — | Step-by-step for major OS (iOS, Android, Windows, macOS, Linux) |
| H2.2 | Secure communications (Signal, Element, encrypted email) | 1pw | — | Setup walkthroughs with screenshots |
| H2.3 | Browser hardening (privacy settings, tracker blockers, fingerprinting resistance) | 1pw | — | Step-by-step for major browsers |
| H2.4 | Mobile-device hygiene (app permissions, location data, backups) | 1pw | — | Specific to iOS + Android |
| H2.5 | Social media OPSEC (account hygiene, deplatforming preparedness) | 1pw | — | Per-platform specifics |
| H2.6 | Travel + border-crossing OPSEC | 1pw | — | Region-specific (US, EU, Gulf, China, India) |
| H2.7 | Family / spouse / child OPSEC (home network, smart devices, voice assistants) | 1pw | — | Practical, non-paranoid framing |
| H2.8 | What to do when you suspect a compromise | 1pw | — | Decision tree; coordination contacts |
| H2.9 | Quick-reference OPSEC cards (printable; per threat model) | 1pw | H1.1-H1.6 | One card per threat model |
| **Total** | | **9.5pw** | | |

---

### Epic H3 — Translation (Phase 1 languages)

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| H3.1 | Urdu translation of all H1 + H2 content | 2pw | H1, H2 | Native-speaker reviewed |
| H3.2 | Arabic translation of all H1 + H2 content | 2pw | H1, H2 | Native-speaker reviewed |
| H3.3 | Establish translation workflow (Crowdin or similar) for ongoing updates | 0.5pw | — | Workflow operational |
| **Total** | | **4.5pw** | | |

---

### Epic H4 — Site Infrastructure

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| H4.1 | Static site generator (Astro / Next.js static) | 0.5pw | — | Builds from markdown content |
| H4.2 | i18n routing (Arabic RTL; Urdu RTL; English LTR) | 1pw | H4.1 | All routes work in 3 languages |
| H4.3 | Tor hidden service mirror | 0.5pw | H4.1 | `.onion` accessible |
| H4.4 | Mirror to multiple servers (geographic redundancy) | 0.5pw | H4.1 | Mirrors auto-update |
| H4.5 | Search across content | 0.5pw | H4.1 | Client-side search; works offline |
| H4.6 | Accessibility (WCAG AA) | 0.5pw | H4.1-H4.5 | Lighthouse a11y ≥95 |
| **Total** | | **3.5pw** | | |

---

### v0.1 Total Effort Summary

| Epic | Effort (pw) |
|---|---|
| H1 Threat models | 5 |
| H2 How-to guides (English) | 9.5 |
| H3 Translation (Urdu, Arabic) | 4.5 |
| H4 Site infrastructure | 3.5 |
| **Subtotal** | **22.5pw** |
| Buffer (15%) | 3.5pw |
| **Total** | **26pw** |

**Team scenarios:**

| Team | Calendar weeks |
|---|---|
| 1 FT writer + 2 part-time translators + 1 part-time engineer | 18-24 weeks |
| 2 FT (writer + engineer) + freelance translators | 12-15 weeks |

**v0.1 budget (rough):**

| Item | Cost |
|---|---|
| Hosting (CDN + 2 mirrors) | $200 / 6 mo |
| Translation (commissioned for first language outside founder pool) | $5,000 |
| Editor (security review) | $5,000 |
| Threat-model expert review (3 reviewers × 5 hours each) | $1,500 |
| Misc (tools, accessibility audit) | $1,000 |
| Writer salary (or contract) | $20,000-40,000 |
| **Total** | **$32,700-52,700** |

---

### v0.1 Exit Criteria

- [ ] 6 documented threat models
- [ ] 9 how-to guides
- [ ] All content available in English, Urdu, Arabic
- [ ] Public site live with Tor mirror
- [ ] Site accessible (WCAG AA)
- [ ] Reviewed by 3+ digital-rights organizations
- [ ] Listed on partner organizations' resource pages

---

## v0.5 — First Tools (target: 9 months after v0.1)

**Goal:** Browser extension for deepfake detection + 2 utility tools. Begin partnership for rapid-response coordination.

### Epic H5 — Deepfake Detection Browser Extension

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| H5.1 | Survey existing deepfake-detection models (Intel FakeCatcher, Microsoft Video Authenticator, academic open models) | 1pw | — | Comparison documented; chosen model justified |
| H5.2 | Select model + license verification | 0.5pw | H5.1 | License compatible with our distribution |
| H5.3 | Browser extension scaffold (Manifest V3, cross-browser via WebExtensions API) | 1pw | — | Extension loads in Chrome + Firefox |
| H5.4 | Right-click "Check this image" / "Check this video" integration | 1.5pw | H5.3 | Context menu works on images / videos |
| H5.5 | Detection runs (client-side where feasible; server-side fallback for video) | 3pw | H5.2, H5.3 | Detection runs; result displayed |
| H5.6 | Result UX (clear: "Likely real / Likely synthetic / Inconclusive" with confidence; explanation) | 1pw | H5.5 | UX reviewed by non-technical users |
| H5.7 | C2PA verification integration (when content has provenance) | 1pw | — | Provenance-bearing content handled separately |
| H5.8 | Privacy policy: extension does NOT phone home unnecessarily; user controls server-fallback | 0.5pw | H5.5 | Policy clear; verifiable in source |
| H5.9 | Submission to Chrome Web Store + Firefox AMO | 1pw | H5.3-H5.8 | Listed in both stores |
| H5.10 | Documentation + onboarding | 0.5pw | H5.9 | Install guide; first-use walkthrough |
| **Total** | | **11pw** | | |

---

### Epic H6 — Metadata Stripper Utility

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| H6.1 | Web tool: upload image → strip EXIF → download | 1pw | — | Works for major formats (JPEG, PNG, HEIC) |
| H6.2 | Web tool: PDF metadata stripper | 1pw | — | Removes author, creation date, hidden text |
| H6.3 | Privacy: all processing client-side; no server involvement | 0.5pw | H6.1, H6.2 | Verifiable in source |
| H6.4 | Mobile-friendly UI | 0.5pw | H6.1, H6.2 | Works on small screens |
| **Total** | | **3pw** | | |

---

### Epic H7 — Threat-Assessment Questionnaire

Interactive: user answers questions; gets personalized OPSEC priority list.

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| H7.1 | Question tree design (covering H1.1-H1.6 threat models) | 1pw | H1 | Tree reviewed by partner orgs |
| H7.2 | UI implementation | 1.5pw | H4 | Works without account |
| H7.3 | Output: personalized priority list with links to relevant H2 guides | 1pw | H7.1, H7.2, H2 | Output is useful, not generic |
| H7.4 | Privacy: assessment runs client-side; no data stored | 0.25pw | H7.2 | Verifiable |
| **Total** | | **3.75pw** | | |

---

### Epic H8 — Rapid Response Coordination

| ID | Task | Effort | Dependencies | Acceptance criteria |
|---|---|---|---|---|
| H8.1 | Identify and approach 5 specialist orgs (Access Now Helpline, EFF, Tactical Tech, CPJ, etc.) for coordination MoUs | 2pw | — | Initial conversations documented |
| H8.2 | Define handoff protocol (when Hisn refers a case; what info is shared) | 1pw | H8.1 | Protocol approved by partners |
| H8.3 | Encrypted intake for acute cases (PGP-protected; minimal metadata) | 1pw | — | Form live; tested |
| H8.4 | Triage logic (when to refer, when to provide self-help) | 1pw | H8.2, H8.3 | Decision tree; volunteer-runnable |
| H8.5 | Volunteer coordinator training | 1pw | H8.4 | 3-5 volunteers trained |
| **Total** | | **6pw** | | |

---

### Epic H9 — Translation Wave 2

| ID | Task | Effort | Acceptance criteria |
|---|---|---|---|
| H9.1 | Bahasa / Bengali / Turkish translation of all H1+H2 content | 5pw | Native-reviewed; published |
| H9.2 | Add new content to translation pipeline | 0.5pw | Workflow integrated |
| **Total** | | **5.5pw** | |

---

### v0.5 Total Effort Summary

| Epic | Effort (pw) |
|---|---|
| H5 Deepfake extension | 11 |
| H6 Metadata utilities | 3 |
| H7 Threat questionnaire | 3.75 |
| H8 Rapid response | 6 |
| H9 Translation wave 2 | 5.5 |
| **Subtotal** | **29.25pw** |
| Buffer (15%) | 4pw |
| **Total** | **33pw** |

---

### v0.5 Exit Criteria

- [ ] Browser extension live in 2+ stores; 1,000+ installs
- [ ] Metadata utilities live; client-side verified
- [ ] Threat questionnaire live
- [ ] 3-5 specialist-org MoUs for rapid-response handoff
- [ ] Content available in 6 languages (English, Urdu, Arabic, Bahasa, Bengali, Turkish)

---

## v1.0 Full Toolkit (Y2-Y3)

| Epic | Description | Effort |
|---|---|---|
| L-H1 | Mobile app (iOS + Android) | 16pw |
| L-H2 | Encrypted notes / journal for activists | 6pw |
| L-H3 | Persian, Hausa, Swahili translations | 6pw |
| L-H4 | OPSEC training program (online curriculum) | 8pw |
| L-H5 | Curriculum certification for NGO digital-security trainers | 4pw |
| **Total** | | **40pw** |

### v1.0 Exit Criteria

- [ ] Mobile apps live
- [ ] 9 languages
- [ ] Training curriculum used by 3+ partner orgs
- [ ] 10,000+ installs across tools

---

## v2.0 — International OPSEC Network (Y4+)

(Sketch only at this stage.)

---

## Risk Register (Hisn-specific)

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Deepfake detection model fails on real-world adversarial examples | High | Severe | Conservative thresholds; "inconclusive" output; never claim certainty; pair with C2PA |
| Browser store removes extension under pressure | Medium | High | Direct download fallback; signed releases; multi-store distribution |
| Tool gives users false sense of security | Medium | Severe | UX framing emphasizes layered security; never single-tool reliance |
| Content out of date as threat models evolve | High | Moderate | Quarterly review cadence; partner-org review; deprecation labels |
| Translation quality issues | Medium | Moderate | Native-speaker review required; community feedback channel |
| Acute-case mishandling | Low | Severe | Triage protocol; volunteer training; specialist handoff |
| Funding instability | Medium | High | Mostly grant-funded; subsidized by Basira mission allocation; consulting income from training |

---

## Funding model (Hisn-specific)

Hisn is **not commercial**. Revenue-positive scenarios:

- **Grants** — digital rights foundations, press freedom funds, Muslim philanthropic waqfs.
- **Basira mission allocation** — share of waqf-allocation funds.
- **Paid training contracts** — NGO digital-security training for Muslim civil society organizations.
- **Curriculum licensing** — established orgs license Hisn's curriculum for their internal use.
- **No direct user fees** — user-facing tools are free.

Target annual budget v0.5 onward: **$200K-500K**.

---

## Cross-references

- Project sketch: `01-overview.md`
- Threat model: `../../docs/08-project-threat-model.md`
- Disclosure framework: `../../docs/03-open-vs-closed.md`
- Financial model: `../../FINANCIAL_MODEL.md`
- Council: `../../COMMITTEE.md`
