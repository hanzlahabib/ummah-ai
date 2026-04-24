# Basira — Overview

## بَصِيرَة — Insight

The commercial anchor of the initiative. An Islamic scholarly research copilot that serves students, scholars, lay Muslims, Islamic finance professionals, chaplains, educators, and journalists who need trustworthy, citation-grounded access to Islamic scholarly knowledge in their native language.

---

## Product Positioning

### What Basira is

- A **retrieval-augmented AI assistant** grounded in verified Islamic sources.
- A **multi-tradition knowledge access layer** that respects scholarly plurality.
- A **productivity tool** for scholars, students, and institutions — not a replacement for qualified scholars.

### What Basira is not

- Not a fatwa-issuing body.
- Not a chatbot that invents Islamic content.
- Not a single-school filter on a generic LLM.
- Not a substitute for personal consultation with qualified scholars on sensitive questions.

---

## Primary Users

### Individual users

- **Students** of Islam at various levels (madrasa students, university students, adult learners).
- **Lay Muslims** seeking accessible access to scholarly knowledge in their own language.
- **Professionals with mission-relevant questions** — Islamic finance analysts, chaplains, educators, journalists covering Muslim issues.
- **Scholars themselves** using Basira as a research productivity tool.

### Institutional users

- **Madaris and Islamic universities** deploying Basira for their students.
- **Islamic banks and finance firms** for Shariah compliance research.
- **Chaplaincy programs** supporting hospital, prison, and military chaplains.
- **Muslim media outlets** for fact-checking and research.

---

## Core Capabilities (Phase 1)

1. **Semantic search across the curated Islamic corpus.**
   - Quran + major hadith collections (Sunni, Shia, Ibadi) + classical tafseer + major fiqh texts.
   - Cross-reference graph — surface related content automatically.

2. **Citation-grounded answers.**
   - Every claim traces to specific sources.
   - Users can click any citation and see the primary source in context.
   - No hallucinated hadiths or attributions.

3. **Multi-school representation.**
   - When schools disagree, positions presented with attribution.
   - User can configure school-filtered view (e.g. Maliki-first for a Maliki student).
   - Default view is full-spectrum.

4. **Multi-language interface.**
   - Primary languages at launch: Arabic, Urdu, English.
   - Next wave: Bahasa Indonesia, Malay, Bengali, Turkish, Persian.
   - Scholarly content accessed in source language; translations attributed.

5. **Uncertainty-aware output.**
   - When sources disagree: disagreement surfaced.
   - When a question is contested: contestation acknowledged.
   - When qualified scholarly judgment is needed: user referred to scholars rather than substitution.

6. **Scholar-verified core content.**
   - Content that reaches users has been reviewed per the scholar-verification workflow.
   - Verification records transparent.

---

## User Experience Principles

### Respect scholarly integrity

- No content presented as Islamic scholarship that has not been sourced.
- Gradings of hadiths included where source works provide them.
- Translator attributions always visible.

### Respect user agency

- No "the answer" framing when scholarly opinion is plural.
- No imposing of any school's view.
- Clear about what Basira does and does not know.

### Respect user privacy

- No tracking. No third-party analytics.
- Query history user-controlled.
- High-threat mode for at-risk users (activists, journalists, dissidents).

### Respect the reader's time

- Fast responses (sub-3-second target for most queries).
- Clear, well-structured output.
- Scan-friendly: headings, lists, citations prominent.

---

## User Flows (Illustrative)

### Flow 1: Casual question

```
User: "What does the Quran say about patience?"
↓
Basira:
  • Lists key verses (with surah:ayah citation)
  • Key hadiths on sabr (with collection + grading)
  • Summary of scholarly discussion
  • "Want deeper sources? Tap any citation to read in context."
```

### Flow 2: Scholarly disagreement

```
User: "Is music permissible in Islam?"
↓
Basira:
  • Acknowledges this is a contested question among scholars.
  • Presents the permissive position with its sources.
  • Presents the prohibitive position with its sources.
  • Presents intermediate positions.
  • Recommends consultation with a qualified scholar of the user's tradition for a personal ruling.
  • Does not issue a verdict.
```

### Flow 3: Classical Arabic research

```
Scholar: [types Arabic phrase] "أجمع الفقهاء على..."
↓
Basira:
  • Returns passages from fiqh literature matching this claim
  • Shows the works that make this claim
  • Flags any counter-evidence or scholarly disputes
  • Provides exact citations for academic use
```

### Flow 4: Institutional use (madrasa)

```
Madrasa admin configures: School-first = Shafi'i, Language = English + Arabic

Student: "How should I perform wudu?"
↓
Basira:
  • Opens with Shafi'i position (configured default)
  • Clearly indicates this is the school-default
  • Link to "see other schools" for broader view
  • Sourced to specific Shafi'i fiqh texts
```

### Flow 5: High-threat mode

```
User is a journalist researching persecuted minority communities.
  They enable high-threat mode.

Basira:
  • Query processed with ephemeral session (no persistent log)
  • Standard responses, same quality
  • Session terminates cleanly; nothing retained server-side
  • Client-side history optional and user-controlled
```

---

## Pricing Model

### Free tier

- Generous free allocation for individual users.
- All core capabilities included.
- Daily query limit (reasonable for personal study).
- No ads, no user-data monetization.

### Plus tier (individual)

- Higher query limits.
- Advanced features (saved libraries, multi-turn research sessions, detailed export).
- Priced for accessibility — our goal is adoption, not extraction.

### Institutional tier

- Per-seat or per-institution pricing.
- Administrative features (school defaults, user management, custom corpora).
- Priority support.
- Custom integration options (LMS integration, API access).

Pricing details determined by pilot conversations with institutional customers. Principle: affordable enough that Muslim institutions in developing markets can access, sustainable enough to fund the mission.

---

## Commercial Strategy Dependencies

Per `../../docs/05-strategy.md`:

- Basira's revenue funds the defensive projects (Shahid, Hisn, Mizan) via the **waqf-style 20% commitment** — legally binding, audited.
- Basira's user base provides **distribution reach** for defensive tools.
- Basira's brand creates **trust capital** that translates to other projects.

Basira's commercial success is therefore mission-critical. But mission-critical does not mean mission-compromising — the principles still bind.

---

## Ecosystem Relationship

- Basira's public corpora (Quran, hadith, classical tafseer, embeddings) published as Layer 2 resources.
- Other Muslim AI developers can build on the same data.
- Basira's evaluation framework for Islamic content released as Layer 1 — a public benchmark others can use and extend.
- We welcome ecosystem competitors; a thriving Muslim AI ecosystem is the mission.

---

## Boundaries

### What Basira will refuse

- Issuing fatawa.
- Personalized religious rulings (refers to scholars).
- Acting as a substitute for qualified scholarly consultation on consequential personal matters.
- Representing one school's view as "the Muslim view" when plurality exists.
- Producing content outside scope (see `../../docs/09-content-policy.md`).
- Assisting in identifying, tracking, or targeting individuals.
- Content that would violate the non-negotiables.

### What Basira will always do

- Cite sources for every scholarly claim.
- Acknowledge uncertainty.
- Respect user agency.
- Protect user privacy.
- Be honest about limits.

---

## Documents in This Section

- `01-overview.md` — this file
- `02-rag-pipeline.md` — retrieval and generation design
- `03-scholar-verification.md` — workflow and data model for scholarly review
- `04-mvp-scope.md` — concrete scope of the first release

Read in order for full design context.
