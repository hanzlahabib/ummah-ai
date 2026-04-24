# Project Portfolio

## The Sub-Projects That Make Up This Initiative

This initiative is not a single product. It is a **portfolio of interconnected projects**, each serving a distinct purpose, together forming a coherent whole.

This document introduces the initial portfolio. Additional projects will be added as the ecosystem grows. Existing projects may be retired or spun off. The portfolio is living.

Each project has:
- A clear mission.
- An explicit disclosure posture (per `docs/03-open-vs-closed.md`).
- A funding model.
- A relationship to the other projects.

---

## The Four Founding Projects

The portfolio begins with four projects. Their Arabic names reflect the values they embody, not branding flourish.

| Project | Arabic | Meaning | Role |
|---|---|---|---|
| **Basira** | بَصِيرَة | Insight, clarity of vision | Scholarly research copilot |
| **Shahid** | شَاهِد | Witness | Atrocity documentation archive |
| **Hisn** | حِصْن | Fortress | Counter-surveillance toolkit |
| **Mizan** | مِيزَان | Balance, scale of justice | Algorithmic accountability auditor |

Each draws on a different facet of the mission. Together they cover research (preservation of knowledge), documentation (witness), protection (defense of the vulnerable), and accountability (confrontation of abuse).

---

## Project 1 — Basira (بَصِيرَة)

**Mission:** Make the vast corpus of Islamic scholarly knowledge — Quran, Hadith, tafseer, fiqh, usul, sirah, history — accessible, searchable, and useful to every Muslim with an internet connection, in their native language, with scholarly integrity preserved and sectarian balance respected.

### Why it matters

The existing options for Muslim knowledge access are:
- **Generic AI chatbots** (ChatGPT, Gemini) — hallucinate on Islamic content, cannot cite scholarly sources, often produce sectarian bias or outright errors, do not understand classical Arabic at research quality.
- **Search engines** — no integration with scholarly methodology; deep research requires expertise most students lack.
- **Existing Islamic sites (IslamQA, Yaqeen Institute, etc.)** — valuable but single-perspective, limited interactivity, not AI-native.
- **Direct scholarly consultation** — accessible only to those with proximity to qualified scholars.

Basira aims to serve the spectrum in between — students, lay Muslims, non-specialist scholars, Islamic finance professionals, chaplains, educators, journalists — who need trustworthy AI-assisted access to scholarly knowledge.

### What it does

- Semantic search across the Quran, major hadith collections (both Sunni and Shia), classical tafseer, foundational fiqh texts, and contemporary fatwa archives.
- Citation-grounded responses — every claim links to its source.
- Multi-school representation — when a question has different positions across schools, all major positions are surfaced.
- Translation between Arabic, Urdu, English, Bahasa, and other Muslim-world languages, with scholarly care for classical Arabic.
- Transparency about uncertainty — when sources disagree, when a question is contested, when a topic requires scholarly consultation, the system says so.

### Disclosure posture

- **Layer 1 (open):** Evaluation frameworks, benchmarks, research papers on Islamic LLM accuracy and bias.
- **Layer 2 (open):** Classical Arabic tokenizers and embeddings, RAG framework, citation tooling.
- **Layer 3 (proprietary):** Specific fine-tuned models, scholar-verification workflow, production APIs.
- **Layer 4 (fortress):** User queries, ulama network identities, premium corpora licenses.

### Funding model

Freemium consumer product (free tier for individual use, premium for power users), B2B licensing (madaris, universities, Islamic banks, chaplaincy services), and institutional contracts (scholarly institutions licensing customized deployments).

### Role in the ecosystem

Basira is the **commercial anchor**. Its revenue funds the other three projects. Its user base and brand trust create the distribution channel through which defensive tools can reach the people who need them.

---

## Project 2 — Shahid (شَاهِد)

**Mission:** Build and maintain a permanent, cryptographically-verified archive of AI-driven harms against civilians, accessible to journalists, researchers, lawyers, and the public.

### Why it matters

AI-driven harms — from Lavender-style targeting to algorithmic censorship to deepfake weaponization — are happening now, are poorly documented, and are frequently denied. When evidence is not gathered, verified, and preserved, these harms become invisible over time. The historical record — and legal accountability that depends on it — requires infrastructure.

Existing human rights documentation organizations (Human Rights Watch, Amnesty International, 7amleh, Forensic Architecture, Bellingcat) do critical work but are generally stretched across many issues. An AI-harms-specific archive, with appropriate technical infrastructure, complements rather than competes with their work.

### What it does

