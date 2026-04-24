# Governance

## How Decisions Are Made and Capture Is Prevented

Every institution in history that served its mission beyond its founding generation had a governance structure that made it possible. Every institution that failed its mission had a governance structure that made capture easier than resistance.

This document sets out the governance of this initiative. Like the rest of the foundation, it is published openly, subject to community challenge, and designed to be revised carefully and transparently.

In the founding phase, governance is minimal — appropriate for a small, trust-based team. It becomes more formal as the project grows. This document describes both the current (minimal) state and the target state.

---

## Governance Objectives

The governance structure exists to achieve five things:

1. **Preserve the principles.** No decision, however strategically attractive, can violate `PRINCIPLES.md`.
2. **Prevent capture.** No single person, funder, sect, nation, or corporation can redirect the project.
3. **Enable good decisions.** Decisions should be made by people who understand them, informed by those affected, in time to matter.
4. **Build trust.** Transparency of process is what allows people to trust outcomes even when they disagree with specific decisions.
5. **Outlast founders.** The institution should be stronger, not weaker, without any specific individual.

---

## Phase-Appropriate Governance

Governance formalization matches institutional maturity. Imposing corporate-style governance on a two-person project wastes energy. Failing to formalize governance as an organization grows produces capture.

### Governance Phase G0 — Founding (current)

**Active when:** Project has 1–5 active contributors, no revenue, no paid staff.

**How decisions are made:** Consultation between the founder and any active contributors. Public documentation of decisions. No formal votes. Founder has operational authority.

**Safeguards:**
- All material decisions are documented publicly (issues, pull requests, documents).
- Disagreement is expressed openly.
- Fork rights are recognized as the ultimate check.

**Known weaknesses:** Heavy dependence on the founder. High bus factor. Decisions can be made quickly but also wrongly. This is accepted as the cost of moving fast in the foundation phase — but mitigated by the bus-factor protocol below.

#### Bus-factor protocol (mandatory from Phase G0 Day 1)

Founder dependence is a known vulnerability. To mitigate it from the earliest moment:

- **Trusted continuators:** The founder identifies at least three trusted individuals who would continue the work if the founder becomes unavailable. Their contact information is documented in a private but recoverable location known to each of them.
- **Distributed repository access:** At least two trusted continuators hold administrative access to the primary repository from the beginning. This is *not* a rotating privilege; it is a standing one.
- **Public foundation:** All foundation documents are public and openly licensed — so the mission survives regardless of any single individual.
- **No single-person secrets:** No credential, account, or critical piece of operational knowledge is held by only one person.
- **Unavailability procedure:** If the founder is unreachable for 30 days with no explanation, the trusted continuators are authorized to announce the situation publicly and assume interim leadership. Within 90 days, they decide whether to appoint a new lead, restructure, or transition to community governance.

This protocol is a commitment, not an aspiration. It is in place from the first day of the project, not deferred to a later phase.

### Governance Phase G1 — Early Institution

**Triggered when:** Project has 5–15 active contributors, or first paid staff member, or first substantial revenue/grant.

**Changes introduced:**
- **Maintainer group:** 3–5 trusted contributors who have demonstrated sustained commitment. They share code review, PR merging, and day-to-day decisions.
- **Advisory council:** 5–10 people including scholars, technologists, ethicists, and impacted-community representatives. Advisory only; not decision-making. Meets quarterly. Minutes published.
- **Written conflict-of-interest policy:** All contributors disclose financial, institutional, or political affiliations that could influence decisions.
- **Documented dispute resolution process.**

### Governance Phase G2 — Mature Institution

**Triggered when:** Multi-project ecosystem active, multiple paid staff, revenue above $1M ARR, or at least one formal organizational legal structure.

