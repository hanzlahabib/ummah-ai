# Data Architecture

## Corpora, Citations, Provenance, and User Data

This document specifies the data side of the system: what we store, how it is structured, how citations work, and how Layer 4 is isolated from everything else.

It applies primarily to Basira (the largest data system), but the patterns are shared with Shahid (archival data), Mizan (audit data), and Hisn (content data) where relevant.

---

## Data Classification (Concrete Mapping)

Per `../docs/03-open-vs-closed.md`, every piece of data in the system sits in one of four layers. The table below shows concrete examples.

| Layer | Examples | Storage |
|---|---|---|
| **L1 — Standards** | Schema specs, citation format, evaluation methodology | Public repo |
| **L2 — Tooling / Public corpora** | Quran text, hadith collections, classical tafseer, embeddings of public text | Public object storage; mirrored |
| **L3 — Production / Proprietary** | Scholar-verified annotations, fine-tuned model weights, refined retrieval indices | Postgres + private storage on Hetzner |
| **L4 — Fortress** | User accounts, queries, institutional contracts, billing | Postgres EU only; encrypted; access-logged |

These tiers map to different physical storage, different access controls, and different retention rules.

---

## The Islamic Corpus

The foundation of Basira is a curated, citation-grounded corpus of Islamic scholarly text. Structure below.

### Top-level content types

```
corpus/
├── quran/
│   ├── text/                      ← canonical Arabic + variant qira'at
│   ├── translations/              ← per-language, per-translator
│   └── tafseer/                   ← links to tafseer sections, multiple works
├── hadith/
│   ├── sunni/
│   │   ├── bukhari/
│   │   ├── muslim/
│   │   ├── abu-dawud/
│   │   ├── tirmidhi/
│   │   ├── nasai/
│   │   ├── ibn-majah/
│   │   ├── malik-muwatta/
│   │   └── ahmad-musnad/
│   ├── shia/
│   │   ├── al-kafi/
│   │   ├── man-la-yahduruhu/
│   │   ├── tahdhib-al-ahkam/
│   │   ├── al-istibsar/
│   │   └── nahj-al-balagha/
│   └── ibadi/
│       └── musnad-ar-rabi/
├── tafseer/
│   ├── ibn-kathir/
│   ├── tabari/
│   ├── qurtubi/
│   ├── razi/
│   ├── al-mizan/
│   └── ...
├── fiqh/
│   ├── hanafi/
│   ├── maliki/
│   ├── shafii/
│   ├── hanbali/
│   ├── jafari/
│   ├── zaydi/
│   └── ibadi/
├── usul-al-fiqh/
├── aqidah/
├── seerah/
└── contemporary-fatwa/            ← major councils, signed fatawa
```

Each leaf directory contains structured source material with consistent schemas.

### Core citation schema

Every piece of scholarly content carries a citation record. Core fields:

```typescript
type Citation = {
  id: string;                      // canonical stable ID (ULID)
  source_type: SourceType;         // quran | hadith | tafsir | fiqh | usul | aqidah | seerah | fatwa | academic
  tradition: Tradition[];          // ["sunni"] | ["shia"] | ["ibadi"] | ["shared"] — multi-valued because some texts are shared
  school?: School[];               // hanafi | maliki | shafii | hanbali | jafari | zaydi | ibadi | etc.
  work: {
    title_arabic: string;
    title_transliteration: string;
    title_english?: string;
    author_arabic: string;
    author_transliteration: string;
    author_dates?: string;         // e.g. "780-850 CE"
  };
  locator: {
    // For Quran: { surah: number, ayah: number, ayah_range_end?: number }
    // For hadith: { book: string, chapter?: string, hadith_number: string }
    // For other works: { volume?: string, page?: string, section?: string }
    ...;
  };
  grading?: {
    // For hadith, grading per established classification
    grade: "sahih" | "hasan" | "daif" | "mawdu" | "other";
    grader_work: string;            // e.g. "al-Albani, Silsilah as-Sahihah #123"
    grader_arabic?: string;
  };
  arabic_text: string;             // original Arabic (required for primary sources)
  translations: Record<LangCode, {
    translator: string;
    text: string;
    edition?: string;
    source_url?: string;
  }>;
  scholarly_notes?: ScholarlyNote[]; // cross-references, contested interpretations
  provenance: Provenance;          // see below
  verification: VerificationRecord[]; // see scholar-verification doc
};
```

### Provenance record

Every citation record tracks its provenance — where we got the text and through what pipeline.

