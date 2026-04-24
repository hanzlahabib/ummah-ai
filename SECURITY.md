# Security Policy

## Reporting Vulnerabilities and Safety Concerns

The Ummah AI Initiative takes security seriously because our systems are designed to be used by people whose safety depends on them. This policy governs how security issues are reported, handled, and disclosed.

---

## Scope

This policy applies to:

- All code in this repository (and future repositories under the initiative).
- Deployed services operated under the initiative (e.g. Basira, Shahid, Hisn, Mizan — once live).
- Infrastructure the initiative controls (domains, servers, accounts).
- Integrations with third-party services where our configuration affects user safety.

This policy **does not** apply to:

- Third-party services we integrate with but do not control (report to those providers).
- Forks of our work operated by others (report to the operators of that fork).
- Non-security bugs, feature requests, or content concerns (use normal issue channels).

---

## What Qualifies as a Security Issue

In addition to standard software vulnerability categories (injection, authentication bypass, privilege escalation, etc.), the following are explicitly in scope for this project:

- **User de-anonymization** — Any way that a user's identity can be linked to their activity, especially for activists, journalists, or users in hostile jurisdictions.
- **Data exfiltration paths** — Any way that Layer 4 data (see `docs/03-open-vs-closed.md`) could be accessed by unauthorized parties.
- **Supply chain compromise** — Issues in our dependencies, build pipelines, or distribution that could be exploited.
- **Model safety issues** — Ways our AI models could be manipulated to produce harmful content, leak training data, or enable misuse.
- **Content integrity failures** — Ways our scholarly content could be corrupted, backdoored, or manipulated such that users receive false information.
- **Social engineering surfaces** — Ways the project's identity, branding, or trust could be exploited to harm users (phishing, impersonation, etc.).
- **Infrastructure compromise** — Ways our hosting, accounts, or credentials could be taken over.

---

## How to Report

### Standard reports

For most issues, email: **security@ummah-ai.org** *(placeholder — an actual address will be established when the initiative incorporates; in the interim, open a GitHub issue marked with the `security` label, or reach a maintainer directly).*

Include:

1. A clear description of the issue.
2. Steps to reproduce, if applicable.
3. Potential impact — who could be harmed and how.
4. Your suggested fix, if you have one.
5. Whether you wish to be credited publicly.

### High-sensitivity reports

For issues involving:
- Active exploitation in the wild
- Compromise of user identities or activist data
- Vulnerabilities affecting at-risk populations (Palestinian, Uyghur, or other populations under severe threat)

Use encrypted communication. **PGP public key:** *(placeholder — will be published when security lead is established).*

If you cannot use PGP, state that the issue is sensitive in your initial message and a secure channel will be established before you share details.

### Anonymous reports

We accept anonymous reports. You do not need to identify yourself to get us to fix a security issue. Contact via:
- A disposable email address.
- A pseudonymous identity (e.g. a GitHub account created for the purpose).
- A trusted third party who forwards on your behalf.

We will not attempt to unmask anonymous reporters.

---

## Response Process

### Acknowledgment

We aim to acknowledge reports within **72 hours** of receipt. If you do not hear back within that window, resend — mail can be lost.

### Triage

Within **7 days** of acknowledgment, we will:

- Confirm whether the issue is in scope.
- Assess severity (see Severity Levels below).
- Assign a handler.
- Communicate a rough timeline.

### Fix timeline targets

These are targets, not guarantees. Severe and complex issues may take longer.

| Severity | Target timeline |
|---|---|
| **Critical** (actively exploited; threatens life or severe safety) | As fast as technically possible; days |
| **High** (serious compromise possible; no active exploitation) | 7–30 days |
| **Medium** (meaningful compromise under specific conditions) | 30–90 days |
| **Low** (theoretical; limited real-world impact) | 90+ days or next major release |

We will communicate delays proactively.

### Disclosure

We follow **coordinated disclosure**:

1. We investigate and fix the issue.
2. We prepare remediation guidance for affected users (if any).
3. We publish a security advisory after the fix is deployed — typically within 90 days of the report, or sooner if the issue is low-sensitivity.
4. Reporters are credited in the advisory unless they request otherwise.
5. CVE identifiers are assigned where applicable.

