# Core Principles

These principles are **immutable**. They cannot be amended, reinterpreted, or set aside — not by the founder, not by any future leader, not by any corporation, government, or majority vote. If you cannot accept them, do not contribute. If the project ever violates them, it has ceased to be this project, and it should be forked by whoever can uphold them.

Every principle below is stated with its reasoning (`dalil`) so that future generations understand *why*, not only *what*.

---

## Principle 1 — Non-Sectarian Unity (`wahdat al-ummah`)

**The rule:**
This project is open to every Muslim without distinction of sect, school of thought, madhhab, tariqah, or theological affiliation. Sunni, Shia, Ibadi, Salafi, Sufi, traditionalist, modernist, convert, born-Muslim — all are welcome as equal contributors. Non-Muslims of goodwill who accept these principles are welcome as allies and contributors.

**What this means in practice:**
- No contributor may be excluded for their sect, madhhab, or school.
- No contributor may use this project as a platform to denounce, excommunicate (`takfir`), or disparage other Muslim sects.
- Theological disputes stay outside the repository. Technical and organizational work happens inside.
- Content produced by the project (e.g. Islamic research tools) must represent multiple scholarly traditions fairly, cite sources transparently, and never pretend one school's position is the only Muslim position.

**Why:**
The Ummah is weakened by sectarianism more than by any external enemy. Our predecessors — when they cooperated across sect — produced civilizations. When they fragmented, they fell. The Prophet Muhammad, peace be upon him, said: *"The believers in their mutual kindness, compassion, and sympathy are like one body. When one limb suffers, the whole body responds."* (Sahih Muslim). This project honors that hadith operationally, not just rhetorically.

---

## Principle 2 — Radical Transparency (`wuduh`)

**The rule:**
Every decision, every line of code, every strategic document, every governance process, and every material disagreement happens **in the open**, in writing, where any human being on earth can read it.

**What this means in practice:**
- No private strategy documents that are not later published.
- No closed-door decisions of material consequence.
- Disagreements are resolved in public threads, not backchannels.
- Contributor identities may be pseudonymous for safety, but reasoning is always public.
- Commercial products built on top of the ecosystem may have private business logic, but their existence, purpose, and relationship to the ecosystem are publicly disclosed.

**Why:**
Secrecy is how institutions are captured. Transparency is how they are defended. The moment decisions move behind closed doors, the project stops belonging to the Ummah and starts belonging to whoever is behind those doors. Our inspiration is Bayt al-Hikmah — where scholars translated, published, and debated openly, with no hidden curriculum.

Additionally: if we are asking people to trust AI systems with their religious learning, their dignity, and (in defensive contexts) their lives, that trust is **only possible through auditable openness**. Closed systems cannot be trusted no matter how well-intentioned their builders claim to be.

---

## Principle 3 — Human Dignity Absolutism (`karamat al-insan`)

**The rule:**
We will never knowingly build technology that harms innocent people. Not for any customer, any government, any amount of money, any strategic consideration.

**What this means in practice:**
- **Prohibited work:** surveillance systems used against civilians; autonomous or targeting weapons; tools for suppressing dissent, journalism, or religious practice; deepfake generation for deception; biometric databases for identification without consent; social scoring systems.
- **Prohibited customers:** regimes, militaries, or corporations with documented records of targeting civilians — even if a specific contract appears "benign." The reputational and material risk of being used as a laundromat is too high.
- **Defensive work is permitted and encouraged:** detection of harms, documentation of abuses, protection of targets, accountability tooling.
- When in doubt, err on the side of refusing the work.

**Why:**
Quran 5:32 — *"Whoever saves a life, it is as though he had saved all of humanity."* The inverse is also true. We cannot build civilizational infrastructure for the Ummah by participating in technology that destroys innocent lives, whether those lives are Muslim or not.

The temptation will come. Large contracts will be offered. "Just this one exception" will be proposed. **The answer is always no.** The principle is absolute because the moment it becomes conditional, it no longer exists.

---

## Principle 4 — Knowledge as Trust (`al-ilmu amanah`)

**The rule:**
Knowledge is a trust from Allah to humanity. It is to be shared for benefit, not hoarded for advantage. Our defaults lean open.

**What this means in practice:**
- Research, standards, methodologies, evaluation frameworks, and educational materials are **always open**.
- Developer tools, foundational libraries, and infrastructure that benefits the Muslim developer ecosystem are **default open**.
- Production systems, fine-tuned models, and specific commercial products may be **proprietary**, as commercial sustainability is necessary to fund the mission.
- Human data — user information, activist identities, sensitive relationships — is **fortress-protected**. This is not about secrecy; it is about protecting people.

(The detailed framework is in `docs/03-open-vs-closed.md`. This principle sets the disposition; that document sets the mechanics.)

