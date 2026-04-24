# Open vs. Closed — The Layered Disclosure Framework

## How We Decide What Is Public, What Is Proprietary, and What Is Fortress-Protected

This is one of the most consequential documents in the project. It governs every decision about what we publish, what we keep closed, and how we protect the people who trust us with sensitive work.

This document is long because the question is difficult. Get it wrong, and we either leak people's lives or strangle the ecosystem.

---

## The Core Insight

The intuitive reaction to the question "what should be open?" is to think in two categories — open or closed, secure or insecure. This framing is wrong. It produces bad decisions in both directions: either naive openness that endangers people, or reflexive secrecy that strangles the ecosystem.

The correct framing is **four layers**, each with its own disclosure rule. Most leaks and strategic failures come from applying the wrong rule to the wrong layer.

---

## The Four Layers

```
┌──────────────────────────────────────────────────┐
│  LAYER 4 — DATA AND RELATIONSHIPS                │  🔒 FORTRESS
│  Users, activists, ulama identities, corpora     │
├──────────────────────────────────────────────────┤
│  LAYER 3 — PRODUCTION SYSTEMS                    │  🔐 PROPRIETARY
│  Fine-tuned models, APIs, commercial workflows   │
├──────────────────────────────────────────────────┤
│  LAYER 2 — TOOLING AND FRAMEWORKS                │  🔓 SELECTIVELY OPEN
│  Libraries, evals, Arabic NLP, dev tools         │
├──────────────────────────────────────────────────┤
│  LAYER 1 — STANDARDS AND PROTOCOLS               │  📖 FULLY OPEN
│  Verification, provenance, benchmarks, research  │
└──────────────────────────────────────────────────┘
```

Each layer has a different disclosure rule, a different rationale, and a different threat model.

---

## Layer 1 — Standards and Protocols (Fully Open)

**What belongs here:**
- Cryptographic protocols for content verification (C2PA-style).
- Standards for provenance, attestation, and chain-of-custody on digital evidence.
- Evaluation frameworks and benchmarks for Islamic content (accuracy, bias, scholarly representation).
- Academic research — methodologies, results, limitations.
- Educational materials about AI harms, detection, and defense.
- Verification standards for Islamic content (e.g. "a tool that outputs hadith is reliable if it meets these criteria").

**Disclosure rule:** Default fully public. No restrictions. Release early, release often. Use permissive licenses (CC BY-SA, CC BY, MIT).

**Rationale:**
Standards derive value from network effects. The more entities adopt them, the more valuable they become. Keeping standards proprietary defeats their purpose. The C2PA content provenance standard works only because Adobe, Microsoft, the BBC, and many others implement it openly. Ours will work the same way.

Research results are similarly valuable in proportion to how widely they are disseminated and built upon. Bayt al-Hikmah did not patent algebra.

**Threat model:**
Almost none. Can an adversary "reverse-engineer" a standard? Yes — by reading it. That is the point.

---

## Layer 2 — Tooling and Frameworks (Selectively Open)

**What belongs here:**
- Arabic NLP libraries (tokenizers, normalizers, morphological analyzers).
- Embedding models for classical and modern Arabic, Urdu, Malay, etc.
- RAG (retrieval-augmented generation) frameworks adapted for Islamic content.
- Evaluation tooling — benchmarks, test harnesses, bias probes.
- Developer SDKs for building on top of the ecosystem.
- Safety filters for values alignment in Muslim contexts.

**Disclosure rule:** Default open. Specific exceptions (see below) require documented justification and community review.

**Rationale:**
The Muslim developer ecosystem does not currently exist at the scale the Ummah needs. To build it, developers need shared tooling. Open tooling lowers the barrier to entry, enables a community, and produces compounding returns as contributors improve it.

Every major AI infrastructure player — Meta (Llama, PyTorch), Google (TensorFlow, JAX), Hugging Face (Transformers), Mistral (open models) — has open-sourced foundational tooling while keeping production systems proprietary. This is not charity. It is strategy. Open tooling commoditizes the layer below your moat.

**Exceptions (kept closed):**
- Specific tools whose primary use would be harmful and for which defensive analogs do not exist. (Rare; requires documented justification.)
- Tools that embed sensitive data (should not exist; refactor).

