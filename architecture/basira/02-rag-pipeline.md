# Basira — RAG Pipeline

## Retrieval-Augmented Generation for Islamic Scholarly Content

This document specifies the end-to-end retrieval and generation pipeline for Basira. It assumes familiarity with `../03-data-architecture.md` (corpus structure) and `../02-tech-stack-decisions.md` (technology choices).

---

## Pipeline Overview

```
┌─────────────┐
│ User query  │
└──────┬──────┘
       │ (natural language, any supported language)
       ▼
┌─────────────────────────┐
│ 1. Query understanding  │
│   - language detection  │
│   - intent classification│
│   - Arabic normalization │
│   - query expansion     │
└──────┬──────────────────┘
       │
       ▼
┌─────────────────────────┐
│ 2. Retrieval (hybrid)   │
│   - BM25 keyword        │
│   - Dense embedding     │
│   - Metadata filter     │
│   - Multi-tradition     │
└──────┬──────────────────┘
       │ (candidate passages: 50-100)
       ▼
┌─────────────────────────┐
│ 3. Reranking            │
│   - cross-encoder scoring │
│   - diversity / source  │
│     balance             │
└──────┬──────────────────┘
       │ (top 8-15 passages with full citation)
       ▼
┌─────────────────────────┐
│ 4. Generation           │
│   - citation-enforced   │
│   - multi-school aware  │
│   - uncertainty marking │
│   - refusal policy      │
└──────┬──────────────────┘
       │ (grounded answer with inline citations)
       ▼
┌─────────────────────────┐
│ 5. Post-processing      │
│   - citation validation │
│   - safety filter       │
│   - formatting          │
└──────┬──────────────────┘
       │
       ▼
┌─────────────┐
│  Response   │
└─────────────┘
```

---

## Stage 1 — Query Understanding

### Language detection

Detect the query's language. Support code-switching (common in Urdu/English-speaking users).

- Primary detection via a fast classifier (e.g. fastText lid).
- Mixed-language handling: process the dominant language's script with Arabic terms recognized.

### Intent classification

Classify query type:

- **Lookup** — "What does Surah X verse Y say?"
- **Explanation** — "What does [concept] mean?"
- **Comparison** — "How do [schools] differ on [topic]?"
- **Research** — "Find all hadiths about [topic]."
- **Personal ruling** — "Is X permissible for me?" → triggers scholar-referral response.
- **Out-of-scope** — not Islamic scholarship → polite redirection.

Intent affects retrieval strategy and generation template.

### Arabic normalization

For Arabic or mixed-Arabic queries:

- Apply the standard Arabic normalization profile (see `../03-data-architecture.md`).
- Optionally strip / add diacritics based on query indication.
- Recognize transliterated Arabic terms in non-Arabic queries (e.g. "sabr" → `صبر`).

### Query expansion

For better recall:

- Add translated variants (if user asks in Urdu, also search in Arabic and English).
- Expand classical Arabic synonyms where they differ from modern usage.
- Include morphological variants of root words.

---

## Stage 2 — Retrieval (Hybrid)

### Hybrid retrieval strategy

Use **both** keyword (BM25) and dense embedding retrieval, merged.

**Why hybrid:**
- Dense retrieval captures semantic meaning.
- Keyword retrieval ensures exact-phrase and named-entity matches (critical for scholar citations, specific works, specific terms).
- Classical Arabic NLP benefits significantly from both signals.

### Implementation

- **BM25** via Postgres full-text search (`tsvector` + Arabic-aware configuration).
- **Dense** via pgvector with the configured embedding model.
- **Merge strategy:** Reciprocal Rank Fusion (RRF) for the initial candidate set.

### Filtering and scoping

Metadata filters applied per query:

- **Source type**: quran / hadith / tafsir / fiqh / etc.
- **Tradition**: sunni / shia / ibadi / shared (default: all).
- **School**: if user or institution has configured a default (e.g. "show Shafi'i first").
- **Language**: restrict to user's language for translations (original always included).
- **Grading**: for hadiths, optionally filter by minimum grading.
- **Scholar-verified**: for production content, only surfaces verified records.