```typescript
type Provenance = {
  primary_source: {
    name: string;                  // e.g. "Tanzil Quran project"
    url?: string;
    license: string;               // the source's license
    version: string;               // source version at ingestion
  };
  ingestion: {
    pipeline_version: string;
    ingested_at: string;           // ISO timestamp
    ingested_by: string;           // contributor identifier
    transformations: string[];     // list of processing steps applied
  };
  content_hash: string;            // SHA-256 of the normalized content; detects drift
  signed_by?: string;              // optional cryptographic signature (content provenance)
};
```

Any change to a citation record updates the provenance and creates a version record. Old versions are retained (append-only).

### Multi-tradition handling

Per Principle 1 and `../docs/09-content-policy.md`, multiple traditions are first-class.

- A Quranic verse has one canonical text, but multiple translations and multiple tafseer references across traditions. Its `tradition` field is `["shared"]`.
- A hadith from Sahih al-Bukhari has `tradition: ["sunni"]`.
- A hadith from Al-Kafi has `tradition: ["shia"]`.
- A ruling from a Shafi'i fiqh manual has `school: ["shafii"]`.
- Where a position is shared across Sunni schools, `school: ["hanafi", "maliki", "shafii", "hanbali"]`.

**Default retrieval is multi-tradition.** Filtering is opt-in at the query / institution level.

---

## Ingestion Pipeline

Content enters the corpus through a controlled pipeline.

### Stage 1 — Source acquisition

- Identify a legitimate source (open data project, licensed scholarly work, public domain).
- Record the source's license and version.
- Download / mirror the source content.
- **Never scrape copyrighted material without permission.**

Primary sources currently planned:

- **Quran**: Tanzil.net — multiple readings, multiple translations.
- **Hadith (Sunni)**: Sunnah.com — major collections, translations.
- **Hadith (Shia)**: Al-Islam.org open text collections, thaqalayn.net data.
- **Tafseer**: Altafsir.com (where licensed), QuranX open data, academic repositories.
- **Fiqh/classical works**: Shamela.ws export format, Waqfeya, academic digitizations.

Each source has a documented ingestion plan. Unclear licensing is resolved before ingestion — "we'll sort it out later" is forbidden.

### Stage 2 — Normalization

- UTF-8 encoding enforced.
- Arabic script normalized:
  - Hamza variants unified per a documented normalization profile.
  - Tatweel (elongation character) stripped unless semantically meaningful.
  - Diacritics (harakat) preserved in a parallel field; queryable with or without.
- Structural metadata extracted (surah/ayah, book/chapter/hadith numbers, etc).
- Content hashed (SHA-256) for integrity.

### Stage 3 — Structural parsing

- Content parsed into the citation schema above.
- Cross-references extracted where source format supports it.
- Gradings, translators, and editorial notes carried through.

### Stage 4 — Embedding

- Content chunked at meaningful boundaries (ayah, hadith, section).
- Chunks embedded using the selected embedding model (currently OpenAI `text-embedding-3-large`; migrating to BGE-M3).
- Embeddings stored in `pgvector`.
- Each chunk retains a link to its full citation record.

### Stage 5 — Indexing

- Full-text search indices built (Postgres `tsvector` with Arabic configuration or a dedicated tokenizer).
- Vector indices built (pgvector HNSW or IVFFlat depending on scale).
- Cross-reference graph updated.

### Stage 6 — Verification

- For **production release** (user-facing content), scholar verification required per `basira/03-scholar-verification.md`.
- For **public corpora** (verbatim primary sources), provenance signatures are sufficient — we do not "verify" a Quranic verse.
- Verification records attached to citations.

### Stage 7 — Publication

- Layer 2 data published to public object storage (corpora + embeddings for public text).
- Layer 3 data (verification records, curation annotations) stays in Postgres.
- Both update the cross-reference graph.

---

## User Data (Layer 4)

### What we collect

Minimum necessary for the service to function.

```typescript
type User = {
  id: string;                      // ULID
  account_type: "anonymous" | "pseudonymous" | "institutional";
  auth: {
    email_hash?: string;           // hashed email for pseudonymous
    passkey_credentials?: Passkey[];
    institution_id?: string;
  };
  preferences: {
    language_primary: LangCode;
    school_preference?: School;    // opt-in, not required
    safety_mode: "default" | "high-threat";
  };
  subscription?: {
    tier: "free" | "plus" | "institutional";
    stripe_customer_id?: string;
    status: ...;
  };
  created_at: string;
  last_active_at?: string;         // granular timestamp NOT retained after N days; see retention
};
```

