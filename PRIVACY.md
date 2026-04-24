# Privacy Commitment

## What We Collect, What We Don't, and How We Protect It

This is the Ummah AI Initiative's user-facing privacy commitment. It governs every current and future product released under the initiative, unless a specific product publishes a more protective policy (never a less protective one).

This is not a legal compliance document dressed up with friendly language. This is a statement of what we believe, architected into how we build.

---

## Our promises

### 1. We minimize collection

We do not collect data we do not need. The safest data is data that does not exist. Every field we collect answers the question: *"What harm would occur if this were compromised — and is the benefit worth that risk?"*

We will always publish, for each product:
- What data is collected.
- Why it is collected.
- How long it is retained.
- Who has access.

### 2. We do not sell data

We do not sell, rent, trade, or monetize user data. Not as a headline product. Not as a side business. Not through data brokers. Not in the form of "anonymized" datasets. Not ever.

Revenue comes from people paying us for services they find valuable — not from turning users into a commodity.

### 3. We protect data at the architecture level

Where it is technically feasible, sensitive data is protected such that we ourselves cannot read it:

- **End-to-end encryption** for communications where the threat model warrants.
- **Client-side processing** for sensitive queries, where feasible.
- **Zero-knowledge architectures** for users under severe threat (activists, journalists in hostile jurisdictions).

Where end-to-end is not feasible, we use strong encryption at rest and in transit, and we restrict internal access to those who strictly need it.

### 4. We minimize retention

Data is deleted on a schedule. The default is: the shortest retention consistent with the service's operation.

Users can delete their data at any time. When data is deleted, it is deleted — not marked hidden in a database that still holds it.

Backup retention follows the same principle: backups age out on a defined schedule, and deleted data does not persist indefinitely in backup form.

### 5. We choose jurisdiction carefully

Where we host services, and where we incorporate, affects what governments can compel of us. We prefer jurisdictions with:

- Strong legal protections for privacy and expression.
- Judicial independence.
- No reciprocal data-sharing obligations with governments that target users we serve.

We publish our current hosting and incorporation choices. We document when they change and why.

### 6. We are transparent about legal process

When we receive legal demands for user data, our defaults are:

- **Challenge overbroad requests.** Not every subpoena is valid. Not every valid subpoena is enforceable against us.
- **Narrow compliance.** If we must comply, we provide the minimum the law requires.
- **Notify affected users** where legally permitted. Where we are gagged, we publish a **warrant canary** that signals when a gagged request has been received.
- **Publish transparency reports** at least annually, summarizing the types and numbers of requests received and our responses.
- **Refuse to build surveillance capabilities** that do not exist. "Technical impossibility" is an acceptable answer to a demand for a capability we do not have. Where we can structure systems so that demands are technically impossible to comply with, we do.

### 7. We protect vulnerable users specifically

Users in certain categories face elevated risks: activists, journalists, dissidents, members of surveilled minorities, survivors of harassment. For these users:

- We provide features designed for their threat model (pseudonymous use, enhanced encryption, panic controls).
- We do not require identifying information we do not strictly need.
- We design defaults assuming a sophisticated adversary.
- Where we receive legal process targeting such users, we give extra scrutiny and maximum transparency consistent with the law.

### 8. We are honest about limits

No system is perfect. We will experience:
- Security incidents.
- Legal orders we cannot refuse.
- Unforeseen vulnerabilities.
- Errors.

When these happen, we will:
- Tell affected users.
- Publish an incident report.
- Document what failed and what we changed.
- Not pretend it didn't happen.

---

## What we do NOT promise

Honesty about what privacy we can actually deliver:

### We cannot promise perfect anonymity

Any service on the modern internet reveals IP addresses, device characteristics, timing patterns, and other metadata. We minimize what we can minimize; we cannot eliminate what is inherent to the medium. Users whose threat model requires true anonymity should use Tor, Tails, or equivalent tools — which our counter-surveillance project (Hisn) will help with.

### We cannot promise to defeat state-level adversaries absolutely