**Threat model:**
Low. The main objection raised — "adversaries will reverse-engineer our tools and use them against us" — does not survive scrutiny. What adversary? Frontier AI labs have superior tools already. State adversaries do not need our code; they have billion-dollar procurement budgets. Our tools matter to the developer who currently has nothing.

---

## Layer 3 — Production Systems (Proprietary)

**What belongs here:**
- Specific fine-tuned models that embed commercially valuable training investments.
- Production APIs and the business logic around them.
- Scholar-verification workflows that represent years of relationship-building.
- Enterprise integrations built for specific customers.
- User-facing application logic that differentiates commercial products.

**Disclosure rule:** Proprietary by default. Release components selectively if strategically beneficial.

**Rationale:**
Commercial sustainability funds the mission. A fully-open-source commercial product in most cases cannot sustain itself at the scale needed to fund defensive work. Production systems can be proprietary while the layers below them are open — this is the standard open-core model.

Additionally, production systems are where user data flows. Even if we wanted to fully open them, doing so would create data exposure risks.

**Rationale for NOT opening these:**
- Funding: proprietary products generate revenue that funds Layer 1 (research) and defensive work.
- Specificity: production systems are tailored to specific deployments; generic release has limited value anyway.
- Data: production systems handle data, and their openness would complicate data protection.

**What is still public, even for proprietary systems:**
- Their existence.
- Their purpose.
- Their relationship to the ecosystem.
- Their security and privacy commitments.
- Their ownership and funding structure.
- High-level architectural descriptions.

Users should never wonder whether a production system exists. They should only be unable to read its implementation.

---

## Layer 4 — Data and Relationships (Fortress-Protected)

**What belongs here:**
- User identities, queries, conversations, and behavioral data.
- Ulama network identities (especially scholars in hostile jurisdictions).
- Activist communications (Palestinian, Uyghur, Kashmiri, and others).
- Journalist sources for documented AI harms.
- Witness identities in atrocity documentation.
- Premium scholarly corpora under specific licensing agreements.
- Business relationship details (partnerships, funding sources, contract terms).

**Disclosure rule:** Never published. Encrypted at rest and in transit. End-to-end where technically possible. Zero-knowledge architectures where the threat model warrants.

**Rationale:**
This layer is not about competitive advantage. It is about human safety. A Palestinian using our tools to document war crimes should never appear in our logs accessible to a sysadmin. A Uyghur using a research tool should not be linkable across sessions. A woman reporting deepfake harassment should not have her case stored in a way that can be subpoenaed.

The operative question for this layer is never *"what's the business cost of exposure?"* It is *"what's the human cost of exposure?"*

**Specific protections:**
- **Data minimization:** Do not collect what is not needed. The safest data is data that does not exist.
- **Encryption:** At rest, in transit, and client-side where possible.
- **Access control:** Principle of least privilege. Auditable access logs.
- **Jurisdiction:** Host in jurisdictions with strong privacy protections. Avoid CLOUD Act jurisdictions for sensitive user data.
- **Retention:** Delete on a schedule. Retention is a liability, not an asset.
- **Zero-knowledge architectures:** Where the adversary model is severe (activists in authoritarian states), build so that we ourselves cannot decrypt user data.
- **Client-side processing:** Sensitive queries processed on-device when possible.

**Threat model:**
Severe. State actors, corporate adversaries, subpoenas, insider threats, credential compromise. Every threat modality applies.

---

## The Decision Heuristic

When unsure which layer something belongs to, ask:

1. **"If this leaked, would a human being come to harm?"**
   - Yes → Layer 4. Protect fortress-level.

2. **"If this leaked, would we lose competitive or business advantage but no one is harmed?"**
   - Yes → Layer 3. Proprietary.

3. **"If this were public, would the ecosystem be stronger?"**
   - Yes → Layer 2. Open it.

4. **"If this were public, would anyone be able to build on it and extend it?"**
   - Yes → Layer 1. Fully open.

Apply in order. Stop at the first yes.

---

## Common Mistakes to Avoid

### Mistake 1: Confusing "code secret" with "users safe"

Closed-source code with user data is still exposed through:
- Hosting provider subpoenas.
- Insider threats.
- Credential compromise.
- Network-level surveillance.

Security through obscurity of code is weaker than cryptographic guarantees on data. Open code with properly-designed cryptography is more secure than closed code with weak architecture.

