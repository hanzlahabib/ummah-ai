# The Ummah AI Council — Structure and Anonymity Protocol

## What this is

The Ummah AI Council is the **decision-making committee** that holds principle-level and strategic authority for the Initiative. It exists to ensure no single founder, funder, or institution can redirect the work — including against the founder's own future temptations.

The Council operates **pseudonymously by default.** Members' real identities are known only to the Initiative's inner trust chain. Public-facing communications, decisions, and signatures use pseudonyms.

This document specifies: how the Council is composed, how identities are protected, how decisions are made and verified, and how adversaries are kept out.

---

## Why pseudonymous

Public Council members are vulnerable to:

| Attack | Mechanism | Mitigation via anonymity |
|---|---|---|
| **Blackmail** | Adversary identifies member, finds leverage (family, finances, past) | Adversary cannot find unknown member |
| **Honey trap** | Targeted social engineering using personal interests | No public profile to study |
| **Bribery** | Direct offer to member | Adversary doesn't know who to approach |
| **Coercion via family** | Threats against member's relatives | Family does not know member is on Council |
| **Public discrediting** | Manufactured scandal to remove member | No public reputation to target |
| **Saviour cult** | Member becomes celebrity, draws devotion away from mission | No personality to follow |
| **Impersonation** | Adversary claims membership | Cryptographic signatures are required for all decisions |
| **State pressure** | Government demands member identity | Initiative does not have it to disclose; cryptographic verification suffices |
| **Targeted physical violence** | Direct threats to member | Adversary cannot locate target |

Pseudonymity is not paranoia. It is response to documented patterns: human rights organizations, journalists, scholars in Muslim-majority states, and AI-accountability researchers have all faced these specific attacks within the past decade.

---

## Council composition

### Size

**12 members** at full operation. Odd-numbered subsets used for specific decisions (5, 7, 9 depending on stake).

### Required representation

The Council must include, at minimum:

| Tradition / role | Seats |
|---|---|
| **Sunni Hanafi scholar** | 1 |
| **Sunni Maliki scholar** | 1 |
| **Sunni Shafi'i scholar** | 1 |
| **Sunni Hanbali scholar** | 1 |
| **Shia Ja'fari scholar** | 1 |
| **Shia (Zaydi or Ismaili)** | 1 |
| **Ibadi scholar** | 1 |
| **Technologist (senior engineer / AI researcher)** | 2 |
| **Security / privacy specialist** | 1 |
| **Legal / governance specialist** | 1 |
| **Community / civil society representative** | 1 |
| **Total** | **12** |

If a seat cannot be filled (e.g., no qualified Ibadi scholar willing under current threat model), the seat remains explicitly vacant — never filled with a substitute from another tradition. Vacant seats are publicly noted.

### Optional advisors (non-voting)

Specialists may join as **advisors** with no voting rights — appropriate for specific decisions (e.g., a climate scientist for a partnership decision touching climate work). Advisors are also pseudonymous.

---

## Eligibility (criteria for admission)

A candidate is admissible only if:

1. **Genuine credentials** — formal scholarly training (for scholar seats) or demonstrated senior expertise (for technical / specialist seats). Verified by independent trusted intermediaries, not self-declared.
2. **Mission alignment** — explicit acceptance of `PRINCIPLES.md` and `docs/07-non-negotiables.md`. Demonstrated, not just signed.
3. **Independence** — no current employment, retainer, or material financial dependence on:
   - Any government's intelligence, military, or interior services.
   - Any frontier AI company (Anthropic, OpenAI, Google, Meta, Microsoft, xAI, etc.).
   - Any organization whose primary mission contradicts the Initiative's principles.
4. **Operational competence** — understands the threat model and is willing to follow the OPSEC protocol below.
5. **Stability** — willing and able to serve the documented term length (typically 5 years, renewable once).
6. **No conflicting public role** — not simultaneously holding a public role that could be exploited (e.g., a sitting government minister of intelligence — even mission-aligned — is not eligible).

A candidate who meets credentials but fails operational competence is rejected; they may serve as a non-voting advisor under more relaxed OPSEC requirements.

---

## Vetting process (admission to Council)