We will **not** demand silence from reporters. If you feel we are not responding adequately, you have the right to disclose publicly after a reasonable period. We ask that you give us a fair opportunity to fix issues before public disclosure, for the sake of affected users — but this is a request, not a demand.

---

## Severity Levels

### Critical
- Active exploitation observed.
- User identities or Layer 4 data exposed or at imminent risk.
- Remote code execution in production systems.
- Issues that could lead to arrest, injury, or death of users.

### High
- Unauthenticated access to sensitive functionality.
- Significant data exposure possible.
- Authentication bypass.
- Supply chain compromise affecting production.

### Medium
- Authenticated vulnerabilities with meaningful impact.
- Issues exploitable only under specific conditions.
- Information disclosure of moderate sensitivity.

### Low
- Hardening opportunities.
- Theoretical issues with no practical exploitation path.
- Non-sensitive information disclosure.

---

## Rewards and Recognition

We currently do not operate a paid bug bounty program. This will change once the initiative has sustainable revenue.

In the interim:

- Public credit in security advisories (if desired).
- Listing in a contributors' acknowledgment section.
- For severe issues affecting at-risk populations, we will do our best to arrange a symbolic reward (e.g. donation to a human rights organization in the reporter's name).

If you are a security researcher working on this project and would benefit from a formal acknowledgment for your professional record (e.g. for academic or industry purposes), ask and we will accommodate.

---

## Safe Harbor

We will not pursue legal action against researchers who:

- Follow this policy in good faith.
- Do not access, modify, or destroy data beyond what is necessary to demonstrate the issue.
- Do not exploit issues against real users or cause service disruption.
- Report issues before public disclosure.

This is not a blanket legal waiver — we cannot waive rights of users or third parties, and we cannot protect researchers against actions by authorities outside our control. But within what we can offer, researchers acting in good faith are safe.

---

## Dependencies and Supply Chain

We take dependency hygiene seriously:

- Critical dependencies are tracked.
- Automated vulnerability scanning runs on all repositories (once CI is established).
- We prefer fewer, well-vetted dependencies over many casual ones.
- Security-sensitive components (cryptographic libraries, authentication code) use established implementations; we do not roll our own.

If you identify a vulnerable dependency, report it as you would any other security issue.

---

## Hosting, Jurisdiction, and State-Level Threats

Our threat model explicitly includes state-level actors attempting to compel or exploit access to our infrastructure (see `docs/08-project-threat-model.md`).

Our defenses:

- **Jurisdictional diversification** — hosting and legal presence structured to reduce single-jurisdiction coercion risk.
- **Zero-knowledge architectures** where feasible — even we cannot access the most sensitive user data.
- **Transparent commitments** about what subpoenas we will and will not comply with, and under what conditions we will notify users (except where gagged by law).
- **Warrant canaries** where legally permitted.

If you identify a weakness in these defenses, treat it as a security issue and report it accordingly.

---

## Incident Communication

In the event of a confirmed incident affecting users:

1. Affected users are notified directly, where feasible.
2. A public incident report is published within 30 days.
3. Root cause, remediation, and preventative measures are documented.
4. If user data was exposed, guidance on personal mitigation is provided.

We will not quietly brush incidents aside. Transparency about failures is a core commitment.

---

## What NOT to Do

Please do **not**:

- Exploit vulnerabilities against real users.
- Perform denial-of-service testing against production systems.
- Access data beyond what is necessary to demonstrate the issue.
- Publicly disclose before giving us a reasonable chance to respond.
- Use threats, blackmail, or other coercive tactics.

Actions that cross these lines are not "research" and the safe harbor does not apply.

---

## Governance of This Policy

This policy is reviewed at least annually, and whenever the threat landscape changes materially. Proposed changes are discussed in public.

The security lead (once appointed) has operational authority over incident response. Strategic security decisions — including changes to infrastructure, jurisdictional choices, and disclosure practices — are made through the governance process described in `docs/06-governance.md`.

---

## Closing

Security is not a feature. It is a continuous practice. We welcome partnership with researchers, security-focused contributors, and organizations who share our commitment to protecting the people who trust this work.

If you have expertise to offer, consider contributing beyond individual reports. The initiative needs ongoing security review as it grows.

*Jazakum Allahu khayran* — may Allah reward you with good — for any effort you make toward keeping our users safe.