### Multi-tradition default

Per Principle 1 and content policy:

- Default retrieval pulls from all traditions equitably — no systematic weighting toward one.
- When a query is clearly within one tradition (e.g. asks about "marja' taqlid"), that tradition is naturally favored by relevance — not by algorithmic bias.
- Tradition filters are opt-in.

### Retrieval output

Top N (tunable, typically 50–100) candidate passages, each with:

- Full citation record (source, locator, tradition, school, grading).
- Arabic text + relevant translations.
- Cross-references to related passages.
- Retrieval score breakdown.

---

## Stage 3 — Reranking

### Cross-encoder reranking

The initial candidate set is reranked using a cross-encoder model that scores query-passage pairs with higher accuracy than the bi-encoder used for initial retrieval.

**Model options:**
- Multilingual cross-encoder (BGE-reranker, Cohere rerank, or similar).
- [TENTATIVE] to be selected based on Arabic performance benchmarks.

### Diversity and source balance

Reranking not purely by score — also:

- **Source diversity**: avoid returning 10 passages from the same work when other works discuss the topic.
- **Tradition balance**: when multiple traditions have relevant content, ensure representation.
- **School balance**: when the topic has schools-of-thought variation, ensure representation.
- **Time diversity**: where relevant, balance classical and contemporary sources.

### Output

Top K passages (typically 8–15) that are:

- Highly relevant to the query.
- Diverse in source, tradition, and school.
- Full citation context attached.

---

## Stage 4 — Generation

### Generation model

Per ADR-005: Anthropic Claude as primary, Groq as fallback.

Model selection within Claude:
- **Claude Sonnet 4.6** — default for most queries.
- **Claude Haiku 4.5** — for simpler lookups where cost matters.
- **Claude Opus 4.7** — for complex scholarly comparisons (future consideration).

Self-hosted models considered in Phase 2+.

### Prompt architecture

The generation prompt has four sections, in order:

1. **System role** — defines Basira's identity, boundaries, and style.
2. **Principles frame** — explicitly includes non-negotiables (cite everything, represent plurality, uncertainty-aware, refusal patterns).
3. **Retrieved context** — the top K passages with full citation records.
4. **User query** — the question as received (normalized, intent-classified).

Each section is templated and versioned. Prompt changes go through evaluation gates (see below).

### Citation enforcement

The generation prompt requires:

- **Every scholarly claim** in the answer must cite a source from the retrieved context.
- **Citations formatted** consistently (e.g. `(Quran 2:183)`, `(Sahih al-Bukhari, Book 1, #1)`, `(Al-Kafi, vol. 1, p. 23)`).
- **No hallucinated citations** — if the model cannot ground a claim, it should say so rather than invent.

Post-generation validation checks every citation against the retrieval set. Citations that don't match trigger regeneration with corrective feedback.

### Multi-school representation

When sources from multiple schools are retrieved:

- The model surfaces distinct positions explicitly.
- Each position attributed to the schools that hold it.
- Neutral framing — no "correct" vs "incorrect" school language.
- When user has a configured school preference, that preference's position is presented first but not exclusively.

### Uncertainty marking

When sources are thin, the query touches contested areas, or the model's confidence is low:

- The answer explicitly marks uncertainty.
- Recommends sources for deeper reading.
- For personal questions, refers the user to qualified scholars.

### Refusal policy

The model refuses to:

- Issue fatawa.
- Provide personalized rulings on consequential matters.
- Produce content violating `../../docs/09-content-policy.md`.
- Help identify, track, or target individuals.
- Produce content that violates non-negotiables.

Refusals include brief explanation and, where possible, alternative help.

---

## Stage 5 — Post-processing

### Citation validation

- Every citation in the output is checked against the retrieval context.
- Format standardized for display.
- Clickable links generated (citation → primary source viewer).

