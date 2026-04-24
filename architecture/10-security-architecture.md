# Security Architecture

## Authentication, Secrets, Encryption, and Defensive Design

This document describes the system-level security posture. It implements requirements from `../PRIVACY.md`, `../SECURITY.md`, `../docs/07-non-negotiables.md` (especially NN-4 and NN-8), and `../docs/08-project-threat-model.md`.

Security is not a feature bolted on. It is how the system is built. Deviations from this document are only acceptable with documented justification and governance review.

---

## Threat Model Summary

Full detail in `../docs/08-project-threat-model.md`. In architectural terms, we design against:

1. **State-level legal compulsion** (subpoenas, NSLs, foreign equivalents).
2. **State-level technical compromise** (targeted malware, infrastructure penetration).
3. **Criminal cyberattack** (ransomware, credential theft, supply chain).
4. **Insider threats** (malicious or coerced contributors/operators).
5. **Account takeover** (individual user compromise).
6. **Reputational attacks** that could compromise user trust.

The design principles: **data minimization, defense in depth, assume compromise, protect the vulnerable.**

---

## Authentication

### User authentication

Per ADR-008 (Better Auth):

- **Passkeys (WebAuthn) primary.** Hardware-backed; phishing-resistant.
- **TOTP (RFC 6238) fallback** for users without passkey support.
- **Email magic-link fallback** only for users who choose it explicitly, with clear understanding of reduced security.
- **No SMS authentication.** SIM-swapping makes SMS-based 2FA unacceptable for our threat model.
- **No passwords.** Passwords are a failure mode; we skip them.

### Session management

- Short-lived access tokens (1 hour) with rotation.
- Refresh tokens bound to device (WebAuthn attestation).
- Revocation on password reset, device loss, or user action.
- Concurrent session limits configurable per user.

### Operator authentication

Anyone with production access:

- **Hardware key required** (YubiKey or equivalent).
- **Separate auth** from user-facing auth (different domain, different secret store).
- **Role-based access control (RBAC)** — least privilege per role.
- **Just-in-time elevation** for sensitive operations — temporary, logged, reason-required.
- **No shared accounts.** Every access is attributable.

### Service authentication

- Services authenticate to each other via **short-lived mutual TLS** (mTLS) where possible, or signed JWTs bound to service identity.
- **No static bearer tokens** in service-to-service communication.
- **Secrets rotated** on schedule and on any suspected compromise.

---

## Secrets Management

Per ADR-011 (Infisical):

- **Single source of truth** for secrets — Infisical, self-hosted.
- **No secrets in git.** Pre-commit hooks (gitleaks) block accidental commits.
- **No secrets in environment files in production.** Services fetch from Infisical at startup.
- **Secret rotation** scheduled and automated where possible.
- **Audit log** of secret access, reviewed weekly.

### Critical secret categories

- Database credentials.
- LLM API keys.
- Stripe webhook secrets.
- Signing keys (content provenance, JWT signing).
- OAuth client secrets (where applicable).
- Infrastructure credentials (cloud provider, DNS).

Each category has a documented rotation policy.

---

## Encryption

### In transit

- **TLS 1.3** minimum for all external connections.
- **mTLS** for service-to-service connections within the trust boundary.
- **Certificate pinning** for mobile clients.
- **HSTS** with preload on all public domains.
- **No plaintext HTTP** — redirect to HTTPS; fail closed if TLS unavailable.

### At rest

- **Full-disk encryption** on all servers (LUKS for Linux hosts).
- **Database-level encryption** (Postgres transparent data encryption or file-level equivalent).
- **Column-level encryption** for especially sensitive fields (query text, email addresses, billing data).
- **Object storage** encrypted at rest (provider-level + optional client-side for highest-sensitivity buckets).

### Key management

- Master keys stored in **Infisical** (or hardware security module when budget permits).
- Data encryption keys derived from master keys; rotated on schedule.
- Backup keys stored offline in physically-separated location.
- Key rotation procedures documented and drilled.

### End-to-end encryption

Specific product features use E2E encryption:

- **Shahid sensitive submissions** — encrypted client-side before upload; we cannot decrypt.
- **Hisn private communications** (if any) — E2E via Signal-protocol or equivalent.
- **Basira high-threat mode queries** — processed ephemerally, never persisted in plaintext.

---

## Network and Infrastructure Security

### Perimeter

- **Cloudflare WAF** on public endpoints (DDoS protection, basic attack filtering, bot management).
- **Rate limiting** at multiple layers — edge (Cloudflare), API gateway, application.
- **IP-level blocking** for confirmed abuse; geo-blocks avoided because they penalize legitimate users (VPN, Tor).

### Internal network

- **Private networking** between services on Hetzner (vSwitch or cloud networks).
- **No public exposure** of database, cache, or internal services.
- **Service mesh** pattern as we grow (not needed at MVP scale).

### Tor support

- **Public content** (Basira free tier, Shahid public archive) accessible via Tor hidden service.
- No blocking of Tor users — they are often exactly the users we serve.
- CAPTCHA only where necessary for abuse prevention, in an accessible form.

---

## Application Security

### Input validation

- All external input validated against explicit schemas (Zod, similar).
- Database queries parameterized; no string concatenation into SQL.
- HTML output properly escaped in React (default); any raw HTML flagged and reviewed.
- File uploads validated by content (not just extension) and size-limited.

### Output safety

- LLM output validated against safety filters before display (prompt injection resistance, content safety).
- User-generated content (where applicable) sanitized before display and before inclusion in context windows.
- Content policy (`../docs/09-content-policy.md`) enforced at the generation layer.

### Dependency hygiene

- Dependencies pinned (lockfiles committed).
- Automated vulnerability scanning (Dependabot, Snyk, or equivalent) on every PR.
- Security advisories reviewed weekly.
- Critical updates applied promptly; non-critical on schedule.