- Intake submissions of documented AI-driven harms (with source verification).
- Cryptographically sign and timestamp submissions (C2PA-style provenance).
- Maintain multiple geographically-distributed backups.
- Provide public search and research interfaces.
- Generate reports on patterns and trends.
- Coordinate with journalists and legal practitioners for specific cases.
- Offer standardized citation formats for academic and journalistic use.

### Disclosure posture

- **Layer 1 (open):** Verification standards, cryptographic protocols, submission specifications.
- **Layer 2 (open):** Verification tooling, metadata schemas, search APIs for public data.
- **Layer 3 (proprietary but transparent):** Archive-hosting infrastructure, access control systems.
- **Layer 4 (fortress):** Source identities, ongoing investigations, witness data — zero-knowledge architecture where possible.

### Funding model

Grant-funded and waqf-funded. Never commercial. The archive's independence from commercial pressure is part of its credibility.

Potential funders: human rights foundations, press freedom organizations, Muslim philanthropic waqfs, academic institutions. No funding accepted from sources that would compromise independence.

### Role in the ecosystem

Shahid is the **memory**. It ensures that what is documented today is available to researchers, lawyers, and future generations. It creates accountability pressure on perpetrators who would otherwise operate without historical record.

---

## Project 3 — Hisn (حِصْن)

**Mission:** Provide accessible defensive tools and education for Muslims and others facing AI-enabled surveillance, harassment, or targeting.

### Why it matters

The populations most targeted by AI surveillance and harassment — Palestinian journalists, Uyghur diaspora, Kashmiri activists, Muslim women facing deepfake harassment, converts in hostile family situations, dissidents in authoritarian states — rarely have the technical expertise or resources to defend themselves. Generic digital security resources exist but are often not translated, not contextualized, and not aware of the specific threats these populations face.

### What it does

- Education: clear, multilingual guides on device security, secure communication, metadata protection, threat modeling.
- Tools: curated recommendations and, where needed, purpose-built utilities (metadata strippers, deepfake detection, secure storage helpers).
- Deepfake detection: browser extension and web interface for verifying suspicious media.
- Localized OPSEC guidance: specific threat models for specific populations, developed with community partners.
- Rapid response: coordination with existing digital security organizations when individual Muslims face acute threats.

### Disclosure posture

- **Layer 1 (open):** General privacy education, threat modeling frameworks, protocol documentation.
- **Layer 2 (open):** Device hardening scripts, metadata strippers, general-purpose deepfake detection.
- **Layer 3 (limited distribution):** Specific OPSEC procedures tailored to severe threat models (e.g. activists in Xinjiang). Distributed through trusted partner organizations, not public.
- **Layer 4 (never central):** Individual user configurations — architecturally, we do not store individual user data centrally where avoidable.

### Funding model

Grant-funded (digital rights foundations, press freedom grants), partnership-funded (collaboration with Access Now, EFF, Muslim Advocates, 7amleh, Tactical Tech), and mission-subsidized (from Basira revenue).

### Role in the ecosystem

Hisn is the **shield**. It translates the documentation from Shahid and the research from academic partners into practical protection for specific people. It is the project closest to direct impact on individual lives.

---

## Project 4 — Mizan (مِيزَان)

**Mission:** Conduct and publish independent audits of how major technology platforms treat Muslim content, Muslim users, and Muslim-relevant issues — and use those audits to pressure platforms toward fairer behavior.

### Why it matters

Platforms (Meta, TikTok, X, YouTube, Google Search) shape public perception and access at global scale. When they systematically suppress, deprioritize, or miscategorize Muslim content, the effects compound: protests underreported, scholars deplatformed, community organizing disrupted, atrocities rendered invisible.

Existing watchdogs — AlgorithmWatch in Europe, the Electronic Frontier Foundation, 7amleh for Palestine specifically — do excellent work but none is Muslim-world-focused and none has the full methodology needed for systematic, reproducible, periodic auditing.

### What it does

- Reproducible methodologies for auditing platform behavior on Muslim-relevant content (hashtag suppression, account suspensions, content moderation error rates by language).
- Periodic public reports (quarterly and/or annually) documenting findings.
- Comparative analysis across platforms.
- Translation of findings into press-ready and policy-ready formats.
- Engagement with platforms when findings warrant — not as activism, but as independent audit.
- Coordination with regulators in jurisdictions where digital services legislation creates accountability mechanisms (EU Digital Services Act, UK Online Safety Act, others).

### Disclosure posture