```
Candidate proposed
        │
        ▼
Existing member sponsors candidate
(at least 2 sponsors required)
        │
        ▼
Independent credential verification
(third-party scholar/peer confirms credentials
 without learning candidate is being vetted for Council)
        │
        ▼
OPSEC briefing + practical evaluation
(can the candidate operate securely?)
        │
        ▼
Conflict-of-interest disclosure (private)
        │
        ▼
Cooling period (3-6 months)
(candidate observes Council activity before voting)
        │
        ▼
Council vote (supermajority — 8 of 11 existing members)
        │
        ▼
Admission + pseudonym assignment
```

Vetting steps may take 6-12 months. Speed is a security risk; deliberation is a security feature.

---

## Pseudonyms

Each Council member is assigned a pseudonym at admission. Pseudonyms follow a structured scheme:

```
[Role-prefix] [Numeric-suffix]
```

Examples (illustrative; actual scheme in private operational document):

- `Faqih-7` — a fiqh-scholar member
- `Mufassir-3` — tafseer specialist
- `Muhaddith-2` — hadith specialist
- `Muhandis-5` — engineer
- `Hakim-4` — legal/governance
- `Hami-1` — security specialist (`hami` = guardian)

The numeric suffix is randomly assigned at admission, not sequential. Pseudonyms are stable for the member's tenure.

### What pseudonyms accomplish

- Public can see Council composition (e.g., "Faqih-7, Muhaddith-2, Muhandis-5 voted in favor of decision X").
- Public can verify the credentialed roles (a Faqih is a Faqih; vetted as such).
- Public cannot identify the human behind any pseudonym.
- Across decisions, observers can track consistency of a given member's reasoning — useful for accountability — without de-anonymizing.

---

## Decision-making

### Decision categories

| Category | Threshold | Examples |
|---|---|---|
| **Routine operations** | Founder + 1 Council member sign-off | Standard hires, routine partnerships, minor budget moves |
| **Material decisions** | Simple majority of Council (7 of 12) | Strategic direction, major hires, significant budget moves, partnership agreements |
| **Principle-adjacent decisions** | Supermajority (9 of 12) | Funding source acceptance with new conditions, governance amendments at G1+ |
| **Non-negotiable amendments** | Effectively impossible — full process per `docs/06-governance.md` | Changes to `PRINCIPLES.md` or `docs/07-non-negotiables.md` |

### Decision flow

```
Proposal documented
        │
        ▼
Public posting (issue, repo)
        │
        ▼
Comment period (varies: 7d routine → 30d principle-adjacent)
        │
        ▼
Council members review privately
(each via secure channel; debate happens in
 encrypted forum visible only to Council)
        │
        ▼
Vote period (open for 72 hours minimum)
        │
        ▼
Each voting member signs their position
(cryptographic signature with member's pseudonym key)
        │
        ▼
Decision tallied; signatures published
        │
        ▼
Public record:
  - Decision text
  - Vote count (e.g., 9 yes / 2 no / 1 abstain)
  - List of signing pseudonyms
  - Cryptographic proof of signatures
  - Reasoning (where signed by majority)
        │
        ▼
Implementation
```

### Cryptographic signature scheme

Each Council member holds:
- **Pseudonymous PGP keypair** (private key with member, public key in repo)
- Member rotates key annually or on suspected compromise

Each decision is signed by each voting member:
- Member computes signature over the decision text + vote.
- Signature includes timestamp + nonce.
- Verifies: (a) decision is authentic, (b) member-pseudonym voted, (c) vote is valid, (d) when it was cast.

Public can verify all of the above without learning real identities.

### Why this works