### Safety filtering

- Content policy check (final line of defense).
- Prompt-injection pattern detection on output.
- Length and formatting sanity checks.

### Formatting

- Structured output with headings where appropriate.
- Inline citations rendered as links.
- Arabic text properly marked with `dir="rtl"` where present.
- Translations attributed inline.

---

## Multi-turn Conversations

Basira supports conversational follow-up:

- Context of previous turns retained for the session.
- Retrieval can be refined based on conversational clarification.
- Citations from previous turns remain available.
- Session context is client-authoritative — user can clear it at any time.

---

## Evaluation Framework

Per Principle 6 (Excellence) and NN-9 (no falsehood):

### Benchmark suites

**Correctness benchmark:**
- Curated set of questions with known-correct answers and expected citations.
- Tests that the model cites the right sources for established scholarly questions.
- Scored automatically; reviewed by scholars periodically.

**Hallucination benchmark:**
- Questions designed to tempt hallucination (obscure topics, nonexistent references).
- Measures rate of invented citations or false attributions.
- Target: zero hallucinated citations in production.

**Bias benchmark:**
- Questions touching inter-school disagreement.
- Measures whether the model systematically favors one school.
- Target: balanced representation within a small tolerance.

**Refusal benchmark:**
- Questions testing refusal patterns (fatwa requests, targeting requests, out-of-scope).
- Measures whether refusals are appropriate (neither over- nor under-restrictive).

**Multilingual benchmark:**
- Same questions across supported languages.
- Measures consistency of quality.

### Gates

- **Every prompt change** must pass the evaluation suite before deployment.
- **Every model change** (version update, provider change) must pass.
- **Regression detection** — if any benchmark score drops significantly, deployment blocked pending review.

### Published benchmarks

Per Layer 1 disclosure, the benchmark itself is public. Scores are published periodically. This lets other Muslim AI developers use our benchmark — and lets third parties audit our claims.

---

## Latency Targets

Representative targets for MVP (tunable as we measure):

- **Simple lookup** (Quran/Hadith direct reference): under 2 seconds.
- **Standard scholarly question**: under 5 seconds.
- **Complex comparison / research query**: under 10 seconds.
- **Streaming**: response streaming starts under 1 second.

Optimization priorities (in order): correctness, safety, latency. We do not trade correctness for latency.

---

## Caching

Where caching is possible without compromising privacy:

- **Retrieval results** cached at passage level (cache key is normalized query; not user-specific).
- **Generation not cached** by default (different users ask same questions in different contexts; also a privacy concern).
- **Per-user session cache** on-device, not server-side, for high-threat users.

Caching is never applied to Layer 4 data.

---

## Observability

- Retrieval latency, rerank latency, generation latency, total latency — tracked per query.
- Retrieval quality metrics (candidate recall, citation match rate).
- Generation quality metrics (citation validity rate, refusal rate, length distribution).
- No query text stored with these metrics — aggregate only.

Dashboards available to operators for debugging; raw metrics never contain user-identifying data.

---

## Failure Modes and Fallbacks

### Retrieval returns nothing

- Acknowledge inability to find relevant sources.
- Suggest query reformulation.
- For structured queries (looking up specific verse/hadith), fall back to direct lookup.

### LLM provider unavailable

- Automatic fallback to secondary provider (Groq).
- If all providers down: user notified; query saved for later if user opts in.

### Citation validation fails

- Regeneration with corrective prompt.
- If repeatedly fails: answer withheld; user notified to retry or rephrase.

### Safety filter triggers

- Request refused with explanation.
- Logged for pattern analysis (without user-identifying content).

---

## Cross-References

- Scholar verification: `03-scholar-verification.md`
- MVP scope: `04-mvp-scope.md`
- Data architecture: `../03-data-architecture.md`
- Security: `../10-security-architecture.md`
- Content policy: `../../docs/09-content-policy.md`