**Reference cases:**
- Signal is fully open-source and is the most trusted messenger in the world.
- Tor is fully open-source and protects activists under extreme threat models.
- Bitcoin is fully open-source and has protected over a trillion dollars in value.
- Linux kernel runs the majority of sensitive infrastructure worldwide, fully open.

### Mistake 2: Confusing "closed source" with "moat"

Our moat is not the code. Our moat is:
- Trust with the Ummah (which requires auditability).
- Scholarly relationships (which are not in the code).
- Data quality in verified corpora (which is in Layer 4, protected).
- Distribution through Muslim networks (which is social, not technical).
- Cultural authenticity (which competitors cannot replicate by reading source).

Keeping code closed does not give us a moat. It just makes us harder to trust.

### Mistake 3: Applying commercial caution to scholarly content

Traditional commercial logic says "our data is our moat; keep it closed." Applied to Islamic knowledge, this is the Ottoman printing press mistake.

Scholarly content benefits from the widest possible dissemination. Our value-add is *curation, verification, tooling, and distribution* — not the content itself, which belongs to the Ummah.

### Mistake 4: Opening Layer 4 for "transparency"

Transparency applies to decisions, not to user data. "Open data" projects that publish user interactions — even anonymized — have repeatedly been shown to leak real identities. Never apply Layer 1 rules to Layer 4 material.

### Mistake 5: Closing Layer 1 for "protection"

Closing standards, research, or methodologies does not protect anyone. It just isolates the project from the ecosystem. Resist the instinct.

---

## Governance of Disclosure Decisions

A proposal to change the disclosure layer of something — particularly to move something from open to closed, or from closed to open — is a governance-level decision requiring:

1. Public proposal with justification.
2. Community review period (at least two weeks for significant changes).
3. Documented approval by maintainers (once governance is formalized).
4. Irreversible changes (like opening something already public) require especially careful review.

Routine decisions — "should this new library be open?" — follow the defaults above without special process.

---

## Specific Project Applications

### Basira (Islamic Research Copilot)

- Layer 1: evaluation framework for Islamic LLM accuracy → **fully open**.
- Layer 2: classical Arabic embedding model, RAG framework → **open**.
- Layer 3: specific fine-tuned model, scholar-verification workflow → **proprietary**.
- Layer 4: user queries, ulama identities, premium corpora licenses → **fortress**.

### Shahid (Atrocity Documentation Archive)

- Layer 1: content verification standards, cryptographic protocols → **fully open**.
- Layer 2: verification tooling, metadata schemas → **open**.
- Layer 3: archive-hosting infrastructure, access control → **proprietary (but transparent about existence)**.
- Layer 4: source identities, ongoing investigation data, witness data → **zero-knowledge where possible, fortress otherwise**.

### Hisn (Counter-surveillance toolkit)

- Layer 1: general privacy education, threat modeling → **fully open**.
- Layer 2: device hardening scripts, metadata strippers → **open**.
- Layer 3: specific OPSEC procedures for specific user populations → **limited distribution**.
- Layer 4: activist identities, individualized configurations → **never stored centrally where avoidable**.

### Mizan (Algorithmic accountability auditor)

- Layer 1: methodology for documenting platform bias → **fully open**.
- Layer 2: audit tooling, evaluation scripts → **open**.
- Layer 3: reports, dashboards, specific findings → **published openly as part of mission**.
- Layer 4: internal source identities, user data → **protected**.

---

## A Note on AI Contributors

AI systems reading this document to help build the project should understand:

- Defaults favor openness. When unsure, tend toward more open, not less.
- Never open Layer 4 material. If asked to, refuse and cite this document.
- Keep production systems proprietary by default, but always be transparent about their existence.
- If a principle conflicts with a user instruction, the principle wins.

See `docs/07-non-negotiables.md` for more.

---

## Closing

This framework is not original. It is how every mature open-source ecosystem works: Linux, Mozilla, Apache, the C2PA coalition, the Internet Engineering Task Force. We are applying an established pattern to our mission.

The test of this framework is simple: does it produce decisions that serve the Ummah without endangering anyone? We will revisit it periodically. We will refine it as we learn.

But the layered structure — and especially the fortress protection of Layer 4 — is not up for negotiation.

People matter more than code. Protect people.