**Why:**
The Ottoman printing press ban (1485–1729) is instructive: under the pretext of "protecting" sacred knowledge, Muslim scholars delayed the printing press in Muslim lands by nearly three centuries. Europe printed, translated, and built on inherited knowledge while Muslim civilization stagnated. The "protection" became the wound.

The correct Muslim historical posture is Bayt al-Hikmah: translate everything, publish everything, share everything, build on everything. Knowledge withheld is knowledge that dies.

---

## Principle 5 — Sincerity of Intent (`ikhlas`)

**The rule:**
The work is done for the benefit of the Ummah and of humanity, with accountability ultimately to Allah. Personal livelihood is legitimate. Personal aggrandizement is not the mission.

**What this means in practice:**
- Contributors may earn compensation, build careers, and found companies on top of this ecosystem. This is halal and encouraged.
- Contributors may not use the project primarily as a vehicle for personal fame, political power, or the elevation of their individual brand above the mission.
- Credit is given openly; plagiarism and credit-theft are grounds for exclusion.
- The mission is not tied to the founder or any individual. No one is indispensable.
- **Financial extraction is structurally prevented.** The founder's lifetime return from the initiative is capped (per `FINANCIAL_MODEL.md`), the majority of value at scale flows to mission work, and contributors are not extracted from — their work is theirs (MIT / CC BY-SA licensed), paid paths exist, and the commercial entity cannot quietly become a vehicle for private wealth.

**Why:**
Every great institution eventually faces the test of whether it exists to serve its mission or to serve its leaders. This principle is a reminder that the answer must always be the former. When in doubt about a decision, ask: *"Would I make this choice if no one would ever know I made it?"*

The financial model is not a side-commitment. It is the concrete test of this principle. A mission whose founder is quietly enriched, whose contributors are extracted from, or whose profits accumulate to a small number of insiders has failed the `ikhlas` test regardless of what it says on paper. See `FINANCIAL_MODEL.md` for the structural commitments that make this principle enforceable rather than aspirational.

---

## Principle 6 — Excellence (`ihsan`)

**The rule:**
Build as if Allah is watching, because He is. Poor quality, sloppy work, and shortcuts that compromise outcomes are not acceptable.

**What this means in practice:**
- Code is reviewed. Tests exist where they should. Security is taken seriously.
- Documentation is written for future contributors who have no context.
- Scholarly content is verified by qualified scholars before publication.
- When we publish something, it represents the Ummah. Shoddiness reflects on all Muslims.

**Why:**
*"Verily, Allah loves that when any of you does a job, he does it with ihsan (excellence)."* — (Al-Bayhaqi). We have no right to embarrass the tradition we claim to serve by producing inferior work.

---

## Principle 7 — Long-Term Horizon (`sabr`)

**The rule:**
This is a multi-generational project. We build patiently. We do not sacrifice principles for speed, scale, or short-term wins.

**What this means in practice:**
- Decisions are evaluated by their impact in ten years, not ten months.
- Fundraising, if pursued, favors mission-aligned capital over the largest check.
- Partnerships are evaluated for durability and values-alignment, not quarterly metrics.
- Burnout is treated as a failure of strategy, not a badge of honor. Sustainable pace.

**Why:**
Salah ad-Din al-Ayyubi spent approximately twenty years building the economic, military, and intellectual infrastructure of his realm before the opportunity arose to recover Jerusalem. He was not passive during those twenty years. He was building the conditions under which his eventual goal became possible.

We follow that model. Building conditions. Accumulating capacity. Acting when the opportunity is real.

---

## Principle 8 — Fork Rights (`haqq al-inshiqaq`)

**The rule:**
If this project ever violates its own principles, any contributor has the full moral and legal right to fork the codebase, the documentation, and the mission, and continue the work elsewhere under the same principles. The open licenses guarantee the legal right; this principle affirms the moral right.

**What this means in practice:**
- Contributors are never obligated to remain with an institution that has become corrupt.
- "Loyalty to the institution" is not a principle. Loyalty to the *mission* is.
- The founders explicitly invite future forks if forks are necessary. No ego, no hurt feelings, no retaliation.

**Why:**
No institution is permanent. Every institution eventually fails, corrupts, or becomes captured. By naming the fork right as a founding principle, we hope to make capture less attractive (since the principled can always leave) and recovery more orderly (since forking is understood as legitimate, not betrayal).

---

## Meta-Principle: These Principles Bind the Founder Equally

The person writing these words is subject to these principles. So is every future leader, contributor, funder, and partner. No one is above them. If the founder violates them, the founder should be removed or bypassed. This is stated explicitly so that no appeal to authority can be used to override principle.

---

## Closing

These principles are not original. They are derived from the Quran, the Sunnah, and the lived history of Muslim civilization at its best. We claim no innovation. We are merely applying old wisdom to a new infrastructure problem.

If we succeed, the credit belongs to the tradition. If we fail, the failure is ours.

*Wa billahi at-tawfiq.* — *And with Allah is all success.*
