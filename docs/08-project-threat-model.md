# Project Threat Model

## Threats to the Initiative Itself — and How We Prepare

`docs/01-why-this-exists.md` documents the external AI-driven threats the initiative exists to address. This document addresses the inverse question: **what threatens the initiative itself, and what defenses do we build in advance?**

A project with this mission will attract adversaries. Anticipating that in the foundation phase — when we are small and the cost of preparation is low — is far better than reacting after a compromise.

Every threat listed here is realistic. None is paranoid speculation. Similar projects in the human rights, anti-surveillance, and minority-rights spaces have experienced all of these.

---

## Adversary Classes

We model seven classes of adversary. Most real attacks combine multiple classes.

### A1 — State actors with offensive capabilities

Governments with demonstrated willingness to use intelligence, legal, or coercive means against projects they consider inconvenient.

Examples of concern:
- States documented as targeting civil society, journalists, or minority populations with offensive cyber capabilities.
- States that maintain reciprocal legal-assistance relationships with governments that would target our users.
- States where our hosting providers or legal presence could be compelled by local law.

Possible actions:
- Legal demands for user data (subpoenas, national security letters, warrants).
- Infrastructure compromise (hosting providers, registrars, cloud).
- Supply chain attacks on dependencies or build pipelines.
- Malware targeting contributors' devices.
- Coercion or detention of contributors in the state's jurisdiction.
- Denial-of-service, account suspension, or deplatforming.

### A2 — Non-state hostile actors

Groups that may target the initiative for ideological reasons — sectarian extremists, anti-Muslim movements, commercial competitors with weak ethics.

Possible actions:
- Doxxing and harassment campaigns against contributors.
- Attempted defacement or content poisoning.
- Coordinated inauthentic behavior campaigns to discredit the initiative.
- Infiltration attempts to introduce bad content.

### A3 — Sectarian or factional interests

Parties within the broader Muslim community whose agenda is narrower than ours.

Possible actions:
- Attempted capture of the project to serve their sectarian position.
- Manufactured controversies positioning the project as "on the wrong side" of some internal dispute.
- Pressure campaigns to exclude contributors from certain backgrounds.
- Attempts to manipulate scholarly verification workflows to produce biased content.

### A4 — Commercial capture attempts

Funders, investors, or acquirers who want the initiative's capabilities redirected toward their priorities.

Possible actions:
- Offering capital with strings that slowly shift priorities.
- Acquiring controlling commercial subsidiary.
- Offering lucrative contracts that require compromise on principles.
- Cultivating dependent relationships that make refusal costly.

### A5 — Malicious insiders

Contributors, employees, or trusted partners who act in bad faith — either from the start (infiltration) or through later corruption.

Possible actions:
- Exfiltrating Layer 4 data.
- Introducing backdoors or subtly poisoned content.
- Leaking internal deliberations to hostile parties.
- Using project access for personal benefit at the cost of mission.

### A6 — Criminal opportunists

Non-ideological actors seeking financial or reputational gain.

Possible actions:
- Ransomware against infrastructure.
- Account takeovers for resale.
- Impersonation and scams using the initiative's brand.
- Generic phishing of contributors.

### A7 — Reputational weaponization

Any actor — state, non-state, commercial, sectarian — using public narrative to damage the initiative without requiring direct compromise.

Possible actions:
- Claiming affiliation with adversarial groups the initiative has no actual connection to.
- Misrepresenting positions or quoting selectively.
- Exploiting moments of controversy to apply pressure to partners, funders, or platforms.
- Manufacturing evidence of wrongdoing.

---

## Threat Scenarios

Representative scenarios, not exhaustive.

### Scenario 1: Subpoena for user data

A government issues a valid (or invalid-but-presented-as-valid) legal demand for identifying information about a user of Basira or Shahid. The user is a journalist documenting human rights issues.

**Pre-built defenses:**
- Data minimization — we did not collect the linkable information to begin with.
- Zero-knowledge architecture where feasible — we cannot decrypt what we never held keys to.
- Jurisdictional structure — legal entity located where protection is strong.
- Legal counsel engaged who will challenge overbroad requests.
- Warrant canary published publicly.