### Build and release security

- **Reproducible builds** where technically feasible (goal for Phase 2+).
- **Signed releases** — release artifacts signed with keys stored in HSM or equivalent.
- **SBOM** (Software Bill of Materials) published alongside releases.
- **Build pipeline integrity**: GitHub Actions workflows audited; self-hosted runners for privileged steps.

### Supply chain

- Keep dependencies minimal.
- Prefer widely-used, well-maintained libraries over niche ones.
- Review major dependency updates manually.
- Pin transitive dependency versions where lockfile supports it.
- **No typosquatting susceptibility** — verify package identities.

---

## Data Security

### Access controls

Per `03-data-architecture.md`:

- **Least privilege** by default.
- **RBAC** with roles: `user`, `scholar-reviewer`, `operator`, `security-admin`, `root`.
- **Access audited** — every Layer 4 access logged with operator, reason, scope.
- **Time-bounded access** for elevated privileges.

### Data minimization

- Collect only what is needed.
- Delete on schedule.
- Aggregate where individual data is unnecessary.
- **No "just in case" collection.**

### Layer 4 isolation

- Physically separate production user database from other data.
- No Layer 4 data in dev, staging, or testing environments.
- Synthetic data for dev/test.
- Production data export restricted to named operators with audit.

### Zero-knowledge patterns

For the most sensitive data flows:

- **Client-side encryption** before upload. Keys derived from user credentials; server never sees plaintext.
- **End-to-end encrypted search** (where feasible) using homomorphic techniques or oblivious protocols.
- **Minimal metadata exposure** — even encrypted data leaks information through size and timing; mitigated where practical.

---

## Abuse Prevention

### Account-level

- Rate limiting per account (queries per minute / per day per tier).
- Suspicious activity detection (pattern-based; not user-profiling).
- Account suspension with appeal for confirmed abuse.

### Network-level

- Cloudflare Bot Management for general bot filtering.
- CAPTCHA challenges for suspicious patterns (accessibility-respecting).
- IP-level rate limits (shared IPs treated carefully — Tor/VPN exit nodes).

### Content-level

- LLM prompts include refusal instructions for harmful requests.
- Policy violations logged (without user-identifying details where avoidable) for tuning.
- Human review for edge cases.

### Preventing misuse for surveillance or targeting

Per NN-1 / NN-3 / NN-8:

- **No bulk export APIs** of user interactions to third parties.
- **No user-lookup APIs** that return personal details.
- **No "find users matching criteria"** functionality in admin tools — because that is exactly the capability a compelled government would want.
- **Refusal patterns** in generation for requests that appear aimed at locating or tracking individuals.

---

## Incident Response

### Detection

- Centralized logging with alerting on security-relevant patterns.
- External monitoring (uptime, certificate expiration, DNS integrity).
- User-reported incident channels.

### Response process

Per `../SECURITY.md`:

1. **Acknowledge** within 72 hours of report.
2. **Triage** within 7 days.
3. **Contain** active incidents immediately.
4. **Fix** per severity timelines.
5. **Disclose** via coordinated disclosure + public advisory.

### Playbooks

Documented incident playbooks for:

- Credential compromise (personal account / service account).
- Data breach (confirmed / suspected).
- Ransomware / destructive attack.
- Service compromise (supply chain / deployment).
- Legal process (subpoena received).
- Coordinated misinformation against the project.

Each playbook has clear steps, decision points, and responsible parties. Drills conducted periodically.

### Post-incident

- Root cause analysis published (with user data appropriately redacted).
- Preventative measures documented and implemented.
- Lessons shared via transparency reports.

---

## Compliance

We are not compliance-driven — we are principle-driven — but compliance frameworks that align with our principles are welcome overlays.

- **GDPR** applies by virtue of EU hosting; we comply beyond the minimum.
- **CCPA** applies for California users.
- **SOC 2** may be pursued when institutional customers require it.
- **ISO 27001** may be pursued at organizational maturity.

Non-compliant compliance requirements (e.g. surveillance mandates presented as "compliance") are refused per non-negotiables.

---

## Legal Process

Per `../PRIVACY.md`:

- **Scrutinize every request** for validity.
- **Minimize compliance** to the legal minimum.
- **Notify users** when permitted.
- **Warrant canary** maintained.
- **Transparency reports** summarize requests annually.

Warrant canary structure:

- Published signed statement affirming no secret legal process.
- Signed with PGP key + timestamped.
- Absence of an updated canary is the signal.
- Coordinated with legal counsel in the hosting jurisdiction.

---

## Operational Security for Contributors

Contributors in sensitive roles follow OPSEC guidance:

- **Full disk encryption** on work devices.
- **Password manager** with strong master password.
- **Hardware security key** for all access.
- **Separate work from personal** accounts and devices where practical.
- **Be aware of phishing** — especially spear-phishing targeting the initiative.
- **Travel precautions** when crossing borders with work devices.

Guidance documented in internal OPSEC handbook (Layer 3).

---

## What We Cannot Promise

Honesty about security limits:

- Perfect security does not exist. A sufficiently resourced adversary can compromise any system.
- Our goal: make attacks expensive, observable, attributable, and survivable.
- Transparency about failures is itself a security practice.

---

## Cross-References

- Vulnerability disclosure: `../SECURITY.md`
- Privacy commitment: `../PRIVACY.md`
- Threat model: `../docs/08-project-threat-model.md`
- Layer disclosure: `../docs/03-open-vs-closed.md`
- Non-negotiables: `../docs/07-non-negotiables.md`

---

This document is updated alongside architectural changes. Proposed changes follow ADR process. Security-sensitive changes require additional review.