A determined state with sufficient resources can compromise any system. Our goal is to make compromise expensive, observable, and attributable — not impossible in principle.

### We cannot promise compliance with every possible regulation

We will comply with the laws of jurisdictions where we operate. If a jurisdiction's law requires us to violate these privacy commitments or our principles, our first response is to challenge the law; our second response is to withdraw service from that jurisdiction; our last response is not to comply with laws that require injustice.

This is not a light commitment. It may mean losing users, revenue, or physical presence in jurisdictions. We accept that cost.

### We cannot promise that this policy never changes

Policies evolve. When material changes are proposed, they will be:
- Announced with advance notice.
- Discussed in public.
- Never applied retroactively to reduce protection for data already collected.

---

## Product-specific policies

Each product will publish its own, more detailed policy covering its specific data handling. Those policies must be **at least as protective** as this overall commitment. They may be more protective; they may not be less.

### Basira (Islamic research copilot)
Will publish its policy at launch, covering: query logs, user accounts, scholar-review workflow, and institutional customer data handling.

### Shahid (atrocity documentation archive)
Will publish a policy covering source protection, witness data handling, and zero-knowledge architecture for sensitive submissions.

### Hisn (counter-surveillance toolkit)
Will publish a policy covering how its tools handle user configuration, and explicitly disclaims collection of user data where architecturally possible.

### Mizan (algorithmic accountability auditor)
Will publish a policy covering researcher data, any whistleblower channels, and the clear separation between audit data (often public) and source identity data (protected).

---

## Your rights

As a user of services released under this initiative, you have the right to:

1. **Know what data we hold about you.** Ask, and we tell you within 30 days.
2. **Correct inaccurate data.**
3. **Delete your data.** Subject to the minimum retention required by law (which is usually narrower than providers pretend).
4. **Export your data** in a machine-readable format.
5. **Object to processing** you consider inappropriate.
6. **Lodge complaints** with regulators in your jurisdiction.
7. **Receive notification** of incidents affecting your data, where legally permitted.
8. **Be free from retaliation** for exercising these rights.

These rights are granted in addition to whatever your local data protection laws grant you. Where local law grants more, you get more.

---

## For activists, journalists, and others under threat

If you are using our services under an elevated threat model, read this section carefully:

- **Account creation:** You may use pseudonymous email addresses (ProtonMail, Tutanota, disposable services). You may use Tor.
- **Browser/app configuration:** Use the most privacy-preserving options we offer. Instructions are published per-product.
- **Operational security:** Our Hisn project provides guides specific to your threat model.
- **If you suspect compromise:** Contact us via encrypted channels (detailed in `SECURITY.md`). Do not use the suspected-compromised account.
- **If you are detained:** Our warrant canary and transparency reports will reflect any legal process we have received. Access to canary status is typically possible through mirrors and archives.

We are a small initiative, not a professional security provider. Our best efforts are real but limited. For the most severe threat models, combine our tools with others (Signal, Tor, Tails, established NGOs like Access Now).

---

## Questions

Privacy questions may be raised as public issues, sent to the project contact (once established), or addressed through the security channel for sensitive matters (`SECURITY.md`).

We prefer public questions where possible. Public answers help others with the same concern.

---

## Governance

This policy is reviewed at least annually and whenever material changes are proposed. Proposed changes are published for community review per the governance process in `docs/06-governance.md`.

The Layer 4 protections described here and in `docs/03-open-vs-closed.md` and `docs/07-non-negotiables.md` (NN-4) are structural commitments. They cannot be removed from this policy without simultaneously removing the corresponding principle — which is not permitted.

In other words: you can rely on these protections not as marketing, but as enforceable commitments woven into the constitution of the initiative.

---

## Closing

Privacy is not a feature. It is a form of respect. The people who use our services — especially those whose safety depends on them — deserve technology that treats their data as what it is: a record of their life, to be held only with permission and only as long as necessary.

We will not always get it perfectly right. We commit to trying, to being transparent when we fail, and to improving continuously.

*Wa Allahu a'lam* — and Allah knows best.