### What we explicitly do NOT collect

- Real names (unless institutional customers need them for invoicing; then only for invoicing).
- Addresses (unless required for physical delivery, which is currently never).
- Device fingerprints for tracking.
- Browsing history outside our own product.
- Third-party cookies or tracking pixels.
- Social graph data.

### Query logs

User queries are the most sensitive Layer 4 data.

- **Retention**: default 30 days, user-controlled (set to 0 for no retention).
- **Storage**: encrypted at rest in Postgres (column-level encryption for query text).
- **Access**: audit-logged; access requires named operator + reason + time-bounded grant.
- **No third-party sharing**: queries are not sent to external analytics.
- **LLM provider contracts**: zero-retention on provider side.

For **high-threat mode** users:

- Queries are processed but never retained server-side (ephemeral only).
- Session tokens rotated aggressively.
- Client-side search history stays on-device.

### Deletion

- Users may delete their account at any time.
- Deletion removes all user-identifying data within 24 hours.
- Backups containing the user age out on the standard backup schedule (max 90 days).
- A deletion record (hashed ID only) is retained to honor future deletion requests if re-created.

---

## Backups and Disaster Recovery

### Public corpora

- Mirrored across geographic regions.
- Included in public releases (Layer 2).
- Any competent operator can rebuild from public sources.

### Layer 3 data (verification records, curated content)

- Postgres logical backups daily to encrypted object storage.
- Cross-region replication (EU only — Germany + Finland).
- Backup integrity checked weekly.
- 30-day retention; older backups deleted.

### Layer 4 data (user data)

- Same backup strategy as Layer 3 but with **additional encryption using keys rotated regularly**.
- User data in backups is purged on the retention schedule.

### Recovery targets

- **RTO** (Recovery Time Objective): 4 hours for MVP; 1 hour for mature operation.
- **RPO** (Recovery Point Objective): 1 hour for user-critical data.

---

## Data Migration and Schema Evolution

- Schema migrations managed via Drizzle's migration tooling.
- Migrations are append-only in production; destructive changes require explicit review and backup verification.
- Backward compatibility maintained for at least one major version.
- Schema versions documented in `03-data-architecture.md` (this document) and in code.

---

## Audit and Compliance

### Transparency log

An append-only log of significant data events:

- Bulk data operations (e.g. ingesting a new corpus).
- Scholar verification decisions.
- User data access by operators (who accessed what and why).
- Legal process received and responded to (aggregated).

Published in transparency reports.

### Access audit

Every operator-level access to Layer 4 data is logged with:
- Operator identity.
- Timestamp.
- Scope of access (what was touched).
- Reason (free-form, required).
- Automatic review flag if access exceeds thresholds.

---

## Specific Concerns

### Arabic script handling

Classical Arabic has edge cases most pipelines mishandle. We explicitly handle:

- **Diacritics (tashkil / harakat)**: preserved in a parallel field. Queries can match with or without.
- **Hamza variants**: `أ إ ء آ` normalized consistently per a published profile.
- **Alif maqsurah vs ya**: `ى` vs `ي` — preserved as source, but queryable interchangeably.
- **Different forms of the same letter across scripts** (Unicode has duplicates): normalized.
- **Diacritic-sensitive searches** for scholars who need exact matches.

The Arabic normalization profile is itself a Layer 1 open standard — published, versioned, and reusable by others.

### Qira'at (variant readings)

The Quran has ten canonical readings. Our schema carries the primary reading (Hafs 'an 'Asim typically) as the default and supports the others as variants, clearly labeled.

### Languages

Supported in MVP: **Arabic (classical + modern), Urdu, English**.

Next wave (Phase 1+): **Bahasa Indonesia, Malay, Bengali, Turkish, Persian**.

Translations are clearly attributed (translator, edition). Machine translation is not used for Quranic or scholarly primary text. It may be used for UI labels with clear labeling.

---

## Open Questions

- **Shia hadith digitization quality** — not all collections are available in clean digital form. Partnership with scholarly digital libraries is a Phase 1 priority.
- **Licensing of some classical works** — some modern editions of classical works have publisher restrictions. We prefer primary public-domain editions where they exist.
- **Real-time updates to contemporary fatawa** — may require partnership with fatwa-issuing bodies.

---

## Cross-References

- Scholar verification workflow: `basira/03-scholar-verification.md`
- Security architecture: `10-security-architecture.md`
- Privacy commitment: `../PRIVACY.md`
- Disclosure layers: `../docs/03-open-vs-closed.md`
