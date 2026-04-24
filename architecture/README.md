# Architecture

## How the Initiative Is Built

This directory translates the principles, strategy, and project portfolio from the foundation documents (`../docs/`) into concrete architectural decisions — technology choices, data structures, system boundaries, and implementation plans.

Foundation documents say **what** and **why**. Architecture documents say **how**.

---

## Relationship to Foundation

Every architectural decision in this directory is ultimately justified by reference to the foundation documents. When you read something here and wonder *"why this choice?"*, the answer is either:

1. A principle from `../PRINCIPLES.md`.
2. A constraint from `../docs/07-non-negotiables.md`.
3. A layer rule from `../docs/03-open-vs-closed.md`.
4. A safety requirement from `../docs/08-project-threat-model.md` or `../PRIVACY.md`.
5. A scope rule from `../docs/09-content-policy.md`.
6. A purely technical reason — in which case we document the reasoning so it can be revisited.

If an architectural decision has no clear foundation in the above, that is a flag. Either it's a reasonable technical call that should be documented as such, or it's drifting. Surface it.

---

## Reading Order

**For new contributors:**

1. `00-principles-to-architecture.md` — how foundation principles map to concrete technical constraints
2. `01-system-overview.md` — big-picture diagram of all projects and shared infrastructure
3. `02-tech-stack-decisions.md` — technology choices with reasoning
4. `03-data-architecture.md` — how corpora, citations, and user data are structured
5. `10-security-architecture.md` — authentication, secrets, encryption, jurisdictional hosting

**For work on a specific project:**

- `basira/` — Islamic research copilot (deepest detail, as this is the first commercial product)
- `shahid/` — atrocity documentation archive
- `hisn/` — counter-surveillance toolkit
- `mizan/` — algorithmic accountability auditor

Each project folder begins with an `01-overview.md`.

---

## Structure

```
architecture/
├── README.md                       ← you are here
├── 00-principles-to-architecture.md   ← bridge from principles to tech
├── 01-system-overview.md              ← cross-project view
├── 02-tech-stack-decisions.md         ← ADRs for core technology
├── 03-data-architecture.md            ← corpora, schemas, provenance
├── 10-security-architecture.md        ← authn, secrets, encryption, hosting
├── basira/
│   ├── 01-overview.md                 ← product scope and user flows
│   ├── 02-rag-pipeline.md             ← retrieval and generation design
│   ├── 03-scholar-verification.md     ← workflow and data model
│   └── 04-mvp-scope.md                ← what ships in version 1
├── shahid/
│   └── 01-overview.md                 ← archive architecture sketch
├── hisn/
│   └── 01-overview.md                 ← counter-surveillance tooling sketch
└── mizan/
    └── 01-overview.md                 ← accountability audit infrastructure sketch
```

The numbered prefix `00–99` is just for reading order, not priority.

---

## Status Markers

Each decision in these documents carries a status:

- **[DECIDED]** — settled; implement accordingly.
- **[TENTATIVE]** — working assumption; may change as we learn.
- **[OPEN]** — explicitly not decided yet.
- **[DEFERRED]** — recognized as needed, postponed by priority.

When you see `[TENTATIVE]` or `[OPEN]`, you are welcome to propose a resolution via PR or issue.

---

## How to Propose Changes

Architecture changes fall into three categories:

1. **Refinement** — clarifying, adding detail, correcting errors. Open a PR. Maintainer approves.
2. **Material decision change** — changing tech stack, data structures, security posture. Requires public proposal, discussion period (minimum one week), maintainer approval, and rationale documented.
3. **Principle-level architectural change** — affects how principles are honored in the system. Requires full governance process per `../docs/06-governance.md`.

When in doubt, open an issue first and discuss before writing the PR.

---

## Architecture Decision Records (ADRs)

For significant decisions, we use the **ADR** (Architecture Decision Record) format within `02-tech-stack-decisions.md` and project-specific files. Each ADR captures:

- **Context** — what forced this decision
- **Options considered** — at least two alternatives
- **Decision** — what we chose
- **Consequences** — what this enables and what it costs
- **Status** — as above

ADRs are immutable once [DECIDED] — if the decision changes later, a new ADR supersedes the old one; both remain in the record. This preserves reasoning history.

---

## Relationship to Implementation

These documents describe the system. They are **not** the system. As implementation progresses:

- Code lives in separate repositories per project (or subdirectories of this repo; to be decided).
- ADRs and major docs here are updated when reality diverges from the design.
- Divergences are flagged, not hidden.

The architecture documents are a shared map. Stale maps mislead travelers. We keep them current.

---

## For AI Assistants

If you are an AI helping with architectural work:

- Do not invent architectural decisions that conflict with foundation principles.
- Do not mark things `[DECIDED]` unless they actually are.
- Do not remove `[TENTATIVE]` or `[OPEN]` markers without explicit human decision.
- When you identify something not yet addressed here, propose it — do not assume your answer is correct.
- Cite foundation documents when explaining *why* a given architectural choice was made.

See `../AI_INSTRUCTIONS.md` for the full binding guidance.

---

## Closing

Architecture is where principles meet code. Done well, it preserves the principles at scale. Done poorly, it lets the principles slip silently as complexity grows.

These documents are the connective tissue. Keep them honest.