**Changes introduced:**
- **Board of Trustees (or equivalent):** A legal-fiduciary body with defined roles, including trustees specifically responsible for mission-preservation (the "mission guardians"). Board membership is staggered so no single event can replace a majority at once.
- **Operations leadership:** Separate from Board. Handles day-to-day. Reports to Board on mission metrics, not just financial metrics.
- **Community council:** Elected representatives of active contributors. Formal voice in major decisions (principle amendments, large partnerships, leadership changes). Not purely advisory.
- **Annual transparency report:** Finances, governance decisions, incident reports, progress against mission.
- **Independent audit:** Financial and mission audits published annually.

### Governance Phase G3 — Federated Ecosystem

**Triggered when:** Multiple sub-projects with their own teams, multiple organizational entities, regional or project-specific autonomy.

**Changes introduced:**
- **Federation structure:** Each sub-project has operational autonomy under shared principles. An umbrella structure manages cross-project coordination.
- **Principles enforcement mechanism:** Independent compliance review that any contributor can trigger. Can require a sub-project to revise or, in extreme cases, exit the federation.
- **Fork and exit protocols:** Formal processes for projects that wish to leave the federation or be expelled from it.

This phase is speculative and will be designed when reality demands it.

---

## Who Decides What

| Decision Type | Phase G0 | Phase G1 | Phase G2 |
|---|---|---|---|
| Routine technical decisions | Founder + contributor consensus | Maintainer group | Project lead |
| Feature direction | Founder | Maintainers + advisory input | Project leads + community input |
| Hiring | Founder | Founder + maintainers | Operations leadership |
| Partnerships | Founder | Maintainers with advisory review | Board for significant partnerships |
| Major funding decisions | Founder | Maintainers + advisory | Board |
| Principle amendments | Not permitted | Not permitted | Requires full community process + supermajority + mission guardians |
| Code of conduct enforcement | Founder | Maintainers | Community council + board escalation |
| Dispute resolution | Direct conversation | Maintainers as mediators | Community council; board for escalations |
| Fork or spin-off | Always permitted to anyone | Always permitted | Always permitted |

---

## Mission Preservation

Every governance structure eventually faces the test of mission drift. This section sets out explicit mechanisms to resist it.

### The Immutable Core

`PRINCIPLES.md` contains eight principles. They are explicitly declared immutable. The governance structure shall not amend them. Even unanimity of trustees cannot amend them. If they are ever amended, the project has ceased to be this project.

The correct response to an amendment of a core principle is **fork**. Fork rights (Principle 8) exist precisely to make this the normal response, not an extraordinary one.

### Mission Guardians

Once Phase G2 governance is established, at least **one-third of any governing body** must be designated "mission guardians" — individuals whose explicit role is to defend the principles, including against strategic or commercial pressure.

Mission guardians:
- Are selected for principles-alignment, not strategic acumen.
- Have veto authority on decisions that would violate principles.
- Serve longer terms than operational leaders (to outlast pressure cycles).
- Are themselves accountable to the published principles — not to the organization's strategic interests.

### Structural Separation

Where the organization operates both commercial and mission (defensive) work, governance distinguishes them:

- Commercial operations are accountable for sustainability and growth.
- Mission operations are accountable for impact on the vulnerable.
- The waqf-style revenue share (minimum 20% of commercial net to mission work) is contractually enforced, not discretionary.
- Senior roles on the commercial side cannot concurrently control mission-side decisions, and vice versa.

This separation is inspired by the structure of some successful mission-driven organizations (e.g. Mozilla Foundation / Mozilla Corporation) where a commercial subsidiary funds a non-profit parent, with careful structural firewalls.

### The Public Record

All material decisions are documented. Minutes of governance meetings (once they exist) are published. Funding sources and amounts are disclosed. Partnership terms are disclosed.

Dissent is published alongside decisions. When a contributor disagrees formally, their dissent enters the record. This allows future observers — including community members today — to evaluate decisions not just on their outcomes but on the quality of the reasoning at the time.

---

## Dispute Resolution

Disputes are normal. How they are resolved determines whether the project is healthy or toxic.

### Types of dispute