- **Layer 1 (fully open):** Audit methodologies, evaluation frameworks, raw anonymized data where legally permissible.
- **Layer 2 (open):** Audit tooling, scripts, analysis pipelines.
- **Layer 3 (published openly):** Reports, dashboards, specific findings. This is the primary output.
- **Layer 4 (protected):** Internal source identities, any sensitive data received from whistleblowers, ongoing investigation data.

### Funding model

Grant-funded (democracy and digital rights foundations), institutionally partnered (university digital rights labs), and possibly publicly crowdfunded for specific investigations.

### Role in the ecosystem

Mizan is the **scale**. It quantifies what is often only asserted. It provides the empirical basis for policy advocacy, press coverage, and legal action. It complements Shahid (which archives specific incidents) by focusing on the systemic patterns of platform behavior.

---

## How the Projects Relate

```
                    ┌────────────────────┐
                    │      BASIRA        │
                    │  commercial anchor │
                    │   → funds others   │
                    └─────────┬──────────┘
                              │
                              │ revenue, brand, distribution
                              ▼
       ┌─────────────┬────────────────┬───────────────┐
       │             │                │               │
       ▼             ▼                ▼               ▼
┌───────────┐ ┌─────────────┐ ┌────────────┐  (future
│  SHAHID   │ │    HISN     │ │   MIZAN    │   projects
│  memory   │ │   shield    │ │   scale    │   extend
│  archive  │ │  defensive  │ │ accountab. │   portfolio)
└───────────┘ └─────────────┘ └────────────┘
       │             │                │
       └─────────────┴────────────────┘
                     │
                     │ findings, data, tooling
                     ▼
               ┌───────────────┐
               │  THE UMMAH    │
               │   AND THE     │
               │   WORLD       │
               └───────────────┘
```

Each project strengthens the others:
- **Basira** funds and distributes.
- **Shahid** documents what Mizan and Hisn respond to.
- **Hisn** uses tooling informed by Shahid's documentation.
- **Mizan** uses Shahid's archive as evidence and creates pressure Hisn's users benefit from.
- All four share the same underlying research, engineering, and organizational foundation.

---

## Future Projects (Not Yet Started)

These are recognized as needed but not yet initiated. Contributors may take initiative on any of them:

- **Hikmah (حِكْمَة — Wisdom):** Islamic AI ethics framework and evaluation standards — what does it mean for an AI system to be aligned with Maqasid al-Shariah? A scholarly-technical collaboration producing usable guidance for developers.
- **Lisan (لِسَان — Tongue):** Low-resource Muslim-world language AI — Pashto, Sindhi, Balochi, Hausa, Somali, regional Malay variants. Open model releases, datasets, evaluation benchmarks.
- **Amanah (أَمَانَة — Trust):** Privacy-preserving infrastructure specifically for Muslim populations under surveillance — encrypted communications, secure storage, sovereign hosting.
- **Ilm (عِلْم — Knowledge):** Curriculum and educational tooling — AI literacy for Muslim communities, madrasa integration, Imam-training on AI and its implications.
- **Adl (عَدْل — Justice):** Legal support infrastructure — template legal documents, connection to pro-bono networks, case documentation tooling for victims of AI-enabled abuse.

None of the above should block action on the four founding projects. Any of the above can be started by a sufficiently motivated contributor at any time.

---

## What Each Project is NOT

**Basira is NOT:**
- A replacement for qualified scholars. It is a tool for access, not for authority.
- A vehicle for one school of thought. It represents the spectrum.
- A fatwa-issuing body. It surfaces scholarly positions; it does not pronounce new ones.

**Shahid is NOT:**
- An advocacy organization. It is a documentation infrastructure.
- A political project. It archives facts; facts belong to no party.
- A replacement for existing human rights organizations. It complements them.

**Hisn is NOT:**
- A weapon. It is defensive. It does not offer offensive tools.
- A full-service security firm. It provides tooling and education; for acute individual cases, it coordinates with specialized organizations.
- Anonymous or unaccountable. Every recommendation is open and auditable.

**Mizan is NOT:**
- A platform-bashing exercise. It audits with rigor; platforms that behave well are credited as such.
- Partisan. Findings are empirical. Conclusions follow data.
- A replacement for platform internal trust and safety teams. It is an independent external check.

---

## Closing

Four projects. One mission. Infinite work ahead.

Each project can stand alone. Together they form an infrastructure. That infrastructure, over time, becomes the foundation for the Ummah's AI capacity.

Contributors may specialize in one. Contributors may move between them. Contributors may start projects we have not imagined.

All we ask is that the work serves the principles.

*Wa ma tawfiqi illa billah.*