- Adversary cannot forge a vote (would require pseudonym's private key).
- Adversary cannot link pseudonym to person.
- Public can audit decisions for legitimacy.
- Members are accountable through their pseudonymous track record without being individually exposed.

---

## OPSEC requirements for members

Every member follows these baseline practices:

### Communications

- All Council communication via **end-to-end encrypted channels only.**
- Approved channels: Signal (preferred), Element on self-hosted Matrix server, encrypted email (PGP) with reply attestation.
- **No SMS, no unencrypted email, no major-platform DMs** for Council business.
- No metadata-leaking platforms (e.g., WhatsApp's metadata graph is unacceptable).

### Devices

- Council communications happen on a **dedicated device or compartmented profile** — not shared with personal life.
- Full disk encryption mandatory.
- Hardware security key for authentication.
- Operating system kept current with security updates within 7 days of release.
- No third-party apps with broad permissions on the Council device.

### Identity

- Member does not disclose Council membership to: spouses (until OPSEC understood), friends, colleagues, employers, fellow scholars outside the Council, or anyone not explicitly cleared.
- Disclosure is minimal-need-to-know. (Some members may need to disclose to one other person — spouse, lawyer — for practical reasons; this is documented and reviewed.)
- Member refuses, politely, to confirm or deny Council membership when asked.

### Travel

- Member notifies Council of travel through high-risk jurisdictions in advance.
- Border-crossing OPSEC followed (no Council material on devices at borders; Council device left at home).
- No discussion of Council business in jurisdictions known to surveil.

### Compromise indicators

- Member must report to Council:
  - Suspicious approaches (recruitment offers, unusual social interest, journalists asking specific questions about Initiative).
  - Apparent surveillance or targeted harassment.
  - Family pressure related to known associates.
  - Any access compromise (lost device, suspected key compromise).

Failure to follow OPSEC is grounds for removal.

---

## When a member's identity must be revealed

Public disclosure of a member's identity is **rare and structured.** Permitted only:

1. **Voluntary, by member.** A member may choose to go public at any time, with appropriate transition planning. Council assists; no penalty.
2. **Death of member.** Memorial (with family consent) may include Council role.
3. **Resignation under threat.** Member resigns due to specific compromise; Council decides whether public acknowledgment helps or hurts.
4. **Legal compulsion that survives full challenge.** If a court of competent jurisdiction issues a final, non-appealable order requiring disclosure of a specific member, the Initiative complies — but this is the strongest legal challenge possible, and the member is given advance notice to take protective measures.
5. **Member is exposed by adversary.** Once exposed, anonymity is moot for that individual; Council adapts.

In every case **other** members' identities remain protected. One disclosure does not cascade.

---

## Replacement and rotation

### Rotation

- Standard term: 5 years.
- One renewal possible (5+5 = 10 years maximum).
- After 10 years, member rotates off; pseudonym retired.
- Replacement vetted using full process above.
- Term lengths are staggered so no more than 3 members rotate in any single year.

### Removal

A member may be removed by 9-of-11 vote of remaining Council for:

- OPSEC violation that endangered Council or Initiative.
- Breach of conflict-of-interest disclosure.
- Behavior contradicting principles.
- Sustained inability to participate.

Removal does not require revealing reasons publicly beyond high-level cause.

### Resignation

A member may resign at any time with 60-day notice (less for safety).

---

## Funding regulations (relating to Council)

Per the principles already established, the Initiative refuses funding that compromises decision-making. Specifically, the Council must reject:

| Funding source pattern | Reason |
|---|---|
| Conditional on revealing Council member identities | Direct attack on Council security |
| Conditional on Council composition (e.g., "we want X representative") | Direct attack on Council independence |
| Conditional on specific decisions or directions | Buying outcomes |
| Conditional on editorial control over outputs | Capture |
| Conditional on access to underlying data beyond what's openly published | Exfiltration risk |
| From entities listed on `docs/07-non-negotiables.md` (NN-2) | Already prohibited |
| From governments / military / intelligence services with documented civilian-targeting records | Already prohibited |
| With non-disclosure clauses preventing public reporting of the funding source | Hidden capture |

**Acceptable funding shape:**

- Pure waqf-style commitment: gift with no strings.
- Operational grants for specific scope without editorial input.
- Subscription / customer revenue: market exchange, no influence over governance.
- Bounty / research contracts: specific deliverable, scoped, no broader influence.

All accepted funding is publicly disclosed (source, amount, conditions). Refused funding offers above a defined threshold are also publicly disclosed (source masked if confidentiality required, but the *fact* that funding was refused and the *reason* are public).

---

## Admission, communication, and key infrastructure

### Operational documents (private, see `private/`)

Stored locally, never committed:

- `private/COMMITTEE_REGISTRY.md` — pseudonym ↔ real-name mapping; emergency contacts; credential verification records.
- `private/COMMITTEE_KEYS.md` — public-key registry; signing key infrastructure documentation.
- `private/OPSEC_HANDBOOK.md` — detailed practices for members (more granular than this public doc).

### Public references

- `COMMITTEE.md` (this document) — structure, principles, pseudonym scheme, decision protocols.
- Public PGP key registry (under `committee-keys/` once Council exists) — pseudonym → public-key mapping for verification.
- Public decision log — every Council decision's text + vote tally + signatures.

---

## Adversary-resistance summary

Threats and how the design counters them:

| Threat | Counter |
|---|---|
| State demands committee names | Initiative does not centrally hold them in a way that satisfies state demand; decisions are cryptographically verifiable without identities |
| Rich actor offers bribe to a member | Actor cannot identify members |
| Honey trap targeting a member | No public profile to study |
| Pressure on member's family | Family doesn't know |
| Discrediting a member publicly | No public reputation |
| Replacing a member with an impostor | Cryptographic signatures prevent forgery |
| Splitting the Council via wedges | Multi-school requirement makes single-faction capture structural impossibility |
| Blackmail discovering past indiscretion | Vetting + ongoing OPSEC; member can resign without identity disclosure |
| Saviour-cult formation around a "leader" member | Pseudonymous; no personality to follow |
| Acquisition / capture of the Initiative | Already prohibited (not for sale, per `FINANCIAL_MODEL.md`) |
| Insider exfiltration of registry | Registry on offline-preferred devices; Shamir-split for sensitive subsets |
| Founder-driven capture | Council can outvote founder on principle-level decisions; founder cannot remove members alone |

---

## What this is NOT

- Not a secret society in the conspiratorial sense — methodology and decisions are public; only identities are private.
- Not a permanent shadow leadership — members rotate; Council itself can be reformed by full governance process.
- Not an end-run around accountability — decisions are auditable, signatures verifiable, dissent recorded.
- Not unique to this initiative — pseudonymous safety committees exist in human rights work, journalism, and dissident movements globally.

---

## Phase rollout

### Phase 0 (foundation, current)

- This document published. Founder is sole decision-maker but commits to populating Council per criteria.
- Search for first 3-5 members begins.
- Vetting infrastructure established (cryptographic key generation, secure communication setup).

### Phase 1 (Basira MVP build)

- 5-7 Council members recruited, vetted, admitted.
- First Council decisions made (e.g., approval of legal entity formation, of partnership templates).

### Phase 2 (commercial traction + first defensive project)

- Full Council of 12 in place.
- Founder's principle-level authority transferred to Council per `docs/06-governance.md` Phase G2.
- Routine Council operations stabilized.

### Phase 3+

- Council operates as the Initiative's principle-level governance body.
- Membership rotates per protocol.
- Decision history accumulates as auditable record.

---

## For prospective members reading this

If you are reading this document because someone has approached you about Council membership, or you are considering it: understand that you are being asked to take on:

- A serious, multi-year commitment to a high-stakes mission.
- An operational security regime that is more rigorous than ordinary professional life.
- A pseudonymous identity that does not give you public credit for your work.
- Personal risk in some scenarios, mitigated but not eliminated.

You are not being asked to:

- Compromise your beliefs or conform to a sectarian line.
- Disclose anything beyond what's required.
- Take on financial risk.
- Sacrifice your other obligations.

The work is meaningful, the company is trustworthy (because of structural safeguards, not blind faith), and the Ummah's future depends in part on whether enough people of your caliber accept this kind of asymmetric responsibility — much credit, no recognition.

If you decline, you will not be approached again. If you accept, the vetting process begins.

*"إِنَّ اللَّهَ يَأْمُرُكُمْ أَن تُؤَدُّوا الْأَمَانَاتِ إِلَىٰ أَهْلِهَا"*
*"Indeed, Allah commands you to render trusts to whom they are due."* — Surah An-Nisa 4:58

---

## Cross-references

- Governance: `docs/06-governance.md`
- Non-negotiables: `docs/07-non-negotiables.md`
- Threat model: `docs/08-project-threat-model.md`
- Financial model: `FINANCIAL_MODEL.md`
- Bus-factor (founder-side, complementary): `private/BUS_FACTOR.md` (gitignored)

---

## Closing

A council of fifteen public faces is a fifteen-name target list for whoever wants to capture the Initiative. A council of twelve pseudonyms is, to that same adversary, almost nothing.

This is not paranoia. It is operational discipline appropriate to the stakes.

We protect our members because they are necessary, and because the work is more important than any one of them.

*Wa Allahu khayru al-hafidhin* — *And Allah is the best of protectors.*