- **Technical disagreements:** How to design a system.
- **Strategic disagreements:** What to prioritize.
- **Principled disagreements:** Whether a proposed action complies with principles.
- **Interpersonal conflicts:** Harassment, bad conduct, accusations.
- **Governance challenges:** Allegations that decisions violate the governance process.

### Resolution ladder

1. **Direct conversation.** Most disputes resolve at this level. Address concerns directly with the other party in good faith.
2. **Public discussion.** If direct conversation does not resolve the issue, the dispute moves to the public record (issue, PR thread, discussion forum).
3. **Maintainer mediation.** Once Phase G1, maintainers may intervene to mediate, clarify principles, or render decisions.
4. **Advisory review.** Phase G1+ — Advisory council can be asked to weigh in on strategic or principled disagreements.
5. **Board adjudication.** Phase G2+ — Board handles serious governance or fiduciary matters.
6. **Independent audit.** For allegations of principle violations not resolved internally, independent audit can be invoked.
7. **Fork.** If the dispute is fundamental and unresolvable, the ultimate recourse is to fork. This is legitimate, not shameful.

### Time expectations

- Routine disputes: resolved within days.
- Strategic disputes: resolved within weeks (public discussion takes time).
- Principled disputes: may take longer; the correct answer is more important than speed.
- Interpersonal/conduct disputes: addressed urgently where safety is at stake.

---

## Meta-Governance: How This Document Changes

This document itself is subject to governance.

**Minor revisions** (clarifications, typos, adding examples): handled through normal PR process. Maintainers approve.

**Significant changes** (changing decision processes, authority allocations, phase triggers): require public proposal, minimum two-week community review, and maintainer approval. Once Phase G2 is reached, requires board review.

**Changes that affect the immutable core or mission guardian structure:** Not permitted. See "The Immutable Core."

---

## Accountability Without Corruption

Governance structures corrupt in predictable ways:

- **Accreted authority:** Individuals accumulate authority beyond their formal role.
- **Informal networks:** Real decisions happen in private before formal bodies ratify them.
- **Perpetual incumbency:** The same people hold the same positions for too long.
- **Gatekeeping:** Access to participation becomes progressively narrower.
- **Capture:** External parties (funders, regulators, states) redirect the institution.

We attempt to mitigate each:

- **Accreted authority:** Role descriptions are explicit. When informal authority emerges, formalize or dissolve it.
- **Informal networks:** Radical transparency. Material decisions in public. Minutes published.
- **Perpetual incumbency:** Term limits on significant governance roles (once G2).
- **Gatekeeping:** No gatekeeping on contribution — anyone can submit work. Quality criteria apply, but affiliation does not.
- **Capture:** Jurisdictional diversification, mission guardians, fork rights, structural separation between commercial and mission sides.

None of these is a complete defense. All of them together is better than none.

---

## A Note on Islamic Governance Tradition

Islamic tradition offers rich governance precedents we draw on:

- **Shura (consultation):** The Prophet Muhammad, peace be upon him, consulted his companions on material decisions. Governance that does not consult is governance that fails.
- **Ijma' (consensus):** Major decisions traditionally required broad scholarly consensus, not individual decree.
- **Waqf (endowment) governance:** Strict fiduciary duty to the waqf's stated purpose, across generations.
- **Transparency of judgment:** Islamic judges traditionally articulated the reasoning for their decisions so that the ruling itself could be evaluated.
- **Multiple madhhabs (schools) coexisting:** Pluralism within a shared framework is the Muslim jurisprudential norm.

We are not implementing a historical Islamic governance system directly. We are applying its wisdom to a contemporary institution.

---

## Closing

Governance is boring in good times and existential in bad times. We invest in it now, while the stakes are low, so that when a real challenge comes — a capture attempt, a funding crisis, a founder's death — the institution survives.

No structure guarantees mission integrity. Only sustained commitment to the principles guarantees it. Governance is a scaffold, not a substitute.

The people in the structure matter more than the structure itself. Choose contributors, advisors, board members for their principles. Everything else is procedural.

*Al-Hukmu lillah.* — *The judgment belongs to Allah.*
