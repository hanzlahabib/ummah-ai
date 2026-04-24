# Shahid — Architectural Sketch

## شَاهِد — Witness

Cryptographically-verified permanent archive of AI-driven harms against civilians. Detailed architecture comes when Phase 2 begins; this document captures the initial design principles and sketches the system at a high level.

---

## Status

**Current phase**: design sketch only. Implementation targeted for Phase 2 (post-Basira MVP).

This document is deliberately short — the full architecture will be written when we start building, so that the design reflects what we learn from Basira.

---

## Mission Recap

Per `../../docs/04-projects.md`:

Build and maintain a permanent, cryptographically-verified archive of AI-driven harms against civilians, accessible to journalists, researchers, lawyers, and the public. Complements existing human rights organizations by providing infrastructure specific to AI-enabled harms.

---

## Core Architectural Requirements

### Permanence

- Multi-jurisdiction replication.
- Append-only data store (tamper-evident).
- Independent from our organization's continued existence — if the initiative fails, the archive survives through mirrors.
- Cryptographic signatures on all entries.

### Verification

- Every entry carries provenance — where it came from, who submitted, what verification was performed.
- **C2PA-style content provenance** for media (images, video, audio).
- **Chain-of-custody** records for documents and digital artifacts.
- **Multi-source corroboration** flagged where available.

### Protection of sources

- **Source identities never public** unless explicit source consent.
- **Zero-knowledge intake** for high-risk submissions — submission possible without the archive ever learning submitter identity.
- **Encrypted submissions** end-to-end where the threat model warrants.
- **Separation** between public entry and private source metadata.

### Accessibility

- **Public search interface** for researchers, journalists, lawyers, students.
- **Programmatic API** for researchers doing bulk analysis.
- **Standard citation formats** (BibTeX, legal citation forms) for academic and legal use.
- **Permalinks** that persist.

### Integration with others

- Partnership with existing human rights orgs (HRW, Amnesty, 7amleh, Forensic Architecture, Bellingcat).
- Don't duplicate — complement.
- Shared standards where possible.

---

## High-Level Architecture Sketch

```
┌─────────────────────────────────────────────────────────┐
│  INTAKE LAYER                                           │
│   • Public web form (verified submitters)               │
│   • Encrypted tip line (E2E)                            │
│   • Journalist API (partners with creds)                │
│   • Research integration (academic partners)            │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│  VERIFICATION LAYER                                     │
│   • Provenance checks                                   │
│   • Metadata analysis                                   │
│   • Corroboration checks (other records)                │
│   • Human review for substantive entries                │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│  ARCHIVE LAYER (append-only)                            │
│   • Entries: content hash + signature + metadata        │
│   • Replicated: primary EU + 2 additional regions       │
│   • Immutable history                                   │
│   • Public blockchain anchor (timestamping)             │
└────────────────────┬────────────────────────────────────┘
                     │
           ┌─────────┴─────────┐
           ▼                   ▼
┌────────────────────┐  ┌──────────────────────────┐
│ PUBLIC INDEX       │  │ PROTECTED INDEX          │
│ Entry searchable   │  │ Source identities,       │
│ w/o source info    │  │ under-investigation      │
│                    │  │ details, witness data    │
│                    │  │ (zero-knowledge /        │
│                    │  │  access-controlled)      │
└────────────────────┘  └──────────────────────────┘
           │
           ▼
┌─────────────────────────────────────────────────────────┐
│  ACCESS LAYER                                           │
│   • Public search + browse                              │
│   • Researcher API (credentialed)                       │
│   • Legal-process interface (limited, audited)          │
│   • Data export (with source protection)                │
└─────────────────────────────────────────────────────────┘
```

---

## Data Model Sketch

### Archive entry

```typescript
type ArchiveEntry = {
  id: string;                      // ULID
  type: EntryType;                 // targeting / surveillance / deepfake / censorship / ...
  subject_category: SubjectCategory; // civilians / journalists / activists / minorities / ...
  geography: string[];             // locations involved
  time_period: { start: string; end?: string };
  description_public: string;      // the public summary
  description_detailed_access: string; // access-controlled, with sensitive context
  artifacts: Artifact[];           // evidence attached (media, docs)
  corroborations: Corroboration[]; // other entries, external sources
  provenance: Provenance;          // who submitted, when, verification level
  signatures: Signature[];         // cryptographic
  timestamps: {
    submitted_at: string;
    verified_at?: string;
    published_at?: string;
    anchor_hash?: string;           // blockchain anchor for tamper-evidence
  };
  source_id_encrypted?: string;    // only accessible via source's key / upon source consent
};
```

### Artifact

Media or documents attached to an entry, with C2PA provenance where available.

### Corroboration

Links to other entries or external sources that support or contextualize this one.

---

## Tech Stack (Tentative)

Reuses the common stack where possible:

- **Postgres** for the archive index.
- **Object storage** (self-hosted on Hetzner with geographic redundancy) for media/document artifacts.
- **Client-side encryption** for high-sensitivity submissions.
- **Public blockchain anchor** for timestamping — e.g. OpenTimestamps (Bitcoin-anchored) or equivalent, not for the data itself but for tamper-evident proof of existence at time T.
- **Tor hidden service** for intake (submitters under threat).
- **Standard web interface** for research/public access.

Specific implementation decisions deferred to when work begins.

---

## Relationship to Other Projects

- **Basira** and **Shahid** share the common auth, infrastructure, and observability stacks.
- **Shahid's public corpus** may become searchable via Basira for research queries — with source protections intact.
- **Hisn** may refer at-risk users to Shahid for both protection guidance and (if they wish) witness-style documentation.
- **Mizan** uses Shahid's archive as primary evidence for pattern-level accountability reports.

---

## Open Questions (to resolve before build)

- **Blockchain anchor vs. distributed timestamp authority** — which gives the strongest tamper-evidence without introducing coin-market volatility or other complications?
- **Legal structure for archive permanence** — is a separate entity for Shahid (distinct from Basira's commercial entity) better for its legal durability?
- **Zero-knowledge architecture specifics** — what cryptographic primitives (e.g. Signal-protocol sealed senders, oblivious intake) suit the threat model?
- **Verification bar** — what level of corroboration is required before an entry is published? Too low = noisy; too high = silences witnesses.
- **Relationship with prosecutorial use** — if an entry might be used as legal evidence, what chain-of-custody standards apply?

---

## Guiding Principles Specific to Shahid

- **Witnesses first.** Everything secondary to protecting witnesses.
- **Permanence is the mission.** An archive that disappears failed its reason for existing.
- **Accuracy over speed.** Publishing a verified entry in two weeks is better than publishing a disputed entry in two days.
- **Neutrality in framing.** Facts, not editorial.
- **Legitimacy through openness** of methodology, not hidden decisions.

---

## Cross-References

- Project description: `../../docs/04-projects.md#project-2--shahid`
- Disclosure framework: `../../docs/03-open-vs-closed.md#shahid-atrocity-documentation-archive`
- Threat model: `../../docs/08-project-threat-model.md`
- Content policy considerations: `../../docs/09-content-policy.md`

---

Full architecture document will be written when Phase 2 implementation begins. This sketch captures the direction; details follow the work.
