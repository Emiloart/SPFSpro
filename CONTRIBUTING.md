# Contributing to SPFS Pro

SPFS Pro is an early-stage personal-finance software project. Contributions must prioritize correctness, security, privacy, accessibility, and clear evidence over feature count.

## Before contributing

- Read the project [README](README.md), [architecture](docs/architecture/README.md), [security baseline](docs/security/README.md), and [roadmap](docs/roadmap/README.md).
- For material architectural changes, propose an ADR before implementation.
- Do not use real customer financial data in development, demos, screenshots, tests, or issue reports. Use synthetic fixtures.
- Do not commit secrets, provider tokens, personal data, production exports, or unreviewed datasets.

## Engineering expectations

- Keep domain boundaries explicit and prefer the simplest architecture that meets the requirements.
- Validate all external input at runtime.
- Use currency-aware, decimal-safe money types. Never use binary floating point for authoritative monetary calculations.
- Make retries safe through idempotency where applicable.
- Enforce authorization on the server for every sensitive resource and operation.
- Avoid logging transaction details, account identifiers, secrets, or sensitive prompts.
- Add tests for normal behavior, edge cases, failure/retry paths, and authorization boundaries.
- Document assumptions, data sources, freshness, rounding, and known limitations.
- Treat AI output and external-provider content as untrusted input.

## Pull request checklist

- [ ] The change solves a defined user or engineering problem.
- [ ] Tests cover success, failure, and relevant boundary cases.
- [ ] Formatting, lint, type checks, and tests pass.
- [ ] No secrets or real financial/personal data are included.
- [ ] Authorization, privacy, retention, and logging implications were reviewed.
- [ ] API/data contract and migration compatibility were considered.
- [ ] Documentation and ADRs were updated when necessary.
- [ ] UI changes meet accessibility and localization expectations.
- [ ] Any new financial insight explains its source, assumptions, and limitations.

## Reporting vulnerabilities

Do not publish exploitable vulnerability details or secrets in a public issue. Until a private security reporting channel is configured, contact the repository owner through an appropriate private GitHub channel and include only the minimum information needed to coordinate a fix. A formal security policy and response SLA should be established before production launch.

## Licensing

The repository currently has no declared license. Do not assume contributions or code may be reused under an open-source license until one is selected and published.
