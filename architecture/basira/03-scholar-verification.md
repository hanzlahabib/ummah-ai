# Basira — Scholar Verification

## Workflow and Data Model for Scholarly Review of Content

No Islamic scholarly content reaches Basira users without passing through the scholar verification process. This document specifies that process — who, how, with what tools, and recorded how.

This is one of the most mission-critical pieces of the system. Failure here — hallucinated hadiths, misattributed positions, one-school bias — undermines the trust that Basira's entire commercial and mission value depends on.

---

## Principles Governing This Workflow

From `../../docs/09-content-policy.md` and `../../PRINCIPLES.md`:

- **No falsehood disguised as scholarship** (NN-9).
- **Scholars are compensated**, not extracted from.
- **Multi-tradition content requires multi-tradition review.**
- **Verification is transparent** — who verified, when, what.
- **Scope of verification bounded** by content policy.

---

## Scope: What Needs Verification

### Always requires verification

- **Curation annotations** — any editorial note, summary, or framing we add around primary sources.
- **Cross-references** — claims that text A is related to text B.
- **Scholarly summaries** — digests of school-of-thought positions.
- **Answer templates** — recurring response patterns for common question types.
- **Translations not from recognized published editions.**

### Does not require verification (but requires provenance)

- **Canonical primary sources** — Quranic text (from recognized authoritative editions), hadith texts from recognized collections, classical works verbatim.
- **Established published translations** (Yusuf Ali, Pickthall, Sahih International, Al-Islam.org Shia translations, etc.) — attributed with edition, not verified by our scholars.
- **Source-verbatim citations** in retrieval output — the retrieval itself isn't "verified," only the annotations around it.

### Generated content per user query

LLM-generated responses to individual user queries are **not** individually verified by scholars (that would be impossible at scale). Instead:

- The **underlying knowledge base** is verified.
- The **retrieval and generation pipeline** is evaluated against benchmarks that scholars helped build.
- **Refusal policies** prevent generation in high-risk categories (fatwa-issuing, personalized rulings).

Scholars verify the **infrastructure**, not each output.

---

## Who Is a Verifying Scholar

### Eligibility

Per `../../docs/09-content-policy.md`:

- Formal training in an established Islamic scholarly institution (ijazah, Azhari or equivalent degree, recognized seminary completion, verified traditional-chain qualification).
- Demonstrated contemporary scholarly practice (teaching, publishing, issuing fatawa, or serving in recognized religious roles).
- Willingness to operate under the initiative's principles — including non-sectarian review of content from traditions other than one's own.

Institutional affiliation not required. Independent scholars with credible qualifications are welcomed.

### Scholar roles

- **Reviewer** — primary role; reviews drafts within their specialty.
- **Cross-school reviewer** — reviews content that touches multiple traditions; typically a senior reviewer from one tradition with demonstrated cross-school fluency.
- **Lead** — senior scholar managing a tradition's review pipeline; coordinates reviewer pool; resolves escalations.
- **Advisory** — senior scholars on the advisory council; set policy; do not do day-to-day reviews.

### Diversity of scholars

The pool must include:

- Scholars from **each major Sunni madhhab** (Hanafi, Maliki, Shafi'i, Hanbali).
- Scholars from **each major Shia school** (Ja'fari, Zaydi, Isma'ili where available).
- Scholars from the **Ibadi tradition**.
- Scholars representing **Sufi, Salafi, traditionalist, modernist** methodological orientations within the above.
- Male and female scholars.
- Scholars spanning **geographic and linguistic** diversity.

Token representation is not sufficient. Active participation across traditions is the target.

### Conflicts of interest

Scholars disclose:

- Formal employment or institutional affiliation.
- Commercial relationships (publishing, consulting).
- Political or governmental roles.
- Family or personal relationships to content authors under review.

Conflicts do not automatically disqualify but must be disclosed and factored into review assignments.

---

## Compensation

### Base principle

Scholars are paid for substantive review work. Unpaid scholarly labor is extractive and contrary to the initiative's principles.

### Structure

- **Hourly rate** for scheduled review work, rates published and negotiated in line with comparable scholarly-consultation rates.
- **Retainer** for scholars providing ongoing review capacity (predictable income for them; predictable availability for us).
- **Research grants** for scholars developing new curation content aligned with the project's priorities.

### Transparency

- Compensation structure published (ranges, not individual contracts).
- Total scholar compensation reported in annual transparency reports.
- Waqf-style allocation includes a dedicated portion for scholar payments — so scholar support does not depend on the moods of commercial priorities.

---

## Workflow

### Stage 1 — Drafting

- A draft unit of content is proposed:
  - Could be a new curation annotation.
  - Could be an update to a summary.
  - Could be a new entry in the corpus (e.g. a less-digitized classical work).
- Draft can be produced by:
  - Human contributors.
  - AI-assisted, with human review (AI-assisted drafts clearly marked).
- Draft enters the review queue with metadata: content type, affected traditions, affected schools.

### Stage 2 — Primary review

- Assigned to at least one **primary reviewer** within the tradition of the content.
- Reviewer assesses:
  - **Accuracy**: are cited sources quoted correctly? Are facts right?
  - **Completeness**: are significant relevant positions included?
  - **Attribution**: are positions correctly attributed?
  - **Sourcing**: are citations specific enough? Are the sources appropriate?
  - **Balance**: within the tradition, is the range of scholarly opinion represented?

Reviewer decision:

- **Approve**: content passes to next stage.
- **Request revision**: specific changes needed; returns to drafting.
- **Reject**: fundamental issues; content returned to drafters.

### Stage 3 — Cross-school review (if applicable)

For content that touches multiple traditions (e.g. a comparison of fiqh positions on a question across Hanafi, Maliki, Shafi'i, Hanbali, Ja'fari):

- Additional reviewer(s) from the **other traditions** review.
- Cross-school reviewer checks:
  - Are the other traditions represented accurately?
  - Are their arguments and sources fairly presented?
  - Is there any implicit preference for one tradition?

Same decision options as primary review.

### Stage 4 — Publication

- Approved content enters the production knowledge base.
- Verification record attached (scholars, date, any flags).
- Users see the content; verification metadata available on click.

### Stage 5 — Periodic re-review

- Content is periodically re-reviewed (annually for high-traffic content; longer cycles for stable content).
- Re-review triggers:
  - Passage of time.
  - User-reported issues.
  - New scholarly work on the topic.
  - Changes in how the content is surfaced.

---

## Data Model

### Verification record

```typescript
type VerificationRecord = {
  content_id: string;              // link to the citation or curation unit
  content_version: string;         // which version was reviewed
  review_cycle: "initial" | "revision" | "periodic";
  reviewer: {
    id: string;                    // scholar's canonical ID
    name: string;                  // public name or "pseudonymous"
    credentials_category: string;  // e.g. "Azhari PhD", "Qom-trained mujtahid", "Deobandi alim"
    tradition: Tradition;          // reviewer's own tradition
    schools: School[];             // reviewer's schools of expertise
  };
  cross_school_reviewers?: Reviewer[]; // if multi-tradition content
  status: "approved" | "revision_requested" | "rejected";
  reviewed_at: string;             // ISO timestamp
  notes?: string;                  // reviewer comments (published with content)
  flags?: string[];                // e.g. "disputed_grading", "contested_attribution"
  signature?: string;              // optional cryptographic signature by scholar
};
```

### Scholar profile

```typescript
type Scholar = {
  id: string;
  display_name: string;
  pseudonymous: boolean;
  credentials: {
    category: string;              // e.g. "Azhari", "Qom-trained"
    institution?: string;
    degree_or_ijazah?: string;
    year_completed?: string;
  }[];
  tradition: Tradition[];
  schools_of_expertise: School[];
  languages: LangCode[];
  specialties: string[];           // e.g. "hadith sciences", "usul al-fiqh"
  active_since: string;
  status: "active" | "inactive" | "retired";
  declared_conflicts_of_interest: string[];
  public_profile_url?: string;
};
```

Public profiles (with scholar's consent) displayed when users click verification metadata on any piece of content.

### Review assignment

- Assignments prioritize matching tradition / school / specialty.
- Scholar workload tracked; overwork avoided.
- Turnaround time targets set (e.g. 5 working days for standard reviews).

---

## Quality Controls

### Inter-reviewer agreement

- Same content occasionally sent to multiple reviewers blind.
- Disagreement analyzed: genuine scholarly disagreement vs. reviewer error.
- Chronic inter-rater disagreement triggers process review.

### Spot checks

- Random audits of published content by senior scholars.
- User-reported issues investigated promptly.

### Appeals

- Contributors whose drafts are rejected may appeal:
  - First to the primary reviewer (discussion).
  - Then to a different reviewer within the tradition.
  - Then to the tradition's lead scholar.
- Resolution recorded.

### Scholar exits

- Scholars may withdraw from the project at any time.
- Content they reviewed remains valid (their review stands).
- If a scholar's credentials are later found to be misrepresented, their prior reviews are flagged for re-review.

---

## Special Cases

### Contested content

Some content is inherently contested — different scholars in the same tradition will disagree about how to present it.

- Contested content is **not hidden**; it's surfaced with the contest explicit.
- Reviewers may request additional sources or reviewers.
- The scholarly advisory council can be consulted for principled guidance.

### Sensitive content

Topics with potential for harm (violent extremist misappropriation of texts, for example):

- Reviewers flag.
- Content team reviews framing before publication.
- In extreme cases, content is scope-limited — available to scholars but not surfaced to general users.

### Rapid-response content

Current events sometimes require rapid curation (a major scholar passes away; a significant fatwa is issued on a breaking topic):

- Expedited review process exists (24-48 hours).
- Normal scholarly rigor still required — rapid is not a synonym for rushed.
- Content marked as "current-events" for appropriate user expectation.

---

## For Scholars Considering Contributing

If you are a scholar reading this considering whether to engage with the project:

- **You are not a rubber stamp.** Your review has real substantive weight.
- **You are not solely responsible for one position.** Cross-school review distributes responsibility.
- **You are compensated fairly.** Not as a favor to us; as fair compensation for your expertise.
- **Your name and credentials are visible** (unless you opt for pseudonymity).
- **You retain intellectual freedom.** You are not asked to endorse positions you disagree with; you are asked to verify accuracy of presentation.
- **You operate within a pluralistic framework.** This is different from operating within a single-school institution. If that's uncomfortable, this may not be the right role.

Outreach to scholars happens through existing academic networks, institutional partnerships, and open calls. Scholars interested in engaging may contact the initiative through the channels in `../../CONTRIBUTING.md`.

---

## Cross-References

- Content policy: `../../docs/09-content-policy.md`
- RAG pipeline (how reviewed content is surfaced): `02-rag-pipeline.md`
- MVP scope: `04-mvp-scope.md`
- Principles: `../../PRINCIPLES.md`