**Reactive procedure:**
1. Assess validity and scope.
2. Narrow compliance to legal minimum.
3. Challenge if grounds exist.
4. Notify user if legally permitted.
5. Transparency report includes the event.

### Scenario 2: Infrastructure compromise

A hosting provider is breached, or a credential theft gives an attacker admin access to production.

**Pre-built defenses:**
- Principle of least privilege — admin credentials limited and logged.
- Hardware-token-based authentication for sensitive roles.
- Data encrypted at rest with keys managed outside the compromised system where possible.
- Offline backups across jurisdictions.
- Layer 4 data structurally inaccessible without user-held keys.

**Reactive procedure:**
1. Isolate affected systems.
2. Rotate credentials.
3. Assess data exposure.
4. Notify affected users per SECURITY.md.
5. Public incident report within 30 days.

### Scenario 3: Doxxing campaign

Contributor identities targeted for harassment, including threats to their families.

**Pre-built defenses:**
- Pseudonymous contribution explicitly supported.
- Contributors in high-risk jurisdictions encouraged to maintain operational security.
- Internal contact lists protected.
- No gratuitous public identification of contributors.

**Reactive procedure:**
1. Offer affected contributors support (security assistance, temporary leave, OPSEC review).
2. Remove any content the initiative published that contributed to exposure, where possible.
3. Coordinate with Access Now or similar for acute individual support.
4. Do not engage the harassers; do not amplify.

### Scenario 4: Capture attempt via major funding offer

A large potential funder offers capital sufficient to accelerate all four founding projects, but with terms that compromise principles (board control, exclusivity, content restrictions).

**Pre-built defenses:**
- Principles declared immutable; board cannot accept such terms.
- Mission guardians in governance specifically charged with resisting.
- Transparent decision-making makes quiet acceptance impossible.
- Multiple funding tracks reduce dependency on any single source.
- Public disclosure of funding sources makes inappropriate terms visible.

**Reactive procedure:**
1. Propose terms consistent with principles; accept if agreed.
2. Refuse if terms cannot be adjusted.
3. Publish the decision and reasoning.

### Scenario 5: Insider exfiltration

A trusted contributor or employee copies Layer 4 data with intent to leak or sell.

**Pre-built defenses:**
- Principle of least privilege — most contributors have no access to Layer 4 data.
- Audit logging of access to sensitive systems.
- Background review of contributors in high-trust roles.
- Cultural norms against unilateral action on sensitive data.
- Exfiltration-detection tooling where scaled services warrant.

**Reactive procedure:**
1. Revoke access immediately.
2. Assess extent of exposure.
3. Notify affected users.
4. Pursue legal and civil remedies where appropriate.
5. Public incident report.
6. Review and improve access controls.

### Scenario 6: Coordinated disinformation against the initiative

A narrative emerges — true or fabricated — positioning the initiative as aligned with a group we are not aligned with.

**Pre-built defenses:**
- Transparent public record of everything we have done.
- Public principles that are hard to misrepresent to anyone who reads them.
- Coalition of allies (scholarly, civil-society, media) who can vouch.
- Relationships with reputable journalists for accurate reporting.

**Reactive procedure:**
1. Determine whether the narrative is based on genuine concern (which may warrant correction or apology) or manufactured (which warrants rebuttal with evidence).
2. Respond publicly with facts, not emotion.
3. Enlist allies to amplify correct information.
4. Do not take the bait of extended public arguments with bad-faith actors.

### Scenario 7: Jurisdictional expulsion

The country of incorporation or primary hosting changes its laws or enforcement such that continued operation there would require violating principles.

**Pre-built defenses:**
- Legal structure designed for relocatability.
- Documentation, code, and data replicated across jurisdictions.
- Contributor network geographically distributed.
- Domain and DNS configuration capable of rapid redirection.

**Reactive procedure:**
1. Migrate legal entity and infrastructure to alternative jurisdiction.
2. Notify users of any service impact.
3. Resume operation as quickly as feasible.

### Scenario 8: Founder loss

The initial founder becomes unavailable — illness, death, arrest, coercion, or voluntary departure.

