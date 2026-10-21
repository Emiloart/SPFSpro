# Security, Privacy, and AI Safety

Security and privacy are product requirements, not launch polish. SPFS Pro handles data that can expose a person's identity, income, debts, purchases, assets, and vulnerabilities. The exact legal obligations depend on the jurisdictions, providers, and services actually launched.

## 1. Security objectives

- Confidentiality: only authorized people and services can access financial data.
- Integrity: financial facts, calculations, consent state, and audit records cannot be silently altered.
- Availability: users can access essential information within published service objectives.
- Accountability: sensitive access and consequential actions are attributable and reviewable.
- Data minimization: collect and retain only what is needed for a disclosed purpose.
- User agency: users can inspect connections, revoke access, export data, and request deletion subject to legal retention requirements.

## 2. Data classification

| Class | Examples | Minimum controls |
|---|---|---|
| Public | Public documentation, published product descriptions | Integrity and change review |
| Internal | Non-sensitive operational runbooks | Least-privilege access |
| Personal | Preferences, support context, device metadata | Access controls, retention limits, encryption |
| Sensitive financial | Transactions, balances, holdings, income, liabilities, tax documents | Strong authorization, encryption, audit, restricted support access, explicit retention policy |
| Secrets | Provider tokens, API keys, signing keys, credentials | Managed secret/key storage, least privilege, rotation, no source-control exposure |

Avoid placing full transaction descriptions, account numbers, tax identifiers, access tokens, or raw prompts containing sensitive data in application logs.

## 3. Identity and authorization

- Use a proven OIDC/OAuth identity provider rather than custom password/session cryptography.
- Prefer passkeys/WebAuthn where supported; provide accessible recovery and step-up authentication.
- Use short-lived access tokens and safe refresh-token rotation/revocation.
- Enforce authorization server-side on every request, background job, export, retrieval, and tool call.
- Treat user ID values supplied by clients as untrusted; derive identity from the authenticated context.
- Use least-privilege service identities and separate production from development/test environments.
- Require re-authentication or step-up verification for sensitive account changes and high-impact actions.
- Support session/device review and revocation.

## 4. Data protection

- Encrypt data in transit using modern TLS and at rest using managed encryption.
- Store provider credentials in managed secret storage; never in browser/mobile local storage or application databases without an explicit, reviewed encryption design.
- Keep cryptographic keys separate from data access; define rotation, revocation, and recovery procedures.
- Restrict production data access to a small, approved set of operators; log and periodically review privileged access.
- Use synthetic or irreversibly de-identified data for development and tests.
- Establish retention schedules for raw imports, transaction history, logs, backups, prompts, model evaluations, and support records.
- Provide export, consent revocation, and deletion flows; explain where legal obligations require retention or where backup expiry is delayed.
- Review vendor subprocessors, retention controls, data residency, and training-use terms before sending sensitive data to third parties.

## 5. Financial correctness and high-impact actions

The AI model must not be the authority for financial state or execution.

- Compute authoritative values in deterministic, versioned application logic.
- Validate currency, scale, sign, date, ownership, and source provenance.
- Make write operations idempotent and safe under retry.
- Use explicit state machines for any consequential workflow.
- Require policy checks and appropriate user confirmation before an external action.
- Use append-only audit events and reversals/corrections rather than silent edits for important records.
- Reconcile external-provider state after any supported action.
- Keep an emergency kill switch for each action category.
- Separate read permissions from write permissions and from authorization to initiate an action.

## 6. AI-specific controls

### Data and prompt boundaries
- Send only the minimum data necessary for the requested task.
- Redact or tokenize identifiers that the model does not need.
- Treat provider responses, user uploads, retrieved pages, transaction descriptions, and tool outputs as untrusted content.
- Do not allow retrieved text to override system policy, permissions, or tool allowlists.
- Never put secrets or hidden system instructions into user-visible model context.
- Apply authorization before retrieval and re-check it before returning results.

