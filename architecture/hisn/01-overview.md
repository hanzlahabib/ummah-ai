# Hisn — Architectural Sketch

## حِصْن — Fortress

Counter-surveillance toolkit and defensive infrastructure for Muslims and others facing AI-enabled surveillance, harassment, or targeting. Detailed architecture deferred to Phase 2 start; this document captures design principles.

---

## Status

**Current phase**: design sketch only. Implementation targeted for Phase 2 alongside or shortly after Shahid.

---

## Mission Recap

Per `../../docs/04-projects.md`:

Provide accessible defensive tools and education for at-risk users. Translate the documentation from Shahid and academic research into practical protection. Serve journalists, activists, dissidents, surveilled minorities, women facing deepfake harassment, and anyone whose threat model exceeds what generic digital security resources assume.

---

## Core Components

Hisn is not a single product; it's a **portfolio of defensive resources** under a unified brand and delivery infrastructure.

### 1. Educational content (largest component)

- Multilingual privacy and security guides.
- Threat-model-specific OPSEC guides (e.g. "Kashmiri journalist safety guide," "woman facing deepfake harassment response guide").
- Device hardening walkthroughs.
- Secure communications primers (Signal, Session, Tor, etc.).
- Understanding AI threats specifically — what facial recognition does, how deepfakes work, how algorithmic targeting operates, what counter-measures exist.

### 2. Deepfake detection

- Browser extension for consumers.
- Web interface for verifying uploaded media.
- Integration with C2PA where content has provenance metadata.
- Backed by established detection models (FakeCatcher-style, Microsoft Video Authenticator-equivalent) with Hisn-specific UX.

### 3. Utility tooling

- Metadata strippers (remove EXIF from photos before sharing).
- Safe-sharing helpers (automated redaction checks).
- Device setup checklists.
- Quick-reference OPSEC cards.

### 4. Rapid response coordination

- For acute individual cases: coordination with established orgs (Access Now, EFF, 7amleh, Committee to Protect Journalists).
- Hisn is **not** a first-response service; we connect users to appropriate specialists.
- For scale threats (coordinated harassment campaigns, mass deplatforming), collective response coordination.

---

## High-Level Architecture Sketch

```
┌─────────────────────────────────────────────────────────┐
│  PUBLIC EDUCATIONAL SITE                                │
│   • Multilingual guides                                 │
│   • Threat-specific walkthroughs                        │
│   • Video explainers                                    │
│   • Accessible via Tor                                  │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│  TOOL SUITE                                             │
│   • Deepfake detection (browser ext + web)              │
│   • Metadata strippers                                  │
│   • Threat assessment questionnaires                    │
│   • Secure setup wizards                                │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│  RAPID RESPONSE INTERFACE                               │
│   • Encrypted intake for acute cases                    │
│   • Automated triage to appropriate partner orgs        │
│   • Case tracking (access-controlled)                   │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  BACKEND (minimal — most work is client-side)           │
│   • Deepfake detection compute                          │
│   • Content guide delivery                              │
│   • Partner coordination tooling (private)              │
└─────────────────────────────────────────────────────────┘
```

---

## Design Principles Specific to Hisn

### Client-side by default

Wherever possible, Hisn tools do their work on the user's device, not on our servers. This:

- Reduces data exposure.
- Allows offline use.
- Makes Hisn useful even if our servers are blocked or compromised.

### No user accounts for most features

- Educational content: no account needed.
- Deepfake detection: optional account only if users want history saved.
- Rapid response intake: intentionally designed with minimum user data collection.

### Tool distribution

- Browser extensions published through Chrome Web Store, Firefox AMO, and direct download.
- Mobile tools considered for Phase 3 after web and extension baselines established.
- Source code open (where publishing doesn't compromise specific deployed defenses).

### Open defaults

Per the layered disclosure framework:

- Educational content: Layer 1 (fully open).
- Tool source code: Layer 2 (open) for standard utilities.
- Specific OPSEC procedures for severe threat models: Layer 3 (limited distribution via trusted partner orgs).
- Individual case data: Layer 4 (never central where avoidable).

---

## Relationship to Other Projects

- **Basira** users who exhibit indicators of being at-risk (their own disclosure in high-threat mode) may be referred to Hisn resources proactively.
- **Shahid** documents harms; Hisn helps people defend against them.
- **Mizan** data on platform behavior informs Hisn's advice on which platforms are safer for which use cases.

---

## Partnership Strategy (Central to Hisn)

Unlike Basira (where we build our own scholarly corpus), Hisn primarily **curates, translates, and complements** existing digital security work.

- **Do not duplicate** what Access Now / EFF / Tactical Tech / 7amleh / Internet Freedom Foundation (India) / Reporters Without Borders already do well.
- **Add Muslim-context specifics** that generic digital security resources miss.
- **Translate and localize** — high-quality digital security content in Urdu, Arabic, Bengali, Bahasa is scarce.
- **Coordinate** when individuals need rapid specialist help.

---

## Tech Stack (Tentative)

- Static site generation (fast, cacheable, accessible via Tor easily).
- Browser extension framework (standard WebExtensions API).
- Minimal backend — edge function style; Vercel or self-hosted.
- Deepfake detection: leverage established open models; UX layer on top.

---

## Guiding Principles Specific to Hisn

- **Protect real people first.** Every decision runs through: "does this make an at-risk person safer?"
- **Meet users where they are.** Someone whose phone is compromised cannot download a sophisticated app. Start with what they can do today.
- **Not fear marketing.** Hisn does not scare users for engagement. Accurate risk communication, calm delivery.
- **Humility about specialization.** We are not an elite digital security firm. We connect, translate, and cover gaps.
- **Open, but careful.** Open educational content; careful with specific case details that could expose vulnerable individuals.

---

## Cross-References

- Project description: `../../docs/04-projects.md#project-3--hisn`
- Disclosure framework: `../../docs/03-open-vs-closed.md#hisn-counter-surveillance-toolkit`
- Threat model: `../../docs/08-project-threat-model.md`
- Privacy commitment: `../../PRIVACY.md`

---

Full architecture documented when Phase 2 implementation begins.