**Pre-built defenses (from Phase G0):**
- Repository access distributed among at least three trusted individuals from the earliest possible moment.
- All foundational documents public and under open licenses (so they survive any individual).
- Written "bus factor" document listing trusted continuators and their contact information.
- No critical operational secret held by only one person.
- Infrastructure accounts structured so that at least two of the trusted group can recover access.

**Reactive procedure:**
1. The remaining trusted group acknowledges the situation publicly where appropriate.
2. One member assumes operational lead on interim basis.
3. Within 90 days, governance review decides next steps — appoint new lead, restructure, or transition to community governance per the current Phase's rules.
4. Fork rights remain available if any disagreement renders the transition contested.

---

## Structural Defenses

Beyond scenario-specific responses, the initiative builds structural defenses into its foundation:

### Geographic diversification
- Contributors distributed across multiple jurisdictions.
- Hosting across multiple providers in multiple countries.
- Legal structure avoiding single-jurisdiction vulnerability.
- Documentation mirrored so that loss of any single location does not erase the record.

### Identity and access
- Pseudonymous contribution supported.
- Multi-factor authentication required for sensitive roles.
- Hardware keys preferred over SMS or TOTP for critical accounts.
- Regular access review — who has access to what, does it still need to.

### Dependency hygiene
- Minimal dependencies where possible.
- Dependencies pinned and reviewed.
- Build pipeline reproducibility as a long-term goal.
- Sigstore or equivalent signing for releases.

### Communication
- Private communications via encrypted channels (Signal, Matrix with e2e, etc.).
- Sensitive contributor-to-contributor communications use forward-secret protocols.
- Meeting minutes that contain sensitive discussion are stripped of identifiers before publication.

### Legal
- Legal counsel engaged from the point of incorporation.
- Defense fund established once resources permit.
- Relationships with digital rights legal organizations (EFF, Access Now, etc.) for rapid response.

### Narrative
- Consistent public communication rooted in the foundation documents.
- Prepared messaging for predictable reputation attacks.
- Relationships with journalists who cover these issues with integrity.

### Continuity
- Bus factor documented and regularly updated.
- Succession planning explicit, not improvised.
- Multiple custodians for critical accounts and secrets.
- Regular drills once infrastructure is nontrivial.

---

## What We Will Not Do, Even Under Pressure

Certain responses are tempting under pressure but off-limits.

- **We will not build backdoors.** Even if asked by a "friendly" government, even under the argument that the capability would only be used against "real bad actors."
- **We will not install surveillance in our products.** Even if asked by a commercial partner offering substantial revenue.
- **We will not quietly censor content to placate hostile parties.** If content must be removed for principled reasons (e.g. accuracy), we remove it transparently.
- **We will not take capital with terms that compromise principles,** regardless of the amount.
- **We will not reveal pseudonymous contributors,** regardless of who asks or under what argument, unless the contributor themselves authorizes.
- **We will not build lists of users by attribute** (religion, politics, sect, geography) for purposes beyond legitimate operation.
- **We will not assist any party in identifying, tracking, or targeting individuals** based on their use of our services.

These are not hypothetical constraints. They are the constraints under which similar projects in the past have failed. We commit to them in advance so that the moment of pressure finds us prepared.

---

## Updating This Document

This threat model is living. It is updated:

- Annually as routine review.
- Whenever a new threat is observed (either to this project or to comparable projects).
- Whenever our structure changes materially (new jurisdiction, new product, new partnership).

Updates follow the governance process in `docs/06-governance.md`. Significant changes are discussed publicly.

---

## Closing

No defense is perfect. The goal is not invulnerability. The goal is:

- To make attacks observable, so they can be contested.
- To make attacks expensive, so casual adversaries do not bother.
- To make attacks attributable, so the cost falls on the attacker.
- To make the institution survivable, so no single attack ends the mission.

We build the defenses in advance because the moments when they are needed will not leave time to build them.

*Fa in tawallaw fa qul hasbi Allahu; la ilaha illa Huwa; 'alayhi tawakkaltu; wa Huwa Rabb al-'Arsh al-'Azim.*

— *"If they turn away, say: Allah is sufficient for me. There is no god except Him. Upon Him I have relied, and He is the Lord of the Great Throne."* — Surah At-Tawbah 9:129