### Tool design
- Use narrow, typed schemas with enumerated actions and bounded inputs.
- No arbitrary SQL, shell, unrestricted HTTP requests, or direct credential access for the model.
- Read-only tools are the default.
- High-impact actions must pass deterministic validation and explicit confirmation outside the model.
- Validate model output against schemas and independent financial calculations.
- Reject unsupported claims, missing source references, invalid currencies, and inconsistent arithmetic.

### Evaluation and monitoring
- Maintain versioned test cases for arithmetic consistency, groundedness, refusal behavior, privacy leakage, prompt injection, localization, and vulnerable-user scenarios.
- Evaluate recommendations against a documented policy and reviewed reference material.
- Track forecast calibration and error by segment; do not rely on aggregate accuracy alone.
- Version prompts, models, tools, retrieval corpora, and policy rules.
- Provide a way to disable a model/provider or feature without disabling core account access.
- Do not describe generated advice as guaranteed, certified, or personalized professional advice unless that claim is justified and legally approved.

## 7. Threat model starter list

| Threat | Example | Core mitigations |
|---|---|---|
| Account takeover | Attacker gains access to a financial profile | Passkeys/MFA, risk-based step-up, session revocation, alerts |
| Broken object authorization | User fetches another user's transactions | Server-side ownership checks, tenant-isolation tests |
| Provider-token compromise | Bank-link token is exposed | Secret manager, narrow scope, redaction, rotation/revocation |
| Prompt injection | Transaction memo or uploaded document instructs model to reveal data | Treat content as data, tool allowlists, output validation |
| Data exfiltration | Excessive support or analytics access | Least privilege, minimization, audit, access review |
| Duplicate execution | Retry creates duplicate action | Idempotency keys, state machine, reconciliation |
| Supply-chain compromise | Malicious dependency or CI action | Lockfiles, pinned actions, scanning, protected CI permissions |
| Regional outage | Region or provider unavailable | Graceful degradation, backups, tested recovery, status communication |
| Forecast error | User over-relies on an inaccurate projection | Scenario ranges, freshness markers, calibration, limitations |
| Insider misuse | Employee views sensitive financial details | JIT access, approval, audit, alerts, periodic review |

This is a starter list and must be expanded through a formal threat-model workshop before production.

## 8. Compliance and launch governance

Do not assume one certification or legal framework is sufficient worldwide. Before launching in each market, map the actual product activities and data flows to applicable obligations, which may include privacy/data protection, consumer protection, financial promotions/advice, open banking, payment services, investment services, tax-preparation rules, outsourcing, and breach reporting.

Maintain a launch dossier containing:
- legal entity and product-role analysis;
- jurisdiction and feature scope;
- provider agreements and permitted use;
- data inventory, purpose, lawful basis/consent, retention, and transfer mapping;
- security and privacy risk assessments;
- incident response and breach notification plan;
- customer disclosures and support escalation;
- accessibility and localization review;
- evidence for any public security/compliance claims.

Never claim GDPR, PCI DSS, SOC 2, ISO 27001, or other compliance/certification status until the scope has been independently assessed and the claim is accurate.

## 9. Incident response

Define named incident roles and severity levels before launch. Maintain playbooks for account compromise, leaked secrets, provider outage, unauthorized access, incorrect financial calculation, AI data leakage, and regional failure. Preserve relevant evidence, contain the issue, revoke credentials when needed, communicate accurately, and complete a blameless post-incident review with tracked corrective actions.

## 10. Release checklist

- [ ] Data flow and threat model reviewed
- [ ] Authentication, authorization, and tenant-isolation tests pass
- [ ] Sensitive data redaction checked in logs/traces/prompts
- [ ] Secrets and dependency scanning enabled
- [ ] Encryption, key ownership, and rotation reviewed
- [ ] Export, revocation, deletion, and retention behavior tested
- [ ] Financial calculations tested with edge cases and independent invariants
- [ ] AI evaluation and prompt-injection tests pass for enabled features
- [ ] Backup restoration and incident playbooks exercised
- [ ] Regional legal, provider, and disclosure approvals complete
- [ ] Rollback and feature kill switches verified
